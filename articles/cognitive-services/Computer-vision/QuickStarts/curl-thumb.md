---
title: Início rápido de para obter miniatura da API de Imagem Digitalizada com cURL | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, irá gerar uma miniatura de uma imagem através da Imagem Digitalizada com cURL nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 4056f05f3f4bf97761f683b5f3a9053666d4ea26
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43771942"
---
# <a name="quickstart-generate-a-thumbnail---rest-curl"></a>Início Rápido: Gerar uma miniatura – REST, cURL

Neste início rápido, irá gerar uma miniatura de uma imagem através da Imagem Digitalizada.

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar a Imagem Digitalizada, necessita de uma chave de subscrição; consulte [A Obter Chaves de Subscrição](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="get-thumbnail-request"></a>Pedido Obter Miniatura

Com o [método Obter Miniatura](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb), pode gerar uma miniatura de uma imagem. Especifica a altura e a largura, que podem ser diferentes da proporção da imagem de entrada. A Imagem Digitalizada utiliza o recorte inteligente para identificar de forma inteligente a região de interesse e gerar coordenadas de recorte com base nessa região.

Para executar o exemplo, siga os passos seguintes:

1. Copie o código seguinte para um editor.
1. Substitua `<Subscription Key>` pela sua chave de subscrição válida.
1. Substitua `<File>` pelo caminho e nome de ficheiro para guardar a miniatura.
1. Altere o URL de Pedido (`https://westcentralus.api.cognitive.microsoft.com/vision/v2.0`) para utilizar a localização onde obteve as suas chaves de subscrição, se necessário.
1. Opcionalmente, altere a imagem (`{\"url\":\"...`) para analisar.
1. Abra uma janela de comando num computador com o cURL instalado.
1. Cole o código na janela e execute o comando.

>[!NOTE]
>Tem de utilizar na sua chamada REST a mesma localização que utilizou para obter as chaves de subscrição. Por exemplo, se tiver obtido as chaves de subscrição a partir de westus, substitua "westcentralus" no URL abaixo por "westus".

```json
curl -H "Ocp-Apim-Subscription-Key: <Subscription Key>" -o <File> -H "Content-Type: application/json" "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"
```

## <a name="get-thumbnail-response"></a>Resposta Obter Miniatura

Uma resposta bem-sucedida escreve a imagem em miniatura para `<File>`. Se o pedido falhar, a resposta contém um código de erro e uma mensagem para ajudar a determinar o que correu mal.

## <a name="next-steps"></a>Passos seguintes

Explore as API de Imagem Digitalizada utilizadas para analisar uma imagem, detetar celebridades e marcos, criar uma miniatura e extrair texto impresso e manuscrito. Para experimentar rapidamente as API de Imagem Digitalizada, experimente a [Consola de teste de API aberta](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Explorar API de Imagem Digitalizada](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
