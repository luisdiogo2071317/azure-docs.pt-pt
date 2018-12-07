---
title: 'Início rápido: obter a intenção-C#'
titleSuffix: Language Understanding - Azure Cognitive Services
description: Neste guia de introdução, irá aprender a utilizar uma aplicação LUIS pública disponível para determinar a intenção de um utilizador a partir do texto de uma conversação. Com C#, envie a intenção do utilizador como texto para o ponto final de predição HTTP da aplicação pública. No ponto final, o LUIS aplica o modelo da aplicação pública para analisar o significado do texto de linguagem natural e assim determinar a intenção geral e extrair os dados relevantes para o domínio de requerente da aplicação.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: c498554bea0722f81d5dd52d77c0bc88d16e2332
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2018
ms.locfileid: "53015070"
---
# <a name="quickstart-get-intent-using-c"></a>Guia de Início Rápido: Obter a intenção com C#

[!INCLUDE [Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Pré-requisitos

* [Visual Studio Community – edição de 2017](https://visualstudio.microsoft.com/vs/community/)
* Linguagem de programação C# (incluída com o VS Community 2017)
* ID da aplicação pública: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Obter chave LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="get-intent-with-browser"></a>Obter a intenção com o browser

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="get-intent-programmatically"></a>Obter a intenção através de programação

Utilize a linguagem C# para consultar o ponto final de predição [API](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78) GET e obter os mesmos resultados que viu na janela do browser na secção anterior. 

1. Crie uma nova aplicação de consola no Visual Studio. 

    ![Acesso ao menu de definições de utilizador do LUIS](media/luis-get-started-cs-get-intent/visual-studio-console-app.png)

2. No projeto do Visual Studio, no Explorador de Soluções, selecione **Adicionar referência** e, em seguida, selecione **System.Web** no separador Assemblagens.

    ![Acesso ao menu de definições de utilizador do LUIS](media/luis-get-started-cs-get-intent/add-system-dot-web-to-project.png)

3. Substitua Program.cs pelo seguinte código:
    
   [!code-csharp[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/csharp/Program.cs)]

4. Substitua o valor de `YOUR_KEY` pela sua chave do LUIS.

5. Compile e execute a aplicação da consola. Apresenta o mesmo JSON que viu anteriormente na janela do browser.

    ![A janela da consola apresenta o resultado JSON do LUIS](./media/luis-get-started-cs-get-intent/console-turn-on.png)

## <a name="luis-keys"></a>Chaves LUIS

[!INCLUDE [Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Limpar recursos

Quando terminar este guia de introdução, feche o projeto do Visual Studio e remova o diretório do projeto do sistema de ficheiros. 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Adicionar expressões e formação com C#](luis-get-started-cs-add-utterance.md)