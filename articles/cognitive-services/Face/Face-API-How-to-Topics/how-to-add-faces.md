---
title: 'Exemplo: Adicionar rostos - Face API'
titleSuffix: Azure Cognitive Services
description: Utilize a API Face para adicionar rostos em imagens.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 722a09b782c902642b599460835151928c16c5f4
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55859033"
---
# <a name="example-how-to-add-faces"></a>Exemplo: Como adicionar rostos

Este guia apresenta as melhores práticas para adicionar um grande número de pessoas e rostos a um PersonGroup.
A mesma estratégia também se aplica à FaceList e ao LargePersonGroup.
Os exemplos foram escritos em C# com a biblioteca de cliente da API Face.

## <a name="step-1-initialization"></a>Passo 1: Inicialização

São declaradas diversas variáveis e é implementada uma função auxiliar para agendar os pedidos.

- `PersonCount` é o número total de pessoas.
- `CallLimitPerSecond` é o número máximo de chamadas por segundo de acordo com o escalão de subscrição.
- `_timeStampQueue` é uma Fila para registrar os carimbo de data/hora dos pedidos.
- `await WaitCallLimitPerSecondAsync()` irá aguardar até ser válido para enviar o pedido seguinte.

```CSharp
const int PersonCount = 10000;
const int CallLimitPerSecond = 10;
static Queue<DateTime> _timeStampQueue = new Queue<DateTime>(CallLimitPerSecond);

static async Task WaitCallLimitPerSecondAsync()
{
    Monitor.Enter(_timeStampQueue);
    try
    {
        if (_timeStampQueue.Count >= CallLimitPerSecond)
        {
            TimeSpan timeInterval = DateTime.UtcNow - _timeStampQueue.Peek();
            if (timeInterval < TimeSpan.FromSeconds(1))
            {
                await Task.Delay(TimeSpan.FromSeconds(1) - timeInterval);
            }
            _timeStampQueue.Dequeue();
        }
        _timeStampQueue.Enqueue(DateTime.UtcNow);
    }
    finally
    {
        Monitor.Exit(_timeStampQueue);
    }
}
```

## <a name="step-2-authorize-the-api-call"></a>Passo 2: Autorizar a chamada de API

Ao utilizar uma biblioteca de cliente, a chave de subscrição é transmitida através do construtor da classe FaceServiceClient. Por exemplo:

```CSharp
FaceServiceClient faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

A chave de subscrição pode ser obtida da página do Marketplace no portal do Azure. Veja [Subscrições](https://www.microsoft.com/cognitive-services/en-us/sign-up).

## <a name="step-3-create-the-persongroup"></a>Passo 3: Criar o PersonGroup

É criado um PersonGroup com o nome "MyPersonGroup" para guardar as pessoas.
A hora do pedido é colocada em fila em `_timeStampQueue` para garantir a validação geral.

```CSharp
const string personGroupId = "mypersongroupid";
const string personGroupName = "MyPersonGroup";
_timeStampQueue.Enqueue(DateTime.UtcNow);
await faceServiceClient.CreatePersonGroupAsync(personGroupId, personGroupName);
```

## <a name="step-4-create-the-persons-to-the-persongroup"></a>Passo 4: Criar as pessoas para o PersonGroup

São criadas pessoas em simultâneo e `await WaitCallLimitPerSecondAsync()` é também aplicado para evitar exceder o limite de chamadas.

```CSharp
CreatePersonResult[] persons = new CreatePersonResult[PersonCount];
Parallel.For(0, PersonCount, async i =>
{
    await WaitCallLimitPerSecondAsync();

    string personName = $"PersonName#{i}";
    persons[i] = await faceServiceClient.CreatePersonAsync(personGroupId, personName);
});
```

## <a name="step-5-add-faces-to-the-persons"></a>Passo 5: Adicionar rostos para as pessoas

A adição de rostos a diferentes pessoas é processada em simultâneo, enquanto para uma pessoa específica é processada sequencialmente.
Uma vez mais, `await WaitCallLimitPerSecondAsync()` é invocado para garantir que a frequência dos pedidos não excede o âmbito de limitação.

```CSharp
Parallel.For(0, PersonCount, async i =>
{
    Guid personId = persons[i].PersonId;
    string personImageDir = @"/path/to/person/i/images";

    foreach (string imagePath in Directory.GetFiles(personImageDir, "*.jpg"))
    {
        await WaitCallLimitPerSecondAsync();

        using (Stream stream = File.OpenRead(imagePath))
        {
            await faceServiceClient.AddPersonFaceAsync(personGroupId, personId, stream);
        }
    }
});
```

## <a name="summary"></a>Resumo

Neste guia, ficou a conhecer o processo de criação de um PersonGroup com um grande número de pessoas e rostos. Alguns lembretes:

- Esta estratégia também se aplica à FaceList e ao LargePersonGroup.
- A adição/eliminação de rostos de diferentes FaceLists ou Pessoas no LargePersonGroup pode ser processada em simultâneo.
- As mesmas operações para uma FaceList ou Pessoa específica no LargePersonGroup devem ser efetuadas sequencialmente.
- Por uma questão de simplicidade, o processamento de uma potencial exceção foi omitido neste guia. Para obter uma maior robustez, deve ser aplicada uma política de repetição adequada.

Segue-se um lembrete rápido das funcionalidades anteriormente explicadas e demonstradas:

- Criar PersonGroups com a API [PersonGroup – Criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)
- Criar pessoas com a API [Pessoa de PersonGroup – Criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)
- Adicionar rostos a pessoas com a API [Pessoa de PersonGroup – Adicionar Rosto](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)

## <a name="related-topics"></a>Tópicos Relacionados

- [Como Identificar Rostos na Imagem](HowtoIdentifyFacesinImage.md)
- [Como Detetar Rostos na Imagem](HowtoDetectFacesinImage.md)
- [Como utilizar a funcionalidade de grande escala](how-to-use-large-scale.md)
