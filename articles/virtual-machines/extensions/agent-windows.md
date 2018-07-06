---
title: Descrição geral do agente de Máquina Virtual do Azure | Documentos da Microsoft
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
ms.openlocfilehash: 51271a875d19d08d84dcc102c85c947c05599d85
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868522"
---
# <a name="azure-virtual-machine-agent-overview"></a>Descrição geral do agente da Máquina Virtual do Azure
O agente de Máquina Virtual do Microsoft Azure (agente de VM) é um processo leve e seguro que gere a interação da máquina virtual (VM) com o controlador de malha do Azure. O agente da VM tem uma função primária na ativação e execução de extensões de máquina virtual do Azure. Extensões de VM ativar a configuração de pós-implementação de VM, como instalar e configurar o software. Extensões de VM também ativar funcionalidades de recuperação como a reposição de palavra-passe administrativa de uma VM. Sem o agente da VM do Azure, não não possível executar as extensões de VM.

Este artigo fornece detalhes sobre a instalação, detecção e remoção do agente de Máquina Virtual do Azure.

## <a name="install-the-vm-agent"></a>Instalar o agente da VM

### <a name="azure-marketplace-image"></a>Imagem do Marketplace do Azure

O agente da VM do Azure está instalado por predefinição em qualquer VM do Windows implementadas a partir de uma imagem do Azure Marketplace. Quando implementa uma imagem de Azure Marketplace a partir do portal, PowerShell, Interface de linha de comando ou um modelo Azure Resource Manager, também é instalado o agente da VM do Azure.

O pacote de agente convidado do Windows é dividido em duas partes:

- Agente de aprovisionamento (PA)
- Agente convidado do Windows (WinGA)

Para inicializar uma VM tem de ter o PA instalado na VM, no entanto o WinGA não precisa de ser instalado. No VM momento da implantação, pode selecionar para não instalar o WinGA. O exemplo seguinte mostra como selecionar o *provisionVmAgent* opção com um modelo Azure Resource Manager:

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

Se não tiver os agentes instalados, é possível utilizar alguns serviços do Azure, tais como cópia de segurança do Azure ou de segurança do Azure. Estes serviços necessitam de uma extensão para ser instalado. Se tiver implementado uma VM sem o WinGA, pode instalar a versão mais recente do agente mais tarde.

### <a name="manual-installation"></a>Instalação manual
O agente da VM do Windows pode ser instalado manualmente com um pacote de instalador do Windows. Instalação manual pode ser necessária quando cria uma imagem VM personalizada que é implementada no Azure. Para instalar manualmente o agente de VM do Windows, [transferir o instalador do agente de VM](http://go.microsoft.com/fwlink/?LinkID=394789).

O agente da VM pode ser instalado clicando duas vezes o ficheiro de instalador do Windows. Para uma instalação automatizada ou autônoma do agente VM, execute o seguinte comando:

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-the-vm-agent"></a>Detetar o agente da VM

### <a name="powershell"></a>PowerShell

O módulo do PowerShell do Azure Resource Manager pode ser utilizado para obter informações sobre as VMs do Azure. Para ver informações sobre uma VM, como o estado de aprovisionamento para o agente de VM do Azure, utilize [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm):

' "powershell" Get-AzureRmVM
```

The following condensed example output shows the *ProvisionVMAgent* property nested inside *OSProfile*. This property can be used to determine if the VM agent has been deployed to the VM:

```PowerShell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : myUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

O seguinte script pode ser utilizado para devolver uma lista concisa de nomes VM e o estado do agente de VM:

```PowerShell
$vms = Get-AzureRmVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>Deteção manual
Quando iniciar sessão a uma VM do Windows Azure, Gestor de tarefas pode servir para examinar os processos em execução. Para verificar se o agente da VM do Azure, abra o Gestor de tarefas, clique a *detalhes* separador e procure um nome de processo **WindowsAzureGuestAgent.exe**. A presença deste processo indica que o agente da VM está instalado.


## <a name="upgrade-the-vm-agent"></a>Atualizar o agente da VM
O Azure VM Agent para Windows é atualizada automaticamente. Conforme novas VMs são implementadas para o Azure, recebem o agente VM mais recente no tempo de aprovisionamento da VM. Imagens de VM personalizadas devem ser atualizadas manualmente para incluir o novo agente VM no momento da criação de imagem.


## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre as extensões de VM, consulte [descrição geral de extensões e funcionalidades de máquina virtual do Azure](overview.md).