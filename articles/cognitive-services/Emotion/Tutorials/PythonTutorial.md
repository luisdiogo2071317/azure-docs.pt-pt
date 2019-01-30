---
title: 'Tutorial: Reconhecer emoções num rosto numa imagem - API de emoções, Python'
titlesuffix: Azure Cognitive Services
description: Utilize um bloco de notas do Jupyter para saber como utilizar a API de Emoções com Python. Visualize os seus resultados com bibliotecas populares.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: tutorial
ms.date: 05/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: ea96495fb9fa453f7c7f9d6a870291329d2793b2
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55220540"
---
# <a name="tutorial-use-the-emotion-api-with-a-jupyter-notebook--python"></a>Tutorial: Utilize a API de emoções com um bloco de notas do Jupyter e Python.

> [!IMPORTANT]
> A API de Emoções vai ser preterida no dia 15 de fevereiro de 2019. A função de reconhecimento de emoções está agora geralmente disponível como parte da [API Face](https://docs.microsoft.com/azure/cognitive-services/face/). 

Para facilitar a introdução à API de Emoções, o bloco de notas do Jupyter na ligação abaixo mostra como utilizar a API em Python e visualizar os resultados com algumas bibliotecas populares.

[Ligação para o bloco de notas no GitHub](https://github.com/Microsoft/Cognitive-Emotion-Python/blob/master/Jupyter%20Notebook/Emotion%20Analysis%20Example.ipynb)

### <a name="using-the-jupyter-notebook"></a>Utilizar o Jupyter Notebook

Para utilizar o bloco de notas de forma interativa, terá de cloná-lo e executá-lo no Jupyter. Para saber como começar a utilizar os blocos de notas interativos do Jupyter, siga as instruções em http://jupyter.readthedocs.org/en/latest/install.html.

Para utilizar este bloco de notas, precisa de uma chave de subscrição para a API de Emoções. Visite a [página Subscrição](https://azure.microsoft.com/try/cognitive-services/) para se inscrever. Na página “Iniciar sessão”, utilize a sua conta Microsoft para iniciar sessão e poderá subscrever e obter chaves gratuitas. Depois de concluir o processo de inscrição, cole a chave na secção de variáveis mostrada abaixo. Tanto a chave primária como a secundária funcionam.

```
Python Example

#Variables

_url = 'https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10

```
