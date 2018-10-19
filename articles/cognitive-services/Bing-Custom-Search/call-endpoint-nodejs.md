---
title: 'Início Rápido: Chamar o ponto final com o Node.js – Pesquisa Personalizada do Bing'
titlesuffix: Azure Cognitive Services
description: Este início rápido mostra como pedir os resultados de pesquisa da sua instância de pesquisa personalizada ao utilizar o Node.js para chamar o ponto final da Pesquisa Personalizada do Bing.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: af77b4c06b61cda4fd18d19ac3578129004c4914
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167210"
---
# <a name="quickstart-call-bing-custom-search-endpoint-nodejs"></a>Início Rápido: Chamar o ponto final da Pesquisa Personalizada do Bing (Node.js)

Este início rápido mostra como pedir os resultados de pesquisa da sua instância de pesquisa personalizada com o Node.js para chamar o ponto final da Pesquisa Personalizada do Bing. 

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de início rápido, necessita de:

- Uma instância de pesquisa personalizada e pronta a utilizar. Veja [Criar a primeira instância de Pesquisa Personalizada do Bing](quick-start.md).
- Ter o [Node.js](https://www.nodejs.org/) instalado.
- Uma chave de subscrição. Pode obter uma chave de subscrição ao ativar a sua [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) ou pode utilizar uma chave de subscrição paga no dashboard do Azure (veja [Conta da API dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).    

## <a name="run-the-code"></a>Executar o código

Para executar este exemplo, siga estes passos:

1. Crie uma pasta para o código.  
  
2. Numa linha de comandos ou terminal, navegue para a pasta que acabou de criar.  
  
3. Instale o módulo Node de **pedidos**:
    <pre>
    npm install request
    </pre>  
    
4. Crie um ficheiro denominado BingCustomSearch.js na pasta que criou e copie o seguinte código para o mesmo. Substitua **YOUR-SUBSCRIPTION-KEY** e **YOUR-CUSTOM-CONFIG-ID** pela sua chave de subscrição e ID de configuração.  
  
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

## <a name="next-steps"></a>Passos seguintes
- [Configurar a sua experiência de IU alojada](./hosted-ui.md)
- [Utilizar marcadores decorativos para realçar o texto](./hit-highlighting.md)
- [Paginação de páginas Web](./page-webpages.md)
