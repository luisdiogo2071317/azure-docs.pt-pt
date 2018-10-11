---
title: 'Guia de Início Rápido: criar uma consulta de pesquisa visual, Python – Pesquisa Visual do Bing'
titleSuffix: Azure Cognitive Services
description: Mostra como carregar uma imagem para a API de Pesquisa Visual do Bing e obter informações sobre a imagem.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 16d3d0ddf77e37e32cc50961a3870b820ac2748e
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48884246"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-python"></a>Guia de Início Rápido: a sua primeira consulta de Pesquisa Visual do Bing em Python

A API de Pesquisa Visual do Bing devolve informações sobre uma imagem que indicar. Pode fornecer a imagem através do URL da mesma, de um token de informações ou do carregamento da mesma. Para obter informações sobre estas opções, veja [What is Bing Visual Search API?](../overview.md) (O que é a API de Pesquisa Visual do Bing?). Este artigo demonstra como carregar uma imagem. Carregar uma imagem pode ser útil em cenários móveis onde tira uma fotografia de um ponto de referência bem conhecido e obtém informações sobre o mesmo. Por exemplo, as informações podem incluir curiosidades sobre esse ponto de referência. 

Se carregar uma imagem local, o seguinte mostra os dados de formato que tem de incluir no corpo de POST. Os dados de formulário têm de incluir o cabeçalho Content-Disposition. O respetivo parâmetro `name` tem de ser definido como “image” e o parâmetro `filename` pode ser definido como qualquer cadeia. Os conteúdos do formulário são o binário da imagem. O tamanho máximo de imagem que pode carregar é 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Este artigo inclui uma aplicação de consola simples que envia um pedido da API de Pesquisa Visual do Bing e apresenta os resultados da pesquisa JSON. Embora esta aplicação seja escrita em Python, a API é um serviço Web RESTful compatível com qualquer linguagem de programação que consiga fazer pedidos HTTP e analisar JSON. 

## <a name="prerequisites"></a>Pré-requisitos

Irá precisar do [Python 3](https://www.python.org/) para executar este código.

Neste guia de início rápido, pode utilizar uma chave de subscrição de uma [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ou uma chave de subscrição paga.

## <a name="running-the-walkthrough"></a>Executar as instruções

Para executar esta aplicação, siga os passos abaixo:

1. Crie um novo projeto de Python no seu editor ou IDE favorito.
2. Crie um ficheiro com o nome visualsearch.py e adicione o código apresentado neste guia de início rápido.
3. Substitua o valor `SUBSCRIPTION_KEY` pela sua chave de subscrição.
3. Substitua o valor `imagePath` pelo caminho da imagem a carregar.
4. Execute o programa.



A seguir, é-lhe mostrado como enviar a mensagem com dados de formulário de várias partes em Python.

```python
"""Bing Visual Search upload image example"""

# Download and install Python at https://www.python.org/
# Run the following in a command console window
# pip3 install requests

import requests, json


BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'

SUBSCRIPTION_KEY = '<yoursubscriptionkeygoeshere>'

HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}

imagePath = '<pathtoyourimagetouploadgoeshere>'

file = {'image' : ('myfile', open(imagePath, 'rb'))}

def main():
    
    try:
        response = requests.post(BASE_URI, headers=HEADERS, files=file)
        response.raise_for_status()
        print_json(response.json())

    except Exception as ex:
        raise ex


def print_json(obj):
    """Print the object as json"""
    print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))



# Main execution
if __name__ == '__main__':
    main()
```


## <a name="next-steps"></a>Passos seguintes

[Get insights about an image using an insights token](../use-insights-token.md) (Utilizar um token de informações para obter informações de uma imagem)  
[Tutorial de carregamento de imagens da Pesquisa Visual do Bing](../tutorial-visual-search-image-upload.md)
[Tutorial de aplicação de página única da Pesquisa Visual do Bing](../tutorial-bing-visual-search-single-page-app.md)  
[Descrição geral da Pesquisa Visual do Bing](../overview.md)  
[Experimente](https://aka.ms/bingvisualsearchtryforfree)  
[Obter uma chave de acesso de avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Visual Search API reference](https://aka.ms/bingvisualsearchreferencedoc) (Referência da API de Pesquisa Visual do Bing)
