---
title: 'Guia de Início Rápido: Atualização da base de dados de conhecimento em Node.js – Criador de FAQ'
titleSuffix: Azure Cognitive Services
description: Como atualizar uma base de dados de conhecimento Node.js para o Criador de FAQ.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 09/12/2018
ms.author: diberry
ms.openlocfilehash: a987993da5202abc9b543aa2dba0f080a622e199
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033623"
---
# <a name="update-a-knowledge-base-in-nodejs"></a>Atualizar uma base de dados de conhecimento no Node.js

O código seguinte atualiza uma base de dados de conhecimento existente, através do método [Atualizar](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600).

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-nodejs-repo-note.md)]

Se ainda não tiver uma base de dados de conhecimento, pode criar uma de exemplo para utilizar neste guia de início rápido: [Criar uma nova base de dados de conhecimento](create-new-kb-nodejs.md).

1. Crie um novo projeto de Node.js no seu IDE preferido. Utilize a versão ECMAScript 6+ do JavaScript.
1. Adicione o código indicado abaixo.
1. Substitua o valor `subscriptionKey` por uma chave de subscrição válida.
1. Substitua o valor `kb` por um ID de uma base de dados de conhecimento válido. Localize este valor ao aceder a uma das suas [bases de dados de conhecimento do Criador de FAQ](https://www.qnamaker.ai/Home/MyServices). Selecione a base de dados de conhecimento que pretende atualizar. Assim que se encontrar na página, localize a secção "kdid=" no URL, como apresentado abaixo. Utilize esse valor para o seu exemplo de código.

    ![ID da base de dados de conhecimento do Criador de FAQ](../media/qnamaker-quickstart-kb/qna-maker-id.png)

1. Execute o programa.

```nodejs
'use strict';

let fs = require ('fs');
let https = require ('https');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace this with a valid subscription key.
let subscriptionKey = 'ADD KEY HERE';

// Replace this with a valid knowledge base ID.
let kb = 'ADD ID HERE';

// Represents the various elements used to create HTTP request URIs
// for QnA Maker operations.
let host = 'westus.api.cognitive.microsoft.com';
let service = '/qnamaker/v4.0';
let method = '/knowledgebases/';

// Formats and indents JSON for display.
let pretty_print = function(s) {
    return JSON.stringify(JSON.parse(s), null, 4);
}

// Call 'callback' after we have the entire response.
let response_handler = function (callback, response) {
    let body = '';
    response.on('data', function(d) {
        body += d;
    });
    response.on('end', function() {
    // Calls 'callback' with the status code, headers, and body of the response.
    callback ({ status : response.statusCode, headers : response.headers, body : body });
    });
    response.on('error', function(e) {
        console.log ('Error: ' + e.message);
    });
};

// HTTP response handler calls 'callback' after we have the entire response.
let get_response_handler = function(callback) {
    // Return a function that takes an HTTP response and is closed over the specified callback.
    // This function signature is required by https.request, hence the need for the closure.
    return function(response) {
        response_handler(callback, response);
    }
}

// Calls 'callback' after we have the entire PATCH request response.
let patch = function(path, content, callback) {
    let request_params = {
        method : 'PATCH',
        hostname : host,
        path : path,
        headers : {
            'Content-Type' : 'application/json',
            'Content-Length' : content.length,
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

    // Pass the callback function to the response handler.
    let req = https.request(request_params, get_response_handler(callback));
    req.write(content);
    req.end ();
}

// Calls 'callback' after we have the entire GET request response.
let get = function(path, callback) {
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path,
        headers : {
            'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };

    // Pass the callback function to the response handler.
    let req = https.request(request_params, get_response_handler(callback));
    req.end ();
}

// Calls 'callback' after we have the response from the /knowledgebases PATCH method.
let update_kb = function(path, req, callback) {
    console.log('Calling ' + host + path + '.');
    // Send the PATCH request.
    patch(path, req, function (response) {
        // Extract the data we want from the PATCH response and pass it to the callback function.
        callback({ operation : response.headers.location, response : response.body });
    });
}

// Calls 'callback' after we have the response from the GET request to check the status.
let check_status = function(path, callback) {
    console.log('Calling ' + host + path + '.');
    // Send the GET request.
    get(path, function (response) {
        // Extract the data we want from the GET response and pass it to the callback function.
        callback({ wait : response.headers['retry-after'], response : response.body });
    });
}

// Dictionary that holds the knowledge base. Modify knowledge base here.
let req = {
  'add': {
    'qnaList': [
      {
        'id': 1,
        'answer': 'You can change the default message if you use the QnAMakerDialog. See this for details: https://docs.botframework.com/en-us/azure-bot-service/templates/qnamaker/#navtitle',
        'source': 'Custom Editorial',
        'questions': [
          'How can I change the default message from QnA Maker?'
        ],
        'metadata': []
      }
    ],
    'urls': []
  },
  'update' : {
    'name' : 'New KB Name'
  },
  'delete': {
    'ids': [
      0
    ]
  }
};

var path = service + method + kb;
// Convert the request to a string.
let content = JSON.stringify(req);

// Sends the request to update the knowledge base.
update_kb(path, content, function (result) {
    console.log(pretty_print(result.response));
    // Loop until the operation is complete.
    let loop = function() {
        path = service + result.operation;
        // Check the status of the operation.
        check_status(path, function(status) {
            // Write out the status.
            console.log(pretty_print(status.response));
            // Convert the status into an object and get the value of the operationState field.
            var state = (JSON.parse(status.response)).operationState;
            // If the operation isn't complete, wait and query again.
            if (state == 'Running' || state == 'NotStarted') {
                console.log('Waiting ' + status.wait + ' seconds...');
                setTimeout(loop, status.wait * 1000);
            }
        });
    }
    // Begin the loop.
    loop();
});
```

## <a name="understand-what-qna-maker-returns"></a>Compreender os resultados do Criador de FAQ

É devolvida uma resposta com êxito em JSON, tal como é apresentado no exemplo seguinte. Os seus resultados poderão ser ligeiramente diferentes. Se o resultado final devolver o estado "Succeeded" (Com êxito), indica que a sua base de dados de conhecimento foi atualizada com êxito. Para a resolução de problemas, aceda aos códigos de resposta em [Update Knowledgebase](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600) (Atualizar a Base de Dados de Conhecimento) da API do Criador de FAQ.

```json
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-04-13T01:49:48Z",
  "lastActionTimestamp": "2018-04-13T01:49:48Z",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "5156f64e-e31d-4638-ad7c-a2bdd7f41658"
}
...
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-04-13T01:49:48Z",
  "lastActionTimestamp": "2018-04-13T01:49:50Z",
  "resourceLocation": "/knowledgebases/140a46f3-b248-4f1b-9349-614bfd6e5563",
  "userId": "2280ef5917bb4ebfa1aae41fb1cebb4a",
  "operationId": "5156f64e-e31d-4638-ad7c-a2bdd7f41658"
}
Press any key to continue.
```

Assim que a sua base de dados de conhecimento é atualizada, pode visualizá-la no seu Portal do Criador de FAQ, na página [My knowledge bases](https://www.qnamaker.ai/Home/MyServices) (As minhas bases de dados de conhecimento). Tenha em atenção que a sua base de dados de conhecimento agora tem um novo nome, "New KB Name", e um par de FAQs adicional.

Para modificar outros elementos da sua base de dados de conhecimento, aceda ao [esquema JSON](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600) no Criador de FAQ e modifique a cadeia `req`.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Referência à API REST do Criador de FAQ](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)