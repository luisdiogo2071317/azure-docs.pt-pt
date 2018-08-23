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
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: a6f89f9a7e5960e4749c14fc9a4adb648f6781f4
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "42057656"
---
# <a name="develop-for-azure-stack"></a>Desenvolver para o Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Pode começar a desenvolver aplicativos de hoje em dia, mesmo que não tenha acesso a um ambiente do Azure Stack. Porque o Azure Stack oferece serviços Microsoft Azure que executar no seu datacenter, pode utilizar ferramentas e processos semelhantes para desenvolver no Azure Stack, tal como faria com o Azure. Com alguma preparação e através da orientação nos tópicos seguintes, pode utilizar o Azure para emular um ambiente do Azure Stack.

* No Azure, pode criar modelos do Azure Resource Manager implementáveis no Azure Stack. Ver [considerações sobre os modelos](azure-stack-develop-templates.md) para obter orientações sobre como desenvolver modelos para garantir a portabilidade.
* Existem diferenças na disponibilidade do serviço e o controlo de versões de serviço entre o Azure e o Azure Stack. Pode utilizar o [módulo de política do Azure Stack](azure-stack-policy-module.md) para restringir os tipos de recursos e a disponibilidade do serviço do Azure para o que está disponível no Azure Stack. Restrição de serviços garante que as aplicações dependem de serviços disponíveis para o Azure Stack.
* O [modelos de início rápido do Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates) são exemplos comuns de cenário que mostram como desenvolver modelos que podem ser implementados no Azure e o Azure Stack.
