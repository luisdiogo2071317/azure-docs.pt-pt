---
title: Tutorial para aprender a adicionar expressões a uma aplicação LUIS com JavaScript | Microsoft Docs
description: Neste tutorial, vai aprender a chamar uma aplicação LUIS com JavaScript.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/18/2017
ms.author: v-geberr
ms.openlocfilehash: b6d021dcfdddb5449aa989c6aa06d7faf326befb
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265464"
---
# <a name="tutorial-add-utterances-to-app-using-javascript"></a>Tutorial: Adicionar expressões a uma aplicação com JavaScript
Neste tutorial, vai escrever um programa para adicionar uma expressão a uma intenção com as APIs de Criação em JavaScript.

<!-- green checkmark -->
> [!div class="checklist"]
> * Criar o projeto de consola do Visual Studio 
> * Adicionar o método para chamar a API LUIS para adicionar a expressão e preparar a aplicação
> * Adicionar o ficheiro JSON com expressões de exemplo para a intenção BookFlight
> * Executar a consola e ver o estado de preparação para as expressões

Para obter mais informações, consulte a documentação técnica para as APIs [adicionar expressão de exemplo à intenção](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [preparar](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) e [estado de preparação](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Para este artigo, precisa de uma conta do [LUIS][LUIS] gratuita para criar a sua aplicação LUIS.

## <a name="prerequisites"></a>Pré-requisitos
* A [**chave de criação**](luis-concept-keys.md#authoring-key) do LUIS. 
* O **ID da aplicação** e o **ID da versão** do LUIS existentes. 
* Um novo ficheiro denominado `add-utterances.html` no VSCode.

> [!NOTE] 
> O ficheiro `add-utterances.html` completo está disponível a partir do repositório do Github [ **LUIS-Samples**](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/authoring-api-samples/javascript/add-utterance.html).


## <a name="write-the-code"></a>Escrever o código
Crie `add-utterances.html` e adicione o seguinte código:

   [!code-javascript[Java Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/javascript/add-utterance.html "Java Dependencies")]

## <a name="view-in-browser"></a>Ver no browser
1. Abra o ficheiro num browser.

2. Adicione o ID de criação do LUIS, o ID da aplicação LUIS e altere a versão, se não for a `0.1`

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
Quando tiver terminado o tutorial, remova o Visual Studio e a aplicação de consola, se já não forem necessários. 

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"]
> [Integrar o LUIS com um bot](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website