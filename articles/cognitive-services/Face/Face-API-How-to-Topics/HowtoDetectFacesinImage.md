---
title: Detetar faces nas imagens com a API de rostos em | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Utilize a API de rostos em serviços cognitivos para detetar faces nas imagens.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 57cd0915450428399fd680638aa4fae2cdbe17c9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351728"
---
# <a name="how-to-detect-faces-in-image"></a>Como detetar enfrenta na imagem

Este guia demonstrar como detetar faces a partir de uma imagem, com atributos de letra, como género, idade ou apresentam extraído. Os exemplos são escritos em c# utilizando a biblioteca de cliente de API de letra. 

## <a name="concepts"></a> Conceitos

Se não estiver familiarizado com qualquer um dos seguintes conceitos neste guia, consulte as definições no nosso [glossário](../Glossary.md) em qualquer altura: 

- Deteção de rostos
- Enfrentam landmarks
- HEAD implicar
- Atributos de rostos em

## <a name="preparation"></a> Preparação

Neste exemplo, vamos demonstrar as seguintes funcionalidades: 

- Detetar faces a partir de uma imagem e marcar utilizando pacotes retangular
- Analisar as localizações do pupils, nose ou boca e, em seguida, marcar a imagem
- Analisar implicar head, sexo e duração do tipo de letra

Para executar estas funcionalidades, terá de preparar uma imagem com pelo menos uma letra limpar. 

## <a name="step1"></a> Passo 1: Autorizar a chamada de API

Cada chamada à API do enfrentam requer uma chave de subscrição. Esta chave tem de ser transmitidos através de um parâmetro de cadeia de consulta, ou especificado no cabeçalho do pedido. Para passar a chave de subscrição através da cadeia de consulta, consulte o URL do pedido para o [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) como exemplo:

```
https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes]
&subscription-key=<Subscription Key>
```

Como alternativa, a chave de subscrição pode ser também especificada no cabeçalho do pedido HTTP: **ocp apim-subscrição-chave: &lt;chave de subscrição&gt;**  quando utilizar uma biblioteca de cliente, a chave de subscrição é transmitida no através do construtor de classe FaceServiceClient. Por exemplo:
```CSharp
faceServiceClient = new FaceServiceClient("<Subscription Key>");
```

## <a name="step2"></a> Passo 2: Carregar uma imagem para o serviço e executar a deteção de rostos em

A forma mais básica para efetuar a deteção de rostos em é através do carregamento de uma imagem diretamente. Isto é feito ao enviar um pedido de "POST" com o tipo de conteúdo application/octet-stream, com os dados lidos a partir de uma imagem JPEG. O tamanho máximo da imagem é 4 MB.

Deteção de rostos em através do carregar utilizando a biblioteca de cliente, é feita ao transmitir um objeto de sequência. Consulte o exemplo abaixo:
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

Tenha em atenção que o método DetectAsync FaceServiceClient async. O método de chamada deverá ser marcado como assíncrono bem, para poder utilizar a cláusula await.
Se a imagem já se encontra na web e tem um URL, a deteção de rostos em pode ser executada por também fornecer o URL. Neste exemplo, o corpo do pedido será cadeia JSON, que contém o URL.
Utilizar a biblioteca de cliente, deteção de rostos em através de um URL pode ser executada facilmente utilizar outra sobrecarga do método DetectAsync.
```CSharp
string imageUrl = "http://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceServiceClient.DetectAsync(imageUrl, true, true);
 
foreach (var face in faces)
{
    var rect = face.FaceRectangle;
    var landmarks = face.FaceLandmarks;
}
``` 

A propriedade de FaceRectangle que é devolvida com faces detetados é essencialmente zações de rostos em pixéis. Normalmente, neste retângulo contém continuará a ser, eyebrows, o nose e da boca – parte superior do cabeçalho, ears e o chin não estão incluídos. Se recortar uma head concluída ou média dimensão que captura vertical (uma imagem de tipo fotografia do ID), pode querer expanda a área da moldura enfrentam retangular porque a área do tipo de letra pode ser demasiado pequena para algumas aplicações. Para localizar um rosto mais precisamente, utilizando landmarks de rostos (localizar funcionalidades enfrentam ou enfrentam os mecanismos de direção) descrito na seguinte secção irá provar para ser útil.

## <a name="step3"></a> Passo 3: Compreensão e utilização de rostos em landmarks

Enfrentam landmarks são uma série de detalhado pontos de um rosto; Normalmente, pontos dos componentes de letra, como o pupils, canthus ou nose. Enfrentam landmarks são atributos opcionais que podem ser analisados durante a deteção de rostos em. Pode a passagem 'true' como um valor booleano para o parâmetro de consulta returnFaceLandmarks ao chamar o [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), ou utilize o parâmetro opcional returnFaceLandmarks para a classe de FaceServiceClient DetectAsync método ordem para incluir os landmarks enfrentam nos resultados da deteção.

Por predefinição, existem 27 pontos de landmark predefinidas. A seguinte figura mostra como todos os pontos de 27 são definidos:

![HowToDetectFace](../Images/landmarks.1.jpg)

Os pontos de devolvido estão em unidades de pixels, tal como moldura enfrentam retangular. Por conseguinte, tornando mais fácil marcar específicos pontos de interesse na imagem. O código seguinte demonstra a obter as localizações do nose e pupils:
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

Para além de marcação de rostos em funcionalidades numa imagem, enfrentam landmarks também podem ser utilizados para calcular com exatidão a direção do tipo de letra. Por exemplo, vamos pode definir a direção de rosto como um vetor de centro da boca para o Centro de continuará a ser. O código abaixo explica isto detalhadamente:

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

Por saber a direção que a letra está a ser, em seguida, pode rodar moldura enfrentam retangular alinhá-lo com o tipo de letra. É claro que utilizar enfrentam landmarks pode fornecer mais detalhes e o utilitário.

## <a name="step4"></a> Passo 4: Utilizar outros atributos de rostos em

Para além de landmarks enfrentam, enfrentam – detetar API também pode analisar vários outros atributos num tipo de letra. Estes atributos incluem:

- Idade
- Género
- Intensidade de Smile
- Facial hair
- Um implicar head 3D

Estes atributos são prever através da utilização de algoritmos de análises e sempre não podem ser preciso de 100%. No entanto, são ainda útil quando pretende classificar faces por estes atributos. Para obter mais informações sobre cada um dos atributos, consulte o [glossário](../Glossary.md).

Segue-se um exemplo simples para extrair os atributos de rostos em durante a deteção de rostos em:
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
## <a name="summary"></a> Resumo

Neste guia aprendeu as funcionalidades de [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API, o que pode detetar faces para local carregado imagens ou URL da imagem na web; como pode detetar faces devolvendo frames Jumbo enfrentam retangular; e como pode também analisar enfrentam landmarks, utilizadores constitui head 3D e outros atributos de rostos em bem.

Para obter mais informações sobre os detalhes de API, consulte o guia de referência da API para [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="related"></a> Tópicos relacionados

[Como identificar enfrenta na imagem](HowtoIdentifyFacesinImage.md)
