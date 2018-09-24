---
title: incluir ficheiro
description: incluir ficheiro
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: df9c291136fc6a48effb08cd59eeb66486eb641c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47003972"
---
### <a name="virtual-networks"></a>Redes Virtuais

|  |  |
|---------|---------|
| [SKUs de Gateways de Aplicação permitidos](../articles/governance/policy/samples/allowed-app-gate-sku.md) | Requer que os gateways de aplicação utilizem um SKU aprovado. Especifica uma matriz de SKUs aprovados. |
| [Sem peering de rede à rede do ER](../articles/governance/policy/samples/no-peering-er-net.md) | Proíbe um peering de rede de ser associado a uma rede num grupo de recursos especificado. Utilize para impedir a ligação com a infraestrutura de rede gerida central. Especifique o nome do grupo de recursos para impedir a associação. |
| [Nenhuma Tabela de Rotas Definida pelo Utilizador](../articles/governance/policy/samples/no-user-def-route-table.md)  |Proíbe a implementação de redes virtuais numa tabela de rotas definida pelo utilizador. |
| [NSG X em cada sub-rede](../articles/governance/policy/samples/nsg-on-subnet.md) | Exige que um grupo de segurança de rede específico seja utilizado em cada sub-rede virtual. Especifica o ID do grupo de segurança de rede a utilizar. |
| [Utilizar sub-rede aprovada para interfaces de rede de VM](../articles/governance/policy/samples/use-approved-subnet-vm-nics.md) | Exige que as interfaces de rede utilizem uma sub-rede aprovada. Especifica o ID da sub-rede aprovada. |
| [Utilizar vNet aprovada para interfaces de rede de VM](../articles/governance/policy/samples/use-approved-vnet-vm-nics.md) | Exige que as interfaces de rede utilizem uma rede virtual aprovada. Especifica o ID da rede virtual aprovada. |