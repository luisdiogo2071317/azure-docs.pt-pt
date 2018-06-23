---
title: Utilize a funcionalidade em grande escala de rostos em API | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Utilize a funcionalidade em grande escala a letra de cognitivos serviços da API.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: f7b3ac57cf6b24c8a90b4ea59757d3a2cfafd781
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351860"
---
# <a name="how-to-use-the-large-scale-feature"></a>Como utilizar a funcionalidade em grande escala

Este guia se encontra num artigo avançado na migração de código para aumentar verticalmente de existentes PersonGroup e FaceList LargePersonGroup e LargeFaceList respetivamente.
Este guia demonstra o processo de migração com pressuposto de saber a utilização básica de PersonGroup e FaceList.
Para obter familiarizado com operações básicas, consulte outros tutoriais como [como identificar faces nas imagens](HowtoIdentifyFacesinImage.md),

API de rostos em Microsoft lançou recentemente duas funcionalidades para permitir cenários em grande escala, LargePersonGroup e LargeFaceList, coletivamente referidas como operações em grande escala.
LargePersonGroup pode conter até 1.000.000 pessoas com um máximo de 248 faces e LargeFaceList pode conter até 1.000.000 faces.

As operações de grande escala são semelhantes aos PersonGroup e FaceList convencionais, mas tem algumas diferenças importantes devido a nova arquitetura.
Este guia demonstra o processo de migração com pressuposto de saber a utilização básica de PersonGroup e FaceList.
Os exemplos são escritos em c# utilizando a biblioteca de cliente de API de letra.

Para ativar o desempenho de pesquisa de letra para identificação e FindSimilar em grande escala, terá de introduzir uma operação de formação para processar previamente os LargeFaceList e LargePersonGroup.
O tempo de formação varia de segundos a prestes a meia hora consoante a capacidade real.
Durante o período de formação, é possível efetuar a identificação e FindSimilar se é efetuada uma formação com êxito antes.
No entanto, a desvantagem é que a pessoas/faces foram adicionadas novas não serão apresentados nos resultados até que uma nova migração de post para formação em grande escala esteja concluída.

## <a name="concepts"></a> Conceitos

Se não estiver familiarizado com os seguintes conceitos neste guia, as definições podem ser encontradas no [glossário](../Glossary.md):

- LargePersonGroup: Uma coleção de pessoas com capacidade até 1,000,000.
- LargeFaceList: Uma coleção de Faces com capacidade até 1,000,000.
- Formação: Um processo pré-lançamento para assegurar o desempenho de identificação/FindSimilar.
- Identificação: Identifica faces um ou mais de um PersonGroup ou LargePersonGroup.
- FindSimilar: Procure faces semelhantes de FaceList ou LargeFaceList.

## <a name="initialization"></a> Passo 1: Autorizar a chamada de API

Quando utilizar a biblioteca de clientes enfrentam reside API, a chave de subscrição e o ponto final de subscrição passam no construtor da classe FaceServiceClient. Por exemplo:

```CSharp
string SubscriptionKey = "<Subscription Key>";
// Use your own subscription endpoint corresponding to the subscription key.
string SubscriptionRegion = "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/";
FaceServiceClient FaceServiceClient = new FaceServiceClient(SubscriptionKey, SubscriptionRegion);
```

