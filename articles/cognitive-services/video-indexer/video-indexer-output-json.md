---
title: Examine a saída do indexador de vídeo do Azure | Microsoft Docs
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
# <a name="examine-the-video-indexer-output-produced-by-v1-api"></a>Examine a saída de vídeo indexador produzida pela v1 API

> [!Note]
> As APIs do vídeo indexador V1 estão agora preteridas e serão removidas no 1ª de Agosto de 2018. Deve começar a utilizar as APIs do indexador de vídeo v2 para evitar interrupções.
>
> Para desenvolver com APIs de v2 do indexador de vídeo, consulte as instruções que se encontram [aqui](https://api-portal.videoindexer.ai/). 

Quando chamar o **obter Breakdowns** API e o estado de resposta está OK, obter um resultado JSON detalhado como o conteúdo da resposta. O conteúdo JSON contém detalhes sobre o especificado vídeo insights incluindo (transcript, OCRs, pessoas). Os detalhes incluem palavras-chave (tópicos), faces, blocos. Cada bloco inclui intervalos de tempo, linhas de transcript, linhas de OCR, sentiments, faces e bloquear as miniaturas.

Pode utilizar o **obter Breakdowns** API ao obter conteúdo a repartição completa de um vídeo como um JSON.  
 
    GET https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/63c6d532ff HTTP/1.1
    Host: videobreakdown.azure-api.net
    Ocp-Apim-Subscription-Key: ••••••••••••••••••••••••••••••••
    
Pode também visualmente examinar insights resumidos as vídeo, premindo o **reproduzir** botão o vídeo no portal do indexador de vídeo. Para obter mais informações, consulte [ver e editar insights vídeos](video-indexer-view-edit.md).

![Informações](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

Este artigo examina o conteúdo JSON devolvido pelo **obter Breakdowns** API. Poderá ser útil rever as [conceitos](video-indexer-concepts.md) artigo.

> [!NOTE]
> Expiração de todos os tokens de acesso no vídeo indexador é uma hora.

## <a name="root-elements"></a>Elementos de raiz

Atributo | Descrição
---|---
ID|O Id deste vídeo. Por exemplo, 63c6d532ff.
partição|Uma partição lógica que o utilizador pode especificar no carregamento para procurá-lo mais tarde.
name|O nome do vídeo. Por exemplo, o Monitor do Azure.
descrição|Descrição do vídeo. Por exemplo, "João Kemnetz junta autoria de Scott Hanselman para mostrar como desbloquear o poder dos dados de monitorização do Azure com a monitorização do Azure."
userName|O criador do vídeo. Por exemplo, Channel9 vídeos.
CreateTime |Hora de criação. Por exemplo, 2017-03-31T16:36:41.4504249 + 00:00.
privacyMode|O vídeo pode ter um dos seguintes modos: **privada**, **pública**. **Pública** -o vídeo está visível para todos os utilizadores na sua conta e qualquer pessoa que tenha uma ligação para o vídeo. **Privada** -o vídeo está visível para todos os utilizadores na sua conta.
isOwned|VERDADEIRO se o utilizador atual possui o vídeo. Caso contrário, FALSO.  
isBase|VERDADEIRO se a repartição baseia-se numa origem de vídeo. FALSO, se estiver a repartição de uma lista de reprodução é derivada de outro divisão.
durationInSeconds|Duração do vídeo.
summarizedInsights|Contém um [summarizedInsights](#summarizedInsights).
breakdowns|Pode conter um ou mais [breakdowns](#breakdowns)
Redes sociais|Contém um **sociais** elemento que descreve o número de vistas do vídeo e likes.

## <a name="summarizedinsights"></a>summarizedInsights

Esta secção mostra o resumo de informações do.

Atributo | Descrição
---|---
name|O nome do vídeo. Por exemplo, o Monitor do Azure.
shortId|O ID do vídeo. Por exemplo, 63c6d532ff.
privacyMode|A divisão pode ter um dos seguintes modos: **privada**, **pública**. **Pública** -o vídeo está visível para todos os utilizadores na sua conta e qualquer pessoa que tenha uma ligação para o vídeo. **Privada** -o vídeo está visível para todos os utilizadores na sua conta.
duração|Contém uma duração que descreve o tempo de que uma informação ocorreu. Duração é em segundos.
thumbnailUrl|Miniatura as vídeo completa URL. Por exemplo, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...." Tenha em atenção que, se o vídeo é privado, o URL contém um token de acesso de uma hora. Depois de uma hora, o URL será já não é válido e terá de obter a repartição novamente com um novo url no mesmo, ou chamar GetAccessToken para obter um novo token de acesso e construir o url completo manualmente ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [ accessToken]').
faces|Pode conter um ou mais [faces](#faces)
Tópicos|Pode conter um ou mais [tópicos](#topics)
sentiments|Pode conter um ou mais [sentiments](#sentiments)
audioEffects| Pode conter um ou mais [audioEffects](#audioEffects)
marcas| Pode conter zero ou mais [marcas](#brands)
Estatísticas|Para obter mais informações, consulte [estatísticas](#Statistics)
.
### <a name="statistics"></a>Estatísticas

|Nome|Descrição|
|---|---|
|CorrespondenceCount|Número de correspondences as vídeo.|
|WordCount|O número de palavras por orador.|
|SpeakerNumberOfFragments|A quantidade de fragmentos de orador tem um vídeo.|
|SpeakerLongestMonolog|Monolog mais longo de orador. Se tiver de altifalante silences dentro de monolog está incluído. Silêncio no início e fim do monolog é removido.| 
|SpeakerTalkToListenRatio|O cálculo baseia-se o tempo gasto em monolog de orador (sem silêncio entre) dividido pelo tempo total do vídeo. A hora é arredondada para o terceiro vírgula decimal.|

## <a name="breakdowns"></a>breakdowns

Esta secção mostra os detalhes das informações.

Atributo | Descrição
---|---
ID|O ID de divisão. Por exemplo, 63c6d532ff.
state|O estado de processamento do id de divisão fornecido. Pode ser um dos seguintes: foi carregado, processamento, processados, com falha.
processingProgress|O progresso. Por exemplo, 10%.
externalId| Pode definir externalId durante o carregamento. Por exemplo, "4f9c3500-eca7-4ab3-987e-a745017af698". Pode procurar posteriormente para os seus vídeos por este ID externo.
URL externo|Pode definir o URL externo durante o carregamento. 
do IdP|Pode definir metadados durante o carregamento. 
informações|Pode conter um ou mais [insights](#insights)
thumbnailUrl|Miniatura as vídeo completa URL. Por exemplo, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...". Tenha em atenção que, se o vídeo é privado, o URL contém um token de acesso de uma hora. Depois de uma hora, o URL será já não é válido e terá de obter a repartição novamente com um novo url no mesmo, ou chamar GetAccessToken para obter um novo token de acesso e construir o url completo manualmente ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [ accessToken]').
publishedUrl|O URL publicado. Por exemplo, "https://BreakdownMedia.azureedge.net:443/d5e5232d-48e2-4fbc-9893-0ea6335da563/Azure%20Monitor%20%20Azure%20Friday.ism/manifest."
viewToken|O token de portador
sourceLanguage|O idioma de origem. São suportadas as seguintes: chinês, inglês, francês, alemão, italiano, japonês, Portuguese, russo, espanhol.
idioma|O idioma do transcript.

## <a name="insights"></a>informações

Atributo | Descrição 
---|---
transcriptBlocks|Pode conter um ou mais [transcriptBlocks](#transcriptBlocks)
Tópicos|Pode conter um ou mais [tópicos](#topics)
faces|Pode conter um ou mais [faces](#faces)
participantes|Pode conter um ou mais [participantes](#participants)
contentModeration|Pode conter um [contentModeration](#contentModeration)
audioEffectsCategories|Pode conter um ou mais [audioEffectsCategories](#audioEffectsCategories)

## <a name="faces"></a>faces

### <a name="summarizedinsights"></a>summarizedInsights

**faces** que são apresentados em **summarizedInsights**, mostra um resumo de cada rosto encontrado no vídeo.

Atributo | Descrição 
---|---
ID|O ID de uma pessoa. Por exemplo, 11775.
shortId|O ID de curto. Porque uma lista de reprodução pode ser derivada de vários breakdowns, este ID é necessário saber qual estes breakdowns é a origem de cada tipo de letra.  
name|Se a letra é reconhecida, é adicionado o nome da pessoa. Por exemplo, "autoria de Scott Hanselman." Se a letra for desconhecida, "# desconhecido" é adicionado. 
descrição|Se a letra é reconhecida, a descrição é preenchida com base na procura a API do Bing. Caso contrário, a descrição é **nulo**.
título|Se a letra é reconhecida, a descrição é preenchida com base na procura a API do Bing. Caso contrário, o título é **nulo**.
thumbnailFullUrl|Miniatura de rostos completa URL. Por exemplo, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...." Tenha em atenção que, se o vídeo é privado, o URL contém um token de acesso de uma hora. Depois de uma hora, o URL será já não é válido e terá de obter a repartição novamente com um novo url no mesmo, ou chamar GetAccessToken para obter um novo token de acesso e construir o url completo manualmente ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [ accessToken]').
appearances|Pode conter um ou mais [appearances](#appearances)
seenDuration|Para quanto enfrentam foi visto (em segundos).
seenDurationRatio|Presença relativos a duração de vídeo (0-1).

### <a name="breakdown-insights"></a>informações de divisão

**faces** que são apresentados em **breakdowns**, descrevem os detalhes sobre cada enfrentam encontrado no vídeo.

Atributo | Descrição 
---|---
ID|O ID de uma pessoa. Por exemplo, 11775.
bingId|
name|Se a letra é reconhecida, é adicionado o nome da pessoa. Por exemplo, "autoria de Scott Hanselman". Se a letra for desconhecida, "# desconhecido" é adicionado. 
thumbnailId|Por exemplo, 616468f0-1636-4efa-94e7-262f2e575059.
descrição|Se a letra é reconhecida, a descrição é preenchida com base na procura a API do Bing. Caso contrário, a descrição é **nulo**.
título|Se a letra é reconhecida, a descrição é preenchida com base na procura a API do Bing. Caso contrário, o título é **nulo**.
imageUrl|Este URL aponta para uma imagem que é obtida a partir da origem de vídeo.  
confiança|A classificação de confiança (superior é melhor).
knownPersonId|O id de uma pessoa conhecido (por exemplo, celebrity). Se uma pessoa não é conhecida, o id contém zeros. Por exemplo, e3eaff5f-ee1b-4eac-80ce-ebac47aadf64.

## <a name="topics"></a>Tópicos

### <a name="summarizedinsights"></a>summarizedInsights

**tópicos** que são apresentados em **summarizedInsights**, mostra um resumo de cada tópico encontrado no vídeo.

Atributo | Descrição 
---|---
name|O nome do tópico (por exemplo, "Azure"). 
appearances|Pode conter um ou mais [appearances](#appearances).
isTranscript|Se for VERDADEIRO, foi encontrado num transcript. Se for FALSO, foi encontrado num OCR.

### <a name="breakdown-insights"></a>informações de divisão

**tópicos** que são apresentados em **breakdowns**, descrevem os detalhes sobre cada tópico encontrado no vídeo.

|Atributo | Descrição |
|---|---|
|ID|ID de tópico exclusivo.|
|name|O nome do tópico.|
|Stem|Atualmente, este atributo não é utilizado.|
|palavras|Atualmente, este atributo não é utilizado.|
|Classificação|Classificação de relevância (superior é melhor).|

## <a name="sentiments"></a>sentiments

Atributo | Descrição
---|---
sentimentKey| Atualmente, são suportados os seguintes sentiments: positivo, independente, negativo. 
appearances|Pode conter um ou mais [appearances](#appearances)|.
seenDurationRatio|Presença relativos a duração de vídeo (0-1).

## <a name="audioeffects"></a>audioEffects

Atributo | Descrição 
---|---
audioEffectKey| Os valores válidos são: HandClaps de reconhecimento de voz, silêncio,.
appearances|Pode conter um ou mais [appearances](#appearances)
seenDurationRatio|Presença relativos a duração de vídeo (0-1).
seenDuration|Para quanto o efeito de áudio estava presente (em segundos).

## <a name="appearances"></a>appearances

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
name|O nome do participante. Por exemplo, orador n. º 1.
pictureUrl|O **pictureUrl** atributo está reservado para utilização futura.

## <a name="contentmoderation"></a>contentModeration

Atributo | Descrição 
---|---
adultClassifierValue|O nível de confiança de que o vídeo tem conteúdo para adultos.
racyClassifierValue|O nível de confiança de que o vídeo tem conteúdo racy.
bannedWordsCount|Número de palavras de profanity. 
bannedWordsRatio|Rácio de palavras de profanity do número total de palavras.
reviewRecommended|Valor booleano que indica se as vídeo deve ser revisto manualmente.
isAdult|Valores booleanos que indica se o vídeo é considerado um adulto vídeo depois de rever manual.

## <a name="audioeffectscategories"></a>audioEffectsCategories

Atributo | Descrição 
---|---
tipo|ID da categoria.
key|Um dos seguintes: HandClaps de reconhecimento de voz, silêncio,. 

## <a name="transcriptblocks"></a>transcriptBlocks

Atributo | Descrição
---|---
ID|ID do bloco.
linhas|Pode conter um ou mais [linhas](#lines)
sentimentIds|O **sentimentIds** atributo está reservado para utilização futura.
thumbnailIds|O **thumbnailIds** atributo está reservado para utilização futura.
Sentiment|O sentimento no bloco de (0-1, negativo para positivo).
faces|Pode conter um ou mais [faces](#faces).
ocrs|Pode conter um ou mais [ocrs](#ocrs).
audioEffectInstances|Pode conter um ou mais [audioEffectInstances](#audioEffectInstances).
em segundo plano|Pode conter um ou mais [em segundo plano](#scenes).
Anotações|Pode conter zero ou mais [anotações](#annotations).

## <a name="ocrs"></a>ocrs

Descreve momento a partir do qual as vídeo que foi encontrado o conteúdo de texto. 

Atributo | Descrição 
---|---
timeRange|O intervalo de tempo as vídeo original.
adjustedTimeRange|AdjustedTimeRange é o intervalo de tempo relativo à lista de reprodução atual. Uma vez que pode criar uma lista de reprodução de linhas diferentes de vídeos diferentes, pode demorar uma hora de um vídeo e utilizar apenas 1 linha a partir dos mesmos, por exemplo, 10:00-10:15. Nesse caso, terá de uma lista de reprodução com 1 linha, onde o intervalo de tempo é 10-10:00:15, mas o adjustedTimeRange é 00:00-00:15.
linhas|Pode conter um ou mais [linhas](#lines).

## <a name="lines"></a>linhas

### <a name="transcriptblocks"></a>transcriptBlocks

**linhas** que são apresentados em **transcriptBlocks**, descrevem as linhas de transcrições encontradas no vídeo.

Atributo | Descrição 
---|---
ID| O ID da linha.
timeRange|O intervalo de tempo as vídeo original.
adjustedTimeRange|AdjustedTimeRange é o intervalo de tempo relativo à lista de reprodução atual. Uma vez que pode criar uma lista de reprodução de linhas diferentes de vídeos diferentes, pode demorar uma hora de um vídeo e utilizar apenas 1 linha a partir dos mesmos, por exemplo, 10:00-10:15. Nesse caso, terá de uma lista de reprodução com 1 linha, onde o intervalo de tempo é 10-10:00:15, mas o adjustedTimeRange é 00:00-00:15.
participantID| O id de orador desta linha.
texto| Transcript.
isIncluded| Na base breakdowns sempre verdadeiras. No derivada listas de reprodução, as linhas que foram incluídas na origem de vídeo, estão definidos para isIncluded = true. Todas as outras linhas são falsas.

### <a name="ocrs"></a>ocrs

**linhas** que são apresentados em **ocrs**, descrevem as linhas de OCRs encontrados no vídeo.

Atributo | Descrição 
---|---
ID|O ID OCR.
Largura|Atualmente, este atributo não é utilizado.
Altura|Atualmente, este atributo não é utilizado.
idioma|O idioma de OCR.
textData|O texto de OCR.
confiança|A classificação de confiança (superior é melhor).

## <a name="scenes"></a>em segundo plano

Atributo | Descrição 
---|---
ID|O ID de cenas.
timeRange|Contém um **timeRange**.
keyFrame|A hora da moldura chave.
capturas de|Pode conter um ou mais [capturas](#shots).

## <a name="shots"></a>capturas de

Atributo | Descrição 
---|---
ID||O ID de captura.
timeRange|Contém um **timeRange**.
keyFrame|A hora da moldura chave.

## <a name="audioeffectinstances"></a>audioEffectInstances

Atributo | Descrição 
---|---
tipo|O índice do evento de áudio: Laughter = 1, HandClaps = 2, música = 3, voz = 4, silêncio = 5
intervalos|Pode conter um ou mais [intervalos](#ranges).

## <a name="ranges"></a>intervalos

**intervalos** que são apresentados em **audioEffectInstances**, descrevem áudio efeitos nesses intervalos.

Atributo | Descrição 
---|---
timeRange|O intervalo de tempo as vídeo original.
adjustedTimeRange|AdjustedTimeRange é o intervalo de tempo relativo à lista de reprodução atual. Uma vez que pode criar uma lista de reprodução de linhas diferentes de vídeos diferentes, pode demorar uma hora de um vídeo e utilizar apenas uma linha a partir dos mesmos, por exemplo, 10:00-10:15. Nesse caso, terá de uma lista de reprodução com 1 linha, onde o intervalo de tempo é 10-10:00:15, mas o adjustedTimeRange é 00:00-00:15.

## <a name="annotations"></a>Anotações

Devolve a etiquetas com base nos objetos reconhecível, beings de maior duração, scenery, ações e padrões de visual.

|Atributo|Descrição|
|---|---|
|ID|O ID da anotação.|
|Nome|O nome da anotação (por exemplo, jogos Athletic, Frames Jumbo negra, pessoa).|
|Appearances|Pode conter um ou mais appearances.|

## <a name="brands"></a>marcas

Empresas e produtos nomes marca detetados na voz transcript de texto e/ou OCR de vídeo. Não inclui reconhecimento visual de marcas ou deteção de logótipo.

Atributo | Descrição
---|---
ID | O ID de uma marca.
name | O nome da marca do Bing ou uma marca personalizada.  
wikiId | O sufixo do url de wikipedia marca. Por exemplo, "Target_Corporation" é o sufixo de [ https://en.wikipedia.org/wiki/Target_Corporation ](https://en.wikipedia.org/wiki/Target_Corporation).
wikiUrl | A marca do url de Wikipedia, se existir. Por exemplo, [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
confiança | O valor de confiança do detector de marca de indexador de vídeo (0-1).
descrição | Descrição da marca extraída de Wikipedia. 
appearances | Pode conter um ou mais appearances.
seenDuration | Presença relativos a duração de vídeo (0-1).

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre como criar a seus próprios repartição, consulte [ver e editar as informações de vídeo indexador](video-indexer-view-edit.md).

Para obter informações sobre como incorporar widgets na sua aplicação, consulte [widgets do indexador de incorporação de vídeo para as suas aplicações](video-indexer-embed-widgets.md). 

## <a name="see-also"></a>Consulte também

[Indexador de vídeo API](https://videobreakdown.portal.azure-api.net/docs/services/582074fb0dc56116504aed75/operations/5857caeb0dc5610f9ce979e4)

[Descrição geral do vídeo indexador](video-indexer-overview.md)

