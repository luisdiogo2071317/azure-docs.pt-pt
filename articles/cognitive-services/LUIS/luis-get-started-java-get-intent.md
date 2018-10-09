---
title: Guia de Início Rápido de Java – prever a intenção – LUIS
titleSuffix: Azure Cognitive Services
description: Neste início rápido, irá aprender a utilizar uma aplicação LUIS pública disponível para determinar a intenção de um utilizador a partir do texto de uma conversação. Com o Java, envie a intenção do utilizador como texto para o ponto final de predição HTTP da aplicação pública. No ponto final, o LUIS aplica o modelo da aplicação pública para analisar o significado do texto de linguagem natural e assim determinar a intenção geral e extrair os dados relevantes para o domínio de requerente da aplicação.
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 03fd376e58062ae9b4593fcaf7904200157c4dc3
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47031429"
---
# <a name="quickstart-get-intent-using-java"></a>Guia de Início Rápido: obter a intenção com Java

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Pré-requisitos

* [JDK SE](http://www.oracle.com/technetwork/java/javase/downloads/index.html) (Kit de Desenvolvimento do Java, Edição Standard)
* [Visual Studio Code](https://code.visualstudio.com/)
* ID da aplicação pública: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Obter chave LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Obter a intenção com o browser

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>Obter a intenção através de programação 

Pode utilizar o Java para aceder aos mesmos resultados que viu na janela do browser no passo anterior. 

1. Copie o seguinte código para criar uma classe num ficheiro denominado `LuisGetRequest.java`:

   [!code-java[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/java/call-endpoint.java)]

2. Substitua o valor da variável `YOUR-KEY` pela sua chave do LUIS.

3. Compile o programa Java com `javac -cp ":lib/*" LuisGetRequest.java`. 

4. Execute a aplicação com `java -cp ":lib/*" LuisGetRequest.java`. Apresenta o mesmo JSON que viu anteriormente na janela do browser.

    ![A janela da consola apresenta o resultado JSON do LUIS](./media/luis-get-started-java-get-intent/console-turn-on.png)
    
## <a name="luis-keys"></a>Chaves LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Limpar recursos

Elimine o ficheiro Java. 

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"]
> [Adicionar expressões](luis-get-started-java-add-utterance.md)