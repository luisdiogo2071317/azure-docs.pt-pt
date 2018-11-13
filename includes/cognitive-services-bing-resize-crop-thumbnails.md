---
author: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/09/2018
ms.author: nitinme
ms.openlocfilehash: b1cf3dc24482da70372f775d0ba63a203df1b285
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572600"
---
Algumas respostas Bing incluem URLs para imagens em miniatura servidas pelo Bing. Pode redimensionar e cortar as imagens em miniatura. 

> [!NOTE]
> Certifique-se o tamanho e corte da miniatura proporcionam um cenário de pesquisa e respeitar os direitos de terceiros, conforme exigido pela utilização da API de pesquisa do Bing e apresentam os requisitos.


Para redimensionar uma imagem, inclua o l (largura) e (altura) parâmetros no URL na miniatura de consulta. Especifique a largura e altura em pixels. Por exemplo:  
  
`https://<host>/th?id=JN.5l3yzwy%2f%2fHj59U6XhssIQ&pid=Api&w=200&h=200`  
  
Se o redimensionamento da imagem, a proporção é mantida. Para manter a proporção, preenchimento branco pode ser adicionado para a borda da imagem. Por exemplo, se redimensionar uma imagem de 480 x 359 para 200 x 200 sem corte, a largura total contém a imagem, mas a altura contém 25 pixels do white preenchimento na parte superior e inferior da imagem. O mesmo seria VERDADEIRO se a imagem foi 359 x 480, exceto à esquerda e direita bordas iria conter preenchimento branco. Se recortar a imagem, não é adicionado preenchimento branco.  

 
A imagem seguinte mostra o tamanho original de uma imagem em miniatura (480 x 300).  
  
![Imagem de paisagem original](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape.PNG)  
  
A imagem seguinte mostra a imagem redimensionada para 200 x 200. A proporção é mantida e os limites superior e inferior são preenchidos com branco (a borda preta é incluída para mostrar o preenchimento).  
  
![Imagem paisagem redimensionada](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape-resized.PNG)  



Se especificar dimensões maiores que largura original e a altura da imagem, a imagem será preenchida com white nas bordas esquerdas e superior.  
  
Para recortar uma imagem, inclua o c (recortar) parâmetro de consulta. Seguem-se os valores possíveis que pode especificar.  
  
- 4&mdash;daltônicos proporção  
- 7&mdash;smart proporção  
  
Se solicitar corte inteligente rácio (c = 7), a imagem é recortada no centro da região da imagem de interesse de direcionamento, mantendo a proporção de aspecto da imagem. A região de interesse é a área da imagem que Bing determina que contém a maioria das partes de importação. O código a seguir mostra uma região de exemplo de interesse.  
  
![Região de interesse](./media/cognitive-services-bing-resize-crop/bing-resize-crop-regionofinterest.PNG)

Se redimensiona uma imagem e corte inteligente a taxa de pedidos, a imagem é reduzida do tamanho solicitado enquanto mantém a proporção de aspecto. A imagem, em seguida, é recortada com base nas dimensões redimensionadas. Por exemplo, se a largura redimensionada for menor que ou igual à altura, a imagem é recortada, para a esquerda e direita do centro da região de interesse. Caso contrário, a imagem é recortada na parte superior e inferior do centro da região de interesse.  
  
 
O código a seguir mostra a imagem reduzida para 200 x 200 usando o corte inteligente proporção.  
  
![Imagem de paisagem recortada para 200 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape200x200c7.PNG)
  
O código a seguir mostra a imagem reduzida para 200 x 100 usando o corte inteligente proporção.  
   
![Imagem de paisagem recortada para 200 x 100](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape200x100c7.PNG)
  
O código a seguir mostra a imagem reduzida para 100 x 200 usando o corte inteligente proporção.  
  
![Imagem de paisagem recortada para 100 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape100x200c7.PNG)



Se Bing não é possível determinar a região da imagem de interesse, o Bing utiliza a proporção cego corte.  
  
Se solicitar o corte de rácio cego (c = 4), o Bing utiliza as seguintes regras para recortar a imagem.  
  
- Se (Original de imagem de largura / altura da imagem Original) < (largura da imagem de pedido / solicitada a altura da imagem), a imagem é medida na parte superior esquerda canto e recortada na parte inferior.  
- Se (Original de imagem de largura / altura da imagem Original) > (largura da imagem de pedido / solicitada a altura da imagem), a imagem é medida no centro da e recortada para a esquerda e direita.  



O código a seguir mostra uma imagem de retrato é 225 x 300.  
  
![Imagem sunflower original](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower.PNG)
  
O código a seguir mostra a imagem reduzida para 200 x 200 usando o corte de rácio cego. A imagem é medida desde o canto superior esquerdo, resultando na parte inferior da imagem a ser recortada.  
  
![Imagem sunflower recortada para 200 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower200x200c4.PNG)
  
O código a seguir mostra a imagem reduzida para 200 x 100 usando o corte de rácio cego. A imagem é medida desde o canto superior esquerdo, resultando na parte inferior da imagem a ser recortada.  
  
![Imagem sunflower recortada para 200 x 100](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower200x100c4.PNG)
  
O código a seguir mostra a imagem reduzida para 100 x 200 usando o corte de rácio cego. A imagem é medida desde o Centro de resulta nas partes esquerdas e direita da imagem a ser recortada.  
  
![Imagem sunflower recortada para 100 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower100x200c4.PNG)

