---
title: Chamar o ponto final ao utilizar o Python - pesquisa personalizada do Bing - serviços cognitivos da Microsoft
description: Este início rápido mostra como solicitar os resultados da pesquisa da sua instância de pesquisa personalizada com o Python para chamar o ponto final de pesquisa personalizada do Bing.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 889762ae0b401438f25546738268c584ddd58389
ms.sourcegitcommit: 15bfce02b334b67aedd634fa864efb4849fc5ee2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "35355735"
---
# <a name="call-bing-custom-search-endpoint-python"></a>Chamar o ponto final de pesquisa personalizada do Bing (Python)

Este início rápido mostra como solicitar os resultados da pesquisa da sua instância de pesquisa personalizada com o Python para chamar o ponto final de pesquisa personalizada do Bing. 

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este guia de início rápido, necessita de:

- Uma instância de pesquisa personalizada. Ver [criar a primeira instância de pesquisa personalizada do Bing](quick-start.md).

-  [Python](https://www.python.org/) instalado.

- R [conta de API dos serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **APIs de pesquisa Bing**. O [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) é suficiente para este início rápido. Precisa da chave de acesso fornecida quando ativar a avaliação gratuita, ou pode usar uma chave de subscrição paga do dashboard do Azure. 

## <a name="run-the-code"></a>Executar o código

Para chamar o ponto de extremidade de pesquisa personalizada do Bing, siga estes passos:

1. Crie uma pasta para o seu código.
2. A partir de uma linha de comandos de administrador ou um terminal, navegue para a pasta que acabou de criar.
3. Instalar o **pedidos** módulo python:
    <pre>
    pip install pipenv
    pipenv install requests
    </pre>
7. Criar o ficheiro BingCustomSearch.py e copie o seguinte código ao mesmo.
8. Substitua **seu-SUBSCRIPTION-KEY** e **seu-personalizada-CONFIG-ID** com o seu ID de chave e a configuração (consulte o passo 1).

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
9. Execute o código com o seguinte comando.
    ```
    python BingCustomSearch.py
    ```

## <a name="next-steps"></a>Passos Seguintes
- [Configurar a sua experiência de interface do Usuário alojada](./hosted-ui.md)
- [Utilizar marcadores decoration para realçar o texto](./hit-highlighting.md)
- [Páginas Web de página](./page-webpages.md)