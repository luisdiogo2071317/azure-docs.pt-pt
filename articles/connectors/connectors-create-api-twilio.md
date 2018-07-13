---
title: Adicionar o conector Twilio nas suas aplicações lógicas do Azure | Documentos da Microsoft
description: Descrição geral do conector do Twilio com parâmetros de REST API
services: logic-apps
documentationcenter: ''
author: ecfan
manager: jeconnoc
editor: ''
tags: connectors
ms.assetid: 43116187-4a2f-42e5-9852-a0d62f08c5fc
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/19/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 8bcf69a7c8e04cb45d795fd0d6f20d477c15865d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38652010"
---
# <a name="get-started-with-the-twilio-connector"></a>Começar com o conector do Twilio
Ligue ao Twilio para enviar e receber mensagens SMS, MMS e IP globais. Com o Twilio, pode:

* Crie o seu fluxo de negócios com base nos dados que obtém do Twilio. 
* Utilize as ações que obtém uma mensagem, listar mensagens e muito mais. Estas ações obtém uma resposta e, em seguida, disponibilizá-a saída para outras ações. Por exemplo, quando receber uma nova mensagem do Twilio, pode efetuar esta mensagem e utilizá-lo um fluxo de trabalho do Service Bus. 

Comece por criar uma aplicação de lógica; ver [criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-twilio"></a>Criar uma ligação ao Twilio
Ao adicionar este conector às suas aplicações lógicas, introduza os seguintes valores de Twilio:

| Propriedade | Necessário | Descrição |
| --- | --- | --- |
| ID da Conta |Sim |Introduza o seu ID de conta do Twilio |
| Token de Acesso |Sim |Introduza o seu token de acesso do Twilio |

> [!INCLUDE [Steps to create a connection to Twilio](../../includes/connectors-create-api-twilio.md)]
> 
> 

Se não tiver um token de acesso do Twilio, consulte [identidade de utilizador e os Tokens de acesso](https://www.twilio.com/docs/api/chat/guides/identity).

## <a name="connector-specific-details"></a>Detalhes específicos do conector

Ver os acionadores e as ações definidas no swagger e também ver quaisquer limites na [detalhes do conector](/connectors/twilio/).

## <a name="more-connectors"></a>Mais conectores
Volte para o [lista APIs](apis-list.md).