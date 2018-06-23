---
title: Moderada de imagens com Moderator conteúdo do Azure | Microsoft Docs
description: Test-Drive moderação de interrupção de imagem na consola do conteúdo Moderator API.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: fec54826c70ae10e56c68406f629c56639985295
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351661"
---
# <a name="moderate-images-from-the-api-console"></a>Moderada imagens a partir da consola de API

Utilize o [imagem moderação interrupção API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c) no Moderator de conteúdo do Azure para iniciar os fluxos de trabalho de moderação de interrupção de análise e revisão de conteúdo da imagem. A tarefa de moderação interrupção analisa o conteúdo para profanity e compara-os aos blacklists personalizados e partilhados.

## <a name="use-the-api-console"></a>Utilize a consola de API
Antes de pode test-drive API na consola do online, terá da chave de subscrição. Isto está localizado no **definições** separador o **Ocp-Apim-Subscription-Key** caixa. Para obter mais informações, consulte [descrição geral](overview.md).

1.  Aceda a [referência da API de moderação interrupção de imagem](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c).

  O **imagem - avaliar** imagem moderação interrupção página abre.

2. Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização. 

  ![Tente imagem - avaliar a seleção de região de página](images/test-drive-region.png)
  
  O **imagem - avaliar** é aberta a consola de API.

3. No **Ocp-Apim-Subscription-Key** box, introduza a chave de subscrição.

  ![Tente imagem - avaliar a chave de subscrição de consola](images/try-image-api-1.PNG)

4. No **corpo do pedido** caixa, utilizar a imagem de exemplo predefinida ou especificar uma imagem de análise. Pode submeter imagem como binário de bit de dados ou especifique um URL acessível publicamente para uma imagem. 

  Neste exemplo, utilizar o caminho fornecido no **corpo do pedido** caixa e, em seguida, selecione **enviar**. 

   ![Tente imagem - avaliar o corpo do pedido de consola](images/try-image-api-2.PNG)

  Esta é a imagem nesse URL:

  ![Tente imagem - avaliar imagem da consola de exemplo](images/sample-image.jpg) 

5. Selecione **Enviar**.

6. A API devolve uma pontuação de probabilidade para cada classificação. Devolve uma determinação de se a imagem satisfaz as condições também (**verdadeiro** ou **falso**). 

  ![Tente imagem - avaliar pontuação de probabilidade de consola e determinação de condição](images/try-image-api-3.PNG)

## <a name="face-detection"></a>Deteção de rostos

Pode utilizar a API de moderação interrupção de imagem para localizar faces numa imagem. Esta opção pode ser útil quando tem problemas de privacidade e pretende impedir que um rosto específico que está a ser publicado na sua plataforma. 

1.  No [referência da API de moderação interrupção de imagem](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c), no menu à esquerda, em **imagem**, selecione **localizar enfrenta**. 

  O **imagem - localizar enfrenta** é aberta a página.

2.  Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização. 

  ![Tente imagem - localizar seleção de região de página Faces](images/test-drive-region.png)

  O **imagem - localizar enfrenta** é aberta a consola de API.

3. Especifique uma imagem de análise. Pode submeter imagem como binário de bit de dados ou especifique um URL acessível publicamente para uma imagem. Este ligações de exemplo para uma imagem que é utilizada numa história CNN.

  ![Tente imagem - encontrou a imagem de exemplo Faces](images/try-image-api-face-image.jpg)

  ![Tente imagem - localizar Faces exemplo de pedido](images/try-image-api-face-request.png)

4. Selecione **Enviar**. Neste exemplo, a API localiza dois faces e devolve as coordenadas na imagem.

   ![Tente imagem - localizar caixa conteúda de resposta de amostra Faces](images/try-image-api-face-response.png)

## <a name="text-detection-via-ocr-capability"></a>Deteção de texto através da capacidade de OCR

Pode utilizar a capacidade de conteúdo Moderator OCR para detetar o texto nas imagens.

1. No [referência da API de moderação interrupção de imagem](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c), no menu à esquerda, em **imagem**, selecione **OCR**. 

  O **imagem - OCR** é aberta a página.

2. Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização. 

  ![Imagem - seleção de região de página OCR](images/test-drive-region.png)

  O **imagem - OCR** é aberta a consola de API.

3. No **Ocp-Apim-Subscription-Key** box, introduza a chave de subscrição.

4. No **corpo do pedido** caixa, utilize a imagem predefinida do exemplo. Esta é a mesma imagem que é utilizada na secção anterior.

5. Selecione **Enviar**. O texto extraído é apresentado no JSON:

  ![Imagem - caixa conteúdo de resposta de amostra OCR](images/try-image-api-ocr.PNG)

## <a name="next-steps"></a>Passos Seguintes

Utilizar a API REST no seu código ou começar com o [início rápido do .NET de moderação interrupção imagem](image-moderation-quickstart-dotnet.md) para integrar com a sua aplicação.
