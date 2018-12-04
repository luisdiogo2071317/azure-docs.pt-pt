---
title: Actividade de função do Azure na fábrica de dados do Azure | Documentos da Microsoft
description: Saiba como utilizar a atividade de função do Azure para executar uma função do Azure no pipeline do Data Factory
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: douglasl
ms.openlocfilehash: ef93c62a2e2084a43eeda578c889a568d04db4f1
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856417"
---
# <a name="azure-function-activity-in-azure-data-factory"></a>Actividade de função do Azure no Azure Data Factory

A actividade de função do Azure permite-lhe executar [as funções do Azure](../azure-functions/functions-overview.md) no pipeline do Data Factory. Para executar uma função do Azure, terá de criar uma ligação de serviço ligado e uma atividade que especifica a função do Azure que pretende executar.

## <a name="azure-function-linked-service"></a>Serviço ligado de função do Azure

| **Propriedade** | **Descrição** | **Necessário** |
| --- | --- | --- |
| tipo   | A propriedade de tipo deve ser definida como: **AzureFunction** | sim |
| url da aplicação de função | URL da aplicação de função do Azure. O formato é `https://<accountname>.azurewebsites.net`. Este URL é o valor sob **URL** secção ao visualizar a aplicação de funções no portal do Azure  | sim |
| tecla de função | Chave de acesso para a função do Azure. Clique no **gerir** secção para obter a respetiva função e copie-o a **tecla de função** ou o **chave de anfitrião**. Saiba mais aqui: [HTTP de funções do Azure acionadores e enlaces](../azure-functions/functions-bindings-http-webhook.md#authorization-keys) | sim |
|   |   |   |

## <a name="azure-function-activity"></a>Actividade de função do Azure

| **Propriedade**  | **Descrição** | **Valores permitidos** | **Necessário** |
| --- | --- | --- | --- |
| nome  | Nome da atividade no pipeline  | Cadeia | sim |
| tipo  | Tipo de atividade é 'AzureFunctionActivity' | Cadeia | sim |
| Serviço ligado | O serviço de função do Azure ligado para a aplicação de funções do Azure correspondente  | Referência de serviço ligado | sim |
| Nome da função  | Nome da função na aplicação de função do Azure que chama esta atividade | Cadeia | sim |
| método  | Método de REST API para a chamada de função | Cadeia de caracteres de tipos suportados: "GET", "POST", "PUT"   | sim |
| cabeçalho  | Cabeçalhos que são enviados para o pedido. Por exemplo, para definir o idioma e o tipo de um pedido: "cabeçalhos": {"Accept-Language": "en-us", "Content-Type": "application/json"} | Cadeia de caracteres (ou expressão com resultType de cadeia de caracteres) | Não |
| Corpo  | corpo que é enviado com o pedido para o método da api de função  | Cadeia de caracteres (ou expressão com resultType de cadeia de caracteres).   | Necessário para métodos PUT/POST |
|   |   |   | |

Ver o esquema do payload de pedido no [esquema de payload de pedido](control-flow-web-activity.md#request-payload-schema) secção.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre as atividades no Data Factory no [Pipelines e atividades no Azure Data Factory](concepts-pipelines-activities.md).