---
title: Exemplos da CLI do Azure Windows | Microsoft Docs
description: Exemplos da CLI do Azure Windows
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 543a794f0c99004b2cb71feeb97bcd8d95407d48
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="azure-cli-samples-for-windows-virtual-machines"></a>Virtual machines do Azure CLI amostras para Windows

A tabela seguinte inclui ligações para bash scripts compiladas com a CLI do Azure que implementar máquinas virtuais do Windows.

| | |
|---|---|
|**Criar máquinas virtuais**||
| [Criar uma máquina virtual](./../scripts/virtual-machines-windows-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma máquina virtual do Windows com configuração mínima. |
| [Criar uma máquina virtual totalmente configurada](./../scripts/virtual-machines-windows-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Cria um grupo de recursos, a máquina virtual e todos os recursos relacionados.|
| [Criar máquinas virtuais altamente disponíveis](./../scripts/virtual-machines-windows-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | Cria várias máquinas virtuais da elevada disponibilidade e a configuração de balanceamento de carga. |
| [Criar uma VM e executar o script de configuração](./../scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma máquina virtual e utiliza a extensão de Script de personalizada do Azure para instalar o IIS. |
| [Criar uma VM e execute a configuração DSC](./../scripts/virtual-machines-windows-cli-sample-create-iis-using-dsc.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma máquina virtual e utiliza a extensão do Azure pretendido Estado Configuration (DSC) para instalar o IIS. |
|**Máquinas virtuais da rede**||
| [Proteger o tráfego de rede entre as máquinas virtuais](./../scripts/virtual-machines-windows-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | Cria duas máquinas virtuais, relacionados todos os recursos e uma grupos de segurança de rede internos e externos (NSG). |
|**Proteger máquinas virtuais**||
| [Encriptar uma VM e discos de dados](./../scripts/virtual-machines-windows-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Cria um cofre de chaves do Azure, uma chave de encriptação e um principal de serviço, em seguida, encripta uma VM. |
|**Monitorizar máquinas virtuais**||
| [Monitorizar uma VM com o Operations Management Suite](./../scripts/virtual-machines-windows-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma máquina virtual, instala o agente do Operations Management Suite e inscreve a VM com uma área de trabalho do OMS.  |
| | |
