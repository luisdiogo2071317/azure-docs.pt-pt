---
title: 'Início rápido: Detetar rostos numa imagem com a API REST do Azure e Python'
titleSuffix: Azure Cognitive Services
description: Neste início rápido, irá utilizar a API de REST do Azure Face com o Python para detetar rostos numa imagem.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: quickstart
ms.date: 11/09/2018
ms.author: pafarley
ms.openlocfilehash: d2a98226895bbe5996785ca4726f20df9b98ffdd
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51683615"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-rest-api-and-python"></a>Início rápido: Detetar rostos numa imagem usando a API de REST de rostos e o Python

Neste início rápido, irá utilizar a API de REST de Face do Azure com o Python para detetar rostos humanos numa imagem. O script irá desenhar quadros em torno de rostos e superimpose idade e género informações sobre a imagem.

![Um homem e uma mulher, cada um com um retângulo desenhado em torno de seus rostos e idade e sex apresentado na imagem](../images/labelled-faces-python.png)

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar. 


## <a name="prerequisites"></a>Pré-requisitos

- Uma chave de assinatura da Face API. Pode obter uma chave de subscrição de avaliação gratuita de [experimentar os serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Em alternativa, siga as instruções em [criar uma conta dos serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para subscrever o serviço de API de rostos e obtenha a chave.

## <a name="run-the-jupyter-notebook"></a>Executar o bloco de notas do Jupyter

Pode executar este guia de início rápido sob a forma de um bloco de notas do Jupyter no [MyBinder](https://mybinder.org). Para iniciar o associador, selecione o botão abaixo. Em seguida, siga as instruções no bloco de notas.

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=FaceAPI.ipynb)

## <a name="next-steps"></a>Passos Seguintes

Em seguida, explore a documentação de referência da Face API para saber mais sobre os cenários suportados.

> [!div class="nextstepaction"]
> [API Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
