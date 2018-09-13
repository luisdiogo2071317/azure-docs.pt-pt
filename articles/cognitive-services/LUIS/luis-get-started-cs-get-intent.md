---
title: Analisar texto de linguagem natural em Language Understanding (LUIS) com C# – Serviços Cognitivos do Azure | Microsoft Docs
description: Neste guia de introdução, irá aprender a utilizar uma aplicação LUIS pública disponível para determinar a intenção de um utilizador a partir do texto de uma conversação. Com C#, envie a intenção do utilizador como texto para o ponto final de predição HTTP da aplicação pública. No ponto final, o LUIS aplica o modelo da aplicação pública para analisar o significado do texto de linguagem natural e assim determinar a intenção geral e extrair os dados relevantes para o domínio de requerente da aplicação.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: 676546a215bbb8964f1cb2d26ae0fb9fd2ed9289
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "43771659"
---
# <a name="quickstart-analyze-text-using-c"></a>Guia de introdução: analisar texto com C#

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Pré-requisitos

* [Visual Studio Community – edição de 2017](https://visualstudio.microsoft.com/vs/community/)
* Linguagem de programação C# (incluída com o VS Community 2017)
* ID da aplicação pública: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Obter chave LUIS

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Analisar texto com um browser

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-c"></a>Analisar texto com C# 

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

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Limpar recursos

Quando terminar este guia de introdução, feche o projeto do Visual Studio e remova o diretório do projeto do sistema de ficheiros. 

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Adicionar expressões e formação com C#](luis-get-started-cs-add-utterance.md)