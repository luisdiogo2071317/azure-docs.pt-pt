---
title: Ligar ao GitHub com Azure Logic Apps | Microsoft Docs
description: Automatizar fluxos de trabalho para o GitHub com Azure Logic Apps
services: logic-apps
documentationcenter: 
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
ms.date: 03/02/2018
ms.author: mandia; ladocs
ms.openlocfilehash: cd7cd3babbfb7efc5917d3a7ec5b9d10112ba791
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
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