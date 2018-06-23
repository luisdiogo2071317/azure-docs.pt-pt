---
title: Deteção de anomalias aplicação Python - serviços cognitivos Microsoft | Microsoft Docs
description: Explore um bloco de notas do Python que utiliza a API de deteção de anomalias nos serviços cognitivos da Microsoft. Enviar pontos de dados original para a API e obter os pontos de anomalias e o valor esperado.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: d35f41ddab21aa155376ad52ff4084298dab8fc5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353936"
---
# <a name="anomaly-detection-python-application"></a>Aplicação de Python de deteção de anomalias

O tutorial mostra como utilizar a API de deteção de anomalias no Python e como visualizar os resultados utilizando bibliotecas populares. Com o Jupyter para executar o tutorial e tentar os seus próprios dados com a sua chave de subscrição. Para saber como começar com blocos de notas do Jupyter interativos, consulte [Jupyter documentação](http://jupyter.readthedocs.io/en/latest/index.html). 

## <a name="prerequisites"></a>Pré-requisitos

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Subscrever a deteção de anomalias e obter uma chave de subscrição 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="download-the-example-code"></a>Transferir o código de exemplo

1. Navegue para o [tutorial bloco de notas no Github](https://github.com/MicrosoftAnomalyDetection/python-sample).
2. Clique no botão verde para clonar ou transferir o tutorial. 

## <a name="opening-the-tutorial-notebook-in-jupyter"></a>Abrir o bloco de notas tutorial no Jupyter

1. Abra uma linha de comandos e avance para o exemplo de python pasta.
2. Execute o bloco de notas do Jupyter de comando na linha de comandos, que irá iniciar Jupyter.
3. Na janela do Jupyter, clique em <em>Example.ipynb de API de deteção de anomalias</em> para abrir o bloco de notas do tutorial.   

## <a name="running-the-tutorial"></a>Executar o tutorial

Para utilizar este bloco de notas, precisa de uma chave de subscrição para a API de deteção de anomalias. Visite a página de subscrição para se inscrever. Na página de "Início de sessão", utilize a sua conta Microsoft para iniciar sessão e poderá subscrever e obter as chaves. Depois de concluir o processo de inscrição, cole a chave a secção de variáveis do bloco de notas (reproduzido abaixo). O principal ou a chave secundária funciona. Certificar-se de que inclua a chave de aspas para que seja uma cadeia.

```Python

            # Variables
            endpoint = 'https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection'
            subscription_key = None #Here you have to paste your primary key

```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência da API REST](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
