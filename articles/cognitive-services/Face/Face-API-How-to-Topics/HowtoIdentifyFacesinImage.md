---
title: Identificar faces nas imagens com a API de rostos em | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Utilize a API de rostos em serviços cognitivos para identificar faces nas imagens.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 3f75db176055d9f784ec978497d7cae077ff629f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355388"
---
# <a name="how-to-identify-faces-in-images"></a>Como identificar faces nas imagens

Este guia demonstra como identificar faces desconhecidos utilizando PersonGroups, que são criados de pessoas conhecidas com antecedência. Os exemplos são escritos em c# utilizando a biblioteca de cliente de API de letra.

## <a name="concepts"></a> Conceitos

Se não estiver familiarizado com os seguintes conceitos neste guia,. Procure as definições no nosso [glossário](../Glossary.md) em qualquer altura:

- Enfrentam - detetar
- Enfrentam – identificar
- PersonGroup

## <a name="preparation"></a> Preparação

Neste exemplo, iremos demonstrar os seguintes:

- Como criar um PersonGroup - PersonGroup este contém uma lista de pessoas conhecidas.
- Como atribuir faces para cada pessoa - estes faces são utilizadas como uma linha de base para identificar as pessoas. É recomendado utilizar faces desmarque front, tal como o seu ID fotografia. Um bom conjunto de fotografias deve conter faces da mesma pessoa em diferentes utilizadores constitui, dos clothes cores ou estilos hair.

Para realizar a demonstração deste exemplo, terá de preparar um bunch de imagens:

- Alguns fotografias com a letra da pessoa. [Clique aqui para transferir fotografias de exemplo](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data) para Anna, fatura e Clare.
- Uma série de fotografias de teste, o que pode ou não pode conter faces Anna, fatura ou Clare utilizado para testar a identificação. Também pode selecionar algumas imagens de exemplo a partir da ligação anterior.

## <a name="step1"></a> Passo 1: Autorizar a chamada de API

Cada chamada à API do enfrentam requer uma chave de subscrição. Esta chave pode ser transmitida através de um parâmetro de cadeia de consulta ou especificada no cabeçalho do pedido. Para passar a chave de subscrição através da cadeia de consulta, consulte o URL do pedido para o [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) como exemplo:
```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription key>
```

Como alternativa, a chave de subscrição pode ser também especificada no cabeçalho do pedido HTTP: **ocp apim-subscrição-chave: &lt;chave de subscrição&gt;**  quando utilizar uma biblioteca de cliente, a chave de subscrição é transmitida no através do construtor de classe FaceServiceClient. Por exemplo:
 
```CSharp 
faceServiceClient = new FaceServiceClient("<Subscription Key>");
```
 
