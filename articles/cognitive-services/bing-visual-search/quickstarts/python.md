---
title: Guia de introdução do Python para pesquisa de Visual Bing API | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Mostra como carregar uma imagem para a API de pesquisa do Bing Visual e obter informações sobre a imagem a novamente.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: a520466825eb429e45e0500b52bd7af502c0a38c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355075"
---
# <a name="your-first-bing-visual-search-query-in-python"></a>A primeira consulta de pesquisa de Visual Bing no Python

API de pesquisa do Bing Visual devolve informações sobre uma imagem que fornecer. Pode fornecer a imagem utilizando o URL da imagem, uma insights token, ou através do carregamento de uma imagem. Para obter informações sobre estas opções, consulte [o que é a API de pesquisa do Bing Visual?](../overview.md) Este artigo demonstra o carregamento de uma imagem. Carregamento de uma imagem pode ser útil em cenários móveis onde tirar uma fotografia de um landmark bem conhecido e voltar a informação sobre a mesma. Por exemplo, as informações podem incluir trivia sobre o landmark. 

Se carregar uma imagem local, o seguinte mostra os dados do formulário que tem de incluir no corpo do POST. Os dados do formulário têm de incluir o cabeçalho de disposição de conteúdo. O `name` parâmetro tem de ser definido como "de imagem" e o `filename` parâmetro pode ser definido como qualquer cadeia. O conteúdo do formulário é o binário da imagem. O tamanho da imagem máximo, pode carregar é de 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Este artigo inclui uma aplicação de consola simples que envia um pedido de API de pesquisa do Bing Visual e apresenta os resultados de pesquisa JSON. Enquanto esta aplicação é escrita no Python, a API é um serviço RESTful Web compatível com qualquer linguagem de programação que pode efetuar pedidos HTTP e analisar JSON. 

## <a name="prerequisites"></a>Pré-requisitos

Terá de [Python 3](https://www.python.org/) para executar este código.

Para este início rápido, pode utilizar um [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) chave de subscrição ou uma chave de subscrição paga.

## <a name="running-the-walkthrough"></a>Executar as instruções

Para executar esta aplicação, siga estes passos:

1. Crie um novo projeto de Python no seu IDE ou editor favorito.
2. Crie um ficheiro denominado visualsearch.py e adicione o código mostrado neste guia de introdução.
3. Substitua o `SUBSCRIPTION_KEY` valor com a sua chave de subscrição.
3. Substitua o `imagePath` valor com o caminho da imagem para carregar.
4. Execute o programa.



O seguinte mostra como enviar a mensagem com a dados do formulário com várias partes no Python.

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


## <a name="next-steps"></a>Passos Seguintes

[Obter conhecimentos aprofundados sobre uma imagem utilizando um token de insights](../use-insights-token.md)  
[Tutorial de aplicação de página única pesquisa Visual do Bing](../tutorial-bing-visual-search-single-page-app.md)  
[Descrição geral de pesquisa de Visual do Bing](../overview.md)  
[Experimente](https://aka.ms/bingvisualsearchtryforfree)  
[Obter uma chave de acesso de avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Referência da API de pesquisa do Bing Visual](https://aka.ms/bingvisualsearchreferencedoc)
