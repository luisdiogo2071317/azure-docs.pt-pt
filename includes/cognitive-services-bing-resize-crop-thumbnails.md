Alguns respostas Bing incluem URLs para imagens em miniatura servidas pelo Bing. Pode redimensionar e recortar as imagens em miniatura. 

> [!NOTE]
> Certifique-se o tamanho e cropping de miniatura de fornecer um cenário de pesquisa e respeitamos direitos de terceiros, conforme necessário, através da utilização da API de pesquisa do Bing e apresentar requisitos.


Redimensionar uma imagem, inclua o w (largura) e o h (altura) parâmetros no URL a miniatura de consulta. Especifica a largura e altura em pixéis. Por exemplo:  
  
`https://<host>/th?id=JN.5l3yzwy%2f%2fHj59U6XhssIQ&pid=Api&w=200&h=200`  
  
Se redimensionar a imagem, a proporção é mantida. Para manter a proporção, preenchimento branco pode ser adicionado ao limite da imagem. Por exemplo, se redimensionar uma imagem de 480 x 359 para 200 x 200 sem cropping, a largura total contém a imagem, mas a altura contém 25 pixels da branco preenchimento na parte superior e inferior da imagem. O mesmo ser VERDADEIRO se a imagem foi 359 x 480 exceto à esquerda e direita limites iriam conter o preenchimento branco. Se recortar imagem, não é adicionado preenchimento branco.  

 
A imagem seguinte mostra o tamanho original de uma imagem em miniatura (480 x 300).  
  
![Imagem de horizontal original](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape.PNG)  
  
A imagem seguinte mostra a imagem redimensionada para 200 x 200. A proporção é mantida e os limites superior e inferior são serão preenchidos com em branco (limite preto é incluído para mostrar o preenchimento).  
  
![Imagem de redimensionado horizontal](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape-resized.PNG)  



Se especificar dimensões que são maiores que a imagem original largura e altura, a imagem será preenchida com em branco em limites esquerdos e superiores.  
  
Para recortar uma imagem, inclua o c (cortar) parâmetro de consulta. Seguem-se os valores possíveis que pode especificar.  
  
- 4&mdash;oculta rácio  
- 7&mdash;smart rácio  
  
Se solicitar cropping rácio inteligente (c = 7), a imagem é recortada do centro da região da imagem de interesse outward mantendo a proporção da imagem. A região de interesse é a área da imagem que o Bing determina contém as maioria das partes de importação. O seguinte mostra uma região de exemplo de interesse.  
  
![Região de interesse](./media/cognitive-services-bing-resize-crop/bing-resize-crop-regionofinterest.PNG)

Se redimensionar uma imagem e cropping inteligente rácio de pedidos, a imagem é reduzida para o tamanho pedido enquanto mantém a proporção. A imagem, em seguida, é recortada com base nas dimensões redimensionadas. Por exemplo, se a largura redimensionada é menor ou igual a altura, a imagem é recortada à esquerda e direita do centro da região de interesse. Caso contrário, a imagem é recortada para a parte superior e inferior centro da região de interesse.  
  
 
O seguinte mostra a imagem reduzida para 200 x 200 utilizando cropping rácio inteligente.  
  
![Imagem de horizontal recortada para 200 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape200x200c7.PNG)
  
O seguinte mostra a imagem reduzida para 200 x 100 utilizando cropping rácio inteligente.  
   
![Imagem de horizontal recortada para 100, 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape200x100c7.PNG)
  
O seguinte mostra a imagem reduzida para 100, 200 utilizando cropping rácio inteligente.  
  
![Imagem de horizontal recortada para 100, 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-landscape100x200c7.PNG)



Se o Bing não é possível determinar a região da imagem de interesse, o Bing utiliza cropping rácio oculta.  
  
Se solicitar rácio oculta cropping (c = 4), Bing utiliza as seguintes regras para recortar imagem.  
  
- Se (Original de imagem de largura / altura de imagem Original) < (pedida largura imagem / pedida a altura da imagem), a imagem é medida na parte superior esquerda canto e recortada na parte inferior.  
- Se (Original de imagem de largura / altura de imagem Original) > (pedida largura imagem / pedida a altura da imagem), a imagem é medida a partir do centro e recortada para a esquerda e direita.  



O seguinte mostra uma imagem de vertical é 225 x 300.  
  
![Imagem sunflower original](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower.PNG)
  
O seguinte mostra a imagem reduzida para 200 x 200 utilizando cropping rácio oculta. A imagem é medida a partir do canto superior esquerdo resultando na parte inferior da imagem a ser recortada.  
  
![Imagem sunflower recortada para 200 x 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower200x200c4.PNG)
  
O seguinte mostra a imagem reduzida para 200 x 100 utilizando cropping rácio oculta. A imagem é medida a partir do canto superior esquerdo resultando na parte inferior da imagem a ser recortada.  
  
![Imagem sunflower recortada para 100, 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower200x100c4.PNG)
  
O seguinte mostra a imagem reduzida para 100, 200 utilizando cropping rácio oculta. A imagem é medida desde o Centro de resultavam as partes da esquerda e direita da imagem a ser recortada.  
  
![Imagem sunflower recortada para 100, 200](./media/cognitive-services-bing-resize-crop/bing-resize-crop-sunflower100x200c4.PNG)

