---
title: Desenvolver aplicações para a pilha do Azure | Microsoft Docs
description: Considerações de desenvolvimento de aplicações de fazer o protótipo na pilha do Azure
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: d3ebc6b1-0ffe-4d3e-ba4a-388239d6cdc3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: a1b5a90ca40ce2b19186220344b22ec0ae77e34b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34199199"
---
# <a name="develop-for-azure-stack"></a>Desenvolver para o Azure Stack

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode começar hoje, a desenvolver aplicações, mesmo se não tiver acesso a um ambiente de pilha do Azure. Porque a pilha do Azure fornece serviços do Microsoft Azure que executam no seu centro de dados, pode utilizar as ferramentas e processos semelhantes para desenvolver em relação a pilha do Azure, tal como faria com o Azure. Com alguma preparação e utilizar as orientações nos seguintes tópicos, pode utilizar o Azure para emular um ambiente de pilha do Azure.

* No Azure, pode criar modelos do Azure Resource Manager são implementáveis com pilha do Azure. Consulte [considerações sobre os modelos](azure-stack-develop-templates.md) para obter orientações sobre como desenvolver modelos para garantir a portabilidade.
* Não existem diferenças na disponibilidade do serviço e o controlo de versões do serviço do Azure e pilha do Azure. Pode utilizar o [módulo de política do Azure pilha](azure-stack-policy-module.md) para restringir os tipos de recursos e a disponibilidade do serviço do Azure para o que está disponível na pilha do Azure. Restrições serviços assegura que as aplicações baseiam-se nos serviços disponíveis para a pilha do Azure.
* O [modelos de início rápido do Azure pilha](https://github.com/Azure/AzureStack-QuickStart-Templates) são exemplos de cenário comum que mostram como desenvolver modelos que podem ser implementados para o Azure e pilha do Azure.
