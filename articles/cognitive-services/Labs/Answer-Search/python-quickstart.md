---
title: 'Início rápido: Pesquisa de resposta de projeto, Python'
titlesuffix: Azure Cognitive Services
description: Introdução ao exemplo do Python com a Pesquisa de Respostas do Projeto.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: answer-search
ms.topic: quickstart
ms.date: 04/13/2018
ms.author: rosh
ms.openlocfilehash: 2c42935e100a55f767c3b1cbac6590850734b57e
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/28/2019
ms.locfileid: "55093330"
---
# <a name="quickstart-project-answer-search-with-python"></a>Início Rápido: Pesquisa de Respostas do Projeto com o Python

O exemplo do Python que se segue cria e envia um pedido para obter informações sobre o “Rochedo de Gibraltar”.

## <a name="prerequisites"></a>Pré-requisitos

Obter uma chave de acesso da avaliação gratuita dos [Laboratórios dos Serviços Cognitivos](https://aka.ms/answersearchsubscription)

Este exemplo utiliza o Python 3.6.4

## <a name="code-scenario"></a>Cenário de código 

O código seguinte cria uma Pré-visualização do URL.
É implementado nos passos seguintes:
1. Declare variáveis para especificar o ponto final por anfitrião e por caminho.
2. Especifique o URL de consulta da pré-visualização e adicione o parâmetro de consulta.  
3. Defina o parâmetro de consulta.
4. Defina a função Pesquisa que cria o pedido e adiciona o cabeçalho *Ocp-Apim-Subscription-Key*.
5. Defina o cabeçalho *Ocp-Apim-Subscription-Key*. 
6. Faça a ligação e envie o pedido.
7. Imprima os resultados JSON.

Segue-se o código completo para esta demonstração:

```
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

```
## <a name="next-steps"></a>Passos Seguintes
- [Início rápido do C#](c-sharp-quickstart.md)
- [Início Rápido do Java](java-quickstart.md)
- [Início rápido do Node](node-quickstart.md)