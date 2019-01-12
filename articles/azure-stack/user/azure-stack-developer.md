---
title: Desenvolver aplicações para o Azure Stack | Documentos da Microsoft
description: Considerações de programação para aplicativos de criação de protótipos no Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: d3ebc6b1-0ffe-4d3e-ba4a-388239d6cdc3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: unknown
ms.openlocfilehash: 60336a56437b3d5c5d39d8c1584d91c909ac47ff
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2019
ms.locfileid: "54246199"
---
# <a name="develop-for-azure-stack"></a>Desenvolver para o Azure Stack

*Aplica-se a: Integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Pode começar a desenvolver aplicativos de hoje em dia, mesmo que não tenha acesso a um ambiente do Azure Stack. Porque o Azure Stack oferece serviços Microsoft Azure que executar no seu datacenter, pode utilizar ferramentas e processos semelhantes para desenvolver no Azure Stack, tal como faria com o Azure.

## <a name="development-considerations"></a>Considerações sobre programação

Com alguma preparação e através da orientação nos tópicos seguintes, pode utilizar o Azure para emular um ambiente do Azure Stack.

* No Azure, pode criar modelos do Azure Resource Manager implementáveis no Azure Stack. Ver [considerações sobre os modelos](azure-stack-develop-templates.md) para obter orientações sobre como desenvolver modelos para garantir a portabilidade.
* Existem diferenças na disponibilidade do serviço e o controlo de versões de serviço entre o Azure e o Azure Stack. Pode utilizar o [módulo de política do Azure Stack](azure-stack-policy-module.md) para restringir os tipos de recursos e a disponibilidade do serviço do Azure para o que está disponível no Azure Stack. Restrição de serviços garante que as aplicações dependem de serviços disponíveis para o Azure Stack.
* O [modelos de início rápido do Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates) são exemplos comuns de cenário que mostram como desenvolver modelos que podem ser implementados no Azure e o Azure Stack.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o desenvolvimento do Azure Stack, veja os artigos seguintes:

* [Azure práticas recomendadas de modelo do Resource Manager](azure-stack-develop-templates.md)
* [Modelos de início rápido do Azure Stack no GitHub](https://github.com/Azure/AzureStack-QuickStart-Templates)