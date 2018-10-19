---
title: 'Início Rápido: Atualizar base de dados de conhecimento – REST, Node.js – Criador de FAQ'
titleSuffix: Azure Cognitive Services
description: Este início rápido ajuda-o a atualizar programaticamente uma base de dados de conhecimento do Criador de FAQ existente.  Este JSON permite-lhe atualizar uma BDC através da adição de novas origens de dados, da alteração de origens de dados ou da eliminação de origens de dados.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: diberry
ms.openlocfilehash: 3bbc55b3bb064b2cf4b140a395e99209b71a5ce1
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816237"
---
# <a name="quickstart-update-a-qna-maker-knowledge-base-in-nodejs"></a>Início Rápido: Atualizar uma base de dados de conhecimento do Criador de FAQ em Node.js

Este início rápido ajuda-o a atualizar programaticamente uma base de dados de conhecimento do Criador de FAQ existente.  Este JSON permite-lhe atualizar uma BDC através da adição de novas origens de dados, da alteração de origens de dados ou da eliminação de origens de dados.

Esta API é equivalente a editar e, em seguida, utilizar o botão **Guardar e preparar** no portal do Criador de FAQ.

Este início rápido chama as API do Criador de FAQ:
* [Atualizar](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600) – O modelo da base de dados de conhecimento é definido no JSON enviado no corpo do pedido da API. 
* [Obter Detalhes da Operação](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/operations_getoperationdetails)

## <a name="prerequisites"></a>Pré-requisitos

* [Node.js 6+](https://nodejs.org/en/download/)
* Tem de ter um [serviço Criador de FAQ](../How-To/set-up-qnamaker-service-azure.md). Para obter a sua chave, selecione **Chaves** em **Gestão de Recursos** no seu dashboard. 
* O ID da base de dados de conhecimento (KB) do Criador de FAQ encontrado no URL no parâmetro de cadeia de consulta kbid, conforme mostrado abaixo.

    ![ID da base de dados de conhecimento do Criador de FAQ](../media/qnamaker-quickstart-kb/qna-maker-id.png)

Se ainda não tiver uma base de dados de conhecimento, pode criar uma de exemplo para utilizar neste guia de introdução: [Criar uma nova base de dados de conhecimento](create-new-kb-nodejs.md).

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-nodejs-repo-note.md)]

## <a name="create-a-knowledge-base-nodejs-file"></a>Criar um ficheiro Node.js de base de dados de conhecimento

Crie um ficheiro com o nome `update-knowledge-base.js`.

## <a name="add-the-required-dependencies"></a>Adicionar as dependências necessárias

Na parte superior de `update-knowledge-base.js`, adicione as linhas seguintes para adicionar as dependências necessárias ao projeto:

[!code-nodejs[Add the dependencies](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=1-4 "Add the dependencies")]

## <a name="add-required-constants"></a>Adicionar constantes necessárias
Depois das dependências necessárias anteriores, adicione as constantes necessárias para aceder ao Criador de FAQ. Substitua o valor da variável `subscriptionKey` pela sua chave do Criador de FAQ. 

[!code-nodejs[Add required constants](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=10-17 "Add required constants")]

## <a name="add-knowledge-base-id"></a>Adicionar o ID da base de dados de conhecimento

Depois das constantes anteriores, adicione o ID da base de dados de conhecimento e adicione-o ao caminho:

[!code-nodejs[Add knowledge base ID](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=19-23 "Add knowledge base ID")]

## <a name="add-the-kb-update-model-definition"></a>Adicionar a definição do modelo de atualização da BDC

Depois das constantes, adicione a definição de atualização de BDC seguinte. A definição de atualização tem três secções:

* adicionar
* update
* delete

Cada seção pode ser utilizada no mesmo pedido simples à API. 

[!code-nodejs[Add the KB update definition](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=25-53 "Add the KB update definition")]


## <a name="add-supporting-functions"></a>Adicionar funções de suporte

Em seguida, adicione as funções de suporte seguintes.

1. Adicione a função seguinte para imprimir JSON num formato legível:

   [!code-nodejs[Add supporting functions, step 1](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=55-58 "Add supporting functions, step 1")]

2. Adicione as funções seguintes para gerir a resposta HTTP, de modo a obter o estado da operação de criação:

   [!code-nodejs[Add supporting functions, step 2](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=60-82 "Add supporting functions, step 2")]

## <a name="add-patch-request-to-update-kb"></a>Adicionar pedido de PATCH para atualizar a BDC

Adicione as funções seguintes para fazer um pedido de PATCH HTTP para atualizar a base de dados de conhecimento. A `Ocp-Apim-Subscription-Key` é a chave de serviço do Criador de FAQ utilizada para autenticação.

[!code-nodejs[Add PATCH request to update KB](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=84-111 "Add PATCH request to update KB")]

Esta chamada à API devolve uma resposta JSON, que inclui o ID da operação. O ID da operação é necessário para o estado do pedido, se a operação não tiver sido concluída.

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-10-02T01:23:00Z",
  "lastActionTimestamp": "2018-10-02T01:23:00Z",
  "userId": "335c3841df0b42cdb00f53a49d51a89c",
  "operationId": "e7be3897-88ff-44e5-a06c-01df0e05b78c"
}
```

## <a name="add-get-request-to-determine-operation-status"></a>Adicionar pedido GET para determinar o estado da operação

Verifique o estado da operação.
    
[!code-nodejs[Add GET request to determine operation status](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=113-137 "Add GET request to determine operation status")]

Esta chamada à API devolve uma resposta JSON que inclui o estado da operação: 

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:22:53Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

Repita a chamada até obter êxito ou falhar: 

```JSON
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:23:08Z",
  "resourceLocation": "/knowledgebases/XXX7892b-10cf-47e2-a3ae-e40683adb714",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

## <a name="add-updatekb-method"></a>Método Add update_kb

O método seguinte atualiza a BDC e repete as verificações de estado. Uma vez que a criação da KB pode demorar algum tempo, terá de repetir as chamadas para verificar o estado até que o estado seja com êxito ou falhe.

[!code-nodejs[Add update_kb method](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=139-169 "Add update_kb method")]

## <a name="run-the-program"></a>Execute o programa

Introduza o comando seguinte numa linha de comandos para executar o programa. Este envia o pedido à API do Criador de FAQ para criar a BDC e, em seguida, efetua uma consulta para obter os resultados a cada 30 segundos. Cada resposta é impressa na janela da consola.

```bash
node update-knowledge-base.js
```

Assim que a sua base de dados de conhecimento estiver atualizada, pode visualizá-la no seu Portal do Criador de FAQ, na página [My knowledge bases](https://www.qnamaker.ai/Home/MyServices) (As minhas base de dados de conhecimento). 

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Referência à API REST do Criador de FAQ](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)