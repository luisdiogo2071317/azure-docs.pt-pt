---
title: Guia de introdução do Python para o URL do projeto de pré-visualização - serviços cognitivos Microsoft | Microsoft Docs
description: Script de exemplo para rapidamente começar a utilizar a pré-visualização de URL do projeto no Microsoft serviços cognitivos no Azure.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/29/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 78b2d83b02aa9ea32509029c7456e04e420b8572
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354014"
---
# <a name="url-preview-python-quickstart"></a>Guia de introdução do Python de pré-visualização do URL

O exemplo de Python seguinte cria uma pré-visualização de Url para o site SwiftKey Web: https://swiftkey.com/en.

## <a name="prerequisites"></a>Pré-requisitos

Obter uma chave de acesso para a versão de avaliação gratuita [laboratórios de serviços cognitivos](https://aka.ms/answersearchsubscription)

Este exemplo utiliza Python 3.6.

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
## <a name="next-steps"></a>Passos Seguintes
- [Início rápido c#](csharp.md)
- [Guia de introdução do Java](java-quickstart.md)
- [Início rápido de JavaScript](javascript.md)
- [Início rápido do URL de nó](node-quickstart.md)