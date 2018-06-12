---
title: Ligar ao GitHub - as do Azure Logic Apps | Microsoft Docs
description: Monitorizar os eventos do GitHub com as APIs REST do GitHub e Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 03/02/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: ce567dc631c3a147b795eb2355a4961faa8881d6
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295817"
---
# <a name="connect-to-github"></a>Ligar ao GitHub

GitHub é um baseada na web Git repositório alojamento serviço que oferece todas o controlo de revisão distribuída e funcionalidade de gestão (SCM) de código de origem do Git e outras funcionalidades.

Para começar a utilizar com o conector do GitHub, [criar uma aplicação lógica pela primeira vez](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-github"></a>Criar uma ligação ao GitHub

Para utilizar o conector do GitHub numa aplicação lógica, primeiro tem de criar um *ligação* e, em seguida, forneça detalhes para estas propriedades: 

| Propriedade | Necessário | Descrição | 
| -------- | -------- | ----------- | 
| Certificado de | Sim | Fornece as credenciais do GitHub. |

Depois de criar a ligação, pode executar as ações e escutar os acionadores descritos neste artigo.

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>Detalhes específicos do conector

Para acionadores e ações definidas no Swagger e quaisquer limites, consulte o [detalhes do conector](/connectors/github/).

## <a name="find-more-connectors"></a>Localizar conectores mais

* Reveja o [lista de conectores](apis-list.md).