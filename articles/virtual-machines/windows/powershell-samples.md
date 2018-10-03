---
title: Exemplos do PowerShell de Máquina Virtual do Azure | Documentos da Microsoft
description: Exemplos do PowerShell de Máquina Virtual do Azure
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/30/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 564e8e2968f31b9e2aef8f7eadbf848885037ccb
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/02/2018
ms.locfileid: "48040767"
---
# <a name="azure-virtual-machine-powershell-samples"></a>Exemplos do PowerShell da Máquina Virtual do Azure

A tabela seguinte inclui ligações para exemplos de scripts do PowerShell que criarem e gerir máquinas virtuais do Windows.

| | |
|---|---|
|**Criar máquinas virtuais**||
| [Criar rapidamente uma máquina virtual](./../scripts/virtual-machines-windows-powershell-sample-create-vm-quick.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria um grupo de recursos, uma máquina virtual e todos os recursos relacionados, com o mínimo de prompts.|
| [Criar uma máquina virtual totalmente configurada](./../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria um grupo de recursos, uma máquina virtual e todos os recursos relacionados.|
| [Criar máquinas virtuais de elevada disponibilidade](./../scripts/virtual-machines-windows-powershell-sample-create-nlb-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria várias máquinas virtuais numa elevada disponibilidade e a configuração de balanceamento de carga.|
| [Criar uma VM e execute o script de configuração](./../scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria uma máquina virtual e utiliza a extensão de Script personalizado do Azure para instalar o IIS. |
| [Criar uma VM e execute a configuração de DSC](./../scripts/virtual-machines-windows-powershell-sample-create-iis-using-dsc.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria uma máquina virtual e utiliza a extensão do Azure Desired State Configuration (DSC) para instalar o IIS. |
| [Carregar um VHD e criar VMs](./../scripts/virtual-machines-windows-powershell-upload-generalized-script.md) | Carrega um ficheiro VHD local para o Azure, cria e de imagem de VHD e, em seguida, cria uma VM a partir dessa imagem. |
| [Criar uma VM a partir de um disco de SO gerido](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria uma máquina virtual ao anexar um disco gerido existente como disco do SO. |
| [Criar uma VM a partir de um instantâneo](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria uma máquina virtual a partir de um instantâneo primeiro criar um disco gerido a partir de instantâneo e, em seguida, anexando-o novo disco gerido como disco do SO. |
|**Gerir o armazenamento**||
| [Criar disco gerido a partir de um VHD numa subscrição idêntica ou diferente](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria um disco gerido a partir de um VHD especializado como um disco de SO ou a partir de um VHD como disco de dados na subscrição idêntica ou diferente de dados.  |
| [Criar um disco gerido a partir de um instantâneo](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria um disco gerido a partir de um instantâneo. |
| [Copiar o disco gerido para uma subscrição idêntica ou diferente](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Cópias geridas disco para uma subscrição idêntica ou diferente, mas na mesma região que o elemento principal de disco gerido. 
| [Exportar um instantâneo como VHD para uma conta de armazenamento](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Exporta um instantâneo gerido como VHD para uma conta de armazenamento numa região diferente. |
| [Exportar o VHD de um disco gerido para uma conta de armazenamento](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Exporta o VHD subjacente de um disco gerido para uma conta de armazenamento numa região diferente. |
| [Criar um instantâneo de um VHD](../scripts/virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria o instantâneo de um VHD para criar vários discos geridos idênticos a partir de instantâneo num curto período de tempo.  |
| [Copiar o instantâneo para uma subscrição idêntica ou diferente](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Instantâneo de cópias para uma subscrição idêntica ou diferente, mas na mesma região do instantâneo principal. |
|**Proteger máquinas virtuais**||
| [Encriptar uma VM e discos de dados](./../scripts/virtual-machines-windows-powershell-sample-encrypt-vm.md?toc=%2fpowershell%2fazure%2ftoc.json) | Cria um Azure Key Vault, a chave de encriptação e o principal de serviço, em seguida, criptografa uma VM. |
|**Monitorizar máquinas virtuais**||
| [Monitorizar uma VM com o Operations Management Suite](./../scripts/virtual-machines-windows-powershell-sample-create-vm-oms.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Cria uma máquina virtual, instala o agente do Operations Management Suite e inscreve a VM numa área de trabalho do OMS.  |
| | |

