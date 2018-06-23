---
title: Moderator conteúdo do Azure - conteúdo fluxos de trabalho de moderação interrupção partir da consola de API | Microsoft Docs
description: Saiba como utilizar os fluxos de trabalho de conteúdo moderação de interrupção da consola de API.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 02/05/2018
ms.author: sajagtap
ms.openlocfilehash: 700b2bea5e902141659266a94d61ceb810c1b802
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351775"
---
# <a name="workflows-from-the-api-console"></a>Fluxos de trabalho da consola de API

Utilize o [operações de fluxo de trabalho](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) no Moderator conteúdo do Azure para criar ou atualizar um fluxo de trabalho ou obter os detalhes de fluxo de trabalho, utilizando a API de revisão. Pode definir expressões mesmo aninhadas, simples e complexas para os fluxos de trabalho ao utilizar esta API. Os fluxos de trabalho são apresentadas na ferramenta de revisão para a sua equipa utilizar. Os fluxos de trabalho também são utilizados por operações de tarefa a API de revisão.

## <a name="prerequisites"></a>Pré-requisitos

1. Vá para o [ferramenta revisão](https://contentmoderator.cognitive.microsoft.com/). Inscrever-se se ainda não o fez, ainda. 
2. Na ferramenta de revisão, sob **definições**, selecione o **fluxos de trabalho** separador, conforme mostrado na ferramenta de revisão [tutorial de fluxo de trabalho](Review-Tool-User-Guide/Workflows.md).

### <a name="browse-to-the-workflows-screen"></a>Navegue para o ecrã de fluxos de trabalho

No dashboard do Moderator conteúdo, selecione **revisão** > **definições** > **fluxos de trabalho**. Verá um fluxo de trabalho predefinido.

  ![Fluxo de trabalho predefinido](images/default-workflow-listed.PNG)

### <a name="get-the-json-definition-of-the-default-workflow"></a>Obter a definição de JSON do fluxo de trabalho predefinido

Selecione o **editar** opção ao fluxo de trabalho e, em seguida, selecione o **JSON** separador. Consulte a seguinte expressão de JSON:

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

Utilize o **fluxo de trabalho - Get** operação para obter detalhes do fluxo de trabalho predefinido existente.

Na ferramenta de revisão, vá para o [credenciais](Review-Tool-User-Guide/credentials.md#the-review-tool) secção.

### <a name="browse-to-the-api-reference"></a>Navegue para a referência de API

1. No **credenciais** visualizar, selecione [referência da API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59). 
2. Quando o **fluxo de trabalho - criar ou atualizar** é aberta a página, visite o [fluxo de trabalho - Get](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b44b3f9b0711b43c4c58) referência.

### <a name="select-your-region"></a>Selecione a região

Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização.

  ![Fluxo de trabalho - seleção de região Get](images/test-drive-region.png)

  O **fluxo de trabalho - Get** é aberta a consola de API.

### <a name="enter-parameters"></a>Introduzir parâmetros

Introduza os valores para **equipa**, **workflowname**, e **Ocp-Apim-Subscription-Key** (a chave de subscrição):

- **equipa**: O ID de equipa que criou quando configurou o [rever conta ferramenta](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: O nome do fluxo de trabalho. Utilize `default`.
- **OCP-Apim-Subscription-Key**: localizado no **definições** separador. Para obter mais informações, consulte [descrição geral](overview.md).

  ![Obter parâmetros de consulta e nos cabeçalhos](images/workflow-get-default.PNG)

### <a name="submit-your-request"></a>Submeter o pedido
  
Selecione **Enviar**. Se a operação for bem sucedida, o **estado de resposta** é `200 OK`e o **conteúdo da resposta** caixa apresenta o seguinte fluxo de trabalho JSON:

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

No **credenciais** visualizar, selecione [referência da API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59). O **fluxo de trabalho - criar ou atualizar** é aberta a página.

### <a name="select-your-region"></a>Selecione a região

Para **consola de teste de Open API**, selecione a região que melhor coincida descreve a localização.

  ![Fluxo de trabalho - criar seleção de região de página ou Update](images/test-drive-region.png)

  O **fluxo de trabalho - criar ou atualizar** é aberta a consola de API.

### <a name="enter-parameters"></a>Introduzir parâmetros

Introduza os valores para **equipa**, **workflowname**, e **Ocp-Apim-Subscription-Key** (a chave de subscrição):

- **equipa**: O ID de equipa que criou quando configurou o [rever conta ferramenta](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: O nome do novo fluxo de trabalho.
- **OCP-Apim-Subscription-Key**: localizado no **definições** separador. Para obter mais informações, consulte [descrição geral](overview.md).

  ![Fluxo de trabalho - criar parâmetros de consulta ou atualização consola e nos cabeçalhos](images/workflow-console-parameters.PNG)

### <a name="enter-the-workflow-definition"></a>Introduza a definição de fluxo de trabalho

1. Editar o **corpo do pedido** caixa para introduzir o pedido do JSON com detalhes para **Descrição** e **tipo** (imagem ou texto). 
2. Para **expressão**, copie a expressão de fluxo de trabalho predefinida da secção anterior, conforme mostrado aqui:

        {
            "Description": "Default workflow from API console",
            "Type": "Image",
            "Expression": 
                // Copy the default workflow expression from the preceding section
        }

    O corpo do pedido aspeto pedido JSON seguinte:

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
  
Selecione **Enviar**. Se a operação for bem sucedida, o **estado de resposta** é `200 OK`e o **conteúdo da resposta** caixa apresenta `true`.

### <a name="check-out-the-new-workflow"></a>Veja o novo fluxo de trabalho

Na ferramenta de revisão, selecione **revisão** > **definições** > **fluxos de trabalho**. O novo fluxo de trabalho é apresentado e está pronto a utilizar.

  ![Lista de ferramenta de revisão de fluxos de trabalho](images/workflow-console-new-workflow.PNG)
  
### <a name="review-your-new-workflow-details"></a>Reveja os detalhes novos do fluxo de trabalho

1. Selecione o **editar** opção ao fluxo de trabalho e, em seguida, selecione o **Designer** e **JSON** separadores.

   ![Separador estruturador para um fluxo de trabalho selecionado](images/workflow-console-new-workflow-designer.PNG)

2. Para ver a vista JSON do fluxo de trabalho, selecione o **JSON** separador.

## <a name="next-steps"></a>Passos Seguintes

* Para obter exemplos mais complexos do fluxo de trabalho, consulte o [descrição geral de fluxos de trabalho](workflow-api.md).
* Saiba como utilizar os fluxos de trabalho com [conteúdo tarefas moderação interrupção](try-review-api-job.md).
