---
title: Ponto final da chamada utilizando Python - pesquisa de personalizados do Bing - Microsoft os serviços cognitivos
description: Este guia de introdução mostra como pedir os resultados da pesquisa da sua instância de pesquisa personalizada com o Python para chamar o ponto final de pesquisa personalizados do Bing.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 889762ae0b401438f25546738268c584ddd58389
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355735"
---
# <a name="call-bing-custom-search-endpoint-python"></a>Chamada de pesquisa do Bing personalizada ponto final (Python)

Este guia de introdução mostra como pedir os resultados da pesquisa da sua instância de pesquisa personalizada com o Python para chamar o ponto final de pesquisa personalizados do Bing. 

## <a name="prerequisites"></a>Pré-requisitos
Para concluir este guia de início rápido, necessita de:

- Uma instância de pesquisa personalizada. Consulte [criar a primeira instância de pesquisa do Bing personalizada](quick-start.md).

-  [Python](https://www.python.org/) instalado.

- A [conta da API de serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **APIs de pesquisa do Bing**. O [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) é suficiente para este início rápido. Tem a chave de acesso fornecida quando ativar a avaliação gratuita, ou pode utilizar uma chave de subscrição paga do dashboard do Azure. 

## <a name="run-the-code"></a>Executar o código

Para chamar o ponto final de pesquisa do Bing personalizada, siga estes passos:

1. Crie uma pasta para o seu código.
2. Uma linha de comandos de administrador ou o terminal, navegue para a pasta que acabou de criar.
3. Instalar o **pedidos** módulo python:
    <pre>
    pip install pipenv
    pipenv install requests
    </pre>
7. Criar o ficheiro BingCustomSearch.py e copie o seguinte código ao mesmo.
8. Substitua **sua-SUBSCRIPTION-KEY** e **sua-personalizada-CONFIG-ID** com o seu ID de chave e a configuração (ver passo 1).

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
9. Execute o código utilizando o seguinte comando.
    ```
    python BingCustomSearch.py
    ```

## <a name="next-steps"></a>Passos Seguintes
- [Configurar a sua experiência de IU alojada](./hosted-ui.md)
- [Utilizar marcadores decoration para realçar texto](./hit-highlighting.md)
- [Páginas de página Web](./page-webpages.md)