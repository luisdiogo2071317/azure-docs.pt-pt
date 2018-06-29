---
title: Tutorial para aprender a adicionar expressões a uma aplicação LUIS com PHP | Microsoft Docs
description: Neste tutorial, vai aprender a chamar uma aplicação LUIS com PHP.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 59150b7ed6782c28f243041be2ed6aa17e69cc01
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36263785"
---
# <a name="tutorial-add-utterances-to-app-using-php"></a>Tutorial: Adicionar expressões a uma aplicação com PHP 
Neste tutorial, vai escrever um programa para adicionar uma expressão a uma intenção com as APIs de Criação em PHP.

<!-- green checkmark -->
> [!div class="checklist"]
> * Criar o projeto de consola do Visual Studio 
> * Adicionar o método para chamar a API LUIS para adicionar a expressão e preparar a aplicação
> * Adicionar o ficheiro JSON com expressões de exemplo para a intenção BookFlight
> * Executar a consola e ver o estado de preparação para as expressões

Para obter mais informações, consulte a documentação técnica para as APIs [adicionar expressão de exemplo à intenção](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [preparar](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) e [estado de preparação](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Para este artigo, precisa de uma conta do [LUIS][LUIS] gratuita para criar a sua aplicação LUIS.

## <a name="prerequisites"></a>Pré-requisitos

* [**PHP**](http://php.net/) mais recente.
* Certifique-se de que openssl está disponível como dependência para PHP.  
* A **[chave de criação](luis-concept-keys.md#authoring-key)** do LUIS. Pode encontrar esta chave em Definições da Conta no site do [LUIS](luis-reference-regions.md).
* O [**ID da aplicação**](./luis-get-started-create-app.md) LUIS existente. O ID da aplicação é apresentado no dashboard de aplicações. A aplicação LUIS com as intenções e entidades utilizadas no ficheiro `utterances.json` tem de existir antes de executar o código em `add-utterances.php`. O código neste artigo não cria as intenções e as entidades. Adiciona apenas as expressões para intenções e entidades existentes. 
* O **ID da versão** na aplicação que recebe as expressões. O ID predefinido é "0.1"
* Crie um novo ficheiro denominado `add-utterances.php` no VSCode.

> [!NOTE] 
> O ficheiro `add-utterances.cs` completo e um ficheiro de exemplo `utterances.json` estão disponíveis no repositório do Github [**LUIS-Samples**](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/authoring-api-samples/php/).


## <a name="write-the-php-code"></a>Escrever o código PHP

Adicione as dependências ao ficheiro.

   [!code-php[PHP and LUIS Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/php/add-utterances.php?range=10-29 "PHP and LUIS Dependencies")]

Adicione o pedido GET utilizado para o estado de preparação.

   [!code-php[SendGet](~/samples-luis/documentation-samples/authoring-api-samples/php/add-utterances.php?range=31-50 "SendGet")]

Adicione o pedido POST utilizado para criar expressões ou iniciar a preparação. 

   [!code-php[SendPost](~/samples-luis/documentation-samples/authoring-api-samples/php/add-utterances.php?range=52-72 "SendPost")]

Adicione a função `AddUtterances`.

   [!code-php[AddUtterances method](~/samples-luis/documentation-samples/authoring-api-samples/php/add-utterances.php?range=74-79 "AddUtterances method")]


Adicione a função `Train`. 

   [!code-php[Train](~/samples-luis/documentation-samples/authoring-api-samples/php/add-utterances.php?range=81-88 "Train")]

Adicione a função `Status`.

   [!code-php[Status](~/samples-luis/documentation-samples/authoring-api-samples/php/add-utterances.php?range=90-94 "Status")]

Para gerir os argumentos de linha de comandos, adicione o bloco de código principal.

   [!code-php[Main code](~/samples-luis/documentation-samples/authoring-api-samples/php/add-utterances.php?range=96-120 "Main code")]

## <a name="specify-utterances-to-add"></a>Especificar as expressões a adicionar
Crie e edite o ficheiro `utterances.json` para especificar a **matriz de expressões** que quer adicionar à aplicação LUIS. A intenção e as entidades **têm** de já estar na aplicação LUIS.

> [!NOTE]
> A aplicação LUIS com as intenções e entidades utilizadas no ficheiro `utterances.json` tem de existir antes de executar o código em `add-utterances.php`. O código neste artigo não cria as intenções e as entidades. Adiciona apenas as expressões para intenções e entidades existentes.

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

Execute a aplicação a partir de uma linha de comandos com PHP.

Chamar `add-utterances.php` com apenas utterance.json como argumento adiciona mas não prepara o LUIS para as novas expressões.
````
> php add-utterances.php ./utterances.json
````

O JSON seguinte é devolvido a partir da chamada à API de expressões a adicionar. O campo `response` está neste formato para as expressões adicionadas. `hasError` é falso, o que indica que a expressão foi adicionada.  

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
Chame `add-utterance.php` com o argumento `-train` para enviar um pedido para preparar. 

````
> php add-utterances.php ./utterances.json -train
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
Chame a aplicação com o argumento `-status` para verificar o estado de preparação e apresentar detalhes do estado.

````
> php add-utterances.php -status
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