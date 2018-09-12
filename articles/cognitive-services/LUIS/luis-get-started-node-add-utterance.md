---
title: Início rápido - alterar o modelo e treinar a aplicação LUIS com Node.js - Serviços Cognitivos do Azure | Microsoft Docs
description: Neste início rápido do Node.js, adicione expressões de exemplo a uma aplicação de Automatização de Casa e treine a aplicação. As expressões de exemplo são texto de utilizador de conversação mapeado para uma intenção. Ao conceder expressões de exemplo para intenções, ensina o LUIS sobre quais são os tipos de texto fornecido pelo utilizador que pertencem a que intenção.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/24/2018
ms.author: diberry
ms.openlocfilehash: fbd8b467fa3894d9cf58e1c8cb78ee00ebd0965e
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44156858"
---
# <a name="quickstart-change-model-using-nodejs"></a>Início rápido: Alterar o modelo com Node.js

[!INCLUDE [Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* [**Node.js**](https://nodejs.org/en/download/) mais recente com NPM.
* Dependências NPM para este artigo: [**request**](https://www.npmjs.com/package/request), [**request-promise**](https://www.npmjs.com/package/request-promise), [**fs-extra**](https://www.npmjs.com/package/fs-extra).  
* [Visual Studio Code](https://code.visualstudio.com/).

[!INCLUDE [Code is available in LUIS-Samples Github repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>Ficheiro JSON de expressões de exemplo

[!INCLUDE [Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]

## <a name="create-quickstart-code"></a>Criar código de início rápido 

Adicione as dependências NPM ao ficheiro chamado `add-utterances.js`.

   [!code-javascript[NPM Dependencies](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=8-11 "NPM Dependencies")]

Adicione as constantes do LUIS ao ficheiro. Copie o código seguinte e altere para a sua chave de criação, ID da aplicação e ID da versão.

   [!code-javascript[LUIS key and IDs](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=13-22 "LUIS key and IDs")]

Adicione o nome e a localização do ficheiro de carregamento com as suas expressões. 

   [!code-javascript[Add upload file](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=24-26 "Add upload file")]

Adicione o método e o objeto para a função `addUtterance`.

   [!code-javascript[Add configuration information for adding utterance](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=28-67 "Add configuration information for adding utterance")]

Adicione o método e o objeto para a função `train`.

   [!code-javascript[Add configuration information for training LUIS](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=69-101 "Add configuration information for training LUIS")]

Adicione a função `sendUtteranceToApi` para enviar e receber chamadas HTTP. 

   [!code-javascript[Send the HTTP request](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=103-119 "Send the HTTP request")]

Adicione o código **principal** que escolhe a ação.

   [!code-javascript[Main function](~/samples-luis/documentation-samples/quickstarts/change-model/node/add-utterances.js?range=121-143 "Main function")]

### <a name="install-dependencies"></a>Instalar dependências

Crie o ficheiro `package.json` com o seguinte texto:

   [!code-json[Package.json](~/samples-luis/documentation-samples/quickstarts/change-model/node/package.json "Package.json")]

Na linha de comandos, no diretório que tem o package.json, instale as dependências com NPM: `npm install`.

## <a name="run-code"></a>Executar código

Execute a aplicação a partir de uma linha de comandos com Node.js.

Chamar `npm start`adiciona as expressões, treinos e obtém o estado do treino.

```CMD
> npm start 
```

Esta linha de comandos apresenta os resultados da chamada à API de expressões a adicionar. 

[!INCLUDE [Quickstart response from API calls](../../../includes/cognitive-services-luis-qs-change-model-json-results.md)]


## <a name="clean-up-resources"></a>Limpar recursos

Quando tiver terminado o início rápido, remova todos os ficheiros criados neste início rápido. 

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"] 
> [Criar uma aplicação LUIS programaticamente](luis-tutorial-node-import-utterances-csv.md)