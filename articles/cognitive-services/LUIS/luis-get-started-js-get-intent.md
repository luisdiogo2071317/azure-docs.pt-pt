---
title: Guia de Início Rápido de Javascript – prever a intenção – LUIS
titleSuffix: Azure Cognitive Services
description: Neste início rápido, irá aprender a utilizar uma aplicação LUIS pública disponível para determinar a intenção de um utilizador a partir do texto de uma conversação. Com o JavaScript, envie a intenção do utilizador como texto para o ponto final de predição HTTP da aplicação pública. No ponto final, o LUIS aplica o modelo da aplicação pública para analisar o significado do texto de linguagem natural e assim determinar a intenção geral e extrair os dados relevantes para o domínio de requerente da aplicação.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: cbbf0d2388f83f4e38f0a63b6bc181d4a24e0aa0
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47039485"
---
# <a name="quickstart-get-intent-using-javascript"></a>Guia de Início Rápido: Obter a intenção com Javascript

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

* [Visual Studio Code](https://code.visualstudio.com/)
* ID da aplicação pública: df67dcdb-c37d-46af-88e1-8b97951ca1c2


## <a name="get-luis-key"></a>Obter chave LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Obter a intenção com o browser

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>Obter a intenção através de programação

Pode utilizar o JavaScript para aceder aos mesmos resultados que viu na janela do browser no passo anterior. 

1. Copie o código que se segue e guarde-o num ficheiro HTML:

   [!code-html[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/javascript/call-endpoint.html)]

2. Abra o ficheiro num browser. Introduza a sua chave de ponto final LUIS no formulário e selecione **Submeter**.

    ![Exemplo de HTML apresentado num browser com resultados do LUIS para a aplicação Automatização de Dispositivos Domésticos](./media/luis-get-started-js-get-intent/html-results.png)

    Os resultados são apresentados abaixo do formulário. 

## <a name="luis-keys"></a>Chaves LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Limpar recursos

Elimine o ficheiro JavaScript.

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"]
> [Adicionar expressões](luis-get-started-javascript-add-utterance.md)
