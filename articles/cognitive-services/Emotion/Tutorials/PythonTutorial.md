---
title: Tutorial do Python de API de emoções | Microsoft Docs
description: Utilize um bloco de notas do Jupyter para saber como utilizar a API dos serviços de emoções cognitivos com o Python. Visualize os resultados utilizando bibliotecas populares.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: 70c8ca48c651601f3d7cbb3717c32bfe112176fe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352399"
---
# <a name="emotion-api-using-python-tutorial"></a>Emoções API utilizando o Tutorial do Python

> [!IMPORTANT]
> Vídeo de API de pré-visualização vai terminar em 30th de Outubro de 2017. Experimentar o novo [pré-visualização do vídeo indexador API](https://azure.microsoft.com/services/cognitive-services/video-indexer/) para extrair facilmente insights de vídeos e a melhorar as experiências de deteção de conteúdos, tais como resultados de pesquisa, através da deteção palavras ditas, faces, carateres e emotions. [Saiba mais](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Para facilitar a introdução à API de emoções, o bloco de notas do Jupyter hiperligações em abaixo mostra como utilizar a API no Python e como visualizar os resultados utilizando algumas bibliotecas populares. 

[Ligação para o bloco de notas no GitHub](https://github.com/Microsoft/Cognitive-Emotion-Python/blob/master/Jupyter%20Notebook/Emotion%20Analysis%20Example.ipynb)

### <a name="using-the-jupyter-notebook"></a>Utilizar o bloco de notas do Jupyter

Para utilizar o bloco de notas interativamente, terá de clone-o e execute-o no Jupyter. Para saber como começar a utilizar blocos de notas do Jupyter interativos, siga as instruções apresentadas em http://jupyter.readthedocs.org/en/latest/install.html. 

Para utilizar este bloco de notas, precisa de uma chave de subscrição para a API de emoções. Visite o [página de subscrição](https://azure.microsoft.com/try/cognitive-services/) para se inscrever. Na página de "Início de sessão", utilize a sua conta Microsoft para iniciar sessão e poderá subscrever e obter chaves livres. Depois de concluir o processo de inscrição, cole a chave a secção de variáveis mostrada abaixo. O principal ou a chave secundária funciona.

```
Python Example 

#Variables

_url = 'https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10

```
