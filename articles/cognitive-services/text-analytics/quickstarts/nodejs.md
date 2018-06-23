---
title: Guia de introdução do node.js para os serviços do Azure cognitivos, análise de texto API | Microsoft Docs
description: Exemplos de código e informações de GET para o ajudar a rapidamente começar a utilizar a API de análise de texto no Microsoft serviços cognitivos no Azure.
services: cognitive-services
documentationcenter: ''
author: ashmaka
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 05/02/2018
ms.author: ashmaka
ms.openlocfilehash: 44c4d7cf91983f5e5ae7021feb19c81f7edd6c61
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354151"
---
# <a name="quickstart-for-text-analytics-api-with-nodejs"></a>Início rápido para análise de texto API com o Node.js 
<a name="HOLTop"></a>

Este artigo mostra como a [detetar idioma](#Detect), [analisar o sentimento](#SentimentAnalysis), [extrair expressões chaves](#KeyPhraseExtraction), e [identificar entidades ligadas](#Entities) utilizando o [APIs de análise de texto](//go.microsoft.com/fwlink/?LinkID=759711) com o Node.JS.

Consulte o [definições da API](//go.microsoft.com/fwlink/?LinkID=759346) documentação técnica para as APIs.

## <a name="prerequisites"></a>Pré-requisitos

Tem de ter um [conta da API de serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **API de análise de texto**. Pode utilizar o **escalão gratuito para transações/mês 5000** para concluir este guia de introdução.

Também tem de ter o [chave de acesso e de ponto final](../How-tos/text-analytics-how-to-access-key.md) que foi gerado durante a sessão cópias de segurança. 

<a name="Detect"></a>

## <a name="detect-language"></a>Detetar idioma

A API de deteção de idioma Deteta o idioma de um texto documento, utilizando o [método detetar idioma](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7).

1. Crie um novo projeto de Node.JS no seu IDE favorito.
2. Adicione o código fornecido abaixo.
3. Substitua o `accessKey` valor com uma chave de acesso válida para a sua subscrição.
4. Substituir a localização na `uri` (atualmente `westus`) para a região que inscreveu no.
5. Execute o programa.

```javascript
'use strict';

let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the accessKey string value with your valid access key.
let accessKey = 'enter key here';

// Replace or verify the region.

// You must use the same region in your REST API call as you used to obtain your access keys.
// For example, if you obtained your access keys from the westus region, replace 
// "westcentralus" in the URI below with "westus".

// NOTE: Free trial access keys are generated in the westcentralus region, so if you are using
// a free trial access key, you should not need to change this region.
let uri = 'westus.api.cognitive.microsoft.com';
let path = '/text/analytics/v2.0/languages';

let response_handler = function (response) {
    let body = '';
    response.on ('data', function (d) {
        body += d;
    });
    response.on ('end', function () {
        let body_ = JSON.parse (body);
        let body__ = JSON.stringify (body_, null, '  ');
        console.log (body__);
    });
    response.on ('error', function (e) {
        console.log ('Error: ' + e.message);
    });
};

let get_language = function (documents) {
    let body = JSON.stringify (documents);

    let request_params = {
        method : 'POST',
        hostname : uri,
        path : path,
        headers : {
            'Ocp-Apim-Subscription-Key' : accessKey,
        }
    };

    let req = https.request (request_params, response_handler);
    req.write (body);
    req.end ();
}

let documents = { 'documents': [
    { 'id': '1', 'text': 'This is a document written in English.' },
    { 'id': '2', 'text': 'Este es un document escrito en Español.' },
    { 'id': '3', 'text': '这是一个用中文写的文件' }
]};

get_language (documents);
```

**Resposta de deteção de idioma**

Uma resposta com êxito é devolvida em JSON, conforme mostrado no exemplo seguinte: 

```json

{
   "documents": [
      {
         "id": "1",
         "detectedLanguages": [
            {
               "name": "English",
               "iso6391Name": "en",
               "score": 1.0
            }
         ]
      },
      {
         "id": "2",
         "detectedLanguages": [
            {
               "name": "Spanish",
               "iso6391Name": "es",
               "score": 1.0
            }
         ]
      },
      {
         "id": "3",
         "detectedLanguages": [
            {
               "name": "Chinese_Simplified",
               "iso6391Name": "zh_chs",
               "score": 1.0
            }
         ]
      }
   ],
   "errors": [

   ]
}


```
<a name="SentimentAnalysis"></a>

## <a name="analyze-sentiment"></a>Analisar sentimento

O detexts de API de análise de dados de sentimento o sentimento de um conjunto de registos de texto, utilizando o [método sentimento](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9). O exemplo seguinte pontuações dois documentos, inglês num e outra na espanhol.

1. Crie um novo projeto de Node.JS no seu IDE favorito.
2. Adicione o código fornecido abaixo.
3. Substitua o `accessKey` valor com uma chave de acesso válida para a sua subscrição.
4. Substituir a localização na `uri` (atualmente `westus`) para a região que inscreveu no.
5. Execute o programa.

```javascript
'use strict';

let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the accessKey string value with your valid access key.
let accessKey = 'enter key here';

// Replace or verify the region.

// You must use the same region in your REST API call as you used to obtain your access keys.
// For example, if you obtained your access keys from the westus region, replace 
// "westcentralus" in the URI below with "westus".

// NOTE: Free trial access keys are generated in the westcentralus region, so if you are using
// a free trial access key, you should not need to change this region.
let uri = 'westus.api.cognitive.microsoft.com';
let path = '/text/analytics/v2.0/sentiment';

let response_handler = function (response) {
    let body = '';
    response.on ('data', function (d) {
        body += d;
    });
    response.on ('end', function () {
        let body_ = JSON.parse (body);
        let body__ = JSON.stringify (body_, null, '  ');
        console.log (body__);
    });
    response.on ('error', function (e) {
        console.log ('Error: ' + e.message);
    });
};

let get_sentiments = function (documents) {
    let body = JSON.stringify (documents);

    let request_params = {
        method : 'POST',
        hostname : uri,
        path : path,
        headers : {
            'Ocp-Apim-Subscription-Key' : accessKey,
        }
    };

    let req = https.request (request_params, response_handler);
    req.write (body);
    req.end ();
}

let documents = { 'documents': [
    { 'id': '1', 'language': 'en', 'text': 'I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable.' },
    { 'id': '2', 'language': 'es', 'text': 'Este ha sido un dia terrible, llegué tarde al trabajo debido a un accidente automobilistico.' },
]};

get_sentiments (documents);
```

**Resposta de análise de dados de sentimento**

Uma resposta com êxito é devolvida em JSON, conforme mostrado no exemplo seguinte: 

```json
{
   "documents": [
      {
         "score": 0.99984133243560791,
         "id": "1"
      },
      {
         "score": 0.024017512798309326,
         "id": "2"
      },
   ],
   "errors": [   ]
}
```

<a name="KeyPhraseExtraction"></a>

## <a name="extract-key-phrases"></a>Extrair expressões-chave

A API de extração de expressão de chave extrai expressões de chave a partir de um texto documento, utilizando o [método de chave expressões](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6). O exemplo seguinte extrai expressões chaves para documentos em inglês e espanhol.

1. Crie um novo projeto de Node.JS no seu IDE favorito.
2. Adicione o código fornecido abaixo.
3. Substitua o `accessKey` valor com uma chave de acesso válida para a sua subscrição.
4. Substituir a localização na `uri` (atualmente `westus`) para a região que inscreveu no.
5. Execute o programa.

```javascript
'use strict';

let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the accessKey string value with your valid access key.
let accessKey = 'enter key here';

// Replace or verify the region.

// You must use the same region in your REST API call as you used to obtain your access keys.
// For example, if you obtained your access keys from the westus region, replace 
// "westcentralus" in the URI below with "westus".

// NOTE: Free trial access keys are generated in the westcentralus region, so if you are using
// a free trial access key, you should not need to change this region.
let uri = 'westus.api.cognitive.microsoft.com';
let path = '/text/analytics/v2.0/keyPhrases';

let response_handler = function (response) {
    let body = '';
    response.on ('data', function (d) {
        body += d;
    });
    response.on ('end', function () {
        let body_ = JSON.parse (body);
        let body__ = JSON.stringify (body_, null, '  ');
        console.log (body__);
    });
    response.on ('error', function (e) {
        console.log ('Error: ' + e.message);
    });
};

let get_key_phrases = function (documents) {
    let body = JSON.stringify (documents);

    let request_params = {
        method : 'POST',
        hostname : uri,
        path : path,
        headers : {
            'Ocp-Apim-Subscription-Key' : accessKey,
        }
    };

    let req = https.request (request_params, response_handler);
    req.write (body);
    req.end ();
}

let documents = { 'documents': [
    { 'id': '1', 'language': 'en', 'text': 'I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable.' },
    { 'id': '2', 'language': 'es', 'text': 'Si usted quiere comunicarse con Carlos, usted debe de llamarlo a su telefono movil. Carlos es muy responsable, pero necesita recibir una notificacion si hay algun problema.' },
    { 'id': '3', 'language': 'en', 'text': 'The Grand Hotel is a new hotel in the center of Seattle. It earned 5 stars in my review, and has the classiest decor I\'ve ever seen.' }
]};

get_key_phrases (documents);
```

**Resposta de extração de expressão de chave**

Uma resposta com êxito é devolvida em JSON, conforme mostrado no exemplo seguinte: 

```json
{
   "documents": [
      {
         "keyPhrases": [
            "HDR resolution",
            "new XBox",
            "clean look"
         ],
         "id": "1"
      },
      {
         "keyPhrases": [
            "Carlos",
            "notificacion",
            "algun problema",
            "telefono movil"
         ],
         "id": "2"
      },
      {
         "keyPhrases": [
            "new hotel",
            "Grand Hotel",
            "review",
            "center of Seattle",
            "classiest decor",
            "stars"
         ],
         "id": "3"
      }
   ],
   "errors": [  ]
}
```

<a name="Entities"></a>

## <a name="identify-linked-entities"></a>Identificar entidades ligadas

A API de associação de entidade identifica as entidades conhecidas de um texto documento, utilizando o [entidade de ligação método](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634). O exemplo seguinte identifica as entidades para documentos em inglês.

1. Crie um novo projeto de Node.JS no seu IDE favorito.
2. Adicione o código fornecido abaixo.
3. Substitua o `accessKey` valor com uma chave de acesso válida para a sua subscrição.
4. Substituir a localização na `uri` (atualmente `westus`) para a região que inscreveu no.
5. Execute o programa.

```javascript
'use strict';

let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the accessKey string value with your valid access key.
let accessKey = 'enter key here';

// Replace or verify the region.

// You must use the same region in your REST API call as you used to obtain your access keys.
// For example, if you obtained your access keys from the westus region, replace 
// "westcentralus" in the URI below with "westus".

// NOTE: Free trial access keys are generated in the westcentralus region, so if you are using
// a free trial access key, you should not need to change this region.
let uri = 'westus.api.cognitive.microsoft.com';
let path = '/text/analytics/v2.0/entities';

let response_handler = function (response) {
    let body = '';
    response.on ('data', function (d) {
        body += d;
    });
    response.on ('end', function () {
        let body_ = JSON.parse (body);
        let body__ = JSON.stringify (body_, null, '  ');
        console.log (body__);
    });
    response.on ('error', function (e) {
        console.log ('Error: ' + e.message);
    });
};

let get_entities = function (documents) {
    let body = JSON.stringify (documents);

    let request_params = {
        method : 'POST',
        hostname : uri,
        path : path,
        headers : {
            'Ocp-Apim-Subscription-Key' : accessKey,
        }
    };

    let req = https.request (request_params, response_handler);
    req.write (body);
    req.end ();
}

let documents = { 'documents': [
    { 'id': '1', 'language': 'en', 'text': 'I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable.' },
    { 'id': '2', 'language': 'en', 'text': 'The Seattle Seahawks won the Super Bowl in 2014.' }
]};

get_entities (documents);
```

**Resposta de ligação de entidade**

Uma resposta com êxito é devolvida em JSON, conforme mostrado no exemplo seguinte: 

```json
{
    "documents": [
        {
            "id": "1",
            "entities": [
                {
                    "name": "Xbox One",
                    "matches": [
                        {
                            "text": "XBox One",
                            "offset": 23,
                            "length": 8
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "Xbox One",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/Xbox_One",
                    "bingId": "446bb4df-4999-4243-84c0-74e0f6c60e75"
                },
                {
                    "name": "Ultra-high-definition television",
                    "matches": [
                        {
                            "text": "4K",
                            "offset": 63,
                            "length": 2
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "Ultra-high-definition television",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/Ultra-high-definition_television",
                    "bingId": "7ee02026-b6ec-878b-f4de-f0bc7b0ab8c4"
                }
            ]
        },
        {
            "id": "2",
            "entities": [
                {
                    "name": "2013 Seattle Seahawks season",
                    "matches": [
                        {
                            "text": "Seattle Seahawks",
                            "offset": 4,
                            "length": 16
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "2013 Seattle Seahawks season",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/2013_Seattle_Seahawks_season",
                    "bingId": "eb637865-4722-4eca-be9e-0ac0c376d361"
                }
            ]
        }
    ],
    "errors": []
}
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Análise de texto com o Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Consulte também 

 [Descrição geral da análise de texto](../overview.md)  
 [Perguntas mais frequentes (FAQ)](../text-analytics-resource-faq.md)
