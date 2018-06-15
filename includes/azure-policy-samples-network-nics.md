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
ms.openlocfilehash: 388d6641fb77d09c1706623b84ec256573807747
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664678"
---
### <a name="network-interfaces"></a>Interfaces de Rede

|  |  |
|---------|---------|
| [NSG X em cada NIC](../articles/azure-policy/scripts/nsg-on-nic.md) | Exige que um grupo de segurança de rede específico seja utilizado em cada interface de rede virtual. Especifica o ID do grupo de segurança de rede a utilizar. |
| [Utilizar sub-rede aprovada para interfaces de rede de VM](../articles/azure-policy/scripts/use-approved-subnet-vm-nics.md) | Exige que as interfaces de rede utilizem uma sub-rede aprovada. Especifica o ID da sub-rede aprovada. |
| [Utilizar vNet aprovada para interfaces de rede de VM](../articles/azure-policy/scripts/use-approved-vnet-vm-nics.md) | Exige que as interfaces de rede utilizem uma rede virtual aprovada. Especifica o ID da rede virtual aprovada. |