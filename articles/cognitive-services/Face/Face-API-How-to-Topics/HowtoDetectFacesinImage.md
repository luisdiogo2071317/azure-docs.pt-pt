---
title: 'Exemplo: Detetar rostos em imagens – API Face'
titleSuffix: Azure Cognitive Services
description: Utilize a API Face para detetar rostos em imagens.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: sample
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: a4c74ff70a4426abf97562bf997479a91afbf17a
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46124053"
---
# <a name="example-how-to-detect-faces-in-image"></a>Exemplo: Como Detetar Rostos na Imagem

Este guia demonstrará como detetar rostos numa imagem, com atributos faciais, como género, idade ou postura extraídos. Os exemplos são escritos em C# com a biblioteca de cliente da API Face. 

## <a name="concepts"></a>Conceitos

Se não estiver familiarizado com algum dos seguintes conceitos neste guia, veja as definições no nosso [Glossário](../Glossary.md) a qualquer momento: 

- Deteção de rostos
- Pontos de referência do rosto
- Postura da cabeça
- Atributos do rosto

## <a name="preparation"></a>Preparação

Neste exemplo, vamos demonstrar as seguintes funcionalidades: 

- Detetar rostos numa imagem e marcá-los através de moldura retangular
- Analisar as localizações de pupilas, do nariz ou da boca e, em seguida, marcá-los na imagem
- Analisar a postura da cabeça, género e idade do rosto

Para executar estas funcionalidades, terá de preparar uma imagem com, pelo menos, um rosto claro. 

## <a name="step-1-authorize-the-api-call"></a>Passo 1: Autorizar a chamada da API

Todas as chamadas para a API Face requerem uma chave de subscrição. Esta chave precisa de passar por um parâmetro de cadeia de consulta ou pode ser especificada no cabeçalho do pedido. Para passar a chave de subscrição através da cadeia de consulta, veja o URL do pedido para [Rosto – Detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) como exemplo:

```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription Key>
```

Como alternativa, a chave de subscrição também pode ser especificada no cabeçalho do pedido HTTP: **ocp-apim-subscription-key: &lt;Chave de Subscrição&gt;**  Quando utilizar uma biblioteca de cliente, a chave de subscrição é passada através do construtor da classe FaceServiceClient. Por exemplo:
```CSharp
faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

## <a name="step-2-upload-an-image-to-the-service-and-execute-face-detection"></a>Passo 2: Carregar uma imagem para o serviço e executar a deteção de rostos

A forma mais básica de executar a deteção de rostos é carregar uma imagem diretamente. Isso é feito ao enviar um pedido de "POST" com o tipo de conteúdo application/octet-stream, com os dados lidos a partir de uma imagem JPEG. O tamanho máximo da imagem são 4 MB.

Com a biblioteca de cliente, a deteção de rostos por meio de carregamento é feita ao transmitir um objeto de Transmissão em fluxo. Veja o exemplo abaixo:

```CSharp
using (Stream s = File.OpenRead(@"D:\MyPictures\image1.jpg"))
{
    var faces = await faceServiceClient.DetectAsync(s, true, true);
 
    foreach (var face in faces)
    {
        var rect = face.FaceRectangle;
        var landmarks = face.FaceLandmarks;
    }
}
```

Observe que o método DetectAsync do FaceServiceClient é assíncrono. O método de chamada deve ser marcado como async (assíncrono), para poder utilizar a cláusula wait (aguardar).
Se a imagem já se encontra na Web e tem um URL, a deteção de rostos pode ser executada ao indicar também o URL. Neste exemplo, o corpo do pedido será uma cadeia JSON, que contém o URL.
Com a biblioteca de cliente, a deteção de rostos por meio de um URL pode ser executada facilmente ao utilizar outra sobrecarga do método DetectAsync.

```CSharp
string imageUrl = "http://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceServiceClient.DetectAsync(imageUrl, true, true);
 
foreach (var face in faces)
{
    var rect = face.FaceRectangle;
    var landmarks = face.FaceLandmarks;
}
``` 

A propriedade de FaceRectangle que é devolvida com rostos detetados é, essencialmente, localizações no rosto em pixéis. Geralmente, este retângulo contém os olhos, as sobrancelhas, o nariz e a boca – a parte superior da cabeça, as orelhas e o queixo não estão incluídos. Se recortar um retrato de cabeça completa ou um plano médio (uma imagem de tipo fotografia do ID), poderá querer expandir a área da moldura de rosto retangular porque a área do rosto pode ser demasiado pequena para algumas aplicações. Para localizar um rosto com maior precisão, utilizar pontos de referência do rosto (funcionalidades de localização no rosto ou mecanismos de direção de rosto) descritos na seguinte seção vai-se revelar útil.

## <a name="step-3-understanding-and-using-face-landmarks"></a>Passo 3: Compreender e utilizar pontos de referência do rosto

Os pontos de referência do rosto são uma série de pontos detalhados de um rosto; normalmente, os pontos dos componentes de rostos, como as pupilas, o canthus ou o nariz. Os pontos de referência do rosto são atributos opcionais que podem ser analisados durante a deteção de rostos. Pode passar 'true' como um valor booleano para o parâmetro de consulta returnFaceLandmarks ao chamar [Rosto – Detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) ou utilizar o parâmetro opcional returnFaceLandmarks para o método DetectAsync classe FaceServiceClient de modo a incluir os pontos de referência do rosto nos resultados da deteção.

Por predefinição, existem 27 pontos de referência predefinidos. A figura a seguir mostra como todos os 27 pontos são definidos:

![HowToDetectFace](../Images/landmarks.1.jpg)

Os pontos retornados são em unidades de pixéis, tal como a moldura retangular do rosto. Portanto, facilitam a marcação de pontos específicos de interesse na imagem. O código a seguir demonstra a obtenção das localizações do nariz e das pupilas:

```CSharp
var faces = await faceServiceClient.DetectAsync(imageUrl, returnFaceLandmarks:true);
 
