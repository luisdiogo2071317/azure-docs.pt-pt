---
title: Examine a saída do indexador de vídeo do Azure produzida pela v2 API | Documentos da Microsoft
description: Este tópico examina a saída do indexador de vídeo produzida pela v2 API.
services: cognitive services
documentationcenter: ''
author: juliako
manager: cfowler
ms.service: cognitive-services
ms.topic: article
ms.date: 07/25/2018
ms.author: juliako
ms.openlocfilehash: 9a926eb274e5e4cec721864d1d9c5faee8ec58ef
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39618344"
---
# <a name="examine-the-video-indexer-output-produced-by-v2-api"></a>Examine a saída do indexador de vídeo produzida pela v2 API

> [!Note]
> A API do Video Indexer V1 foi preterido no dia 1 de Agosto de 2018. Agora, deve utilizar a API do Video Indexer v2. <br/>Para programar com APIs do Video Indexer v2, veja as instruções presentes [aqui](https://api-portal.videoindexer.ai/). 

Quando chama a **índice de vídeo de introdução** API e o estado de resposta está OK, obtém uma saída JSON detalhada como o conteúdo de resposta. O conteúdo JSON contém detalhes de informações de vídeo especificadas. As informações incluem dimensões, como: transcrições, ocrs, rostos, tópicos, blocos, etc. As dimensões tem instâncias de intervalos de tempo que mostram quando cada dimensão apareceu no vídeo.  

Pode também visualmente examinar informações resumidas do vídeo ao premir o **reproduzir** botão do vídeo no portal do Video Indexer. Para obter mais informações, consulte [ver e editar informações de vídeo](video-indexer-view-edit.md).

![Informações](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

Este artigo examina o conteúdo JSON devolvido pelos **índice de vídeo de introdução** API. 

> [!NOTE]
> Expiração de todos os tokens de acesso no indexador de vídeo é uma hora.


## <a name="root-elements"></a>Elementos de raiz

|Nome|Descrição|
|---|---|
|accountId|VI a lista de reprodução conta ID.|
|ID|ID de. a lista de reprodução|
|nome|Nome da lista de reprodução.|
|descrição|Descrição da lista de reprodução.|
|userName|O nome de utilizador que criou a lista de reprodução.|
|criado|Hora de criação da lista de reprodução.|
|privacyMode|Modo de privacidade para a lista de reprodução (público/privado).|
|state|A lista de reprodução (carregado, processamento, processados, com falhas, em quarentena).|
|isOwned|Indica se a lista de reprodução foi criada pelo utilizador atual.|
|isEditable|Indica se o utilizador atual tem autorização para editar a lista de reprodução.|
|isBase|Indica se a lista de reprodução é uma lista de reprodução base (um vídeo) ou uma lista de reprodução é constituído por outros vídeos (derivados).|
|durationInSeconds|A duração total da lista de reprodução.|
|summarizedInsights|Contém um [summarizedInsights](#summarizedinsights).
|vídeos|Uma lista de [vídeos](#videos) construir a lista de reprodução.<br/>Se esta lista de reprodução de construído de intervalos de tempo de outros vídeos (derivados), os vídeos nesta lista irá conter apenas os dados dos intervalos de tempo incluídos.|

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

Esta secção mostra o resumo das informações.

|Atributo | Descrição|
|---|---|
|nome|O nome do vídeo. Por exemplo, o Azure Monitor.|
|shortId|O ID do vídeo. Por exemplo, 63c6d532ff.|
|privacyMode|Sua análise detalhada pode ter um dos seguintes modos: **privada**, **público**. **Público** -o vídeo é visível para todas as pessoas na sua conta e qualquer pessoa que tenha uma ligação para o vídeo. **Privada** -o vídeo é visível para todas as pessoas na sua conta.|
|duração|Contém uma duração que descreve o tempo de que uma informação ocorreu. A duração é em segundos.|
|thumbnailVideoId|O id do vídeo a partir do qual foi tirada a miniatura.
|thumbnailId|Id de miniatura do vídeo. Para obter a chamada de miniatura real Get-miniatura (https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-thumbnail) e passá-lo thumbnailVideoId e thumbnailId.|
|rostos|Pode conter zero ou mais rostos. Para obter mais informações, consulte [rostos](#faces).|
|palavras-chave|Pode conter zero ou mais palavras-chave. Para obter mais informações, consulte [palavras-chave](#keywords).|
|sentimentos|Pode conter zero ou mais sentimentos. Para obter mais informações, consulte [sentimentos](#sentiments).|
|audioEffects| Pode conter zero ou mais audioEffects. Para obter mais informações, consulte [audioEffects](#audioeffects).|
|etiquetas| Pode conter zero ou mais etiquetas. Para obter mais informações, consulte [etiquetas](#labels).|
|marcas| Pode conter zero ou mais marcas. Para obter mais informações, consulte [marcas](#brands).|
|Estatísticas | Para obter mais informações, consulte [estatísticas](#statistics).|

## <a name="videos"></a>vídeos

|Nome|Descrição|
|---|---|
|accountId|VI o vídeo conta ID.|
|ID|ID do vídeo|
|nome|Nome do vídeo.
|state|Estado do vídeo (carregado, processamento, processados, com falhas, em quarentena).|
|processingProgress|O progresso do processamento durante o processamento (por exemplo, 20%).|
|failureCode|O código de falha se processou (por exemplo, "UnsupportedFileType').|
|failureMessage|A mensagem de falha se não foi possível processar.|
|externalId|ID externo o vídeo (se especificado pelo utilizador).|
|URL externo|Url externo o vídeo (se especificado pelo utilizador).|
|do IdP|Os metadados do vídeo externo (se especificado pelo utilizador).|
|isAdult|Indica se o vídeo foi revisado e identificado como um vídeo para adultos manualmente.|
|informações|O objeto de informações. Para obter mais informações, consulte [insights](#insights).|
|thumbnailId|Id de miniatura do vídeo. Para obter a chamada de miniatura real Get-miniatura (https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-thumbnail) e transmita-o id do vídeo e thumbnailId.|
|publishedUrl|Um url para transmitir o vídeo.|
|publishedUrlProxy|Um url para transmitir o vídeo de (para dispositivos da Apple).|
|viewToken|Um token de vista de resumo de duração para o vídeo de transmissão em fluxo.|
|sourceLanguage|Idioma de origem do vídeo.|
|Idioma|Idioma da real do vídeo (tradução).|
|indexingPreset|A configuração predefinida utilizada para o vídeo de índice.|
|streamingPreset|A configuração predefinida utilizada para publicar o vídeo.|
|linguisticModelId|O modelo CRIS utilizado de transcrever o vídeo.|
|Estatísticas | Para obter mais informações, consulte [estatísticas](#statistics).|

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

As informações são um conjunto de dimensões (por exemplo, linhas de transcrição, rostos, marcas, etc.), onde cada dimensão é uma lista de elementos exclusivos (por exemplo, face1, face2, face3), e cada elemento tem seus próprios metadados e uma lista das suas instâncias (que são os intervalos de tempo com opcional metadados adicionais).

Um rosto pode ter um ID, um nome, uma miniatura, outros metadados e uma lista das suas instâncias temporais (por exemplo: 00: 00:05 – 00:00:10, 01: 00:00 - 00:02:30 e 00:41:21 – 00:41:49.) Cada instância temporal pode ter metadados adicionais. Por exemplo, o retângulo da face as coordenadas (20,230,60,60).

|Versão|A versão de código|
|---|---|
|sourceLanguage|Idioma de origem do vídeo (supondo que um idioma principal). Na forma de um [BCP 47](https://tools.ietf.org/html/bcp47) cadeia de caracteres.|
|Idioma|O idioma de informações (traduzido do idioma de origem). Na forma de um [BCP 47](https://tools.ietf.org/html/bcp47) cadeia de caracteres.|
|transcrição|O [transcrição](#transcript) dimensão.|
|OCR|O [ocr](#ocr) dimensão.|
|palavras-chave|O [palavras-chave](#keywords) dimensão.|
|blocos|Pode conter um ou mais [blocos](#blocks)|
|rostos|O [rostos](#faces) dimensão.|
|etiquetas|O [etiquetas](#labels) dimensão.|
|capturas de|O [capturas](#shots) dimensão.|
|marcas|O [marcas](#brands) dimensão.|
|audioEffects|O [audioEffects](#audioEffects) dimensão.|
|sentimentos|O [sentimentos](#sentiments) dimensão.|
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

#### <a name="blocks"></a>blocos

Atributo | Descrição
---|---
ID|ID do bloco.|
instâncias|Uma lista de intervalos de tempo deste bloco.|

#### <a name="transcript"></a>transcrição

|Nome|Descrição|
|---|---|
|ID|O ID de linha.|
|texto|A transcrição em si.|
|Idioma|O idioma de transcrição. A finalidade oferecer suporte a transcrição em que cada linha pode ter um idioma diferente.|
|instâncias|Uma lista de intervalos de tempo em que esta linha apareceu. Se a instância de transcrição, ele terá apenas 1 instância.|

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
|ID|O ID de linha de OCR.|
|texto|O texto de OCR.|
|confiança|A confiança de reconhecimento.|
|Idioma|O idioma de OCR.|
|instâncias|Uma lista de intervalos de tempo em que este OCR apareceu (o mesmo OCR pode aparecer várias vezes).|

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

#### <a name="keywords"></a>palavras-chave

|Nome|Descrição|
|---|---|
|ID|O ID de palavra-chave.|
|texto|O texto de palavra-chave.|
|confiança|Confiança de reconhecimento da palavra-chave.|
|Idioma|O idioma de palavra-chave (quando traduzido).|
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

#### <a name="faces"></a>rostos

|Nome|Descrição|
|---|---|
|ID|O ID do rosto.|
|nome|O nome de face. Pode ser ' desconhecido n º 0", uma celebridade identificada ou uma pessoa de preparação do cliente.|
|confiança|A confiança de identificação de face.|
|descrição|Uma descrição da celebridade. |
|thumbnalId|O ID da miniatura do que enfrentam.|
|knownPersonId|Se se trata de uma pessoa conhecida, sua ID de interno.|
|referenceId|Se for uma celebridade do Bing, o ID do Bing.|
|referenceType|Atualmente, apenas o Bing.|
|título|Se for uma celebridade, seu título (por exemplo "CEO da Microsoft").|
|imageUrl|Se for uma celebridade, o seu url da imagem.|
|instâncias|Estes são instâncias de onde o mostrador apareceu no intervalo de tempo especificado. Cada instância tem também um thumbnailsId. |

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

#### <a name="labels"></a>etiquetas

|Nome|Descrição|
|---|---|
|ID|O ID da etiqueta.|
|nome|O nome de etiqueta (por exemplo, "Computador", "Programas de TV").|
|Idioma|Idioma de nome etiqueta (quando traduzido). BCP 47|
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
|quadros-chave|Uma lista de quadros-chave dentro da captura (cada um tem um ID e uma lista de intervalos de tempo de instâncias). Instâncias de quadros-chave têm um campo de thumbnailId com miniatura do quadro-chave ID.|
|instâncias|Uma lista de intervalos de tempo desta captura (capturas de tem apenas 1 instância).|

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

Negócios e produto nomes de marca detetados na conversão de voz a transcrição de texto e/ou OCR de vídeo. Não inclui reconhecimento visual de marcas ou deteção de logótipo.

|Nome|Descrição|
|---|---|
|ID|O ID da marca.|
|nome|O nome de marcas.|
|referenceId | O sufixo do url da wikipédia marca. Por exemplo, "Target_Corporation" é o sufixo de [ https://en.wikipedia.org/wiki/Target_Corporation ](https://en.wikipedia.org/wiki/Target_Corporation).
|referenceUrl | A marca do url da Wikipédia, se existir. Por exemplo, [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|descrição|A descrição de marcas.|
|etiquetas|Uma lista de etiquetas predefinidas que foram associados essa marca.|
|confiança|O valor de confiança do detetor de marca do Video Indexer (0-1).|
|instâncias|Uma lista de intervalos de tempo desta marca. Cada instância possui um brandType, que indica se esta marca apareceu na transcrição ou no OCR.|

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
    "referenceId": "Microsoft",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft",
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

#### <a name="statistics"></a>Estatísticas

|Nome|Descrição|
|---|---|
|CorrespondenceCount|Número de correspondências no vídeo.|
|WordCount|O número de palavras por orador.|
|SpeakerNumberOfFragments|A quantidade de fragmentos o orador tem um vídeo.|
|SpeakerLongestMonolog|Monolog de mais longo do orador. Se o orador tiver silences dentro o monolog está incluído. Silence, no início e fim do monolog é removido.| 
|SpeakerTalkToListenRatio|O cálculo baseia-se o tempo gasto em monolog do orador (sem o silence entre) dividido pelo tempo total do vídeo. A hora é arredondada para o terceiro ponto decimal.|

#### <a name="audioeffects"></a>audioEffects

|Nome|Descrição|
|---|---|
|ID|O ID do efeito de áudio.|
|tipo|O tipo de efeito de áudio (por exemplo, Clapping, conversão de voz, silêncio).|
|instâncias|Uma lista de intervalos de tempo em que esse efeito de áudio apareceu.|

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

#### <a name="sentiments"></a>sentimentos

Sentimentos são agregados pelo respetivo campo sentimentType (positivo/neutra/negativo). Por exemplo, 0-0.1, 0,1 0,2.

|Nome|Descrição|
|---|---|
|ID|O ID de sentimentos.|
|averageScore |A média de todas as pontuações de todas as instâncias desse tipo de sentimento - positivo/neutra/negativo|
|instâncias|Uma lista de intervalos de tempo em que esse sentimento apareceu.|
|sentimentType |O tipo pode ser "Neutral", 'Positivo' ou 'Negativo'.|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
    "sentimentType": "Positive",
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

O bloco de visualContentModeration contém intervalos de tempo que o indexador de vídeo encontrado potencialmente ter conteúdo para adultos. Se visualContentModeration estiver vazio, não há nenhum conteúdo para adultos que foi identificado.

Vídeos que encontram-se para conter o conteúdo de adultos poderão estar disponíveis para ver privada apenas. Os utilizadores têm a opção para submeter um pedido para uma revisão de segurança do conteúdo, em que, caso o atributo IsAdult irá conter o resultado da revisão humana.

|Nome|Descrição|
|---|---|
|ID|O ID de moderação de conteúdos visual.|
|adultScore|Como sendo para adultos (a partir do moderador de conteúdo).|
|racyScore|O classificado como indecoroso (a partir de moderação de conteúdos).|
|instâncias|Uma lista de intervalos de tempo em que este moderação de conteúdos visual apareceu.|

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
|ID|O ID de moderação de conteúdo textual.|
|bannedWordsCount |O número de palavras excluídas.|
|bannedWordsRatio |O rácio do número total de palavras.|


## <a name="next-steps"></a>Passos Seguintes

[API do indexador de vídeo](https://videobreakdown.portal.azure-api.net/docs/services/582074fb0dc56116504aed75/operations/5857caeb0dc5610f9ce979e4)

Para obter informações sobre como incorporar widgets na sua aplicação, consulte [widgets de incorporar o Video Indexer nas suas aplicações](video-indexer-embed-widgets.md). 

