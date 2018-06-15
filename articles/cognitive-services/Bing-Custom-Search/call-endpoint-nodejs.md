---
title: Ponto final da chamada utilizando Node.js - pesquisa de personalizados do Bing - Microsoft os serviços cognitivos
description: Este guia de introdução mostra como pedir os resultados da pesquisa da sua instância de pesquisa personalizada utilizando o Node.js para chamar o ponto final de pesquisa personalizados do Bing.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 48fc234e15ce3b9172d766f6fae11b51a017ce70
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355730"
---
# <a name="call-bing-custom-search-endpoint-nodejs"></a>Chamada de pesquisa do Bing personalizada ponto final (Node.js)

Este guia de introdução mostra como pedir os resultados da pesquisa da sua instância de pesquisa personalizada utilizando o Node.js para chamar o ponto final de pesquisa personalizados do Bing. 

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este guia de início rápido, necessita de:

- Uma instância de pesquisa personalizada. Consulte [criar a primeira instância de pesquisa do Bing personalizada](quick-start.md).

- [NODE.js](https://www.nodejs.org/) instalado.

-  [Conta da API de serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **APIs de pesquisa do Bing**. O [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) é suficiente para este início rápido. Tem a chave de acesso fornecida quando ativar a avaliação gratuita, ou pode utilizar uma chave de subscrição paga do dashboard do Azure.

## <a name="run-the-code"></a>Executar o código

Para chamar o ponto final de pesquisa do Bing personalizada, siga estes passos:

1. Crie uma pasta para o seu código.
2. Uma linha de comandos ou terminal, navegue para a pasta que acabou de criar.
3. Instalar o **pedido** módulo de nó:
    <pre>
    npm install request
    </pre>
4. Criar o ficheiro BingCustomSearch.js e copie o seguinte código ao mesmo.
5. Substitua **sua-SUBSCRIPTION-KEY** e **sua-personalizada-CONFIG-ID** com o seu ID de chave e a configuração (ver passo 1).

    ``` javascript
    var request = require("request");
    
    var subscriptionKey = 'YOUR-SUBSCRIPTION-KEY';
    var customConfigId = 'YOUR-CUSTOM-CONFIG-ID';
    var searchTerm = 'microsoft';
    
    var options = {
        url: 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 
          'q=' + searchTerm + 
          '&customconfig=' + customConfigId,
        headers: {
            'Ocp-Apim-Subscription-Key' : subscriptionKey
        }
    }
    
    request(options, function(error, response, body){
        var searchResponse = JSON.parse(body);
        for(var i = 0; i < searchResponse.webPages.value.length; ++i){
            var webPage = searchResponse.webPages.value[i];
            console.log('name: ' + webPage.name);
            console.log('url: ' + webPage.url);
            console.log('displayUrl: ' + webPage.displayUrl);
            console.log('snippet: ' + webPage.snippet);
            console.log('dateLastCrawled: ' + webPage.dateLastCrawled);
            console.log();
        }
    })
    ```
6. Execute o código utilizando o seguinte comando.
    ```    
    node BingCustomSearch.js
   ``` 

## <a name="next-steps"></a>Passos Seguintes
- [Configurar a sua experiência de IU alojada](./hosted-ui.md)
- [Utilizar marcadores decoration para realçar texto](./hit-highlighting.md)
- [Páginas de página Web](./page-webpages.md)