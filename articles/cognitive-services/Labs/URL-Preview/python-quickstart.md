---
title: 'Início Rápido: Pré-visualização do URL do Projeto, Python'
titlesuffix: Azure Cognitive Services
description: Exemplo de script para começar a utilizar rapidamente a Pré-visualização do URL do Projeto com o Python.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: project-url-preview
ms.topic: quickstart
ms.date: 03/29/2018
ms.author: rosh
ms.openlocfilehash: c4af701188c6453bd3c067b838519c32862af13d
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867277"
---
# <a name="quickstart-url-preview-with-python"></a>Início Rápido: Pré-visualização do URL com o Python

O exemplo de Python seguinte cria uma Pré-visualização do URL do site da SwiftKey: https://swiftkey.com/en.

## <a name="prerequisites"></a>Pré-requisitos

Obter uma chave de acesso da avaliação gratuita dos [Laboratórios dos Serviços Cognitivos](https://aka.ms/answersearchsubscription)

Este exemplo utiliza o Python 3.6.

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

````
import http.client, urllib.parse
import json

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'your-subscription-key'

host = 'api.labs.cognitive.microsoft.com'
path = '/urlpreview/v7.0/search'

query = 'https://SwiftKey.com'

params = '?q=' + urllib.parse.quote (query)

def get_preview ():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_preview ()
print (json.dumps(json.loads(result), indent=4))
````
## <a name="next-steps"></a>Passos seguintes
- [Início rápido do C#](csharp.md)
- [Início rápido do Java](java-quickstart.md)
- [Início rápido do JavaScript](javascript.md)
- [Início rápido do URL do Node](node-quickstart.md)