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
ms.date: 02/25/2017
ms.author: kefre
ms.openlocfilehash: 59e88ecb253bc6da803ddf34a4a02229b99e259d
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45981716"
---
# <a name="tutorial-computer-vision-api-python"></a>Tutorial: API de Imagem Digitalizada com o Python

Este tutorial mostra-lhe como utilizar a API de Imagem Digitalizada no Python e como utilizar algumas bibliotecas populares para visualizar os resultados. Utilize o Jupyter para executar o tutorial. Para saber como começar a utilizar os blocos de notas interativos do Jupyter, veja: [Documentação do Jupyter](http://jupyter.readthedocs.io/en/latest/index.html). 

### <a name="opening-the-tutorial-notebook-in-jupyter"></a>Abrir o Bloco de Notas de Tutorial no Jupyter 

1. Navegue para o [bloco de notas de tutorial no GitHub](https://github.com/Microsoft/Cognitive-Vision-Python). 
2. Clique no botão verde para clonar ou transferir o tutorial. 
3. Abra uma linha de comandos e vá para a pasta _Cognitive-Vision-Python-master\Jupyter Notebook_. 
4. Execute o comando **jupyter notebook** a partir da linha de comandos. Esta ação inicia o Jupyter.
5. Na janela do Jupyter, clique em _Computer Vision API Example.ipynb_ para abrir o bloco de notas de tutorial 

### <a name="running-the-tutorial"></a>Executar o Tutorial

Para utilizar este bloco de notas, precisa de uma chave de subscrição para a API de Imagem Digitalizada. Visite a [página Subscrição](https://azure.microsoft.com/try/cognitive-services/) para se inscrever. Na página "Iniciar sessão", utilize a sua conta Microsoft para iniciar sessão e poderá subscrever e obter chaves gratuitas. Depois de concluir o processo de inscrição, cole a chave na secção de variáveis do bloco de notas (reproduzida abaixo). Tanto a chave primária como a secundária funcionam. Certifique-se de que coloca a chave entre aspas para que seja considerada uma cadeia de carateres.

```python
# Variables

_url = 'https://westcentralus.api.cognitive.microsoft.com/vision/v1/analyses'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10
```
