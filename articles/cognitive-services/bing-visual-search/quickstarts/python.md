---
title: Início rápido de Python para a API de pesquisa Visual do Bing | Documentos da Microsoft
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Mostra como carregar uma imagem para a API de pesquisa Visual do Bing e obter informações sobre a imagem.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 96bd94e37c75d10726245fbcea7044d4ae2ed07e
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39070380"
---
# <a name="your-first-bing-visual-search-query-in-python"></a>Sua primeira consulta de pesquisa Visual do Bing em Python

API de pesquisa Visual do Bing devolve informações sobre uma imagem que fornece. Pode fornecer a imagem utilizando o URL da imagem, um insights token, ou ao carregar uma imagem. Para obter informações sobre estas opções, consulte [o que é a API de pesquisa Visual do Bing?](../overview.md) Este artigo demonstra a carregar uma imagem. Carregar uma imagem pode ser útil em cenários móveis onde tirar uma fotografia de um ponto de referência bem conhecido e obter informações sobre ele. Por exemplo, as informações podem incluir trivia sobre o ponto de referência. 

Se carregar uma imagem do local, o código a seguir mostra os dados do formulário que tem de incluir no corpo da POSTAGEM. Os dados do formulário tem de incluir o cabeçalho Content-Disposition. Seus `name` parâmetro tem de ser definido como "imagem" e o `filename` parâmetro pode ser definido como qualquer cadeia de caracteres. O conteúdo do formulário é o binário da imagem. O tamanho da imagem máximo que pode carregar é de 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Este artigo inclui uma aplicação de consola simples que envia um pedido de API de pesquisa Visual do Bing e exibe os resultados da pesquisa JSON. Embora esse aplicativo é escrito em Python, a API é um serviço RESTful Web compatível com qualquer linguagem de programação que pode fazer solicitações HTTP e analisar JSON. 

## <a name="prerequisites"></a>Pré-requisitos

Precisa [Python 3](https://www.python.org/) executar esse código.

Neste início rápido, pode utilizar um [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) chave de subscrição ou uma chave de subscrição paga.

## <a name="running-the-walkthrough"></a>Executar o passo a passo

Para executar esta aplicação, siga estes passos:

1. Crie um novo projeto de Python no seu IDE ou editor favorito.
2. Crie um ficheiro denominado visualsearch.py e adicione o código mostrado neste início rápido.
3. Substitua o `SUBSCRIPTION_KEY` valor com a sua chave de subscrição.
3. Substitua o `imagePath` valor com o caminho da imagem para carregar.
4. Execute o programa.



O código a seguir mostra como enviar a mensagem de utilizar os dados de formulário com várias partes no Python.

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

[Obtenha informações sobre uma imagem com um token de insights](../use-insights-token.md)  
[Tutorial de carregamento de imagem de pesquisa Visual do Bing](../tutorial-visual-search-image-upload.md)
[tutorial de aplicação de página única de pesquisa Visual do Bing](../tutorial-bing-visual-search-single-page-app.md)  
[Descrição geral de pesquisa Visual do Bing](../overview.md)  
[Experimente-o](https://aka.ms/bingvisualsearchtryforfree)  
[Obter uma chave de acesso de avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Referência da API de pesquisa Visual do Bing](https://aka.ms/bingvisualsearchreferencedoc)
