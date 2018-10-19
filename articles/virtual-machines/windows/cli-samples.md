---
title: Exemplos da CLI do Azure Windows | Documentos da Microsoft
description: Exemplos da CLI do Azure Windows
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
ms.date: 06/01/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 6861399b63b7f06bac7599704a6dd1aa87800ebf
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/18/2018
ms.locfileid: "49403343"
---
# <a name="azure-cli-samples-for-windows-virtual-machines"></a>Máquinas de virtuais de amostras de CLI para Windows do Azure

A tabela seguinte inclui ligações para scripts bash criados com a CLI do Azure que implementar as máquinas virtuais do Windows.

| | |
|---|---|
|**Criar máquinas virtuais**||
| [Criar uma máquina virtual](./../scripts/virtual-machines-windows-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma máquina virtual do Windows com configuração mínima. |
| [Criar uma máquina virtual totalmente configurada](./../scripts/virtual-machines-windows-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Cria um grupo de recursos, uma máquina virtual e todos os recursos relacionados.|
| [Criar máquinas virtuais de elevada disponibilidade](./../scripts/virtual-machines-windows-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | Cria várias máquinas virtuais numa elevada disponibilidade e a configuração de balanceamento de carga. |
| [Criar uma VM e execute o script de configuração](./../scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma máquina virtual e utiliza a extensão de Script personalizado do Azure para instalar o IIS. |
| [Criar uma VM e execute a configuração de DSC](./../scripts/virtual-machines-windows-cli-sample-create-iis-using-dsc.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma máquina virtual e utiliza a extensão do Azure Desired State Configuration (DSC) para instalar o IIS. |
|**Máquinas de virtuais de rede**||
| [Proteger o tráfego de rede entre máquinas virtuais](./../scripts/virtual-machines-windows-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | Cria duas máquinas virtuais, todos os recursos relacionados e um grupos de segurança de rede interna e externa (NSG). |
|**Proteger máquinas virtuais**||
| [Encriptar uma VM e discos de dados](./../scripts/virtual-machines-windows-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Cria um Azure Key Vault, a chave de encriptação e o principal de serviço, em seguida, criptografa uma VM. |
|**Monitorizar máquinas virtuais**||
| [Monitorizar uma VM com o Log Analytics](./../scripts/virtual-machines-windows-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | Cria uma máquina virtual, instala o agente do Log Analytics e inscreve a VM numa área de trabalho do Log Analytics.  |
| | |
