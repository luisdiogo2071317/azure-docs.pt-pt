---
title: Chamar o ponto final com o node. js - pesquisa personalizada do Bing - serviços cognitivos da Microsoft
description: Este início rápido mostra como solicitar os resultados da pesquisa da sua instância de pesquisa personalizada utilizando node. js para chamar o ponto final de pesquisa personalizada do Bing.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 5d9391cc486dc868a1a291ccc7095291cddd3e4c
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37858464"
---
# <a name="call-bing-custom-search-endpoint-nodejs"></a>Chamar o ponto final de pesquisa personalizada do Bing (node. js)

Este início rápido mostra como solicitar os resultados da pesquisa da sua instância de pesquisa personalizada utilizando node. js para chamar o ponto final de pesquisa personalizada do Bing. 

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este guia de início rápido, necessita de:

- Uma instância de pesquisa personalizada. Ver [criar a primeira instância de pesquisa personalizada do Bing](quick-start.md).

- [NODE. js](https://www.nodejs.org/) instalado.

-  [Conta de API dos serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **APIs de pesquisa Bing**. O [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) é suficiente para este início rápido. Precisa da chave de acesso fornecida quando ativar a avaliação gratuita, ou pode usar uma chave de subscrição paga do dashboard do Azure.

## <a name="run-the-code"></a>Executar o código

Para chamar o ponto de extremidade de pesquisa personalizada do Bing, siga estes passos:

1. Crie uma pasta para o seu código.

2. A partir de uma linha de comandos ou terminal, navegue para a pasta que acabou de criar.

3. Instalar o **pedido** módulo de nó:
    <pre>
    npm install request
    </pre>
    
4. Criar o ficheiro BingCustomSearch.js e copie o seguinte código ao mesmo.

5. Substitua **seu-SUBSCRIPTION-KEY** e **seu-personalizada-CONFIG-ID** com o seu ID de chave e a configuração (consulte o passo 1).

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
6. Execute o código com o seguinte comando.
    ```    
    node BingCustomSearch.js
   ``` 

## <a name="next-steps"></a>Passos Seguintes
- [Configurar a sua experiência de interface do Usuário alojada](./hosted-ui.md)
- [Utilizar marcadores decoration para realçar o texto](./hit-highlighting.md)
- [Páginas Web de página](./page-webpages.md)
