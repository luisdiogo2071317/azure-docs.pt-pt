---
title: Guia de Início Rápido de Python – prever a intenção – LUIS
titleSuffix: Azure Cognitive Services
description: Neste início rápido, vai aprender a chamar uma aplicação LUIS com Python.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: e560aeffecf63f63966a49053e0f79d012b4a0a3
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038277"
---
# <a name="quickstart-get-intent-using-python"></a>Guia de Início Rápido: Obter a intenção com Python
Neste início rápido, transmita expressões a um ponto final de LUIS e obtenha a intenção e as entidades.

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Pré-requisitos

* [Python 3.6](https://www.python.org/downloads/) ou posterior.
* [Visual Studio Code](https://code.visualstudio.com/)

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Obter chave LUIS

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Obter a intenção com o browser

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent--programmatically"></a>Obter a intenção através de programação

Pode utilizar o Python para aceder aos mesmos resultados que viu na janela do browser no passo anterior.

1. Copie um dos seguintes fragmentos de código para um ficheiro chamado `quickstart-call-endpoint.py`:

   [!code-python[Console app code that calls a LUIS endpoint for Python 2.7](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/2.x/quickstart-call-endpoint-2-7.py)]

   [!code-python[Console app code that calls a LUIS endpoint for Python 3.6](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/3.x/quickstart-call-endpoint-3-6.py)]

2. Substitua o valor do campo `Ocp-Apim-Subscription-Key` pela sua chave de ponto final de LUIS.

3. Instale dependências com `pip install requests`.

4. Execute o script com `python ./quickstart-call-endpoint.py`. Apresenta o mesmo JSON que viu anteriormente na janela do browser.

## <a name="luis-keys"></a>Chaves LUIS

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Limpar recursos
Elimine o ficheiro python. 

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Adicionar expressões](luis-get-started-python-add-utterance.md)