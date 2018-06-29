---
title: Tutorial para aprender a adicionar expressões a uma aplicação LUIS com Java | Microsoft Docs
description: Neste tutorial, vai aprender a chamar uma aplicação LUIS com Java.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 5b9c7b90ca96b23fbabfeb1e1d06e4124e65a0dc
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266044"
---
# <a name="tutorial-add-utterances-to-app-using-java"></a>Tutorial: Adicionar expressões a uma aplicação com Java 
Neste tutorial, vai escrever um programa para adicionar uma expressão a uma intenção com as APIs de Criação em Java.

<!-- green checkmark -->
> [!div class="checklist"]
> * Criar o projeto de consola do Visual Studio 
> * Adicionar o método para chamar a API LUIS para adicionar a expressão e preparar a aplicação
> * Adicionar o ficheiro JSON com expressões de exemplo para a intenção BookFlight
> * Executar a consola e ver o estado de preparação para as expressões

Para obter mais informações, consulte a documentação técnica para as APIs [adicionar expressão de exemplo à intenção](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [preparar](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) e [estado de preparação](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Para este artigo, precisa de uma conta do [LUIS][LUIS] gratuita para criar a sua aplicação LUIS.

## <a name="prerequisites"></a>Pré-requisitos

* Oracle [**Java/JDK**](http://www.oracle.com/technetwork/java/javase/downloads/index.html) mais recente.
* [Biblioteca JSON GSON da Google](https://github.com/google/gson).
* A **[chave de criação](luis-concept-keys.md#authoring-key)** do LUIS. Pode encontrar esta chave em Definições da Conta no site do [LUIS](luis-reference-regions.md).
* O [**ID da aplicação**](./luis-get-started-create-app.md) LUIS existente. O ID da aplicação é apresentado no dashboard de aplicações. A aplicação LUIS com as intenções e entidades utilizadas no ficheiro `utterances.json` tem de existir antes de executar o código em `AddUtterances.java`. O código neste artigo não criará as intenções e as entidades. Adicionará apenas as expressões para intenções e entidades existentes. 
* O **ID da versão** na aplicação que recebe as expressões. O ID predefinido é "0.1"
* Crie um novo ficheiro de texto com o nome `AddUtterances.java`.

> [!NOTE] 
> O ficheiro `add-utterances.cs` completo e um ficheiro de exemplo `utterances.json` estão disponíveis no repositório do Github [**LUIS-Samples**](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/authoring-api-samples/java).


## <a name="write-the-java-code"></a>Escrever o código Java

Adicione as dependências de Java ao ficheiro.

   [!code-java[Java Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=31-34 "Java Dependencies")]

Criar a classe `AddUtterances`

```Java
public class AddUtterances {
    // Insert code here
}
```

Esta classe contém todos os fragmentos de código que se seguem.

Adicione as constantes do LUIS à classe. Copie o código seguinte e altere para a sua chave de criação, ID da aplicação e ID da versão.

   [!code-java[LUIS-based IDs](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=41-53 "LUIS-based IDs")]

Adicione o método à chamada na API LUIS. 

   [!code-java[HTTP request to LUIS](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=55-178 "HTTP request to LUIS")]

Adicione o método para a resposta HTTP a partir da API LUIS.

   [!code-java[HTTP response from LUIS](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=180-226 "HTTP response from LUIS")]


Adicione o processamento de exceções. 

   [!code-java[Exception Handling](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=228-256 "Exception Handling")]

Adicione o processamento de saída/impressão.

   [!code-java[Add output handling](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=258-267 "Add configuration information for adding utterance")]

Adicione a função principal.

   [!code-java[Add main function](~/samples-luis/documentation-samples/authoring-api-samples/java/AddUtterances.java?range=269-345 "Add main function")]


## <a name="specify-utterances-to-add"></a>Especificar as expressões a adicionar
Crie e edite o ficheiro `utterances.json` para especificar a **matriz de expressões** que quer adicionar à aplicação LUIS. A intenção e as entidades **têm** de já estar na aplicação LUIS.

> [!NOTE]
> A aplicação LUIS com as intenções e entidades utilizadas no ficheiro `utterances.json` tem de existir antes de executar o código em `AddUtterances.java`. O código neste artigo não criará as intenções e as entidades. Adicionará apenas as expressões para intenções e entidades existentes.

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

Compilar AddUtterance com as dependências

```
> javac -classpath gson-2.8.2.jar AddUtterances.java
```

Chamar `AddUtterance` sem argumentos adiciona as expressões do LUIS à aplicação, sem prepará-la.
````
> java -classpath .;gson-2.8.2.jar AddUtterances
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
Chame `add-utterance` com o argumento `-train` para enviar um pedido para preparar. 

````
> java -classpath .;gson-2.8.2.jar AddUtterances -train
````

> [!NOTE]
> As expressões duplicadas não são adicionadas novamente, mas não originam um erro. `response` contém o ID da expressão original.

Quando chamar a aplicação com o argumento `-train`, cria um ficheiro `training-results.json` que indica que o pedido para preparar a aplicação LUIS foi colocado em fila de espera com êxito. 

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
Chame a aplicação com o argumento `-status` para verificar o estado de preparação e escrever os detalhes do estado num ficheiro.

````
> java -classpath .;gson-2.8.2.jar AddUtterances -status
````

## <a name="clean-up-resources"></a>Limpar recursos
Quando tiver terminado o tutorial, remova o Visual Studio e a aplicação de consola, se já não forem necessários. 

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"] 
> [Criar uma aplicação LUIS programaticamente](luis-tutorial-node-import-utterances-csv.md) 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website