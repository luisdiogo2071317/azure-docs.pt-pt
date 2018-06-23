---
title: Guia de introdução do Python cognitivos nos serviços da Microsoft, pesquisa de resposta do projeto | Microsoft Docs
description: Exemplo de Python começar a utilizar a pesquisa de resposta de projeto, Microsoft serviços cognitivos no Azure.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 9cb5406c616ed8e96d73c00c788a0d20f66dcabd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353930"
---
# <a name="project-answer-search-python-quickstart"></a>Guia de introdução do projeto Python de pesquisa de resposta

O exemplo de Python seguinte cria e envia um pedido para obter informações sobre "Rock de Gibraltar".

## <a name="prerequisites"></a>Pré-requisitos

Obter uma chave de acesso para a versão de avaliação gratuita [laboratórios de serviços cognitivos](https://aka.ms/answersearchsubscription)

Este exemplo utiliza o Python 3.6.4

## <a name="code-scenario"></a>Cenário de código 

O código seguinte cria uma pré-visualização de URL.
Está implementado nos passos seguintes:
1. Declare as variáveis para especificar o ponto final por anfitrião e caminho.
2. Especifique o URL de consulta para pré-visualizar e adicione o parâmetro de consulta.  
3. Defina o parâmetro de consulta.
4. Definir a função de pesquisa que cria o pedido e adiciona o *Ocp-Apim-Subscription-Key* cabeçalho.
5. Definir o *Ocp-Apim-Subscription-Key* cabeçalho. 
6. Estabelecer a ligação e enviar o pedido.
7. Imprima os resultados JSON.

O código de completado para esta demonstração segue:

````
import http.client, urllib.parse
import json

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'

host = 'https://api.labs.cognitive.microsoft.com'
path = '/answerSearch/v7.0/search '

query = 'Rock of Gibraltar'

params = '?q=' + urllib.parse.quote (query) + '&mkt=en-us'

def get_local():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_local()
print (json.dumps(json.loads(result), indent=4))

````
## <a name="next-steps"></a>Passos Seguintes
- [Início rápido c#](c-sharp-quickstart.md)
- [Guia de introdução do Java](java-quickstart.md)
- [Guia de introdução do nó](node-quickstart.md)