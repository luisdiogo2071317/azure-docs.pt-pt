---
title: Configurar pontos finais numa VM clássica do Windows | Documentos da Microsoft
description: Saiba como configurar pontos finais para uma VM clássica do Windows no portal do Azure para permitir a comunicação com uma máquina virtual do Windows no Azure.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 8afc21c2-d3fb-43a3-acce-aa06be448bb6
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/23/2018
ms.author: cynthn
ms.openlocfilehash: 373003eb8be0482ed96d7d11ecd879237f69b47a
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50957170"
---
# <a name="set-up-endpoints-on-a-windows-virtual-machine-by-using-the-classic-deployment-model"></a>Configurar pontos finais numa máquina virtual do Windows utilizando o modelo de implementação clássica
Máquinas de virtuais de Windows (VMs) que criar no Azure, utilizando o modelo de implementação clássica podem automaticamente comunicar através de um canal de rede privada com outras VMs no mesmo serviço cloud ou rede virtual. No entanto, os computadores na internet ou de outras redes virtuais requerem pontos finais para direcionar o tráfego de rede de entrada para uma VM. 

Também pode configurar pontos finais no [máquinas virtuais do Linux](../../linux/classic/setup-endpoints.md).

> [!IMPORTANT]
> O Azure tem dois modelos de implementação para criar e trabalhar com recursos: [Resource Manager e Clássico](../../../resource-manager-deployment-model.md). Este artigo abrange o modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager.  
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Na **Resource Manager** modelo de implementação, pontos finais são configurados utilizando **grupos de segurança de rede (NSGs)**. Para obter mais informações, consulte [permitir acesso externo à sua VM com o portal do Azure](../nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Quando cria uma VM do Windows no portal do Azure, pontos de extremidade como, por exemplo, pontos finais para a área de trabalho remota e a comunicação remota do Windows PowerShell, são normalmente criados automaticamente. Pode configurar pontos finais adicionais mais tarde, conforme necessário.

[!INCLUDE [virtual-machines-common-classic-setup-endpoints](../../../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Passos Seguintes
* Para utilizar um cmdlet do Azure PowerShell para configurar um ponto final da VM, consulte [Add-AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).
* Para utilizar um cmdlet do Azure PowerShell para gerir uma ACL num ponto final, veja [gerir acesso listas de controle (ACLs) para pontos finais com o PowerShell](../../../virtual-network/virtual-networks-acl-powershell.md).
* Se tiver criado uma máquina virtual no modelo de implementação do Resource Manager, pode utilizar o Azure PowerShell para [criar grupos de segurança de rede](../../../virtual-network/tutorial-filter-network-traffic.md) para controlar o tráfego para a VM.
