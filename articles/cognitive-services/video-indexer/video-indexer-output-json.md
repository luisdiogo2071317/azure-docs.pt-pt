---
title: Examine a saída do indexador de vídeo do Azure | Documentos da Microsoft
description: Este tópico examina a saída do indexador de vídeo.
services: cognitive services
documentationcenter: ''
author: juliako
manager: cflower
ms.service: cognitive-services
ms.topic: article
ms.date: 05/30/2018
ms.author: juliako
ms.openlocfilehash: 7f61833d70cea3a6d176f5a08eb0db1c30e70e86
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355891"
---
# <a name="examine-the-video-indexer-output-produced-by-v1-api"></a>Examine a saída do indexador de vídeo produzida pela API do v1

> [!Note]
> As APIs de V1 do indexador de vídeo foram preteridas e serão removidas no dia 1 de Agosto de 2018. Deve começar a utilizar as APIs do Video Indexer v2 para evitar interrupções.
>
> Para programar com APIs do Video Indexer v2, veja as instruções presentes [aqui](https://api-portal.videoindexer.ai/). 

Quando chama a **divisões obter** API e o estado de resposta está OK, obtém uma saída JSON detalhada como o conteúdo de resposta. O conteúdo JSON contém detalhes das especificado vídeo informações incluindo (transcrição, OCRs, as pessoas). Os detalhes incluem palavras-chave (tópicos), rostos, blocos. Cada bloco inclui intervalos de tempo, linhas de transcrição, linhas de OCR, sentimentos, rostos e bloquear miniaturas.

Pode utilizar o **divisões obter** API para obter conteúdo a divisão completa de um vídeo como um JSON.  
 
    GET https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/63c6d532ff HTTP/1.1
    Host: videobreakdown.azure-api.net
    Ocp-Apim-Subscription-Key: ••••••••••••••••••••••••••••••••
    
Pode também visualmente examinar informações resumidas do vídeo ao premir o **reproduzir** botão do vídeo no portal do Video Indexer. Para obter mais informações, consulte [ver e editar informações de vídeo](video-indexer-view-edit.md).

![Informações](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

Este artigo examina o conteúdo JSON devolvido pelos **divisões obter** API. Poderá ser útil rever as [conceitos](video-indexer-concepts.md) artigo.

> [!NOTE]
> Expiração de todos os tokens de acesso no indexador de vídeo é uma hora.

## <a name="root-elements"></a>Elementos de raiz

Atributo | Descrição
---|---
ID|O Id deste vídeo. Por exemplo, 63c6d532ff.
partição|Uma partição lógica que o utilizador pode especificar no carregamento para pesquisá-la mais tarde.
nome|O nome do vídeo. Por exemplo, o Azure Monitor.
descrição|Descrição do vídeo. Por exemplo, "João Kemnetz junta-se a Scott Hanselman para mostrar como desbloquear o poder dos dados de monitorização do Azure com o Azure Monitor."
userName|O criador do vídeo. Por exemplo, vídeos do Channel 9.
CreateTime |Hora de criação. Por exemplo, 2017-03-31T16:36:41.4504249 + 00:00.
privacyMode|O vídeo pode ter um dos seguintes modos: **privada**, **público**. **Público** -o vídeo é visível para todas as pessoas na sua conta e qualquer pessoa que tenha uma ligação para o vídeo. **Privada** -o vídeo é visível para todas as pessoas na sua conta.
isOwned|VERDADEIRO se o utilizador atual é o proprietário do vídeo. Caso contrário, FALSO.  
isBase|VERDADEIRO se a divisão baseia-se numa origem de vídeo. FALSO, se a divisão é de uma lista de reprodução que é derivada de outra divisão.
durationInSeconds|Duração do vídeo.
summarizedInsights|Contém um [summarizedInsights](#summarizedInsights).
divisões|Pode conter um ou mais [divisões](#breakdowns)
Redes sociais|Contém um **social** elemento que descreve o número de gostos e modos de exibição do vídeo.

## <a name="summarizedinsights"></a>summarizedInsights

Esta secção mostra o resumo das informações.

Atributo | Descrição
---|---
nome|O nome do vídeo. Por exemplo, o Azure Monitor.
shortId|O ID do vídeo. Por exemplo, 63c6d532ff.
privacyMode|Sua análise detalhada pode ter um dos seguintes modos: **privada**, **público**. **Público** -o vídeo é visível para todas as pessoas na sua conta e qualquer pessoa que tenha uma ligação para o vídeo. **Privada** -o vídeo é visível para todas as pessoas na sua conta.
duração|Contém uma duração que descreve o tempo de que uma informação ocorreu. A duração é em segundos.
thumbnailUrl|Miniatura do vídeo total URL. Por exemplo, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...." Tenha em atenção que, se o vídeo é privado, o URL contém um token de acesso de uma hora. Depois de uma hora, o URL já não será válido e terá de obter a divisão novamente com um novo url no mesmo ou chamar GetAccessToken para obter um novo token de acesso e construir o url completo manualmente ("https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [ accessToken]').
rostos|Pode conter um ou mais [rostos](#faces)
Tópicos|Pode conter um ou mais [tópicos](#topics)
sentimentos|Pode conter um ou mais [sentimentos](#sentiments)
audioEffects| Pode conter um ou mais [audioEffects](#audioEffects)
marcas| Pode conter zero ou mais [marcas](#brands)
Estatísticas|Para obter mais informações, consulte [estatísticas](#Statistics)
.
### <a name="statistics"></a>Estatísticas

|Nome|Descrição|
|---|---|
|CorrespondenceCount|Número de correspondências no vídeo.|
|WordCount|O número de palavras por orador.|
|SpeakerNumberOfFragments|A quantidade de fragmentos o orador tem um vídeo.|
|SpeakerLongestMonolog|Monolog de mais longo do orador. Se o orador tiver silences dentro o monolog está incluído. Silence, no início e fim do monolog é removido.| 
|SpeakerTalkToListenRatio|O cálculo baseia-se o tempo gasto em monolog do orador (sem o silence entre) dividido pelo tempo total do vídeo. A hora é arredondada para o terceiro ponto decimal.|

## <a name="breakdowns"></a>divisões

Esta secção mostra os detalhes das informações.

Atributo | Descrição
---|---
ID|O ID de divisão. Por exemplo, 63c6d532ff.
state|O estado de processamento do id de divisão fornecido. Pode ser um dos seguintes: carregado, processamento, processados, com falhas.
processingProgress|O progresso. Por exemplo, 10%.
externalId| Pode definir externalId durante o carregamento. Por exemplo, "4f9c3500-eca7-4ab3-987e-a745017af698". Pode procurar posteriormente para os seus vídeos por este ID externo.
URL externo|Pode definir o URL externo durante o carregamento. 
do IdP|Pode definir metadados durante o carregamento. 
informações|Pode conter um ou mais [insights](#insights)
thumbnailUrl|Miniatura do vídeo total URL. Por exemplo, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...". Tenha em atenção que, se o vídeo é privado, o URL contém um token de acesso de uma hora. Depois de uma hora, o URL já não será válido e terá de obter a divisão novamente com um novo url no mesmo ou chamar GetAccessToken para obter um novo token de acesso e construir o url completo manualmente ("https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [ accessToken]').
publishedUrl|O URL publicado. Por exemplo, "https://BreakdownMedia.azureedge.net:443/d5e5232d-48e2-4fbc-9893-0ea6335da563/Azure%20Monitor%20%20Azure%20Friday.ism/manifest."
viewToken|O token de portador
sourceLanguage|O idioma de origem. São suportadas as seguintes: chinês, inglês, francês, alemão, italiano, japonês, português, russo, espanhol.
Idioma|O idioma de transcrição.

## <a name="insights"></a>informações

Atributo | Descrição 
---|---
transcriptBlocks|Pode conter um ou mais [transcriptBlocks](#transcriptBlocks)
Tópicos|Pode conter um ou mais [tópicos](#topics)
rostos|Pode conter um ou mais [rostos](#faces)
participantes|Pode conter um ou mais [participantes](#participants)
contentModeration|Pode conter um [contentModeration](#contentModeration)
audioEffectsCategories|Pode conter um ou mais [audioEffectsCategories](#audioEffectsCategories)

## <a name="faces"></a>rostos

### <a name="summarizedinsights"></a>summarizedInsights

**rostos** que são apresentados em **summarizedInsights**, mostrar um resumo de cada rosto encontrado no vídeo.

Atributo | Descrição 
---|---
ID|O ID de uma pessoa. Por exemplo, 11775.
shortId|O ID de curto. Porque pode derivar uma lista de reprodução do vários divisões, este ID é necessária para descobrir qual essas divisões é a origem de cada rosto.  
nome|Se o mostrador for reconhecido, é adicionado o nome da pessoa. Por exemplo, "Scott Hanselman." Se o mostrador for desconhecido, é adicionado a "# desconhecido". 
descrição|Se o mostrador for reconhecido, a descrição é preenchida com base na pesquisa de API do Bing. Caso contrário, a descrição é **nulo**.
título|Se o mostrador for reconhecido, a descrição é preenchida com base na pesquisa de API do Bing. Caso contrário, o título é **nulo**.
thumbnailFullUrl|Miniatura do mostrador completa de URL. Por exemplo, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...." Tenha em atenção que, se o vídeo é privado, o URL contém um token de acesso de uma hora. Depois de uma hora, o URL já não será válido e terá de obter a divisão novamente com um novo url no mesmo ou chamar GetAccessToken para obter um novo token de acesso e construir o url completo manualmente ("https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [ accessToken]').
aparências|Pode conter um ou mais [aparências](#appearances)
seenDuration|Para o tempo que o rosto foi visto (em segundos).
seenDurationRatio|Presença em relação a duração do vídeo (0-1).

### <a name="breakdown-insights"></a>informações de divisão

**rostos** que são apresentados em **divisões**, descreva os detalhes sobre cada rosto encontrado no vídeo.

Atributo | Descrição 
---|---
ID|O ID de uma pessoa. Por exemplo, 11775.
bingId|
nome|Se o mostrador for reconhecido, é adicionado o nome da pessoa. Por exemplo, "Scott Hanselman". Se o mostrador for desconhecido, é adicionado a "# desconhecido". 
thumbnailId|Por exemplo, 616468f0-1636-4efa-94e7-262f2e575059.
descrição|Se o mostrador for reconhecido, a descrição é preenchida com base na pesquisa de API do Bing. Caso contrário, a descrição é **nulo**.
título|Se o mostrador for reconhecido, a descrição é preenchida com base na pesquisa de API do Bing. Caso contrário, o título é **nulo**.
imageUrl|Este URL aponta para uma imagem que está a ser utilizada da origem de vídeo.  
confiança|A pontuação de confiança (superior é melhor).
knownPersonId|O id de uma pessoa conhecido (por exemplo, celebridade). Se uma pessoa não é conhecida, o id contém zeros. Por exemplo, e3eaff5f-ee1b-4eac-80ce-ebac47aadf64.

## <a name="topics"></a>Tópicos

### <a name="summarizedinsights"></a>summarizedInsights

**tópicos** que são apresentados em **summarizedInsights**, mostrar um resumo de cada tópico encontrado no vídeo.

Atributo | Descrição 
---|---
nome|O nome de tópico (por exemplo, "Azure"). 
aparências|Pode conter um ou mais [aparências](#appearances).
isTranscript|TRUE, se encontrar na transcrição. Se FALSE, encontrada num OCR.

### <a name="breakdown-insights"></a>informações de divisão

**tópicos** que são apresentados em **divisões**, descreva os detalhes sobre cada tópico encontrado no vídeo.

|Atributo | Descrição |
|---|---|
|ID|ID de tópico exclusivo.|
|nome|O nome de tópico.|
|Stem|Atualmente, este atributo não é utilizado.|
|palavras|Atualmente, este atributo não é utilizado.|
|classificação|Pontuação de relevância (superior é melhor).|

## <a name="sentiments"></a>sentimentos

Atributo | Descrição
---|---
sentimentKey| Atualmente, são suportados os seguintes sentimentos: positivo, neutro, negativo. 
aparências|Pode conter um ou mais [aparências](#appearances)|.
seenDurationRatio|Presença em relação a duração do vídeo (0-1).

## <a name="audioeffects"></a>audioEffects

Atributo | Descrição 
---|---
audioEffectKey| Os valores válidos são: conversão de voz, silêncio, HandClaps.
aparências|Pode conter um ou mais [aparências](#appearances)
seenDurationRatio|Presença em relação a duração do vídeo (0-1).
seenDuration|Para o tempo que o efeito de áudio estava presente (em segundos).

## <a name="appearances"></a>aparências

Atributo | Descrição 
---|---
startTime| Valor de tempo.
endTime|Valor de tempo.
startSeconds| Valor de tempo.
endSeconds| Valor de tempo.

## <a name="participants"></a>participantes

Atributo | Descrição 
---|---
ID|O ID do participante.
nome|O nome do participante. Por exemplo, o orador n. º 1.
pictureUrl|O **pictureUrl** atributo está reservado para utilização futura.

## <a name="contentmoderation"></a>contentModeration

Atributo | Descrição 
---|---
adultClassifierValue|O nível de confiança que o vídeo tem conteúdo para adultos.
racyClassifierValue|O nível de confiança que o vídeo tem conteúdo para adultos.
bannedWordsCount|Número de palavras de linguagem inapropriada. 
bannedWordsRatio|Rácio de palavras de linguagem inapropriada do número total de palavras.
reviewRecommended|Valor booleano que indica se o vídeo deve ser revisto manualmente.
isAdult|Valores booleanos que indica se o vídeo é considerado um adulto vídeo após a revisão manual.

## <a name="audioeffectscategories"></a>audioEffectsCategories

Atributo | Descrição 
---|---
tipo|ID da categoria.
key|Um dos seguintes: conversão de voz, silêncio, HandClaps. 

## <a name="transcriptblocks"></a>transcriptBlocks

Atributo | Descrição
---|---
ID|ID do bloco.
linhas|Pode conter um ou mais [linhas](#lines)
sentimentIds|O **sentimentIds** atributo está reservado para utilização futura.
thumbnailIds|O **thumbnailIds** atributo está reservado para utilização futura.
sentimento|O sentimento no bloco de (0-1, negativo para positivo).
rostos|Pode conter um ou mais [rostos](#faces).
ocrs|Pode conter um ou mais [ocrs](#ocrs).
audioEffectInstances|Pode conter um ou mais [audioEffectInstances](#audioEffectInstances).
nos bastidores|Pode conter um ou mais [plano](#scenes).
anotações|Pode conter zero ou mais [anotações](#annotations).

## <a name="ocrs"></a>ocrs

Descreve em que ponto o vídeo que foi encontrado o conteúdo de texto. 

Atributo | Descrição 
---|---
timeRange|O intervalo de tempo no vídeo original.
adjustedTimeRange|AdjustedTimeRange é o intervalo de tempo relativo à lista de reprodução atual. Uma vez que pode criar uma lista de reprodução de linhas diferentes de vídeos diferentes, pode demorar uma hora de um vídeo e usar apenas 1 linha dela, por exemplo, 10:00-10:15. Nesse caso, terá uma lista de reprodução com 1 linha, em que o intervalo de tempo é 10:00-10:15, mas o adjustedTimeRange é 00:00 00:15.
linhas|Pode conter um ou mais [linhas](#lines).

## <a name="lines"></a>linhas

### <a name="transcriptblocks"></a>transcriptBlocks

**linhas** que são apresentados em **transcriptBlocks**, descrevem as linhas de transcrições encontradas no vídeo.

Atributo | Descrição 
---|---
ID| O ID da linha.
timeRange|O intervalo de tempo no vídeo original.
adjustedTimeRange|AdjustedTimeRange é o intervalo de tempo relativo à lista de reprodução atual. Uma vez que pode criar uma lista de reprodução de linhas diferentes de vídeos diferentes, pode demorar uma hora de um vídeo e usar apenas 1 linha dela, por exemplo, 10:00-10:15. Nesse caso, terá uma lista de reprodução com 1 linha, em que o intervalo de tempo é 10:00-10:15, mas o adjustedTimeRange é 00:00 00:15.
participantID| O id do orador desta linha.
texto| A transcrição.
isIncluded| Em divisões bases sempre verdadeiros. Em listas de reprodução derivadas, as linhas que foram incluídas na origem de vídeo, estão definidas como isIncluded = true. Todas as outras linhas forem falsos.

### <a name="ocrs"></a>ocrs

**linhas** que são apresentados em **ocrs**, descrevem as linhas de OCRs encontrados no vídeo.

Atributo | Descrição 
---|---
ID|O ID de OCR.
Largura|Atualmente, este atributo não é utilizado.
Altura|Atualmente, este atributo não é utilizado.
Idioma|O idioma de OCR.
textData|O texto de OCR.
confiança|A pontuação de confiança (superior é melhor).

## <a name="scenes"></a>nos bastidores

Atributo | Descrição 
---|---
ID|O ID da cena.
timeRange|Contém um **timeRange**.
quadro-chave|A hora do quadro-chave.
capturas de|Pode conter um ou mais [capturas](#shots).

## <a name="shots"></a>capturas de

Atributo | Descrição 
---|---
ID||O ID de captura.
timeRange|Contém um **timeRange**.
quadro-chave|A hora do quadro-chave.

## <a name="audioeffectinstances"></a>audioEffectInstances

Atributo | Descrição 
---|---
tipo|O índice do evento áudio: Laughter = 1, HandClaps = 2, música = 3, voz = 4, silêncio = 5
intervalos|Pode conter um ou mais [intervalos](#ranges).

## <a name="ranges"></a>intervalos

**intervalos** que são apresentados em **audioEffectInstances**, descrevem os efeitos de áudio nesses intervalos.

Atributo | Descrição 
---|---
timeRange|O intervalo de tempo no vídeo original.
adjustedTimeRange|AdjustedTimeRange é o intervalo de tempo relativo à lista de reprodução atual. Uma vez que pode criar uma lista de reprodução de linhas diferentes de vídeos diferentes, pode demorar uma hora de um vídeo e utilizar apenas uma linha a partir dela, por exemplo, 10:00-10:15. Nesse caso, terá uma lista de reprodução com 1 linha, em que o intervalo de tempo é 10:00-10:15, mas o adjustedTimeRange é 00:00 00:15.

## <a name="annotations"></a>anotações

Devolve etiquetas com base em objetos reconhecíveis, seres vivos, paisagens, ações e padrões de visual.

|Atributo|Descrição|
|---|---|
|ID|O ID da anotação.|
|Nome|O nome da anotação (por exemplo, pessoa, atleta jogo, quadros de preto).|
|Aparências|Pode conter um ou mais aparências.|

## <a name="brands"></a>marcas

Negócios e produto nomes de marca detetados na conversão de voz a transcrição de texto e/ou OCR de vídeo. Não inclui reconhecimento visual de marcas ou deteção de logótipo.

Atributo | Descrição
---|---
ID | O ID de uma marca.
nome | O nome da marca do Bing ou uma marca personalizada.  
wikiId | O sufixo do url da wikipédia marca. Por exemplo, "Target_Corporation" é o sufixo de [ https://en.wikipedia.org/wiki/Target_Corporation ](https://en.wikipedia.org/wiki/Target_Corporation).
wikiUrl | A marca do url da Wikipédia, se existir. Por exemplo, [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
confiança | O valor de confiança do detetor de marca do Video Indexer (0-1).
descrição | Descrição da marca extraída da Wikipédia. 
aparências | Pode conter um ou mais aparências.
seenDuration | Presença em relação a duração do vídeo (0-1).

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre como criar sua própria análise detalhada, consulte [ver e editar informações do indexador de vídeo](video-indexer-view-edit.md).

Para obter informações sobre como incorporar widgets na sua aplicação, consulte [widgets de incorporar o Video Indexer nas suas aplicações](video-indexer-embed-widgets.md). 

## <a name="see-also"></a>Consulte também

[API do indexador de vídeo](https://videobreakdown.portal.azure-api.net/docs/services/582074fb0dc56116504aed75/operations/5857caeb0dc5610f9ce979e4)

[Descrição geral do indexador vídeo](video-indexer-overview.md)

