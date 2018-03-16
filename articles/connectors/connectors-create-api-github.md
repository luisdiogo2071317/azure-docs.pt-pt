---
title: Ligar ao GitHub com Azure Logic Apps | Microsoft Docs
description: Automatizar fluxos de trabalho para o GitHub com Azure Logic Apps
services: logic-apps
documentationcenter: 
author: ecfan
manager: anneta
editor: 
tags: connectors
ms.assetid: 8f873e6c-f4c0-4c2e-a5bd-2e953efe5e2b
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 03/02/2018
ms.author: estfan; ladocs
ms.openlocfilehash: 830c3e6b5462c5d2d917388c0d0924e444342f93
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
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