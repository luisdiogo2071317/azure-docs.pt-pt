---
title: Criar o FQDN para uma VM do Windows no portal do Azure | Microsoft Docs
description: Saiba como criar um nome de domínio completamente qualificado, ou FQDN, para um Gestor de recursos com base em máquina virtual no portal do Azure.
services: virtual-machines-windows
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: a2ae5887-76df-485e-ae19-0efd96df8600
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8ebc1ef89b24a9aa21f39e5b05051c16351f08cd
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2017
ms.locfileid: "26707413"
---
# <a name="create-a-fully-qualified-domain-name-in-the-azure-portal-for-a-windows-vm"></a>Criar um nome de domínio completamente qualificado no portal do Azure para uma VM do Windows

Quando cria uma máquina virtual (VM) no [portal do Azure](https://portal.azure.com), é criado automaticamente um recurso IP público para a máquina virtual. Utilize este endereço IP para aceder remotamente a VM. Embora o portal não cria um [nome de domínio completamente qualificado](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), ou FQDN, pode criar um assim que a VM ser criada. Este artigo demonstra os passos para criar um nome DNS ou um FQDN.

## <a name="create-a-fqdn"></a>Criar um FQDN
Este artigo pressupõe que já criou uma VM. Se necessário, pode [criar uma VM no portal do](quick-create-portal.md) ou [com o Azure PowerShell](quick-create-powershell.md). Assim que a VM está a funcionar, siga estes passos:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Agora pode ligar remotamente para a VM com este nome DNS, tal como para o protocolo de ambiente de trabalho remoto (RDP).

## <a name="next-steps"></a>Passos seguintes
Agora que a VM tem um nome IP e DNS público, pode implementar comuns das estruturas de aplicações ou serviços como o IIS, SQL Server ou SharePoint.

Também pode ler mais sobre [com o Resource Manager](../../azure-resource-manager/resource-group-overview.md) para sugestões sobre como criar as suas implementações do Azure.