foreach (var face in faces)
{
    var rect = face.FaceRectangle;
    var landmarks = face.FaceLandmarks;
 
    double noseX = landmarks.NoseTip.X;
    double noseY = landmarks.NoseTip.Y;
 
    double leftPupilX = landmarks.PupilLeft.X;
    double leftPupilY = landmarks.PupilLeft.Y;
 
    double rightPupilX = landmarks.PupilRight.X;
    double rightPupilY = landmarks.PupilRight.Y;
}
``` 

Além de marcar características de rostos numa imagem, os pontos de referência do rosto também podem ser utilizados para calcular com precisão a direção do rosto. Por exemplo, podemos definir a direção do rosto como um vetor do centro da boca ao centro dos olhos. O código abaixo explica-o em detalhe:

```CSharp
var landmarks = face.FaceLandmarks;
 
var upperLipBottom = landmarks.UpperLipBottom;
var underLipTop = landmarks.UnderLipTop;
 
var centerOfMouth = new Point(
    (upperLipBottom.X + underLipTop.X) / 2,
    (upperLipBottom.Y + underLipTop.Y) / 2);
 
var eyeLeftInner = landmarks.EyeLeftInner;
var eyeRightInner = landmarks.EyeRightInner;
 
var centerOfTwoEyes = new Point(
    (eyeLeftInner.X + eyeRightInner.X) / 2,
    (eyeLeftInner.Y + eyeRightInner.Y) / 2);
 
Vector faceDirection = new Vector(
    centerOfTwoEyes.X - centerOfMouth.X,
    centerOfTwoEyes.Y - centerOfMouth.Y);
``` 

Ao saber a direção em que está o rosto, então pode girar a moldura retangular do rosto e alinhá-lo com o mesmo. É evidente que a utilização de pontos de referência do rosto pode oferecer mais detalhes e ser mais útil.

## <a name="step-4-using-other-face-attributes"></a>Passo 4: Utilizar outros atributos faciais

Além dos pontos de referência do rosto, a API Face – Detetar também pode analisar vários outros atributos num rosto. Estes atributos incluem:

- Idade
- Género
- Intensidade do sorriso
- Pelo facial
- Postura da cabeça a 3D

Estes atributos são previstos com algoritmos estatísticos e podem nem sempre ser 100% exatos. No entanto, eles são ainda úteis quando pretende classificar rostos por estes atributos. Para obter mais informações sobre cada um dos atributos, veja o [Glossário](../Glossary.md).

Segue-se um exemplo simples de extração de atributos faciais durante a deteção de rostos:

```CSharp
var requiredFaceAttributes = new FaceAttributeType[] {
                FaceAttributeType.Age,
                FaceAttributeType.Gender,
                FaceAttributeType.Smile,
                FaceAttributeType.FacialHair,
                FaceAttributeType.HeadPose,
                FaceAttributeType.Glasses
            };
var faces = await faceServiceClient.DetectAsync(imageUrl,
    returnFaceLandmarks: true,
    returnFaceAttributes: requiredFaceAttributes);

foreach (var face in faces)
{
    var id = face.FaceId;
    var attributes = face.FaceAttributes;
    var age = attributes.Age;
    var gender = attributes.Gender;
    var smile = attributes.Smile;
    var facialHair = attributes.FacialHair;
    var headPose = attributes.HeadPose;
    var glasses = attributes.Glasses;
}
``` 

## <a name="summary"></a>Resumo

Neste guia aprendeu as funcionalidades da API [Rosto – Detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), como pode detetar rostos para imagens carregadas localmente ou em URLs de imagens na Web; como pode detetar rostos ao devolver molduras de rosto retangulares; e como também pode analisar pontos de referência do rosto, posturas da cabeça a 3D e outros atributos faciais.

Para obter mais informações sobre os detalhes da API, veja o guia de referência da API para [Rosto – Detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="related-topics"></a>Tópicos relacionados

[Como Identificar Rostos na Imagem](HowtoIdentifyFacesinImage.md)
