---
title: incluir ficheiro
description: incluir ficheiro
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/17/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: b7bbf1fb8ffc1f57a53682d3baf288077b144572
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664695"
---
### <a name="virtual-networks"></a>Redes Virtuais

|  |  |
|---------|---------|
| [SKUs de Gateways de Aplicação permitidos](../articles/azure-policy/scripts/allowed-app-gate-sku.md) | Requer que os gateways de aplicação utilizem um SKU aprovado. Especifica uma matriz de SKUs aprovados. |
| [Sem peering de rede à rede do ER](../articles/azure-policy/scripts/no-peering-er-net.md) | Proíbe um peering de rede de ser associado a uma rede num grupo de recursos especificado. Utilize para impedir a ligação com a infraestrutura de rede gerida central. Especifique o nome do grupo de recursos para impedir a associação. |
| [Nenhuma Tabela de Rotas Definida pelo Utilizador](../articles/azure-policy/scripts/no-user-def-route-table.md)  |Proíbe a implementação de redes virtuais numa tabela de rotas definida pelo utilizador. |
| [NSG X em cada sub-rede](../articles/azure-policy/scripts/nsg-on-subnet.md) | Exige que um grupo de segurança de rede específico seja utilizado em cada sub-rede virtual. Especifica o ID do grupo de segurança de rede a utilizar. |
| [Utilizar sub-rede aprovada para interfaces de rede de VM](../articles/azure-policy/scripts/use-approved-subnet-vm-nics.md) | Exige que as interfaces de rede utilizem uma sub-rede aprovada. Especifica o ID da sub-rede aprovada. |
| [Utilizar vNet aprovada para interfaces de rede de VM](../articles/azure-policy/scripts/use-approved-vnet-vm-nics.md) | Exige que as interfaces de rede utilizem uma rede virtual aprovada. Especifica o ID da rede virtual aprovada. |