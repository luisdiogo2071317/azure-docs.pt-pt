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
ms.openlocfilehash: eedce1ef3a4e7d56cfaf3142a72e370c96f9f575
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34664648"
---
### <a name="virtual-machines"></a>Virtual Machines

|  |  |
|---------|---------|
| [Permitir que a imagem de VM personalizada seja de um Grupo de Recursos](../articles/azure-policy/scripts/allow-custom-vm-image.md) |  Requer que as imagens personalizadas sejam provenientes de um grupo de recursos aprovado. Especifica o nome do grupo de recursos aprovado. |
| [SKUs Permitidos para Contas de Armazenamento e Máquinas Virtuais](../articles/azure-policy/scripts/allowed-skus-storage.md) | Requer que as contas de armazenamento e as máquinas virtuais utilizem SKUs aprovados. Utiliza políticas incorporadas para garantir SKUs aprovados. Especifique uma matriz de SKUs de máquinas virtuais aprovadas e uma matriz de SKUs de contas de armazenamento aprovadas. |
| [Imagens de VM aprovadas](../articles/azure-policy/scripts/allowed-custom-images.md) | Requer que sejam implementadas apenas imagens personalizadas aprovadas no seu ambiente. Especifica uma matriz de IDs de imagens aprovadas. |
| [Auditar se a extensão não existir](../articles/azure-policy/scripts/audit-ext-not-exist.md) | Faz uma auditoria se uma extensão não estiver implementada numa máquina virtual. Especifica o publicador da extensão e o tipo para verificar se foi implementada. |
| [Extensões de VM não permitidas](../articles/azure-policy/scripts/not-allowed-vm-ext.md) | Proíbe a utilização de extensões especificadas. Especifica uma matriz que contém os tipos de extensão proibidos. |