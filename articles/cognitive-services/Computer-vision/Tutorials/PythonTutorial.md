---
title: 'Tutorial: API de Imagem Digitalizada com o Python'
titlesuffix: Azure Cognitive Services
description: Saiba como utilizar a API de Imagem Digitalizada com Python através dos blocos de notas do Jupyter. Utilize bibliotecas populares para visualizar os seus resultados.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: tutorial
ms.date: 11/6/2018
ms.author: kefre
ms.openlocfilehash: 16054d19c1ce6f211ebd3e2f0bbc4d152a255dda
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51276972"
---
# <a name="tutorial-computer-vision-api-python"></a>Tutorial: API de Imagem Digitalizada com o Python

Este tutorial mostra-lhe como utilizar a API de Imagem Digitalizada no Python e como utilizar bibliotecas populares para visualizar os resultados. Vai utilizar o Jupyter para executar o tutorial. Para saber como começar a utilizar os blocos de notas interativos do Jupyter, veja: [Documentação do Jupyter](http://jupyter.readthedocs.io/en/latest/index.html).

## <a name="prerequisites"></a>Pré-requisitos

- [Python 2.7+ ou 3.5+](https://www.python.org/downloads/)
- ferramenta [pip](https://pip.pypa.io/en/stable/installing/)
- [Jupyter Notebook](http://jupyter.org/install) instalado

## <a name="open-the-tutorial-notebook-in-jupyter"></a>Abrir o Bloco de Notas de Tutorial no Jupyter 

1. Vá para o repositório do GitHub[Python da Visão Cognitiva](https://github.com/Microsoft/Cognitive-Vision-Python). 
2. Clique no botão verde para clonar ou transferir o repositório. 
3. Abra uma linha de comandos e navegue para a pasta **Cognitive-Vision-Python\Jupyter Notebook**.
1. Certifique-se de que tem todas as bibliotecas exigidas ao executar o comando `pip install requests opencv-python numpy matplotlib` na linha de comandos.
1. Inicie o Jupyter ao executar o comando `jupyter notebook` na linha de comandos.
1. Na janela do Jupyter, clique em _API de Imagem Digitalizada Example.ipynb_ para abrir o bloco de notas de tutorial.

## <a name="run-the-tutorial"></a>Executar o Tutorial

Para utilizar este bloco de notas, precisa de uma chave de subscrição para a API de Imagem Digitalizada. Visite a [página Subscrição](https://azure.microsoft.com/try/cognitive-services/) para se inscrever. Na página **Iniciar sessão**, utilize a sua conta Microsoft para iniciar sessão e poderá subscrever e obter chaves gratuitas. Depois de concluir o processo de inscrição, cole a chave na secção `Variables` do bloco de notas (reproduzida abaixo). Tanto a chave primária como a secundária irão funcionar. Certifique-se de que coloca a chave entre aspas para que seja considerada uma cadeia de carateres.

Também precisa de se certificar que o campo `_region` corresponde à região correspondente à sua subscrição.

```python
# Variables
_region = 'westcentralus' #Here you enter the region of your subscription
_url = 'https://{}.api.cognitive.microsoft.com/vision/v2.0/analyze'.format(_region)
_key = None #Here you have to paste your primary key
_maxNumRetries = 10
```

Quando executa o tutorial, pode adicionar imagens para analisar, tanto a partir de um URL, como do armazenamento local. O script irá apresentar as informações de análise e imagens no bloco de notas.