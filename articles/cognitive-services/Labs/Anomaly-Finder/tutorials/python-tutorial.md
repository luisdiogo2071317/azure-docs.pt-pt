---
title: Deteção de anomalias aplicação Python - serviços cognitivos da Microsoft | Documentos da Microsoft
description: Explore um bloco de notas do Python que utiliza a API de deteção de anomalias nos serviços cognitivos da Microsoft. Enviar pontos de dados original a API e obter o valor esperado e os pontos de anomalias.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 87cd9e976d231291ad13acecf188cfd668d692b6
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248232"
---
# <a name="anomaly-detection-python-application"></a>Aplicação de Python de deteção de anomalias

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

O tutorial mostra como utilizar a API de deteção de anomalias em Python e a visualizar os resultados usando bibliotecas populares. Com o Jupyter para executar o tutorial e testar os seus dados com a sua chave de subscrição. Para saber como começar com blocos de notas do Jupyter interativos, consulte [Jupyter documentação](http://jupyter.readthedocs.io/en/latest/index.html). 

## <a name="prerequisites"></a>Pré-requisitos

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Subscrever a deteção de anomalias e obter uma chave de subscrição 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="download-the-example-code"></a>Baixe o código de exemplo

1. Navegue para o [tutorial bloco de notas no Github](https://github.com/MicrosoftAnomalyDetection/python-sample).
2. Clique no botão verde para clonar ou transferir o tutorial. 

## <a name="opening-the-tutorial-notebook-in-jupyter"></a>Abrir o bloco de notas tutorial no Jupyter

1. Abra um prompt de comando e vá para o exemplo de python de pasta.
2. Execute o bloco de notas do Jupyter do comando no prompt de comando, que iniciará o Jupyter.
3. Na janela do Jupyter, clique em <em>Example.ipynb de API de deteção de anomalias</em> para abrir o bloco de notas do tutorial.   

## <a name="running-the-tutorial"></a>Executar o tutorial

Para utilizar este bloco de notas, terá uma chave de subscrição para a API de deteção de anomalias. Visite a página de subscrição para se inscrever. Na página de "Início de sessão", utilize a sua conta Microsoft para iniciar sessão e poderá inscrever-se e obtenha as suas chaves. Depois de concluir o processo de inscrição, cole a chave na secção de variáveis do bloco de notas (reproduzida abaixo). O principal ou a chave secundária funciona. Certifique-se de incluir a chave de aspas para que seja uma cadeia de caracteres.

```Python

            # Variables
            endpoint = 'https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection'
            subscription_key = None #Here you have to paste your primary key

```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência da API REST](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
