Bing suporta acessos realce que marca a termos de consulta (ou outros termos que o Bing encontra relevantes) nas cadeias de visualização de algumas as respostas. Por exemplo, uma página Web `name`, `displayUrl`, e `snippet` campos podem marcar os termos de consulta.

Por predefinição, o Bing não inclui realce marcadores nas cadeias de apresentação. Para incluir os marcadores, inclua o `textDecorations` parâmetro do seu pedido de consulta e defina-o como **verdadeiro**. Bing marca os termos de consulta a utilizar os carateres E000 e E001 Unicode para marcar o início e fim do período da. Por exemplo, se o termo de consulta é Sailing Dinghy e o termo existe no campo, o termo é colocado entre acessos carateres realce conforme mostrado no exemplo seguinte:  
  
![Detetor de ocorrências](./media/cognitive-services-bing-hit-highlighting/bing-hit-highlighting.PNG) 

Antes de apresentar a cadeia na sua interface de utilizador, seria substitua os carateres Unicode carateres que são adequados para o formato de apresentação. Por exemplo, se estiver a apresentação de texto como HTML, poderá realce o termo de consulta, substituindo E000 com < b\> e E001 com </b\>. Se não pretender aplicar a formatação, remova os marcadores da cadeia. 

Bing fornece a opção de utilização de carateres Unicode ou tags de HTML como marcadores. Para especificar os marcadores de utilizar, inclua o `textFormat` parâmetro de consulta. Para marcar o conteúdo com carateres Unicode, defina `textFormat` Raw (predefinição) e marcar o conteúdo com tags de HTML, defina `textFormat` em HTML. 
  
Se `textDecorations` é **verdadeiro**, Bing pode incluir os seguintes marcadores nas cadeias de visualização de respostas. Se não existir nenhum equivalente HTML, a célula de tabela HTML está vazia.

|Unicode|HTML|Descrição
|-|-|-
|U + E000|\<b >|Marca o início do período da consulta (realce acessos)
|U + E001|\</ b >|Marca o fim do período da consulta
|U + E002|\<posso >|Marca o início de conteúdo em itálico 
|U + E003|\</ i >|Marca o fim do conteúdo em itálico
|U + E004|\<br / >|Marca uma quebra de linha
|U + E005||Marca o início de um número de telefone
|U + E006||Marca o fim de um número de telefone
|U + E007||Marca o início de um endereço
|U + E008||Marca o fim de um endereço
|U + E009|\&nbsp;|Marca um espaço sem quebra
|U + E00C|\<strong >|Marca o início do conteúdo negrito
|U + E00D|\</ strong >|Marca o fim do conteúdo negrito
|U + E00E||Marca o início do conteúdo cujo em segundo plano deve ser mais ligeira à respetiva surrounding em segundo plano
|U + E00F||Marca o fim do conteúdo cujo em segundo plano deve ser mais ligeira à respetiva surrounding em segundo plano
|U + E010||Marca o início do conteúdo cujo em segundo plano deve ser darker ao respetivo surrounding em segundo plano
|U + E011||Marca o fim do conteúdo cujo em segundo plano deve ser darker ao respetivo surrounding em segundo plano
|U + E012|\<del >|Marca o início de conteúdo que deve ser struck através do
|U + E013|\</del >|Marca o fim do conteúdo que deve ser struck através do
|U + E016|\<Sub >|Marca o início do conteúdo de índice
|U + E017|\</ sub >|Marca o fim do conteúdo de índice
|U + E018|\<sup >|Marca o início do conteúdo de expoente
|U + E019|\</ sup >|Marca o fim do conteúdo de expoente

O seguinte exemplo mostra um `Computation` resposta que contém os marcadores de índice para um termo de consulta log(2). O `expression` campo contém o marcadores apenas se for `textDecoration` é **verdadeiro**.

![marcadores de computação](./media/cognitive-services-bing-hit-highlighting/bing-markers-computation.PNG) 

Se o pedido não pediu decorações, a expressão será log10(2). 
  
