---
title: Fluxos de trabalho de moderação de conteúdos a partir da consola de API - Content Moderator
titlesuffix: Azure Cognitive Services
description: Saiba como utilizar fluxos de trabalho de moderação de conteúdos a partir da consola de API.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 02/05/2018
ms.author: sajagtap
ms.openlocfilehash: 4ef8951b30fa7aede08a1af3c834192b5ed18649
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223855"
---
# <a name="workflows-from-the-api-console"></a>Fluxos de trabalho a partir da consola de API

Utilize o [operações de fluxo de trabalho](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) no Azure Content Moderator para criar ou atualizar um fluxo de trabalho ou para obter detalhes de fluxo de trabalho com a API de revisão. Pode definir expressões simples, complexas e até mesmo aninhadas para os fluxos de trabalho ao utilizar esta API. Os fluxos de trabalho são apresentados na ferramenta de revisão para a sua equipa a utilizar. Os fluxos de trabalho também são utilizados por operações de tarefa a API de revisão.

## <a name="prerequisites"></a>Pré-requisitos

1. Vá para o [ferramenta de revisão](https://contentmoderator.cognitive.microsoft.com/). Inscreva-se se ainda não tiver feito isso ainda. 
2. Na ferramenta de revisão, sob **configurações**, selecione a **fluxos de trabalho** separador, como mostra a ferramenta de revisão [tutorial de fluxo de trabalho](Review-Tool-User-Guide/Workflows.md).

### <a name="browse-to-the-workflows-screen"></a>Navegue para a tela de fluxos de trabalho

No dashboard do Content Moderator, selecione **revisão** > **definições** > **fluxos de trabalho**. Verá um fluxo de trabalho predefinida.

  ![Fluxo de trabalho predefinida](images/default-workflow-listed.PNG)

### <a name="get-the-json-definition-of-the-default-workflow"></a>Obter a definição de JSON do fluxo de trabalho predefinida

Selecione o **edite** opção para seu fluxo de trabalho e, em seguida, selecione a **JSON** separador. Verá a seguinte expressão de JSON:

    {
        "Type": "Logic",
        "If": {
            "ConnectorName": "moderator",
            "OutputName": "isAdult",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition"
            },
        "Then": {
        "Perform": [
        {
            "Name": "createreview",
            "CallbackEndpoint": null,
            "Tags": []
        }
        ],
        "Type": "Actions"
        }
    }

## <a name="get-workflow-details"></a>Obter os detalhes de fluxo de trabalho

Utilize o **fluxo de trabalho - Get** operação para obter detalhes de fluxo de trabalho padrão existente.

Na ferramenta de revisão, vá para o [credenciais](Review-Tool-User-Guide/credentials.md#the-review-tool) secção.

### <a name="browse-to-the-api-reference"></a>Navegue para a referência de API

1. Na **credenciais** visualizar, selecione [referência da API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59). 
2. Quando o **fluxo de trabalho - criar ou atualizar** é aberta a página, vá para o [fluxo de trabalho - Get](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b44b3f9b0711b43c4c58) referência.

### <a name="select-your-region"></a>Selecione a sua região

Para **consola de teste de API aberta**, selecione a região que melhor descreve sua localização.

  ![Fluxo de trabalho - seleção de região de Get](images/test-drive-region.png)

  O **fluxo de trabalho - Get** é aberta a consola de API.

### <a name="enter-parameters"></a>Introduzir parâmetros

Introduza os valores para **equipe**, **workflowname**, e **Ocp-Apim-Subscription-Key** (a chave de subscrição):

- **equipe**: O ID da equipa que criou quando configurou seu [rever a conta de ferramenta](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: O nome do seu fluxo de trabalho. Utilize `default`.
- **OCP-Apim-Subscription-Key**: localizado no **definições** separador. Para obter mais informações, consulte [descrição geral](overview.md).

  ![Obter parâmetros de consulta e cabeçalhos](images/workflow-get-default.PNG)

### <a name="submit-your-request"></a>Submeter o pedido
  
Selecione **Enviar**. Se a operação for bem-sucedida, o **estado de resposta** é `200 OK`e o **conteúdo da resposta** caixa apresenta o seguinte fluxo de trabalho JSON:

    {
        "Name": "default",
        "Description": "Default",
        "Type": "Image",
        "Expression": {
        "If": {
            "ConnectorName": "moderator",
            "OutputName": "isadult",
            "Operator": "eq",
            "Value": "true",
            "AlternateInput": null,
            "Type": "Condition"
            },
        "Then": {
            "Perform": [{
                "Name": "createreview",
                "Subteam": null,
                "CallbackEndpoint": null,
                "Tags": []
            }],
            "Type": "Actions"
            },
            "Else": null,
            "Type": "Logic"
            }
    }


## <a name="create-a-workflow"></a>Criar um fluxo de trabalho

Na ferramenta de revisão, vá para o [credenciais](Review-Tool-User-Guide/credentials.md#the-review-tool) secção.

### <a name="browse-to-the-api-reference"></a>Navegue para a referência de API

Na **credenciais** visualizar, selecione [referência da API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59). O **fluxo de trabalho - criar ou atualizar** é aberta a página.

### <a name="select-your-region"></a>Selecione a sua região

Para **consola de teste de API aberta**, selecione a região que melhor descreve sua localização.

  ![Fluxo de trabalho - seleção de região de página de atualização ou de criar](images/test-drive-region.png)

  O **fluxo de trabalho - criar ou atualizar** é aberta a consola de API.

### <a name="enter-parameters"></a>Introduzir parâmetros

Introduza os valores para **equipe**, **workflowname**, e **Ocp-Apim-Subscription-Key** (a chave de subscrição):

- **equipe**: O ID da equipa que criou quando configurou seu [rever a conta de ferramenta](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: O nome do seu novo fluxo de trabalho.
- **OCP-Apim-Subscription-Key**: localizado no **definições** separador. Para obter mais informações, consulte [descrição geral](overview.md).

  ![Fluxo de trabalho - criar parâmetros de consulta de consola ou de atualização e cabeçalhos](images/workflow-console-parameters.PNG)

### <a name="enter-the-workflow-definition"></a>Introduza a definição de fluxo de trabalho

1. Editar a **corpo do pedido** caixa para introduzir o pedido do JSON com detalhes para **Descrição** e **tipo** (imagem ou texto). 
2. Para **expressão**, copie a expressão de fluxo de trabalho padrão da secção anterior, conforme mostrado aqui:

        {
            "Description": "Default workflow from API console",
            "Type": "Image",
            "Expression": 
                // Copy the default workflow expression from the preceding section
        }

    O corpo do pedido é parecido com o seguinte pedido JSON:

        {
            "Description": "Default workflow from API console",
            "Type": "Image",
            "Expression": {
                "Type": "Logic",
                "If": {
                    "ConnectorName": "moderator",
                    "OutputName": "isAdult",
                    "Operator": "eq",
                    "Value": "true",
                    "Type": "Condition"
                    },
                "Then": {
                "Perform": [
                {
                    "Name": "createreview",
                    "CallbackEndpoint": null,
                    "Tags": [ ]
                }
                ],
                "Type": "Actions"
                }
            }
        }
 
### <a name="submit-your-request"></a>Submeter o pedido
  
Selecione **Enviar**. Se a operação for bem-sucedida, o **estado de resposta** é `200 OK`e o **conteúdo da resposta** caixa apresenta `true`.

### <a name="check-out-the-new-workflow"></a>Confira o novo fluxo de trabalho

Na ferramenta de revisão, selecione **revisão** > **definições** > **fluxos de trabalho**. Seu novo fluxo de trabalho é apresentada e está pronto a utilizar.

  ![Lista de ferramentas de revisão de fluxos de trabalho](images/workflow-console-new-workflow.PNG)
  
### <a name="review-your-new-workflow-details"></a>Reveja os detalhes do fluxo de trabalho nova

1. Selecione o **edite** opção para seu fluxo de trabalho e, em seguida, selecione a **Designer** e **JSON** separadores.

   ![Separador de Designer para um fluxo de trabalho selecionado](images/workflow-console-new-workflow-designer.PNG)

2. Para ver a vista JSON do fluxo de trabalho, selecione o **JSON** separador.

## <a name="next-steps"></a>Passos Seguintes

* Para obter exemplos de fluxo de trabalho mais complexos, consulte a [descrição geral de fluxos de trabalho](workflow-api.md).
* Saiba como utilizar fluxos de trabalho com [tarefas de moderação de conteúdos](try-review-api-job.md).
