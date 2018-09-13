---
title: Analisar texto de linguagem natural em Language Understanding (LUIS) com Node.js – Serviços Cognitivos – Serviços Cognitivos do Azure | Microsoft Docs
description: Neste guia de introdução, irá aprender a utilizar uma aplicação LUIS pública disponível para determinar a intenção de um utilizador a partir do texto de uma conversação. Com o Node.js, envie a intenção do utilizador como texto para o ponto final de predição HTTP da aplicação pública. No ponto final, o LUIS aplica o modelo da aplicação pública para analisar o significado do texto de linguagem natural e assim determinar a intenção geral e extrair os dados relevantes para o domínio de requerente da aplicação.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: b85b8ef19d4cc46d80d600d1cb4404edd71e2374
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158012"
---
# <a name="quickstart-analyze-text-using-nodejs"></a>Guia de introdução: analisar texto com o Node.js

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Pré-requisitos

* Linguagem de programação [Node.js](https://nodejs.org/) 
* [Visual Studio Code](https://code.visualstudio.com/)
* ID da aplicação pública: df67dcdb-c37d-46af-88e1-8b97951ca1c2


> [!NOTE] 
> A solução Node.js completa está disponível a partir do [**repositório do Github** LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/analyze-text/node).

## <a name="get-luis-key"></a>Obter chave LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Analisar texto com um browser

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-nodejs"></a>Analisar texto com o Node.js

Pode utilizar o Node.js para aceder aos mesmos resultados que viu na janela do browser no passo anterior.

1. Copie o seguinte fragmento de código:

   [!code-nodejs[Console app code that calls a LUIS endpoint for Node.js](~/samples-luis/documentation-samples/quickstarts/analyze-text/node/call-endpoint.js)]

2. Crie um ficheiro `.env` com o seguinte texto ou defina estas variáveis no ambiente do sistema:

    ```CMD
    LUIS_APP_ID=df67dcdb-c37d-46af-88e1-8b97951ca1c2
    LUIS_ENDPOINT_KEY=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    ```

3. Defina a variável de ambiente `LUIS_ENDPOINT_KEY` para a sua chave.

4. Instale dependências ao executar o comando `npm install` na linha de comandos.

5. Execute o código com o comando `npm start`. Apresenta os mesmos valores que viu anteriormente na janela do browser.

## <a name="luis-keys"></a>Chaves LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Limpar recursos

Elimine o ficheiro Node.js.

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"]
> [Adicionar expressões](luis-get-started-node-add-utterance.md)