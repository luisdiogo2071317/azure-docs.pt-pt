---
title: Guia de introdução do nó cognitivos nos serviços da Microsoft, pesquisa de resposta do projeto | Microsoft Docs
description: Começar a utilizar a pesquisa de resposta de projeto, Microsoft serviços cognitivos no Azure.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 36b2709d39230aae7929164ba4c9306f57043b43
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353972"
---
# <a name="project-answer-search-node-quickstart"></a>Início rápido do nó de pesquisa de resposta de projeto

O exemplo de nó seguinte cria uma consulta para obter informações sobre o Yosemite National Park.

## <a name="prerequisites"></a>Pré-requisitos

Obter uma chave de acesso para a versão de avaliação gratuita [laboratórios de serviços cognitivos](https://aka.ms/answersearchsubscription)

Este exemplo utiliza o nó v8.9.4

## <a name="code-scenario"></a>Cenário de código 

O código seguinte obtém as respostas.
Está implementado nos passos seguintes:
1. Declare as variáveis para especificar o ponto final por anfitrião e caminho.
2. Especifique o URL de consulta para pré-visualizar e adicione o parâmetro de consulta.  
3. Crie uma função de processador para a resposta.
4. Definir a função de pesquisa que cria o pedido e adiciona o *Ocp-Apim-Subscription-Key* cabeçalho.
5. Execute a função de pesquisa. 

O código de completado para esta demonstração segue:

````
'use strict';

let https = require('https');

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'; 

let host = 'api.labs.cognitive.microsoft.com';
let path = '/answerSearch/v7.0/search';

let mkt = 'en-us';
let q = 'Yosemite National Park';

let params = '?q=' + encodeURI(q) + '&mkt=en-us';

let response_handler = function (response) {
    let body = '';
    response.on('data', function (d) {
        body += d;
    });
    response.on('end', function () {
        let body_ = JSON.parse(body);
        let body__ = JSON.stringify(body_, null, '  ');
        console.log(body__);
    });
    response.on('error', function (e) {
        console.log('Error: ' + e.message);
    });
};

let Search = function () {
    let request_params = {
        method: 'GET',
        hostname: host,
        path: path + params,
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey,
        }
    };

    let req = https.request(request_params, response_handler);
    req.end();
}

Search();

````

## <a name="next-steps"></a>Passos Seguintes
- [Código de exemplo do c#](c-sharp-quickstart.md)
- [Guia de introdução do Java](java-quickstart.md)
- [Guia de introdução do Python](python-quickstart.md)