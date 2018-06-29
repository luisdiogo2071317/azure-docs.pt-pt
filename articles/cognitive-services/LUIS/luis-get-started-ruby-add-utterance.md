---
title: Tutorial para aprender a adicionar expressões a uma aplicação LUIS com Ruby | Microsoft Docs
description: Neste tutorial, vai aprender a chamar uma aplicação LUIS com Ruby.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 7a470fd551a58978e6f2be0450a2e2a6cd471fc4
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266061"
---
# <a name="tutorial-add-utterances-to-app-using-ruby"></a>Tutorial: Adicionar expressões a uma aplicação com Ruby 
Neste tutorial, vai escrever um programa para adicionar uma expressão a uma intenção com as APIs de Criação em Ruby.

<!-- green checkmark -->
> [!div class="checklist"]
> * Criar o projeto de consola do Visual Studio 
> * Adicionar o método para chamar a API LUIS para adicionar a expressão e preparar a aplicação
> * Adicionar o ficheiro JSON com expressões de exemplo para a intenção BookFlight
> * Executar a consola e ver o estado de preparação para as expressões

Para obter mais informações, consulte a documentação técnica para as APIs [adicionar expressão de exemplo à intenção](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [preparar](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) e [estado de preparação](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Para este artigo, precisa de uma conta do [LUIS][LUIS] gratuita para criar a sua aplicação LUIS.

## <a name="prerequisites"></a>Pré-requisitos

* [Ruby](http://rubyinstaller.org/) 
* A **[chave de criação](luis-concept-keys.md#authoring-key)** do LUIS. Pode encontrar esta chave em Definições da Conta no site do [LUIS](luis-reference-regions.md).
* O [**ID da aplicação**](./luis-get-started-create-app.md) LUIS existente. O ID da aplicação é apresentado no dashboard de aplicações. A aplicação LUIS com as intenções e entidades utilizadas no ficheiro `utterances.json` tem de existir antes de executar o código em `add-utterances.rb`. O código neste artigo não cria as intenções e as entidades. Adiciona apenas as expressões para intenções e entidades existentes. 
* O **ID da versão** na aplicação que recebe as expressões. O ID predefinido é "0.1"
* Crie um novo ficheiro denominado `add-utterances.rb` no VSCode.

> [!NOTE] 
> O ficheiro `add-utterances.cs` completo e um ficheiro de exemplo `utterances.json` estão disponíveis no repositório do Github [**LUIS-Samples**](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/authoring-api-samples/ruby/).


## <a name="write-the-ruby-code"></a>Escrever o código Ruby

Adicione as dependências ao ficheiro.

   [!code-ruby[Ruby and LUIS Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=1-28 "Ruby and LUIS Dependencies")]

Adicione o pedido GET utilizado para o estado de preparação.

   [!code-ruby[SendGet](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=30-40 "SendGet")]

Adicione o pedido POST utilizado para criar expressões ou iniciar a preparação. 

   [!code-ruby[SendPost](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=42-54 "SendPost")]

Adicione a função `AddUtterances`.

   [!code-ruby[AddUtterances method](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=56-61 "AddUtterances method")]


Adicione a função `Train`. 

   [!code-ruby[Train](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=63-69 "Train")]

Adicione a função `Status`.

   [!code-ruby[Status](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=71-75 "Status")]

Para gerir argumentos, adicione o código principal.

   [!code-ruby[Main code](~/samples-luis/documentation-samples/authoring-api-samples/ruby/add-utterances.rb?range=77-93 "Main code")]

## <a name="specify-utterances-to-add"></a>Especificar as expressões a adicionar
Crie e edite o ficheiro `utterances.json` para especificar a **matriz de expressões** que quer adicionar à aplicação LUIS. A intenção e as entidades **têm** de já estar na aplicação LUIS.

> [!NOTE]
> A aplicação LUIS com as intenções e entidades utilizadas no ficheiro `utterances.json` tem de existir antes de executar o código em `add-utterances.rb`. O código neste artigo não cria as intenções e as entidades. Adiciona apenas as expressões para intenções e entidades existentes.

O campo `text` contém o texto da expressão. O campo `intentName` tem de corresponder ao nome de uma intenção na aplicação LUIS. O campo `entityLabels` é obrigatório. Se não quiser etiquetar nenhuma entidade, forneça uma lista vazia, conforme mostrado no exemplo seguinte:

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

Execute a aplicação a partir de uma linha de comandos com Ruby.

Chamar `add-utterances.rb` com apenas utterance.json como argumento adiciona mas não prepara o LUIS para as novas expressões.
````
> ruby add-utterances.rb ./utterances.json
````

Esta linha apresenta os resultados da chamada à API de expressões a adicionar. O campo `response` está neste formato para as expressões adicionadas. `hasError` é falso, o que indica que a expressão foi adicionada.  

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
Chame add-utterance com o argumento `-train` para enviar um pedido para preparar.

````
> ruby add-utterances.rb ./utterances.json -train
````

> [!NOTE]
> As expressões duplicadas não são adicionadas novamente, mas não originam um erro. `response` contém o ID da expressão original.

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
Chame o exemplo com o argumento `-status` para verificar o estado de preparação.

````
> ruby add-utterances.rb ./utterances.json -status
````

```
Requested training status.
[
   {
      "modelId": "eb2f117c-e10a-463e-90ea-1a0176660acc",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "c1bdfbfc-e110-402e-b0cc-2af4112289fb",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "863023ec-2c96-4d68-9c44-34c1cbde8bc9",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "82702162-73ba-4ae9-a6f6-517b5244c555",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "37121f4c-4853-467f-a9f3-6dfc8cad2763",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "de421482-753e-42f5-a765-ad0a60f50d69",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "80f58a45-86f2-4e18-be3d-b60a2c88312e",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "c9eb9772-3b18-4d5f-a1e6-e0c31f91b390",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "2afec2ff-7c01-4423-bb0e-e5f6935afae8",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "95a81c87-0d7b-4251-8e07-f28d180886a1",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   }
]
```

## <a name="clean-up-resources"></a>Limpar recursos
Quando tiver terminado o tutorial, remova o Visual Studio e a aplicação de consola, se já não forem necessários.

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"] 
> [Criar uma aplicação LUIS programaticamente](luis-tutorial-node-import-utterances-csv.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