A chave de subscrição com o ponto final correspondente pode ser obtida a partir da página do portal do Azure Marketplace.
Consulte [subscrições](https://azure.microsoft.com/services/cognitive-services/directory/vision/).

## <a name="migrate"></a> Passo 2: Migração de código em ação

Esta secção centra-se na implementação de PersonGroup/FaceList migrar para LargePersonGroup/LargeFaceList.
Embora LargePersonGroup/LargeFaceList difere PersonGroup/FaceList estrutura e implementação interna, as interfaces de API são semelhantes de back-compatibilidade.

Não é suportada a migração de dados, terá de recriar o LargePersonGroup/LargeFaceList em vez disso.

## <a name="largepersongroup"></a> Passo 2.1: Migrar PersonGroup para LargePersonGroup

A migração de PersonGroup para LargePersonGroup é uniforme como partilham exatamente as mesmas operações de nível de grupo.

Para PersonGroup/pessoa relacionadas com a implementação, só é necessário alterar os caminhos de API ou SDK classe/módulo LargePersonGroup e LargePersonGroup pessoa.

Em termos de migração de dados, consulte [como adicionar enfrenta](how-to-add-faces.md) para referência.

## <a name="largefacelist"></a> Passo 2.2: Migrar FaceList para LargeFaceList

| APIs de FaceList | APIs de LargeFaceList |
|:---:|:---:|
| Criar | Criar |
| Eliminar | Eliminar |
| Get | Get |
| Lista | Lista |
| Atualizar | Atualizar |
| - | Formação |
| - | Obter o estado de preparação |

A tabela anterior é uma comparação das operações de nível de lista entre FaceList e LargeFaceList.
Como é mostrado, LargeFaceList inclui novas operações, formação e obter o estado de preparação, quando comparados com FaceList.
Obter LargeFaceList preparado é pré-condição de [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) operação ao daí não é nenhum formação necessária para FaceList.
O fragmento seguinte é uma função de programa auxiliar de aguardar a formação de um LargeFaceList.

```CSharp
/// <summary>
/// Helper function to train LargeFaceList and wait for finish.
/// </summary>
/// <remarks>
/// The time interval can be adjusted considering the following factors:
/// - The training time which depends on the capacity of the LargeFaceList.
/// - The acceptable latency for getting the training status.
/// - The call frequency and cost.
///
/// Estimated training time for LargeFaceList in different scale:
/// -     1,000 faces cost about  1 to  2 seconds.
/// -    10,000 faces cost about  5 to 10 seconds.
/// -   100,000 faces cost about  1 to  2 minutes.
/// - 1,000,000 faces cost about 10 to 30 minutes.
/// </remarks>
/// <param name="largeFaceListId">The Id of the LargeFaceList for training.</param>
/// <param name="timeIntervalInMilliseconds">The time interval for getting training status in milliseconds.</param>
/// <returns>A task of waiting for LargeFaceList training finish.</returns>
private static async Task TrainLargeFaceList(
    string largeFaceListId,
    int timeIntervalInMilliseconds = 1000)
{
    // Trigger a train call.
    await FaceServiceClient.TrainLargeFaceListAsync(largeFaceListId);

    // Wait for training finish.
    while (true)
    {
        Task.Delay(timeIntervalInMilliseconds).Wait();
        var status = await FaceServiceClient.GetLargeFaceListTrainingStatusAsync(largeFaceListId);

        if (status.Status == Status.Running)
        {
            continue;
        }
        else if (status.Status == Status.Succeeded)
        {
            break;
        }
        else
        {
            throw new Exception("The train operation is failed!");
        }
    }
}
```

Anteriormente, seria uma utilização típica do FaceList com adicionar faces e FindSimilar

```CSharp
// Create a FaceList.
const string FaceListId = "myfacelistid_001";
const string FaceListName = "MyFaceListDisplayName";
const string ImageDir = @"/path/to/FaceList/images";
FaceServiceClient.CreateFaceListAsync(FaceListId, FaceListName).Wait();

// Add Faces to the FaceList.
Parallel.ForEach(
    Directory.GetFiles(ImageDir, "*.jpg"),
    async imagePath =>
        {
            using (Stream stream = File.OpenRead(imagePath))
            {
                await FaceServiceClient.AddFaceToFaceListAsync(FaceListId, stream);
            }
        });

// Perform FindSimilar.
const string QueryImagePath = @"/path/to/query/image";
var results = new List<SimilarPersistedFace[]>();
using (Stream stream = File.OpenRead(QueryImagePath))
{
    var faces = FaceServiceClient.DetectAsync(stream).Result;
    foreach (var face in faces)
    {
        results.Add(await FaceServiceClient.FindSimilarAsync(face.FaceId, FaceListId, 20));
    }
}
```

Quando migrá-lo para LargeFaceList, este deve tornar-se

```CSharp
// Create a LargeFaceList.
const string LargeFaceListId = "mylargefacelistid_001";
const string LargeFaceListName = "MyLargeFaceListDisplayName";
const string ImageDir = @"/path/to/FaceList/images";
FaceServiceClient.CreateLargeFaceListAsync(LargeFaceListId, LargeFaceListName).Wait();

// Add Faces to the LargeFaceList.
Parallel.ForEach(
    Directory.GetFiles(ImageDir, "*.jpg"),
    async imagePath =>
        {
            using (Stream stream = File.OpenRead(imagePath))
            {
                await FaceServiceClient.AddFaceToLargeFaceListAsync(LargeFaceListId, stream);
            }
        });

// Train() is newly added operation for LargeFaceList.
// Must call it before FindSimilarAsync() to ensure the newly added faces searchable.
await TrainLargeFaceList(LargeFaceListId);

// Perform FindSimilar.
const string QueryImagePath = @"/path/to/query/image";
var results = new List<SimilarPersistedFace[]>();
using (Stream stream = File.OpenRead(QueryImagePath))
{
    var faces = FaceServiceClient.DetectAsync(stream).Result;
    foreach (var face in faces)
    {
        results.Add(await FaceServiceClient.FindSimilarAsync(face.FaceId, largeFaceListId: LargeFaceListId));
    }
}
```

Conforme é mostrado acima, a gestão de dados e a parte FindSimilar são quase os mesmos.
A única exceção é que uma operação de formação previamente processamento raiz deve ser concluída no LargeFaceList antes de FindSimilar funciona.

## <a name="train"></a>Passo 3: Sugestões de formação

Apesar da operação de formação acelera o [FindSimilar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) e [identificação](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), o tempo de formação sofrerá especialmente quando estiver a grande escala.
O tempo estimado formação em diferentes escalas está listado na seguinte tabela:

| Escala (faces ou pessoas) | Tempo de preparação de estimado |
|:---:|:---:|
| 1,000 | s 1-2 |
| 10,000 | 5 a 10 s |
| 100,000 | 1 - mínimo de 2 |
| 1 000 000 | 10 - 30 minutos |

Para melhor utilizar a funcionalidade em grande escala, são recomendadas algumas estratégias para tem em consideração.

## <a name="interval"></a>Passo 3.1: Personalizar o intervalo de tempo

Como é mostrado no `TrainLargeFaceList()`, há um `timeIntervalInMilliseconds` para atrasar o estado de preparação infinita a verificar o processo.
Para LargeFaceList com mais faces, utilizar um intervalo maior reduz as contagens de chamada e os custos.
O intervalo de tempo deve ser personalizado, de acordo com a capacidade da LargeFaceList esperada.

Estratégia de mesma também se aplica a LargePersonGroup.
Por exemplo, quando se prepara um LargePersonGroup com 1.000.000 pessoas, o `timeIntervalInMilliseconds` dever 60 000 (a.k.a. intervalo de 1 minuto).

## <a name="buffer"></a>Memória intermédia de pequena escala passo 3.2

Pessoas/Faces no LargePersonGroup/LargeFaceList são pesquisáveis apenas após a ser preparado.
Num cenário dinâmico, novo pessoas/faces são constantemente adicionadas e tem de ser imediatamente pesquisável ainda formação pode demorar mais do que o pretendido.
Para atenuar este problema, pode utilizar um LargePersonGroup/LargeFaceList extra pequena escala como uma memória intermédia apenas para as entradas adicionadas recentemente.
Esta memória intermédia demora algum tempo mais curto para preparar devido a muito mais pequeno e a pesquisa imediata desta memória intermédia temporária deverão funcionar.
Utilize desta memória intermédia em combinação com formação em LargePersonGroup/LargeFaceList mestra executando a formação principal num intervalo mais dispersa, por exemplo, no noite intermédio e diariamente.

Um fluxo de trabalho de exemplo:
1. Crie um mestre LargePersonGroup/LargeFaceList (coleção principal) e uma memória intermédia LargePersonGroup/LargeFaceList (coleção de memória intermédia). A coleção de memória intermédia é apenas para pessoas/Faces adicionados recentemente.
1. Adicione novo pessoas/Faces para a coleção principal e a coleção de memória intermédia.
1. Preparar apenas a coleção de memória intermédia com um intervalo de tempo curto para garantir que as entradas recém-adicionado tendo efeito.
1. Chamar identificação/FindSimilar contra a coleção principal e a coleção de memória intermédia e intercale os resultados.
1. Quando aumenta o tamanho de coleção de memória intermédia para um limiar ou a um tempo de inatividade do sistema, crie uma nova coleção de memória intermédia e acionar o comboio sai coleção principal.
1. Elimine a coleção de memória intermédia antigo após a conclusão de formação na coleção de principal.

## <a name="standalone"></a>Passo 3.3 autónomo formação

Se uma latência relativamente longa é aceitável, não é necessário acionar a operação de formação imediatamente após a adição de novos dados.
Em vez disso, a operação de formação pode ser a lógica principal de divisão e acionada regularmente.
Esta estratégia é adequada para cenários dinâmicos com latência aceitável e pode ser aplicada aos cenários estáticos para reduzir a frequência de formação.

Suponhamos há um `TrainLargePersonGroup` função semelhante do `TrainLargeFaceList`.
Uma implementação típica de autónoma formação em LargePersonGroup invocando o [ `Timer` ](https://msdn.microsoft.com/library/system.timers.timer(v=vs.110).aspx) classe no `System.Timers` seria:

```CSharp
private static void Main()
{
    // Create a LargePersonGroup.
    const string LargePersonGroupId = "mylargepersongroupid_001";
    const string LargePersonGroupName = "MyLargePersonGroupDisplayName";
    FaceServiceClient.CreateLargePersonGroupAsync(LargePersonGroupId, LargePersonGroupName).Wait();

    // Setup a standalone training at regular intervals.
    const int TimeIntervalForStatus = 1000 * 60; // 1 minute interval for getting training status.
    const double TimeIntervalForTrain = 1000 * 60 * 60; // 1 hour interval for training.
    var trainTimer = new Timer(TimeIntervalForTrain);
    trainTimer.Elapsed += (sender, args) => TrainTimerOnElapsed(LargePersonGroupId, TimeIntervalForStatus);
    trainTimer.AutoReset = true;
    trainTimer.Enabled = true;

    // Other operations like creating persons, adding faces and Identification except for Train.
    // ...
}

private static void TrainTimerOnElapsed(string largePersonGroupId, int timeIntervalInMilliseconds)
{
    TrainLargePersonGroup(largePersonGroupId, timeIntervalInMilliseconds).Wait();
}
```

Mais informações sobre a gestão de dados e de implementações relacionadas com a identificação, consulte [como adicionar enfrenta](how-to-add-faces.md) e [como identificar enfrenta na imagem](HowtoIdentifyFacesinImage.md).

## <a name="summary"></a> Resumo

Neste guia, aprendeu como migrar o código de PersonGroup/FaceList existente (não dados) para LargePersonGroup/LargeFaceList:

- LargePersonGroup e LargeFaceList funciona semelhante PersonGroup/FaceList, exceto LargeFaceList necessita de operação de formação.
- A atualização de dados dinâmicos para o conjunto de dados em grande escala, até a estratégia de formação correta.

## <a name="related"></a> Tópicos relacionados

- [Como identificar enfrenta na imagem](HowtoIdentifyFacesinImage.md)
- [Como adicionar enfrenta](how-to-add-faces.md)
