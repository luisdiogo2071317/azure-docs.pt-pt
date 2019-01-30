---
title: 'Início rápido: Detetar rostos numa imagem com a API REST do Azure e Python'
titleSuffix: Azure Cognitive Services
description: Neste início rápido, irá utilizar a API de REST do Azure Face com o Python para detetar rostos numa imagem.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 11/09/2018
ms.author: pafarley
ms.openlocfilehash: 606f01059620443c8c42a24f416128d0856eff50
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55213791"
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
