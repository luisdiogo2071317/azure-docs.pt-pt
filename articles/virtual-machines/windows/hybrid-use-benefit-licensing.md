---
title: Benefício de híbridos do Azure para o Windows Server | Microsoft Docs
description: Saiba como maximizar as vantagens do Windows Software Assurance para colocar licenças no local do Azure
services: virtual-machines-windows
documentationcenter: ''
author: xujing
manager: jeconnoc
editor: ''
ms.assetid: 332583b6-15a3-4efb-80c3-9082587828b0
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 4/22/2018
ms.author: xujing-ms
ms.openlocfilehash: a4b0baefc8c3c839a06d6540e57b34657138c8ff
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
---
# <a name="azure-hybrid-benefit-for-windows-server"></a>Benefício Híbrido do Azure para o Windows Server
Para clientes com Software Assurance, o Azure híbrida benefício para o Windows Server permite-lhe utilizar as licenças do Windows Server no local e executar máquinas virtuais do Windows no Azure um custo reduzido. Pode utilizar o Azure híbrida benefício para o Windows Server para implementar novas máquinas virtuais com o sistema operativo Windows. Este artigo vai sobre os passos sobre como implementar novas VMs com o Azure híbrida benefício para o Windows Server e como pode atualizar existentes VMs em execução. Para obter mais informações sobre o Azure híbrida benefício para o Windows Server reduções de custos e licenciamento, consulte o [página de licenciamento do Azure híbrida benefício para o Windows Server](https://azure.microsoft.com/pricing/hybrid-use-benefit/).

> [!Important]
> Cada licença de dois processadores ou cada conjunto de licenças de 16 núcleos são elegíveis para duas instâncias de até oito núcleos ou uma instância de até 16 núcleos. O Benefício Híbrido do Azure para licenças da Edição Standard só pode ser utilizado uma vez, no local ou no Azure. Os benefícios da Datacenter Edition permitem a utilização no local e no Azure em simultâneo.
>

> [!Important]
> Utilizar o Azure híbrida benefício para o Windows Server com as VMs do SO do Windows Server em execução agora são suportados em todas as regiões, incluindo VMs com software de mercado de terceiros ou software adicionais, tais como o SQL Server. 
>

> [!NOTE]
> Para VMs clássicas, implementar apenas uma nova VM de imagens personalizadas no local é suportada. Para tirar partido das capacidades suportadas neste artigo, tem de migrar VMs clássicas para o modelo do Resource Manager.
>


## <a name="ways-to-use-azure-hybrid-benefit-for-windows-server"></a>Formas de utilizar o Azure híbrida benefício para o Windows Server
Existem algumas formas de utilizar máquinas virtuais do Windows com a vantagem de híbrida do Azure:

1. É possível implementar VMs de uma das fornecido [imagens do Windows Server no Azure Marketplace](#https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview)
2. Pode carregar uma VM personalizada e implementar a utilizar um modelo do Resource Manager ou o Azure PowerShell
3. Pode alternar e converter VM existente entre em execução com o benefício de híbrida do Azure ou paga um custo a pedido para o Windows Server
4. Também pode aplicar Azure híbrida benefício para o Windows Server em conjunto, bem como de dimensionamento de máquina virtual


## <a name="create-a-vm-with-azure-hybrid-benefit-for-windows-server"></a>Criar uma VM com o benefício de híbridos do Azure para o Windows Server
Todas as imagens de sistema operativo de servidor do Windows com base são suportadas para o Azure híbrida benefício para o Windows Server. Pode utilizar imagens de suporte de plataforma do Azure ou carregar as suas próprias imagens personalizadas do Windows Server. 

### <a name="portal"></a>Portal
Para criar uma VM com o Azure híbrida benefício para o Windows Server, utilize o botão de alternar na secção "Poupar dinheiro".

### <a name="powershell"></a>PowerShell
```powershell
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Location "East US" `
    -ImageName "Win2016Datacenter" `
    -LicenseType "Windows_Server"
```

### <a name="cli"></a>CLI
```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --license-type Windows_Server
```

### <a name="template"></a>Modelo
Dentro os modelos do Resource Manager, um parâmetro adicional `licenseType` tem de ser especificado. Pode ler mais sobre [criação de modelos Azure Resource Manager](../../resource-group-authoring-templates.md)
```json
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   }
```

## <a name="convert-an-existing-vm-using-azure-hybrid-benefit-for-windows-server"></a>Converter VM existente com o Azure híbrida benefício para o Windows Server
Se tiver uma VM existente que pretende converter para tirar partido do Azure híbrida benefício para o Windows Server, pode atualizar o tipo de licença da VM da seguinte forma:

### <a name="portal"></a>Portal
No painel VM do portal, pode atualizar a VM para utilizar o benefício de híbrida do Azure, selecionando a opção "Configuração de" e ative a opção "beneficiar híbridos do Azure"

### <a name="powershell"></a>PowerShell
- Converter VMs do Windows Server existente para o Azure híbrida benefício para o Windows Server

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
    $vm.LicenseType = "Windows_Server"
    Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
    ```
    
- Converter VMs do Windows Server com a vantagem pay as you go

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroup "rg-name" -Name "vm-name"
    $vm.LicenseType = "None"
    Update-AzureRmVM -ResourceGroupName rg-name -VM $vm
    ```
    
### <a name="cli"></a>CLI
- Converter VMs do Windows Server existente para o Azure híbrida benefício para o Windows Server

    ```azurecli
    az vm update --resource-group myResourceGroup --name myVM --set licenseType=Windows_Server
    ```
    
### <a name="how-to-verify-your-vm-is-utilizing-the-licensing-benefit"></a>Como verificar a VM está a utilizar o benefício de licenciamento
Depois de ter implementado a sua VM através do ou PowerShell, o modelo do Resource Manager ou do portal, pode verificar a definição nos seguintes métodos.

### <a name="portal"></a>Portal
No painel VM do portal, pode ver o botão de alternar para o Azure híbrida benefício para o Windows Server selecionando o separador "Configuração de".

### <a name="powershell"></a>PowerShell
O exemplo seguinte mostra o tipo de licença para uma única VM
```powershell
Get-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
```

Saída:
```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Saída Isto contrasta com a seguinte VM implementada sem licenciamento do Azure híbrida benefício para o Windows Server:
```powershell
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              :
```

### <a name="cli"></a>CLI
```azurecli
az vm get-instance-view -g MyResourceGroup -n MyVM --query '[?licenseType==Windows_Server]' -o table
```

## <a name="list-all-vms-with-azure-hybrid-benefit-for-windows-server-in-a-subscription"></a>Lista todas as VMs com o Azure híbrida benefício para o Windows Server numa subscrição
Para ver e contagem de todas as máquinas virtuais implementadas com o Azure híbrida benefício para o Windows Server, pode executar o seguinte comando da sua subscrição:

### <a name="portal"></a>Portal
Da Máquina Virtual ou Máquina Virtual escala conjuntos de painel de recursos, pode ver uma lista de todas as VM e tipo de licenciamento, configurando a coluna de tabela para incluir o "Benefício híbrida do Azure". A definição de VM pode ser na "Ativado", "Não está activado" ou "Não suportado" de estado.

### <a name="powershell"></a>PowerShell
```powershell
$vms = Get-AzureRMVM 
$vms | ?{$_.LicenseType -like "Windows_Server"} | select ResourceGroupName, Name, LicenseType
```

### <a name="cli"></a>CLI
```azurecli
az vm list --query '[?licenseType==Windows_Server]' -o table
```

## <a name="deploy-a-virtual-machine-scale-set-with-azure-hybrid-benefit-for-windows-server"></a>Implementar um conjunto de dimensionamento de Máquina Virtual com o benefício de híbridos do Azure para o Windows Server
A máquina virtual de conjunto de dimensionamento de modelos do Resource Manager, um parâmetro adicional `licenseType` tem de ser especificado dentro da sua propriedade VirtualMachineProfile. Pode fazê-lo durante a criação ou atualização para o seu conjunto através do modelo ARM, do Powershell, a CLI do Azure ou REST de dimensionamento.

O exemplo seguinte utiliza o modelo ARM com uma imagem do Datacenter do Windows Server 2016:
```json
"virtualMachineProfile": {
    "storageProfile": {
        "osDisk": {
            "createOption": "FromImage"
        },
        "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2016-Datacenter",
            "version": "latest"
        }
    },
    "licenseType": "Windows_Server",
    "osProfile": {
            "computerNamePrefix": "[parameters('vmssName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
    }
```
Também pode saber mais sobre como [modificar um conjunto de dimensionamento de máquina virtual](../../virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set.md) para obter mais maneiras de atualizar o dimensionamento definido.

## <a name="next-steps"></a>Passos Seguintes
- Leia mais sobre [como poupar dinheiro com a vantagem de híbrida do Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/)
- Leia mais sobre [perguntas mais frequentes sobre o benefício de híbrida do Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/faq/)
- Saiba mais sobre [Azure híbrida benefício para o Windows Server orientações detalhadas de licenciamento](https://docs.microsoft.com/windows-server/get-started/azure-hybrid-benefit)
- Saiba mais sobre [Azure híbrida benefício para o Windows Server e o Azure Site Recovery disponibilizar aplicações migrar para o Azure ainda mais económico](https://azure.microsoft.com/blog/hybrid-use-benefit-migration-with-asr/)
- Saiba mais sobre [Windows 10 no Azure com o direito de alojamento de multi-inquilino](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment)
- Saiba mais sobre [modelos utilizando o Resource Manager](../../azure-resource-manager/resource-group-overview.md)
