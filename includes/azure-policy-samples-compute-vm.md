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
ms.openlocfilehash: eadff6f61bd9e5979ae835230de53562ff8542d1
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47004005"
---
### <a name="virtual-machines"></a>Virtual Machines

|  |  |
|---------|---------|
| [Permitir que a imagem de VM personalizada seja de um Grupo de Recursos](../articles/governance/policy/samples/allow-custom-vm-image.md) |  Requer que as imagens personalizadas sejam provenientes de um grupo de recursos aprovado. Especifica o nome do grupo de recursos aprovado. |
| [SKUs Permitidos para Contas de Armazenamento e Máquinas Virtuais](../articles/governance/policy/samples/allowed-skus-storage.md) | Requer que as contas de armazenamento e as máquinas virtuais utilizem SKUs aprovados. Utiliza políticas incorporadas para garantir SKUs aprovados. Especifique uma matriz de SKUs de máquinas virtuais aprovadas e uma matriz de SKUs de contas de armazenamento aprovadas. |
| [Imagens de VM aprovadas](../articles/governance/policy/samples/allowed-custom-images.md) | Requer que sejam implementadas apenas imagens personalizadas aprovadas no seu ambiente. Especifica uma matriz de IDs de imagens aprovadas. |
| [Auditar se a extensão não existir](../articles/governance/policy/samples/audit-ext-not-exist.md) | Faz uma auditoria se uma extensão não estiver implementada numa máquina virtual. Especifica o publicador da extensão e o tipo para verificar se foi implementada. |
| [Extensões de VM não permitidas](../articles/governance/policy/samples/not-allowed-vm-ext.md) | Proíbe a utilização de extensões especificadas. Especifica uma matriz que contém os tipos de extensão proibidos. |