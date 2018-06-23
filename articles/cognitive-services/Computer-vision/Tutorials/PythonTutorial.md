---
title: Tutorial de visão API Python do computador | Microsoft Docs
description: Saiba como utilizar a API de visão do computador com o Python utilizando blocos de notas do Jupyter no serviços cognitivos da Microsoft. Visualize os resultados utilizando bibliotecas populares.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 02/25/2017
ms.author: kefre
ms.openlocfilehash: a093c2d066e70a8daf1fe1cd33ccf794ecb196af
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352376"
---
# <a name="computer-vision-api-python-tutorial"></a>Tutorial de Python de API de visão do computador

Este tutorial mostra como utilizar a API de visão do computador no Python e como visualizar os resultados utilizando algumas bibliotecas populares. Utilize o Jupyter para executar o tutorial. Para saber como começar com blocos de notas do Jupyter interativos, consulte: [Jupyter Documementation](http://jupyter.readthedocs.io/en/latest/index.html). 

### <a name="opening-the-tutorial-notebook-in-jupyter"></a>Abrir o bloco de notas Tutorial no Jupyter 

1. Navegue para o [tutorial bloco de notas no GitHub](https://github.com/Microsoft/Cognitive-Vision-Python). 
2. Clique no botão verde para clonar ou transferir o tutorial. 
3. Abra uma linha de comandos e aceda à pasta _Cognitive visão-Python master\Jupyter bloco de notas_. 
4. Execute o comando **bloco de notas do jupyter** na linha de comandos. Esta ação iniciará Jupyter.
5. Na janela do Jupyter, clique em _computador visão API Example.ipynb_ para abrir o bloco de notas do tutorial 

### <a name="running-the-tutorial"></a>Executar o Tutorial

Para utilizar este bloco de notas, precisa de uma chave de subscrição para a API de visão do computador. Visite o [página de subscrição](https://azure.microsoft.com/try/cognitive-services/) para se inscrever. Na página de "Início de sessão", utilize a sua conta Microsoft para iniciar sessão e poderá subscrever e obter chaves livres. Depois de concluir o processo de inscrição, cole a chave a secção de variáveis do bloco de notas (reproduzido abaixo). O principal ou a chave secundária funciona. Certificar-se de que inclua a chave de aspas para que seja uma cadeia.

```python
# Variables

_url = 'https://westcentralus.api.cognitive.microsoft.com/vision/v1/analyses'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10
```
