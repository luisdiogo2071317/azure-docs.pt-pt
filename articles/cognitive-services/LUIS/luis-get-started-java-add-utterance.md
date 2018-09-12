---
title: Início rápido - alterar o modelo e treinar a aplicação LUIS com Java - Serviços Cognitivos do Azure | Microsoft Docs
description: Neste início rápido do Java, adicione expressões de exemplo a uma aplicação de Automatização de Casa e treine a aplicação. As expressões de exemplo são texto de utilizador de conversação mapeado para uma intenção. Ao conceder expressões de exemplo para intenções, ensina o LUIS sobre quais são os tipos de texto fornecido pelo utilizador que pertencem a que intenção.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/24/2018
ms.author: diberry
ms.openlocfilehash: ddb22bce77dda55ad6e83efa8c0ca2c476f78836
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44161123"
---
# <a name="quickstart-change-model-using-java"></a>Início rápido: Alterar o modelo com Java 

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [Quickstart prerequisites for endpoint](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* [JDK SE](http://www.oracle.com/technetwork/java/javase/downloads/index.html) (Kit de Desenvolvimento do Java, Edição Standard)
* [Biblioteca JSON GSON da Google](https://github.com/google/gson).

[!INCLUDE [Quickstart note about code repository](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>Ficheiro JSON de expressões de exemplo

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Criar código de início rápido

1. Adicione as dependências de Java ao ficheiro chamado `AddUtterances.java`.

   [!code-java[Java Dependencies](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=23-26 "Java Dependencies")]

2. Crie a classe `AddUtterances`. Esta classe contém todos os fragmentos de código que se seguem.

    ```Java
    public class AddUtterances {
        // Insert code here
    }
    ```

3. Adicione as constantes do LUIS à classe. Copie o código seguinte e altere para a sua chave de criação, ID da aplicação e ID da versão.

   [!code-java[LUIS-based IDs](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=33-44 "LUIS-based IDs")]

4. Adicione o método para chamar a API LUIS à classe AddUtterances. 

   [!code-java[HTTP request to LUIS](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=46-168 "HTTP request to LUIS")]

5. Adicione o método para a resposta HTTP s partir da API LUIS para a classe AddUtterances.

   [!code-java[HTTP response from LUIS](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=170-202 "HTTP response from LUIS")]

6. Adicione o processamento da exceção à classe AddUtterances. 

   [!code-java[Exception Handling](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=205-243 "Exception Handling")]

7. Adicione a função principal à classe AddUtterances.

   [!code-java[Add main function](~/samples-luis/documentation-samples/quickstarts/change-model/java/AddUtterances.java?range=245-278 "Add main function")]

## <a name="build-code"></a>Compilar código

Compilar AddUtterance com as dependências

```CMD
> javac -classpath gson-2.8.2.jar AddUtterances.java
```

## <a name="run-code"></a>Executar código
Chamar `AddUtterance` sem argumentos adiciona as expressões do LUIS à aplicação, sem prepará-la.

```CMD
> java -classpath .;gson-2.8.2.jar AddUtterances
```

Esta linha de comandos apresenta os resultados da chamada à API de expressões a adicionar. 

[!INCLUDE [Quickstart response from API calls](../../../includes/cognitive-services-luis-qs-change-model-json-results.md)]

## <a name="clean-up-resources"></a>Limpar recursos
Quando tiver terminado o início rápido, remova todos os ficheiros criados neste início rápido. 

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"] 
> [Criar uma aplicação LUIS programaticamente](luis-tutorial-node-import-utterances-csv.md) 