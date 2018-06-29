---
title: Tutorial para aprender a adicionar expressões a uma aplicação LUIS com Node.js | Microsoft Docs
description: Neste tutorial, vai aprender a chamar uma aplicação LUIS com Node.js.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: a1e028395ce4b1679a367ce110c2ba120128c501
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264231"
---
# <a name="tutorial-add-utterances-app-using-nodejs"></a>Tutorial: Adicionar expressões a uma aplicação com Node.js 
Neste tutorial, vai escrever um programa para adicionar uma expressão a uma intenção com as APIs de Criação em Node.js.

<!-- green checkmark -->
> [!div class="checklist"]
> * Criar o projeto de consola do Visual Studio 
> * Adicionar o método para chamar a API LUIS para adicionar a expressão e preparar a aplicação
> * Adicionar o ficheiro JSON com expressões de exemplo para a intenção BookFlight
> * Executar a consola e ver o estado de preparação para as expressões

Para obter mais informações, consulte a documentação técnica para as APIs [adicionar expressão de exemplo à intenção](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [preparar](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) e [estado de preparação](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Para este artigo, precisa de uma conta do [LUIS][LUIS] gratuita para criar a sua aplicação LUIS.

## <a name="prerequisites"></a>Pré-requisitos

* [**Node.js**](https://nodejs.org/en/download/) mais recente com NPM.
* Dependências NPM para este artigo: [**request**](https://www.npmjs.com/package/request), [**request-promise**](https://www.npmjs.com/package/request-promise), [**fs-extra**](https://www.npmjs.com/package/fs-extra).  
* **[Recomendado]**  [Visual Studio Code](https://code.visualstudio.com/) para IntelliSense e depuração.
* A **[chave de criação](luis-concept-keys.md#authoring-key)** do LUIS. Pode encontrar esta chave em Definições da Conta no site do [LUIS](luis-reference-regions.md).
* O [**ID da aplicação**](./luis-get-started-create-app.md) LUIS existente. O ID da aplicação é apresentado no dashboard de aplicações. A aplicação LUIS com as intenções e entidades utilizadas no ficheiro `utterances.json` tem de existir antes de executar o código em `add-utterances.js`. O código neste artigo não criará as intenções e as entidades. Adicionará apenas as expressões para intenções e entidades existentes. 
* O **ID da versão** na aplicação que recebe as expressões. O ID predefinido é "0.1"
* Crie um novo ficheiro denominado `add-utterances.js` no VSCode.

> [!NOTE] 
> O ficheiro `add-utterances.js` completo está disponível a partir do repositório do Github [ **LUIS-Samples**](https://github.com/Microsoft/LUIS-Samples/tree/master/examples/add-utterances/nodejs).


## <a name="write-the-nodejs-code"></a>Escrever o código Node.js

Adicione as dependências NPM ao ficheiro.

   [!code-javascript[NPM Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=16-19 "NPM Dependencies")]

Adicione as constantes do LUIS ao ficheiro. Copie o código seguinte e altere para a sua chave de criação, ID da aplicação e ID da versão.

   [!code-javascript[LUIS key and IDs](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=22-29 "LUIS key and IDs")]

Adicione o nome e a localização do ficheiro de carregamento com as suas expressões. 

   [!code-javascript[Add upload file](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=31-33 "Add upload file")]

Adicione as variáveis com os valores de linha de comandos.

   [!code-javascript[Add upload file](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=35-49 "Add upload file")]


Adicione a função `sendUtteranceToApi` para enviar e receber chamadas HTTP. 

   [!code-javascript[Send the HTTP request](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=135-151 "Send the HTTP request")]

Adicione o objeto JSON de configuração utilizado pela função `addUtterance`.

   [!code-javascript[Add configuration information for adding utterance](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=52-59 "Add configuration information for adding utterance")]

Adicione a função `addUtterance` para gerir o pedido de API e a resposta utilizada por `SendUtteranceToApp`.

   [!code-javascript[Add configuration information for adding utterance](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=62-92 "Add configuration information for adding utterance")]

Adicione o objeto JSON de configuração utilizado pela função `train`.

   [!code-javascript[Add configuration information for training LUIS](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=94-101 "Add configuration information for training LUIS")]

Adicione a função `train` para iniciar o processo de preparação. 

   [!code-javascript[Train the application](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=103-133 "Train the application")]

Adicione o código que escolhe a ação a efetuar (adicionar expressão ou preparação) com base nas variáveis de linha de comandos.

   [!code-javascript[Train the application](~/samples-luis/documentation-samples/authoring-api-samples/node/add-utterances.js?range=153-184 "Train the application")]

## <a name="specify-utterances-to-add"></a>Especificar as expressões a adicionar
Crie e edite o ficheiro `utterances.json` para especificar a **matriz de expressões** que quer adicionar à aplicação LUIS. A intenção e as entidades **têm** de já estar na aplicação LUIS.

> [!NOTE]
> A aplicação LUIS com as intenções e entidades utilizadas no ficheiro `utterances.json` tem de existir antes de executar o código em `add-utterances.js`. O código neste artigo não criará as intenções e as entidades. Adicionará apenas as expressões para intenções e entidades existentes.

O campo `text` contém o texto da expressão. O campo `intentName` tem de corresponder ao nome de uma intenção na aplicação LUIS. O campo `entityLabels` é obrigatório. Se não quiser etiquetar nenhuma entidade, forneça uma lista vazia, conforme mostrado no exemplo seguinte.

Se a lista entityLabels não estiver vazia, `startCharIndex` e `endCharIndex` têm de marcar a entidade referida no campo `entityName`. Ambos os índices são contagens baseadas em zero, o que significa que o 6 no exemplo anterior se refere ao "S" de Seattle e não ao espaço antes do S maiúsculo.

```json
[
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
    },
    {
        "text": "book a flight",
        "intentName": "BookFlight",
        "entityLabels": []
    }
]
```

## <a name="add-an-utterance-from-the-command-line"></a>Adicionar uma expressão a partir da linha de comandos

Execute a aplicação a partir de uma linha de comandos com Node.js.

Chamar add-utterance sem argumentos adiciona uma expressão à aplicação, sem prepará-la.
````
> node add-utterances.js
````

Este exemplo cria um ficheiro com `results.json` que contém os resultados da chamada à API de expressões a adicionar. O campo `response` está neste formato para as expressões adicionadas. `hasError` é falso, o que indica que a expressão foi adicionada.  

```json
    "response": [
        {
            "value": {
                "UtteranceText": "go to seattle",
                "ExampleId": -5123383
            },
            "hasError": false
        },
        {
            "value": {
                "UtteranceText": "book a flight",
                "ExampleId": -169157
            },
            "hasError": false
        }
    ]
```

## <a name="add-an-utterance-and-train-from-the-command-line"></a>Adicionar uma expressão e preparar a partir da linha de comandos
Chame add-utterance com o argumento `-train` para enviar um pedido para preparar e, subsequentemente, o estado de preparação do pedido. O estado é colocado em fila de espera imediatamente após o início da preparação. Os detalhes do estado são escritos num ficheiro.

````
> node add-utterances.js -train
````

> [!NOTE]
> As expressões duplicadas não são adicionadas novamente, mas não originam um erro. `response` contém o ID da expressão original.

Quando chamar o exemplo com o argumento `-train`, cria um ficheiro `training-results.json` que indica que o pedido para preparar a aplicação LUIS foi colocado em fila de espera com êxito. 

Em seguida, é mostrado o resultado de um pedido para preparar com êxito:
```json
{
    "request": null,
    "response": {
        "statusId": 9,
        "status": "Queued"
    }
}
```

Depois de o pedido para preparar ser colocado em fila de espera, pode demorar alguns minutos a concluir a preparação.

## <a name="get-training-status-from-the-command-line"></a>Obter o estado de preparação a partir da linha de comandos
Chame o exemplo com o argumento `-status` para verificar o estado de preparação e escrever os detalhes do estado num ficheiro.

````
> node add-utterances.js -status
````

## <a name="clean-up-resources"></a>Limpar recursos
Quando tiver terminado o tutorial, remova o Visual Studio e a aplicação de consola, se já não forem necessários. 

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"] 
> [Criar uma aplicação LUIS programaticamente](luis-tutorial-node-import-utterances-csv.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
