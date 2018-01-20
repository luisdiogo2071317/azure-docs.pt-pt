---
title: Conector de GitHub no Azure Logic Apps | Microsoft Docs
description: "Crie aplicações lógicas com o App service do Azure. GitHub é um repositório de Git baseada na web que aloja o serviço. Oferece todos os a revisão distribuída origem e de controlo de código (SCM) funcionalidade de gestão de Git, bem como adicionar as suas próprias funcionalidades."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 8f873e6c-f4c0-4c2e-a5bd-2e953efe5e2b
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: c9120babaa5f6da4f33bd60ba27434e24cb2f45e
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-github-connector"></a>Começar a utilizar o conector do GitHub
GitHub é um repositório de Git baseada na web que aloja o serviço. Oferece todos os a revisão distribuída origem e de controlo de código (SCM) funcionalidade de gestão de Git, bem como adicionar as suas próprias funcionalidades.

Pode começar por criar uma aplicação lógica agora, consulte [criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-github"></a>Criar uma ligação ao GitHub
Para criar aplicações lógicas com o GitHub, primeiro tem de criar um **ligação** , em seguida, forneça os detalhes para as seguintes propriedades: 

| Propriedade | Necessário | Descrição |
| --- | --- | --- |
| Token |Sim |Fornecer Credenciais do GitHub |

Depois de criar a ligação, pode utilizá-lo para executar as ações e escutar os acionadores descritos neste artigo. 

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>Detalhes específicos do conector

Ver todos os acionadores e ações definidas no swagger e consulte também os limites no [detalhes do conector](/connectors/github/).

## <a name="more-connectors"></a>Mais conectores
Volte à [lista APIs](apis-list.md).