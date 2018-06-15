---
title: Examine a saída do indexador de vídeo do Azure produzida pela v2 API | Microsoft Docs
description: Este tópico examina a saída de vídeo indexador produzida pela v2 API.
services: cognitive services
documentationcenter: ''
author: juliako
manager: cfowler
ms.service: cognitive-services
ms.topic: article
ms.date: 05/30/2018
ms.author: juliako
ms.openlocfilehash: 6c410b054ba98961d15a4db0ff7eaa2804245cb0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355873"
---
# <a name="examine-the-video-indexer-output-produced-by-v2-api"></a>Examine a saída de vídeo indexador produzida pela v2 API

> [!Note]
> As APIs do vídeo indexador V1 estão agora preteridas e serão removidas no 1ª de Agosto de 2018. Deve começar a utilizar as APIs do indexador de vídeo v2 para evitar interrupções.
>
> Para desenvolver com APIs de v2 do indexador de vídeo, consulte as instruções que se encontram [aqui](https://api-portal.videoindexer.ai/). 

Quando chamar o **obter o índice de vídeo** API e o estado de resposta está OK, obter um resultado JSON detalhado como o conteúdo da resposta. O conteúdo JSON contém detalhes sobre as informações de vídeos especificadas. As informações incluem dimensões, como: transcrições ocrs, faces, tópicos, blocos, etc. As dimensões tem instâncias de intervalos de tempo que mostram quando cada dimensão apareceu no vídeo.  

Pode também visualmente examinar insights resumidos as vídeo, premindo o **reproduzir** botão o vídeo no portal do indexador de vídeo. Para obter mais informações, consulte [ver e editar insights vídeos](video-indexer-view-edit.md).

![Informações](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

Este artigo examina o conteúdo JSON devolvido pelo **obter o índice de vídeo** API. 

> [!NOTE]
> Expiração de todos os tokens de acesso no vídeo indexador é uma hora.


## <a name="root-elements"></a>Elementos de raiz

|Nome|Descrição|
|---|---|
|accountId|VI a lista de reprodução conta ID.|
|ID|ID de. a lista de reprodução|
|name|Nome da lista de reprodução.|
|descrição|Descrição da lista de reprodução.|
|userName|O nome de utilizador que criou a lista de reprodução.|
|criado|Hora de criação de listas de reprodução.|
|privacyMode|Modo de privacidade para a lista de reprodução (público/privado).|
|state|A lista de reprodução (carregado, processamento, processados, com falhas, em quarentena).|
|isOwned|Indica se a lista de reprodução foi criada pelo utilizador atual.|
|isEditable|Indica se o utilizador atual tem autorização para editar a lista de reprodução.|
|isBase|Indica se a lista de reprodução está uma lista de reprodução base (um vídeo) ou uma lista de reprodução constituídos por outros vídeos (derivados).|
|durationInSeconds|A duração total da lista de reprodução.|
|summarizedInsights|Contém um [summarizedInsights](#summarizedinsights).
|vídeos|Uma lista de [vídeos](#videos) construir a lista de reprodução.<br/>Se esta lista de reprodução de construído de intervalos de tempo de outros vídeos (derivados), os vídeos nesta lista irá conter apenas os dados de intervalos de tempo incluídos.|

```json
{
  "accountId": "bca61527-1221-bca6-1527-a90000002000",
  "id": "abc3454321",
  "name": "My first video",
  "description": "I am trying VI",
  "userName": "Some name",
  "created": "2018/2/2 18:00:00.000",
  "privacyMode": "Private",
  "state": "Processed",
  "isOwned": true,
  "isEditable": false,
  "isBase": false,
  "durationInSeconds": 120, 
  "summarizedInsights" : { . . . }
  "videos": [{ . . . }]
}
```

## <a name="summarizedinsights"></a>summarizedInsights

Esta secção mostra o resumo de informações do.

|Atributo | Descrição|
|---|---|
|name|O nome do vídeo. Por exemplo, o Monitor do Azure.|
|shortId|O ID do vídeo. Por exemplo, 63c6d532ff.|
|privacyMode|A divisão pode ter um dos seguintes modos: **privada**, **pública**. **Pública** -o vídeo está visível para todos os utilizadores na sua conta e qualquer pessoa que tenha uma ligação para o vídeo. **Privada** -o vídeo está visível para todos os utilizadores na sua conta.|
|duração|Contém uma duração que descreve o tempo de que uma informação ocorreu. Duração é em segundos.|
|thumbnailUrl|Miniatura as vídeo completa URL. Por exemplo, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...". Tenha em atenção que, se o vídeo é privado, o URL contém um token de acesso de uma hora. Depois de uma hora, o URL será já não é válido e terá de obter a repartição novamente com um novo url no mesmo, ou chamar GetAccessToken para obter um novo token de acesso e construir o url completo manualmente ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [ accessToken]').|
|faces|Pode conter um ou mais faces. Para obter mais informações, consulte [faces](#faces).|
|Tópicos|Pode conter um ou mais tópicos. Para obter mais informações, consulte [tópicos](#topics).|
|sentiments|Pode conter um ou mais sentiments. Para obter mais informações, consulte [sentiments](#sentiments).|
|audioEffects| Pode conter um ou mais audioEffects. Para obter mais informações, consulte [audioEffects](#audioeffects).|
|marcas| Pode conter zero ou mais marcas. Para obter mais informações, consulte [marcas](#brands).|
|Estatísticas | Para obter mais informações, consulte [estatísticas](#statistics).|

### <a name="statistics"></a>Estatísticas

|Nome|Descrição|
|---|---|
|CorrespondenceCount|Número de correspondences as vídeo.|
|WordCount|O número de palavras por orador.|
|SpeakerNumberOfFragments|A quantidade de fragmentos de orador tem um vídeo.|
|SpeakerLongestMonolog|Monolog mais longo de orador. Se tiver de altifalante silences dentro de monolog está incluído. Silêncio no início e fim do monolog é removido.| 
|SpeakerTalkToListenRatio|O cálculo baseia-se o tempo gasto em monolog de orador (sem silêncio entre) dividido pelo tempo total do vídeo. A hora é arredondada para o terceiro vírgula decimal.|

## <a name="videos"></a>vídeos

|Nome|Descrição|
|---|---|
|accountId|VI o vídeo conta ID.|
|ID|ID de. as vídeo|
|name|Nome das vídeo.
|state|Estado das vídeo (carregado, processamento, processados, com falhas, em quarentena).|
|processingProgress|O progresso de processamento durante o processamento (por exemplo, 20%).|
|failureCode|O código de falha se processou (por exemplo, ' UnsupportedFileType').|
|failureMessage|A mensagem de falha se não foi possível processar.|
|externalId|Externo id o vídeo (se especificado pelo utilizador).|
|URL externo|Url externo o vídeo (se especificado pelo utilizador).|
|do IdP|Externo metadados o vídeo (se especificado pelo utilizador).|
|isAdult|Se o vídeo foi manualmente revistos e identificado como um vídeo para adultos.|
|informações|O objeto de informações.|
|thumbnailUrl|Miniatura as vídeo completa URL. Por exemplo, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...". Tenha em atenção que, se o vídeo é privado, o URL contém um token de acesso de uma hora. Depois de uma hora, o URL será já não é válido e terá de obter a repartição novamente com um novo url no mesmo, ou chamar GetAccessToken para obter um novo token de acesso e construir o url completo manualmente ('https://www.videoindexer.ai/api/Thumbnail/[shortId] / [ThumbnailId]? accessToken = [ accessToken]').|
|publishedUrl|Um url para transmitir o vídeo.|
|publishedUrlProxy|Um url para transmitir o vídeo de (para dispositivos da Apple).|
|viewToken|Um token de vista suma lived para o vídeo de transmissão em fluxo.|
|sourceLanguage|Idioma da origem de vídeo.|
|idioma|Idioma da real as vídeo (tradução).|
|indexingPreset|A predefinição utilizada para o vídeo de índice.|
|streamingPreset|A predefinição utilizada para publicar o vídeo.|
|linguisticModelId|O modelo de CRIS utilizado para transcribe o vídeo.|

```json
{
    "videos": [{
        "accountId": "2cbbed36-1972-4506-9bc7-55367912df2d",
        "id": "142a356aa6",
        "state": "Processed",
        "privacyMode": "Private",
        "processingProgress": "100%",
        "failureCode": "General",
        "failureMessage": "",
        "externalId": null,
        "externalUrl": null,
        "metadata": null,
        "insights": {. . . },
        "thumbnailId": "89d7192c-1dab-4377-9872-473eac723845",
        "publishedUrl": "https://videvmediaservices.streaming.mediaservices.windows.net:443/d88a652d-334b-4a66-a294-3826402100cd/Xamarine.ism/manifest",
        "publishedProxyUrl": null,
        "viewToken": "Bearer=<token>",
        "sourceLanguage": "En-US",
        "language": "En-US",
        "indexingPreset": "Default",
        "linguisticModelId": "00000000-0000-0000-0000-000000000000"
    }],
}
```
### <a name="insights"></a>informações

As informações são um conjunto de dimensões (por exemplo, linhas de transcript faces, marcas, etc.), onde cada dimensão é uma lista de elementos exclusivos (por exemplo, face1 face2, face3) e cada elemento tem os suas próprias metadados e uma lista das suas instâncias (que são intervalos de tempo com opcional metadados adicionais).

Um rosto poderá ter um ID, um nome, uma miniatura, outros metadados e uma lista das suas instâncias temporais (por exemplo: 00:00:05 – 00:00:10, 00:01:00 - 00:02:30 e 00:41:21 – 00:41:49.) Cada instância temporal pode ter metadados adicionais. Por exemplo, o retângulo de rostos coordena (20,230,60,60).

|Versão|A versão de código|
|---|---|
|sourceLanguage|Idioma da origem de vídeo (partindo do princípio de um idioma principal). Sob a forma de um [BCP 47](https://tools.ietf.org/html/bcp47) cadeia.|
|idioma|O idioma de insights (traduzido do idioma de origem). Sob a forma de um [BCP 47](https://tools.ietf.org/html/bcp47) cadeia.|
|transcript|O [transcript](#transcript) dimensão.|
|OCR|O [ocr](#ocr) dimensão.|
|Palavras-chave|O [palavras-chave](#keywords) dimensão.|
|faces|O [faces](#faces) dimensão.|
|Etiquetas|O [etiquetas](#labels) dimensão.|
|capturas de|O [capturas](#shots) dimensão.|
|marcas|O [marcas](#brands) dimensão.|
|audioEffects|O [audioEffects](#audioEffects) dimensão.|
|sentiments|O [sentiments](#sentiments) dimensão.|
|visualContentModeration|O [visualContentModeration](#visualcontentmoderation) dimensão.|
|textualConentModeration|O [textualConentModeration](#textualconentmoderation) dimensão.|

Exemplo:

```json
{
  "version": "0.9.0.0",
  "sourceLanguage": "en-US",
  "language": "es-ES",
  "transcript": ...,
  "ocr": ...,
  "keywords": ...,
  "faces": ...,
  "labels": ...,
  "shots": ...,
  "brands": ...,
  "audioEffects": ...,
  "sentiments": ...,
  "visualContentModeration": ...,
  "textualConentModeration": ...
}
```

#### <a name="transcript"></a>transcript

|Nome|Descrição|
|---|---|
|ID|O ID de linha.|
|texto|O transcript próprio.|
|idioma|O idioma de transcript. Destina-se suportar transcript onde cada linha pode ter um idioma diferente.|
|instâncias|Uma lista de intervalos de tempo em que esta linha apareceu. Se a instância for transcript, terá apenas 1 instância.|

Exemplo:

```json
"transcript": [
{
    "id": 0,
    "text": "Hi I'm Doug from office.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    }
    ]
},
{
    "id": 1,
    "text": "I have a guest. It's Michelle.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:02.7200000",
        "end": "00:00:03.9600000"
    }
    ]
}
] 
```

#### <a name="ocr"></a>OCR

|Nome|Descrição|
|---|---|
|ID|O ID de linha OCR.|
|texto|O texto de OCR.|
|confiança|Confiança de reconhecimento.|
|idioma|O idioma de OCR.|
|instâncias|Uma lista de intervalos de tempo em que esta OCR apareceu (o mesmo OCR pode aparecer várias vezes).|

```json
"ocr": [
    {
      "id": 0,
      "text": "LIVE FROM NEW YORK",
      "confidence": 0.91,
      "language": "en-US",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:52"
        }
      ]
    },
    {
      "id": 1,
      "text": "NOTICIAS EN VIVO",
      "confidence": 0.9,
      "language": "es-ES",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:28"
        },
        {
          "start": "00:00:32",
          "end": "00:00:38"
        }
      ]
    }
  ],
```

#### <a name="keywords"></a>Palavras-chave

|Nome|Descrição|
|---|---|
|ID|O id de palavra-chave.|
|texto|O texto da palavra-chave.|
|confiança|Confiança de reconhecimento da palavra-chave.|
|idioma|O idioma de palavra-chave (quando convertidos).|
|instâncias|Uma lista de intervalos de tempo em que esta palavra-chave apareceu (uma palavra-chave pode aparecer várias vezes).|

```json
"keywords": [
{
    "id": 0,
    "text": "office",
    "confidence": 1.6666666666666667,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    },
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    }
    ]
},
{
    "id": 1,
    "text": "icons",
    "confidence": 1.4,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    },
    {
        "start": "00:00:13.9900000",
        "end": "00:00:15.6100000"
    }
    ]
}
] 

```

#### <a name="faces"></a>faces

|Nome|Descrição|
|---|---|
|ID|O ID de letra.|
|name|O nome de letra. Pode ser 'Desconhecido #0', uma celebrity identificado ou uma pessoa treinado do cliente.|
|confiança|Confiança de identificação de letra.|
|descrição|Se for um celebrity, poderá ser respetiva descrição: "Satya Nadella nasceu em...". |
|thumbnalId|O id de miniatura desse tipo de letra.|
|knownPersonId|Se se tratar de uma pessoa conhecida, um ID interno.|
|ReferenceId|Se for um celebrity Bing, o ID do Bing.|
|referenceType|Atualmente, apenas o Bing.|
|título|Se for um celebrity, o título dele (por exemplo "CEO da Microsoft").|
|imageUrl|Se for um celebrity, o url da imagem.|
|instâncias|Estes são instâncias de onde o tipo de letra apareceu no intervalo de tempo especificado. Cada instância tem também uma thumbnailsId. |

```json
"faces": [{
    "id": 2002,
    "name": "Xam 007",
    "confidence": 0.93844,
    "description": null,
    "thumbnailId": "00000000-aee4-4be2-a4d5-d01817c07955",
    "knownPersonId": "8340004b-5cf5-4611-9cc4-3b13cca10634",
    "referenceId": null,
    "title": null,
    "imageUrl": null,
    "instances": [{
        "thumbnailsIds": ["00000000-9f68-4bb2-ab27-3b4d9f2d998e",
        "cef03f24-b0c7-4145-94d4-a84f81bb588c"],
        "adjustedStart": "00:00:07.2400000",
        "adjustedEnd": "00:00:45.6780000",
        "start": "00:00:07.2400000",
        "end": "00:00:45.6780000"
    },
    {
        "thumbnailsIds": ["00000000-51e5-4260-91a5-890fa05c68b0"],
        "adjustedStart": "00:10:23.9570000",
        "adjustedEnd": "00:10:39.2390000",
        "start": "00:10:23.9570000",
        "end": "00:10:39.2390000"
    }]
}]
```

#### <a name="labels"></a>Etiquetas

|Nome|Descrição|
|---|---|
|ID|O ID da etiqueta.|
|name|O nome de etiqueta (por exemplo, 'Computador', 'TV').|
|idioma|Idioma de nome etiqueta (quando convertidos). BCP 47|
|instâncias|Uma lista de intervalos de tempo em que esta etiqueta apareceu (uma etiqueta pode aparecer várias vezes). Cada instância tem um campo de confiança. |


```json
"labels": [
    {
      "id": 0,
      "name": "person",
      "language": "en-US",
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 00.0000000",
          "end": "00: 00: 25.6000000"
        },
        {
          "confidence": 1.0,
          "start": "00: 01: 33.8670000",
          "end": "00: 01: 39.2000000"
        }
      ]
    },
    {
      "name": "indoor",
      "language": "en-US",
      "id": 1,
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 06.4000000",
          "end": "00: 00: 07.4670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 09.6000000",
          "end": "00: 00: 10.6670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 11.7330000",
          "end": "00: 00: 20.2670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 21.3330000",
          "end": "00: 00: 25.6000000"
        }
      ]
    }
  ] 
```

#### <a name="shots"></a>capturas de

|Nome|Descrição|
|---|---|
|ID|O ID de captura.|
|keyFrames|Uma lista de chaves fotogramas dentro de captura (cada um tem um ID e uma lista de intervalos de tempo de instâncias). Instâncias de fotogramas chave tem um campo de thumbnailId com miniatura do keyFrame ID.|
|instâncias|Uma lista de intervalos de tempo desta captura (capturas tem apenas 1 instância).|

```json
"Shots": [
    {
      "id": 0,
      "keyFrames": [
        {
          "id": 0,
          "instances": [
            {
          "thumbnailId": "00000000-0000-0000-0000-000000000000",
              "start": "00: 00: 00.1670000",
              "end": "00: 00: 00.2000000"
            }
          ]
        }
      ],
      "instances": [
        {
       "thumbnailId": "00000000-0000-0000-0000-000000000000",   
          "start": "00: 00: 00.2000000",
          "end": "00: 00: 05.0330000"
        }
      ]
    },
    {
      "id": 1,
      "keyFrames": [
        {
          "id": 1,
          "instances": [
            {
          "thumbnailId": "00000000-0000-0000-0000-000000000000",        
              "start": "00: 00: 05.2670000",
              "end": "00: 00: 05.3000000"
            }
          ]
        }
      ],
      "instances": [
        {
      "thumbnailId": "00000000-0000-0000-0000-000000000000",
          "start": "00: 00: 05.2670000",
          "end": "00: 00: 10.3000000"
        }
      ]
    }
  ]
```

#### <a name="brands"></a>marcas

Empresas e produtos nomes marca detetados na voz transcript de texto e/ou OCR de vídeo. Não inclui reconhecimento visual de marcas ou deteção de logótipo.

|Nome|Descrição|
|---|---|
|ID|O ID de marca.|
|name|O nome de marcas.|
|wikiId | O sufixo do url de wikipedia marca. Por exemplo, "Target_Corporation" é o sufixo de [ https://en.wikipedia.org/wiki/Target_Corporation ](https://en.wikipedia.org/wiki/Target_Corporation).
|wikiUrl | A marca do url de Wikipedia, se existir. Por exemplo, [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|descrição|A descrição de marcas.|
|etiquetas|Uma lista de etiquetas predefinidas que se encontravam associadas esta marca.|
|confiança|O valor de confiança do detector de marca de indexador de vídeo (0-1).|
|instâncias|Uma lista de intervalos de tempo desta marca. Cada instância tem um brandType, que indica se esta marca apareceu no transcript ou no OCR.|

```json
"brands": [
{
    "id": 0,
    "name": "MicrosoftExcel",
    "referenceId": "Microsoft_Excel",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft_Excel",
    "referenceType": "Wiki",
    "description": "Microsoft Excel is a sprea..",
    "tags": [],
    "confidence": 0.975,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 00: 31.3000000",
        "end": "00: 00: 39.0600000"
    }
    ]
},
{
    "id": 1,
    "name": "Microsoft",
    "wikiId": "Microsoft",
    "wikiUrl": "http: //en.wikipedia.org/wiki/Microsoft",
    "description": "Microsoft Corporation is...",
    "tags": [
    "competitors",
    "technology"
    ],
    "confidence": 1.0,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 01: 44",
        "end": "00: 01: 45.3670000"
    },
    {
        "brandType": "Ocr",
        "start": "00: 01: 54",
        "end": "00: 02: 45.3670000"
    }
    ]
}
]
```

#### <a name="audioeffects"></a>audioEffects

|Nome|Descrição|
|---|---|
|ID|O ID de áudio em vigor.|
|tipo|O tipo de áudio efeito (por exemplo, silêncio Clapping, voz,).|
|instâncias|Uma lista de intervalos de tempo em que este efeito áudio apareceu.|

```json
"audioEffects": [
{
    "id": 0,
    "type": "Clapping",
    "instances": [
    {
        "start": "00:00:00",
        "end": "00:00:03"
    },
    {
        "start": "00:01:13",
        "end": "00:01:21"
    }
    ]
}
]
```

#### <a name="sentiments"></a>sentiments

Sentiments são agregados pelo respetivo campo sentimentType (neutras/positivos/negativo). Por exemplo, 0-0.1 0.1 0,2.

|Nome|Descrição|
|---|---|
|ID|O ID de dados de sentimento.|
|averageScore |A média de todas as pontuações de todas as instâncias desse tipo de dados de sentimento - positivos/neutras/negativo|
|instâncias|Uma lista de intervalos de tempo em que esta sentimento apareceu.|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
    "instances": [
    {
        "start": "00:00:13",
        "end": "00:00:21"
    }
    ]
}
]
```

#### <a name="visualcontentmoderation"></a>visualContentModeration

O bloco de visualContentModeration contém intervalos de tempo que o indexador de vídeo detetado potencialmente conteúdo para adultos. Se visualContentModeration estiver vazia, não há nenhum conteúdo para adultos que foi identificado.

Vídeos encontram-se que contém o conteúdo para adultos ou racy poderão estar disponíveis para apenas vista privada. Os utilizadores têm a opção para submeter um pedido para uma revisão humana do conteúdo, na qual caso o atributo IsAdult irá conter o resultado da revisão humano.

|Nome|Descrição|
|---|---|
|ID|O ID de visual moderação de interrupção de conteúdo.|
|adultScore|A classificação para adultos (a partir do conteúdo moderator).|
|racyScore|A classificação racy (a partir do conteúdo moderação de interrupção).|
|instâncias|Uma lista de intervalos de tempo em que antes eram este visual moderação de interrupção de conteúdo.|

```json
"VisualContentModeration": [
{
    "id": 0,
    "adultScore": 0.00069,
    "racyScore": 0.91129,
    "instances": [
    {
        "start": "00:00:25.4840000",
        "end": "00:00:25.5260000"
    }
    ]
},
{
    "id": 1,
    "adultScore": 0.99231,
    "racyScore": 0.99912,
    "instances": [
    {
        "start": "00:00:35.5360000",
        "end": "00:00:35.5780000"
    }
    ]
}
] 
```

#### <a name="textualconentmoderation"></a>textualConentModeration 

|Nome|Descrição|
|---|---|
|ID|O ID de moderação de interrupção conteúdo textual.|
|bannedWordsCount |O número de palavras banned.|
|bannedWordsRatio |O rácio do número total de palavras.|


## <a name="next-steps"></a>Passos Seguintes

[Indexador de vídeo API](https://videobreakdown.portal.azure-api.net/docs/services/582074fb0dc56116504aed75/operations/5857caeb0dc5610f9ce979e4)

Para obter informações sobre como incorporar widgets na sua aplicação, consulte [widgets do indexador de incorporação de vídeo para as suas aplicações](video-indexer-embed-widgets.md). 

