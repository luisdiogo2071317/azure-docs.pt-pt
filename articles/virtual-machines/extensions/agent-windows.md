---
title: Descrição geral do agente de Máquina Virtual do Azure | Microsoft Docs
description: Descrição geral do agente de Máquina Virtual do Azure
services: virtual-machines-windows
documentationcenter: virtual-machines
author: danielsollondon
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 0a1f212e-053e-4a39-9910-8d622959f594
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/30/2018
ms.author: danis
ms.openlocfilehash: fb29f0f931715b8a6ba5b4528294eb61ef5762c8
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="azure-virtual-machine-agent-overview"></a>Descrição geral do agente da Máquina Virtual do Azure
O agente de Máquina Virtual do Microsoft Azure (agente da VM) é um processo de seguro e simples que gere a interação de máquina virtual (VM) com o controlador de recursos de infraestrutura do Azure. O agente da VM tem uma função primária em ativar e executar as extensões de máquina virtual do Azure. Extensões de VM ative a configuração de pós-implementação da VM, tais como instalar e configurar o software. Extensões VM também ativar as funcionalidades de recuperação, tal como repor a palavra-passe administrativa de uma VM. Sem o agente da VM do Azure, não não possível executar as extensões de VM.

Este artigo fornece detalhes sobre a instalação, a deteção e a remoção do agente de Máquina Virtual do Azure.

## <a name="install-the-vm-agent"></a>Instalar o agente da VM

### <a name="azure-marketplace-image"></a>Imagem do Marketplace do Azure

O agente da VM do Azure está instalado por predefinição em qualquer VM do Windows implementadas a partir de uma imagem do Azure Marketplace. Quando implementa uma imagem do Azure Marketplace partir do portal, do PowerShell, Interface de linha de comandos ou um modelo Azure Resource Manager, também é instalado o agente da VM do Azure.

O pacote de agente de convidados do Windows é dividido em duas partes:

- Agente de aprovisionamento (PA)
- Agente de convidados do Windows (WinGA)

Para efetuar o arranque de uma VM tem de ter o PA instalado na VM, no entanto a WinGA não tem de ser instalado. Na VM implementar tempo, pode selecionar para não instalar o WinGA. O exemplo seguinte mostra como selecionar o *provisionVmAgent* opção com um modelo Azure Resource Manager:

```json
"resources": [{
"name": "[parameters('virtualMachineName')]",
"type": "Microsoft.Compute/virtualMachines",
"apiVersion": "2016-04-30-preview",
"location": "[parameters('location')]",
"dependsOn": ["[concat('Microsoft.Network/networkInterfaces/', parameters('networkInterfaceName'))]"],
"properties": {
    "osProfile": {
    "computerName": "[parameters('virtualMachineName')]",
    "adminUsername": "[parameters('adminUsername')]",
    "adminPassword": "[parameters('adminPassword')]",
    "windowsConfiguration": {
        "provisionVmAgent": "false"
}
```

Se não instalar os agentes, não é possível utilizar alguns serviços do Azure, tais como a cópia de segurança do Azure ou de segurança do Azure. Estes serviços necessitam de uma extensão para ser instalada. Se tiver implementado uma VM sem o WinGA, pode instalar a versão mais recente do agente mais tarde.

### <a name="manual-installation"></a>Instalação manual
O agente da VM do Windows pode ser instalado manualmente com um pacote de instalador do Windows. Instalação manual pode ser necessária quando cria uma imagem VM personalizada que é implementada no Azure. Para instalar manualmente o agente de VM do Windows, [transferir o instalador do agente de VM](http://go.microsoft.com/fwlink/?LinkID=394789).

O agente da VM pode ser instalado, fazendo duplo clique o ficheiro do Windows installer. Para uma instalação automatizada ou automática do agente VM, execute o seguinte comando:

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-the-vm-agent"></a>Detetar o agente da VM

### <a name="powershell"></a>PowerShell

O módulo do PowerShell do Azure Resource Manager pode ser utilizado para obter informações sobre as VMs do Azure. Para ver informações sobre uma VM, como o estado de aprovisionamento para o agente da VM do Azure, utilize [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm):

' 'powershell' Get-AzureRmVM
```

The following condensed example output shows the the *ProvisionVMAgent* property nested inside *OSProfile*. This property can be used to determine if the VM agent has been deployed to the VM:

```PowerShell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : myUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

O script seguinte pode ser utilizado para devolver uma lista de nomes VM e o estado do agente de VM concisa:

```PowerShell
$vms = Get-AzureRmVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>Deteção manual
Quando tem sessão iniciada VM do Windows Azure, o Gestor de tarefas pode ser utilizado para examinar os processos em execução. Para verificar se o agente da VM do Azure, abra o Gestor de tarefas, clique em de *detalhes* separador e procure um nome de processo **WindowsAzureGuestAgent.exe**. A presença deste processo indica que o agente VM está instalado.


## <a name="upgrade-the-vm-agent"></a>Atualize o agente VM
O Azure VM agente para o Windows é atualizado automaticamente. Como novas VMs são implementadas para o Azure, estes recebem o agente VM mais recente no tempo de aprovisionamento de VM. Imagens VM personalizadas devem ser atualizadas manualmente para incluir o novo agente da VM no momento de criação da imagem.


## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre as extensões VM, consulte [descrição geral de funcionalidades e as extensões de máquina virtual do Azure](overview.md).