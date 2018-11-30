---
title: 'Início Rápido: Chamar o ponto final com Python – Pesquisa Personalizada do Bing'
titlesuffix: Azure Cognitive Services
description: Este início rápido mostra como pedir os resultados de pesquisa da sua instância de pesquisa personalizada ao utilizar o Python para chamar o ponto final da Pesquisa Personalizada do Bing.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: aahi
ms.openlocfilehash: d7977f49a484665f133a119f491bd8986c0b6fc4
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/26/2018
ms.locfileid: "52310724"
---
# <a name="quickstart-call-bing-custom-search-endpoint-python"></a>Início Rápido: Chamar o ponto final da Pesquisa Personalizada do Bing (Python)

Este início rápido mostra como pedir os resultados de pesquisa da sua instância de pesquisa personalizada com o Python para chamar o ponto final da Pesquisa Personalizada do Bing. 

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de início rápido, necessita de:

- Uma instância de pesquisa personalizada e pronta a utilizar. Veja [Criar a primeira instância de Pesquisa Personalizada do Bing](quick-start.md).
- O [Python](https://www.python.org/) instalado.
- Uma chave de subscrição. Pode obter uma chave de subscrição ao ativar a sua [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) ou pode utilizar uma chave de subscrição paga no dashboard do Azure (veja [Conta da API dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).   Consulte também [dos serviços cognitivos preços - API de pesquisa Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).


## <a name="run-the-code"></a>Executar o código

Para executar este exemplo, siga estes passos:

1. Crie uma pasta para o código.  
  
2. Numa linha de comandos ou terminal de administrador, navegue para a pasta que acabou de criar.  
  
3. Instale o módulo Python de **pedidos**:  
  
    <pre>
    pip install pipenv
    pipenv install requests
    </pre>  
      
4. Crie um ficheiro denominado BingCustomSearch.py na pasta que criou e copie o seguinte código para o mesmo. Substitua **YOUR-SUBSCRIPTION-KEY** e **YOUR-CUSTOM-CONFIG-ID** pela sua chave de subscrição e ID de configuração.  
  
    ``` Python
    import json
    import requests
    
    subscriptionKey = "YOUR-SUBSCRIPTION-KEY"
    customConfigId = "YOUR-CUSTOM-CONFIG-ID"
    searchTerm = "microsoft"
    
    url = 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=' + searchTerm + '&customconfig=' + customConfigId
    r = requests.get(url, headers={'Ocp-Apim-Subscription-Key': subscriptionKey})
    print(r.text)
    ```  
  
7. Execute o código com o seguinte comando.  
  
    ```
    python BingCustomSearch.py
    ```

## <a name="next-steps"></a>Passos Seguintes
- [Configurar a sua experiência de IU alojada](./hosted-ui.md)
- [Utilizar marcadores decorativos para realçar o texto](./hit-highlighting.md)
- [Paginação de páginas Web](./page-webpages.md)
