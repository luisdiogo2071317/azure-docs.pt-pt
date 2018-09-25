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
ms.openlocfilehash: 73c31c7175bd4dfcb182fb76784937c176ac7702
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46977881"
---
# <a name="call-bing-custom-search-endpoint-nodejs"></a>Chamar o ponto final de pesquisa personalizada do Bing (node. js)

Este guia de introdução mostra como solicitar os resultados da pesquisa da sua instância de pesquisa personalizada com o node. js para chamar o ponto final de pesquisa personalizada do Bing. 

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de início rápido, necessita de:

- Uma instância de pesquisa personalizada do prontos a utilizar. Ver [criar a primeira instância de pesquisa personalizada do Bing](quick-start.md).
- [NODE. js](https://www.nodejs.org/) instalado.
- Uma chave de subscrição. Pode obter uma chave de subscrição ao ativar o seu [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search), ou pode utilizar uma chave de subscrição paga do dashboard do Azure (consulte [conta de API dos serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).    

## <a name="run-the-code"></a>Executar o código

Para executar este exemplo, siga estes passos:

1. Crie uma pasta para o seu código.  
  
2. A partir de uma linha de comandos ou terminal, navegue para a pasta que acabou de criar.  
  
3. Instalar o **pedido** módulo de nó:
    <pre>
    npm install request
    </pre>  
    
4. Crie um ficheiro denominado BingCustomSearch.js na pasta que criou e copie o seguinte código para o mesmo. Substitua **seu-SUBSCRIPTION-KEY** e **seu-personalizada-CONFIG-ID** com sua chave de subscrição e a configuração de ID.  
  
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
  
6. Execute o código com o seguinte comando:  
  
    ```    
    node BingCustomSearch.js
    ``` 

## <a name="next-steps"></a>Passos Seguintes
- [Configurar a sua experiência de interface do Usuário alojada](./hosted-ui.md)
- [Utilizar marcadores decoration para realçar o texto](./hit-highlighting.md)
- [Páginas Web de página](./page-webpages.md)
