---
title: Início rápido para aprender a adicionar expressões a uma aplicação LUIS com JavaScript - Serviços Cognitivos do Azure | Microsoft Docs
description: Neste início rápido, vai aprender a chamar uma aplicação LUIS com JavaScript.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/24/2018
ms.author: diberry
ms.openlocfilehash: 0920a194d3e9c93883b88b7131f7e81dc8fb3302
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44159695"
---
# <a name="quickstart-change-model-using-javascript"></a>Início rápido: Alterar o modelo com JavaScript

[!INCLUDE [Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* [Visual Studio Code](https://code.visualstudio.com/).

[!INCLUDE [Code is available in LUIS-Samples Github repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>Ficheiro JSON de expressões de exemplo

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]


## <a name="create-quickstart-code"></a>Criar código de início rápido

Crie `add-utterances.html` e adicione o seguinte código:

   [!code-html[Html code](~/samples-luis/documentation-samples/quickstarts/change-model/javascript/add-utterance.html "Javascript code")]

## <a name="run-code"></a>Executar código

1. Abra o ficheiro num browser.

2. Adicione o ID de criação do LUIS, o seu ID de aplicação do LUIS.

3. Modifique a **matriz de expressões** a adicionar à sua aplicação. Estão armazenadas na variável utteranceJSON. Altere estes valores consoante as suas próprias necessidades de domínio e expressão. 

    ```json
    // example batch utterances
    var utteranceJSON = [
        {
            "text": "go to Seattle",
            "intentName": "BookFlight",
            "entityLabels": [
                {
                    "entityName": "Location::LocationTo",
                    "startCharIndex": 6,
                    "endCharIndex": 12
                }
            ]
        }
    ,
        {
            "text": "book a flight",
            "intentName": "BookFlight",
            "entityLabels": []
        }
    ];
    ```

4. Selecione o botão `Upload utterance`. Os resultados do LUIS são apresentados abaixo dos botões.

5. Selecione o botão `Train model` para preparar a sua aplicação com estas novas expressões.

6. Selecione o botão `Train Status` para ver o estado de preparação. 

    ![Add-utterances.html](./media/luis-quickstart-javascript-add-utterance/add-utterance.png)

## <a name="clean-up-resources"></a>Limpar recursos
Quando tiver terminado o início rápido, remova todos os ficheiros criados neste início rápido. 

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"]
> [Integrar o LUIS com um bot](luis-csharp-tutorial-build-bot-framework-sample.md)
