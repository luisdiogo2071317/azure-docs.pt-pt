---
title: Adicionar faces com a API de rostos em | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Utilize a API de rostos em serviços cognitivos para adicionar faces nas imagens.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 3306c13d6c3d231ddbda38cfcbc5419fcdbd30db
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351572"
---
# <a name="how-to-add-faces"></a>Como adicionar faces

Este guia demonstra a melhor prática para adicionar grande número de pessoas e faces para um PersonGroup.
A estratégia de mesma também se aplica a FaceList e LargePersonGroup.
Os exemplos são escritos em c# utilizando a biblioteca de cliente de API de letra.

## <a name="step1"></a> Passo 1: inicialização

Várias variáveis declaradas e uma função de programa auxiliar é implementada para agendar os pedidos.

- `PersonCount` é o número total de pessoas.
- `CallLimitPerSecond` é as chamadas máximas por segundo, de acordo com a camada de subscrição.
- `_timeStampQueue` é uma fila para registar os carimbos de pedido.
- `await WaitCallLimitPerSecondAsync()` irá esperar até que seja válido para enviar o pedido seguinte.

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

## <a name="step2"></a> Passo 2: Autorizar a chamada de API

Quando utilizar uma biblioteca de cliente, a chave de subscrição é transmitida através do construtor de classe FaceServiceClient. Por exemplo:

```CSharp
FaceServiceClient faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

A chave de subscrição pode ser obtida a partir da página do portal do Azure Marketplace. Consulte [subscrições](https://www.microsoft.com/cognitive-services/en-us/sign-up).

## <a name="step3"></a> Passo 3: Criar o PersonGroup

É criado um PersonGroup com o nome "MyPersonGroup" para guardar as pessoas.
O tempo do pedido é colocados em fila para `_timeStampQueue` para garantir que a validação geral.

```CSharp
const string personGroupId = "mypersongroupid";
const string personGroupName = "MyPersonGroup";
_timeStampQueue.Enqueue(DateTime.UtcNow);
await faceServiceClient.CreatePersonGroupAsync(personGroupId, personGroupName);
```

## <a name="step4"></a> Passo 4: Criar as pessoas a PersonGroup

Pessoas que é criados em simultâneo e `await WaitCallLimitPerSecondAsync()` também é aplicada para evitar exceder o limite de chamada.

```CSharp
CreatePersonResult[] persons = new CreatePersonResult[PersonCount];
Parallel.For(0, PersonCount, async i =>
{
    await WaitCallLimitPerSecondAsync();

    string personName = $"PersonName#{i}";
    persons[i] = await faceServiceClient.CreatePersonAsync(personGroupId, personName);
});
```

## <a name="step5"></a> Passo 5: Adicionar faces para as pessoas

Adicionar faces para diferentes pessoas que são processados em simultâneo, enquanto para uma pessoa específica é sequencial.
Novamente, `await WaitCallLimitPerSecondAsync()` é invocado para garantir que a frequência de pedido estiver dentro do âmbito de limitação.

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

## <a name="summary"></a> Resumo

Neste guia, aprendeu o processo de criação de um PersonGroup com grande número de pessoas e faces. Vários lembretes:

- Esta estratégia também se aplica a FaceList e LargePersonGroup.
- Adicionar/eliminação faces para FaceLists ou pessoas em LargePersonGroup diferentes podem ser processada em simultâneo.
- Mesmas operações para um FaceList ou pessoa em LargePersonGroup específica devem ser feitas sequencialmente.
- Para manter a simplicidade, o processamento de excepções potenciais for omitido neste guia. Se pretender melhorar robustez mais, deve ser aplicada a política de repetição adequado.

Seguem-se um lembrete rápido das funcionalidades anteriormente explicado e demonstrou:

- Criar PersonGroups utilizando o [PersonGroup - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API
- Criar pessoas utilizando o [PersonGroup pessoa - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API
- Adicionar faces para pessoas com o [PersonGroup pessoa - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) API

## <a name="related"></a> Tópicos relacionados
- [Como identificar enfrenta na imagem](HowtoIdentifyFacesinImage.md)
- [Como detetar enfrenta na imagem](HowtoDetectFacesinImage.md)
- [Como utilizar a funcionalidade em grande escala](how-to-use-large-scale.md)
