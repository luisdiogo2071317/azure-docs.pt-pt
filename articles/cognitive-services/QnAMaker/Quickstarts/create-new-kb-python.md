---
title: 'Início rápido: Criar base de dados de conhecimento - REST, Python - QnA Maker'
titlesuffix: Azure Cognitive Services
description: Este início rápido baseado em REST do Python descreve a criação programática de uma base de dados de conhecimento do Criador de FAQ de exemplo, que será apresentada no Dashboard do Azure da sua conta da API dos Serviços Cognitivos.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 01/24/2019
ms.author: diberry
ms.openlocfilehash: 766fe32736ae0d8ea23805ea017d93b32119636a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873126"
---
# <a name="quickstart-create-a-knowledge-base-in-qna-maker-using-python"></a>Início rápido: Criar uma base de dados de conhecimento no QnA Maker com Python

Este início rápido descreve a criação e publicação, através de programação, de uma base de dados de conhecimento do Criador de FAQ. O Criador de FAQ extrai automaticamente perguntas e respostas de conteúdos semiestruturados, como FAQs, a partir de [origens de dados](../Concepts/data-sources-supported.md). O modelo da base de dados de conhecimento é definido no JSON enviado no corpo do pedido da API. 

Este início rápido chama as APIs do Criador de FAQ:
* [Criar KB](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
* [Obter Detalhes da Operação](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/operations_getoperationdetails)

## <a name="prerequisites"></a>Pré-requisitos

* [Python 3.7](https://www.python.org/downloads/)
* Tem de ter um serviço Criador de FAQ. Para obter a sua chave, selecione Chaves em Gestão de Recursos no seu dashboard.

[!INCLUDE [Code is available in Azure-Samples GitHub repo](../../../../includes/cognitive-services-qnamaker-python-repo-note.md)]

## <a name="create-a-knowledge-base-python-file"></a>Criar um ficheiro do Python de base de dados de conhecimento

Crie um ficheiro com o nome `create-new-knowledge-base-3x.py`.

## <a name="add-the-required-dependencies"></a>Adicionar as dependências necessárias

Na parte superior do `create-new-knowledge-base-3x.py`, adicione as seguintes linhas para adicionar as dependências necessárias ao projeto:

[!code-python[Add the required dependencies](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=1-1 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Adicionar as constantes necessárias
Depois das dependências necessárias anteriores, adicione as constantes necessárias para aceder ao Criador de FAQ. Substitua o valor da variável `subscriptionKey` pela sua chave do Criador de FAQ.

[!code-python[Add the required constants](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=5-13 "Add the required constants")]

## <a name="add-the-kb-model-definition"></a>Adicionar a definição de modelo de KB

Depois das constantes, adicione a seguinte definição de modelo de KB. Este modelo converte-se numa cadeia de carateres após a definição.

[!code-python[Add the KB model definition](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=15-41 "Add the KB model definition")]

## <a name="add-supporting-function"></a>Adicionar função de suporte

Adicione a função seguinte para imprimir JSON num formato legível:

[!code-python[Add supporting function](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=43-45 "Add supporting function")]

## <a name="add-function-to-create-kb"></a>Adicionar função para criar KB

Adicione a seguinte função para fazer um pedido POST de HTTP para criar a base de dados de conhecimento. Esta chamada à API devolve uma resposta JSON, que inclui o ID da operação no campo do cabeçalho **Localização**. Utilize o ID da operação para determinar se a KB for criada com êxito. A `Ocp-Apim-Subscription-Key` é a chave de serviço do Criador de FAQ utilizada para autenticação. 

[!code-python[Add function to create KB](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=48-59 "Add function to create KB")]

Esta chamada à API devolve uma resposta JSON, que inclui o ID da operação. Utilize o ID da operação para determinar se a KB for criada com êxito. 

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:19:01Z",
  "lastActionTimestamp": "2018-09-26T05:19:01Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "8dfb6a82-ae58-4bcb-95b7-d1239ae25681"
}
```

## <a name="add-function-to-check-creation-status"></a>Adicionar função para verificar o estado de criação

A seguinte função verifica o estado de criação a enviar no ID de operação no fim da rota do URL. A chamada para `check_status` está dentro do ciclo principal _enquanto_.

[!code-python[Add function to check creation status](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=61-67 "Add function to check creation status")]

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

## <a name="add-main-code-block"></a>Adicionar bloco de código principal
O seguinte ciclo consulta o estado da operação de criação periodicamente até a operação estar concluída. 

[!code-python[Add main code block](~/samples-qnamaker-python/documentation-samples/quickstarts/create-knowledge-base/create-new-knowledge-base-3x.py?range=70-96 "Add main code block")]

## <a name="build-and-run-the-program"></a>Compilar e executar o programa

Introduza o comando seguinte numa linha de comandos para executar o programa. Enviará o pedido à API do Criador de FAQ para criar a KB e, em seguida, irá consultar para obter os resultados a cada 30 segundos. Cada resposta é impressa na janela da consola.

```bash
python create-new-knowledge-base-3x.py
```

Assim que a sua base de dados de conhecimento é criada, pode visualizá-la no seu Portal do Criador de FAQ, na página [My knowledge bases](https://www.qnamaker.ai/Home/MyServices) (As minhas bases de dados de conhecimento). Selecione o nome da sua base de dados de conhecimento, por exemplo, FAQ do Criador de FAQ, para a visualizar.

[!INCLUDE [Clean up files and KB](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência à API REST do Criador de FAQ](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