A chave de subscrição pode ser obtida a partir da página do portal do Azure Marketplace. Consulte [subscrições](https://azure.microsoft.com/try/cognitive-services/).

## <a name="step2"></a> Passo 2: Criar o PersonGroup

Neste passo, criámos um PersonGroup com o nome "MyFriends" que contém três pessoas: Anna, fatura e Clare. Cada pessoa tenha vários faces registados. Os faces tem de ser detetadas a partir de imagens. Após todos os destes passos, tem um PersonGroup como a imagem seguinte:

![HowToIdentify1](../Images/group.image.1.jpg)

### <a name="step2-1"></a> 2.1 definir pessoas para o PersonGroup
Uma pessoa é uma unidade básica de identificar. Uma pessoa pode ter um ou mais faces conhecidos registados. No entanto, um PersonGroup é uma coleção de pessoas e é definida cada pessoa dentro de um determinado PersonGroup. É feita a identificação contra um PersonGroup. Por isso, a tarefa é criar um PersonGroup e, em seguida, crie as pessoas na mesma, como Anna, fatura e Clare.

Em primeiro lugar, tem de criar um novo PersonGroup. Esta é executada ao utilizar o [PersonGroup - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API. A biblioteca de cliente correspondente API é o método de CreatePersonGroupAsync para a classe de FaceServiceClient. O ID de grupo especificado ao criar o grupo é exclusivo para cada subscrição – também pode obter, atualizar ou eliminar PersonGroups com outras APIs PersonGroup. Depois de um grupo é definido, pessoas, em seguida, podem ser definidas dentro da mesma, utilizando o [PersonGroup pessoa - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API. O método de biblioteca de cliente é CreatePersonAsync. Pode adicionar enfrentam para cada pessoa depois são criados.

```CSharp 
// Create an empty PersonGroup
string personGroupId = "myfriends";
await faceServiceClient.CreatePersonGroupAsync(personGroupId, "My Friends");
 
// Define Anna
CreatePersonResult friend1 = await faceServiceClient.CreatePersonAsync(
    // Id of the PersonGroup that the person belonged to
    personGroupId,    
    // Name of the person
    "Anna"            
);
 
// Define Bill and Clare in the same way
```
### <a name="step2-2"></a> 2.2 detetar faces e registe-os para as pessoas corretas
A deteção é feita ao enviar um pedido de web de "POST" para o [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API com o ficheiro de imagem no corpo do pedido HTTP. Quando utilizar a biblioteca de clientes, a deteção de rostos em é executada através do método DetectAsync para a classe de FaceServiceClient.

Para cada enfrentam detetados pode chamar [PersonGroup pessoa – adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) adicioná-lo para a pessoa correta.

O código seguinte demonstra o processo de como detetar a partir de uma imagem de uma letra e adicioná-lo a uma pessoa:
```CSharp 
// Directory contains image files of Anna
const string friend1ImageDir = @"D:\Pictures\MyFriends\Anna\";
 
foreach (string imagePath in Directory.GetFiles(friend1ImageDir, "*.jpg"))
{
    using (Stream s = File.OpenRead(imagePath))
    {
        // Detect faces in the image and add to Anna
        await faceServiceClient.AddPersonFaceAsync(
            personGroupId, friend1.PersonId, s);
    }
}
// Do the same for Bill and Clare
``` 
Tenha em atenção que, se a imagem contém mais do que uma letra, apenas o maior enfrentam é adicionado. Pode adicionar outros faces à pessoa através da transmissão de uma cadeia no formato de "targetFace = à esquerda, superior, a largura, altura" para [PersonGroup pessoa - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) da API targetFace parâmetro de consulta, ou utilizando o parâmetro opcional targetFace para o Método de AddPersonFaceAsync para adicionar outros faces. Cada enfrentam adicionado à pessoa terá um ID exclusivo enfrentam persistente, que pode ser utilizado na [PersonGroup pessoa – eliminar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523e) e [enfrentam – identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).
## <a name="step3"></a> Passo 3: Preparar o PersonGroup

O PersonGroup tem preparado para que uma identificação pode ser possível efetuar a utilizá-la. Além disso, tem de ser retrained após a adição ou remoção de qualquer pessoa, ou se qualquer pessoa tenha os seus enfrentam registado editá-lo. A formação, é necessário o [PersonGroup – formação](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API. Quando utilizar a biblioteca de clientes, é simplesmente uma chamada ao método TrainPersonGroupAsync:
 
```CSharp 
await faceServiceClient.TrainPersonGroupAsync(personGroupId);
```
 
A formação é um processo assíncrono. Não pode ser concluída, mesmo depois do método TrainPersonGroupAsync devolvido. Poderá ter de consultar o estado de preparação por [PersonGroup - obter o estado de preparação](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395247) método API ou GetPersonGroupTrainingStatusAsync da biblioteca de clientes. O código seguinte demonstra uma lógica simples de esperar PersonGroup formação para concluir:
 
```CSharp 
TrainingStatus trainingStatus = null;
while(true)
{
    trainingStatus = await faceServiceClient.GetPersonGroupTrainingStatusAsync(personGroupId);
 
    if (trainingStatus.Status != Status.Running)
    {
        break;
    }
 
    await Task.Delay(1000);
} 
``` 

## <a name="step4"></a> Passo 4: Identificar um rosto em relação a um PersonGroup definido
Quando efetuar identificações, a API de rostos em pode calcular a semelhança de um rosto teste entre todos os faces dentro de um grupo e devolve o person(s) mais comparáveis para esse teste de letra. Isto é feito através de [enfrentam – identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API ou o método IdentifyAsync da biblioteca de clientes.

A teste letra tem de ser detetada através dos passos anteriores, e, em seguida, o ID de rostos em é passado para o identificar API como um segundo argumento. Vários enfrentam os IDs pode ser identificado em simultâneo, e o resultado irá conter todos os resultados de identificar. Por predefinição, a identidade devolve apenas uma pessoa que corresponda a melhor a letra de teste. Se preferir, pode especificar o parâmetro opcional maxNumOfCandidatesReturned para permitir que a identidade devolver mais candidatos.

O código seguinte demonstra o processo de identificar:
```CSharp 
string testImageFile = @"D:\Pictures\test_img1.jpg";

using (Stream s = File.OpenRead(testImageFile))
{
    var faces = await faceServiceClient.DetectAsync(s);
    var faceIds = faces.Select(face => face.FaceId).ToArray();
 
    var results = await faceServiceClient.IdentifyAsync(personGroupId, faceIds);
    foreach (var identifyResult in results)
    {
        Console.WriteLine("Result of face: {0}", identifyResult.FaceId);
        if (identifyResult.Candidates.Length == 0)
        {
            Console.WriteLine("No one identified");
        }
        else
        {
            // Get top 1 among all candidates returned
            var candidateId = identifyResult.Candidates[0].PersonId;
            var person = await faceServiceClient.GetPersonAsync(personGroupId, candidateId);
            Console.WriteLine("Identified as {0}", person.Name);
        }
    }
}
``` 

Quando tiver concluído os passos, pode tentar identificar faces diferentes e ver se faces Anna, fatura ou Clare podem ser corretamente identificados, de acordo com as imagens carregadas para a deteção de rostos em. Veja os seguintes exemplos:

![HowToIdentify2](../Images/identificationResult.1.jpg )

## <a name="step5"></a> Passo 5: Pedido de grande escala

Como é conhecido, um PersonGroup pode manter até 10 000 pessoas devido à limitação de conceção anterior.
Para obter mais informações sobre a cópia de segurança para cenários de milhões de hiper escala, consulte [como utilizar a funcionalidade em grande escala](how-to-use-large-scale.md).

## <a name="summary"></a> Resumo

Neste guia, aprendeu o processo de criação de um PersonGroup e identificar uma pessoa. Seguem-se um lembrete rápido das funcionalidades anteriormente explicado e demonstrou:

- Detetar enfrenta utilizando o [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d) API
- Criar PersonGroups utilizando o [PersonGroup - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) API
- Criar pessoas utilizando o [PersonGroup pessoa - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c) API
- Dar formação sobre um PersonGroup utilizando o [PersonGroup – formação](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) API
- Identificar faces desconhecidos contra a utilização de PersonGroup o [enfrentam – identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) API

## <a name="related"></a> Tópicos relacionados

- [Como detetar enfrenta na imagem](HowtoDetectFacesinImage.md)
- [Como adicionar enfrenta](how-to-add-faces.md)
- [Como utilizar a funcionalidade em grande escala](how-to-use-large-scale.md)
