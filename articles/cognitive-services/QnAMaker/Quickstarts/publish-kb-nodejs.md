---
title: 'Início Rápido: publicar a base de dados de conhecimento – REST, Node.js – Criador de FAQ'
titleSuffix: Azure Cognitive Services
description: Este início rápido descreve a publicação, através de programação, da sua base de dados de conhecimento (KB). A publicação emite a versão mais recente da base de dados de conhecimento para um índice dedicado do Azure Search e cria um ponto final que pode ser chamado na sua aplicação ou chatbot.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 10/19/2018
ms.author: diberry
ms.openlocfilehash: e1e349f4ddbebdd9df38d7f0babf50d726241d4f
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49648739"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-nodejs"></a>Início Rápido: publicar uma base de dados de conhecimento no Criador de FAQ com o Node.js

Este início rápido descreve a publicação, através de programação, da sua base de dados de conhecimento (KB). A publicação emite a versão mais recente da base de dados de conhecimento para um índice dedicado do Azure Search e cria um ponto final que pode ser chamado na sua aplicação ou chatbot.

Este início rápido chama as APIs do Criador de FAQ:
* [Publicar](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe) - esta API não requer quaisquer informações no corpo do pedido.

## <a name="prerequisites"></a>Pré-requisitos

* [Node.js 6+](https://nodejs.org/en/download/)
* Tem de ter um [serviço Criador de FAQ](../How-To/set-up-qnamaker-service-azure.md). Para obter a sua chave, selecione **Chaves** em **Gestão de Recursos** no seu dashboard. 
* O ID da base de dados de conhecimento (KB) do Criador de FAQ encontrado no URL no parâmetro de cadeia de consulta kbid, conforme mostrado abaixo.

    ![ID da base de dados de conhecimento do Criador de FAQ](../media/qnamaker-quickstart-kb/qna-maker-id.png)

Se ainda não tiver uma base de dados de conhecimento, pode criar uma de exemplo para utilizar neste guia de introdução: [Criar uma nova base de dados de conhecimento](create-new-kb-nodejs.md).

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-nodejs-repo-note.md)]

## <a name="create-a-knowledge-base-nodejs-file"></a>Criar um ficheiro Node.js de base de dados de conhecimento

Crie um ficheiro com o nome `publish-knowledge-base.js`.

## <a name="add-required-dependencies"></a>Adicionar as dependências necessárias

Na parte superior do `publish-knowledge-base.js`, adicione as seguintes linhas para adicionar as dependências necessárias ao projeto:

[!code-nodejs[Add the dependencies](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=1-4 "Add the dependencies")]

## <a name="add-required-constants"></a>Adicionar constantes necessárias

Depois das dependências necessárias anteriores, adicione as constantes necessárias para aceder ao Criador de FAQ. Substitua o valor da variável `subscriptionKey` pela sua chave do Criador de FAQ. 

[!code-nodejs[Add required constants](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=10-17 "Add required constants")]

## <a name="add-knowledge-base-id"></a>Adicionar o ID da base de dados de conhecimento

Depois das constantes anteriores, adicione o ID da base de dados de conhecimento e adicione-o ao caminho:

[!code-nodejs[Add knowledge base ID](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=19-23 "Add knowledge base ID")]

## <a name="add-supporting-functions"></a>Adicionar funções de suporte

Em seguida, adicione as funções de suporte seguintes.

1. Adicione a função seguinte para imprimir JSON num formato legível:

   [!code-nodejs[Add supporting functions, step 1](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=25-28 "Add supporting functions, step 1")]

2. Adicione as seguintes funções para gerir a resposta HTTP, de modo a obter o estado da operação de criação:

   [!code-nodejs[Add supporting functions, step 2](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=30-52 "Add supporting functions, step 2")]

## <a name="add-the-publishkb-function-and-main-function"></a>Adicionar a função publish_kb e a função principal

O código seguinte faz um pedido HTTPS à API do Criador de FAQ para publicar uma KB e recebe a resposta:

[!code-nodejs[Add POST request to publish KB](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=54-71 "Add POST request to publish KB")]

[!code-nodejs[Add the publish_kb function](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=73-91 "Add the publish_kb function and main function")]

## <a name="add-the-main-function"></a>Adicionar a função principal

Adicione a seguinte função para gerir o pedido e a resposta:

[!code-nodejs[Add the main function](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/publish-knowledge-base/publish-knowledge-base.js?range=94-97 "Add the main function")]

## <a name="run-the-program"></a>Execute o programa

Compile e execute o programa. O programa enviará automaticamente o pedido à API do Criador de FAQ para publicar a KB e, em seguida, a resposta é impressa na janela da consola.

Assim que a sua base de dados de conhecimento for publicada, pode consultá-la a partir do ponto final com uma aplicação cliente ou um chatbot. 

```bash
node publish-knowledge-base.js
```

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Referência à API REST do Criador de FAQ](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)