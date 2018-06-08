---
title: Implementar uma aplicação para um conjunto de dimensionamento da máquina virtual do Azure | Microsoft Docs
description: Saiba como implementar aplicações para Linux e Windows instâncias de máquina virtual num conjunto de dimensionamento
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: iainfou
ms.openlocfilehash: bbbe677b0a0d47147ace41ff5a229282f80bbf1b
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34839520"
---
# <a name="deploy-your-application-on-virtual-machine-scale-sets"></a>Implementar a sua aplicação em conjuntos de dimensionamento de máquina virtual
Para executar aplicações em instâncias de máquina virtual (VM) num conjunto de dimensionamento, primeiro tem de instalar os componentes da aplicação e os ficheiros necessários. Este artigo apresenta formas para criar uma imagem VM personalizada para instâncias na escala definido ou executam automaticamente os scripts de instalação em instâncias de VM existentes. Também irá aprender a gerir aplicações ou atualizações do SO através de um conjunto de dimensionamento.


## <a name="build-a-custom-vm-image"></a>Criar uma imagem VM personalizada
Quando utilizar uma das imagens da plataforma do Azure para criar as instâncias do conjunto de dimensionamento, o software adicional não está instalado ou configurado. Pode automatizar a instalação destes componentes, no entanto que adiciona ao tempo que demora a aprovisionar instâncias VM para os conjuntos de dimensionamento. Se aplicar demasiadas alterações de configuração para as instâncias de VM, há gestão sobrecarga com os scripts de configuração e tarefas.

Para reduzir a gestão de configuração e a hora para Aprovisionar uma VM, pode criar uma imagem VM personalizada que está pronta para executar a sua aplicação, assim que uma instância é aprovisionada no conjunto de dimensionamento. Para obter mais informações sobre como criar e utilizar uma imagem VM personalizada com uma escala definir, consulte os seguintes tutoriais:

- [CLI 2.0 do Azure](tutorial-use-custom-image-cli.md)
- [Azure PowerShell](tutorial-use-custom-image-powershell.md)


## <a name="already-provisioned"></a>Instalar uma aplicação com a extensão de Script personalizado
A Extensão de Script Personalizado transfere e executa scripts em VMs do Azure. Esta extensão é útil para a configuração pós-implementação, instalação de software ou qualquer outra tarefa de gestão/configuração. Os scripts podem ser transferidos a partir do armazenamento do Azure ou do GitHub, ou fornecidos para o portal do Azure no runtime da extensão. Para obter mais informações sobre como criar e utilizar uma imagem VM personalizada com uma escala definir, consulte os seguintes tutoriais:

- [CLI 2.0 do Azure](tutorial-install-apps-cli.md)
- [Azure PowerShell](tutorial-install-apps-powershell.md)
- [Modelo do Azure Resource Manager](tutorial-install-apps-template.md)


## <a name="install-an-app-to-a-windows-vm-with-powershell-dsc"></a>Instalar uma aplicação para uma VM com o PowerShell DSC do Windows
[Configuração de estado pretendido ' (DSC) da PowerShell](https://msdn.microsoft.com/powershell/dsc/overview) é uma plataforma de gestão para definir a configuração das máquinas de destino. Configurações de DSC definem o que instalar num computador e como configurar o anfitrião. Um motor de Gestor de configuração Local (MMC) é executado em cada nó de destino que processa o pedido ações com base nas configurações premidas.

A extensão de DSC do PowerShell permite-lhe personalizar a instâncias de VM um conjunto com o PowerShell de dimensionamento. O exemplo seguinte:

- Indica as instâncias de VM para transferir um pacote de DSC a partir do GitHub- *https://github.com/Azure-Samples/compute-automation-configurations/raw/master/dsc.zip*
- Define a extensão para executar um script de instalação- `configure-http.ps1`
- Obtém informações sobre um conjunto com de dimensionamento [Get AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss)
- Aplica-se a extensão para as instâncias de VM com [AzureRmVmss de atualização](/powershell/module/azurerm.compute/update-azurermvmss)

A extensão de DSC é aplicada à *myScaleSet* instâncias de VM no grupo de recursos com o nome *myResourceGroup*. Introduza os seus próprios nomes da seguinte forma:

```powershell
# Define the script for your Desired Configuration to download and run
$dscConfig = @{
  "wmfVersion" = "latest";
  "configuration" = @{
    "url" = "https://github.com/Azure-Samples/compute-automation-configurations/raw/master/dsc.zip";
    "script" = "configure-http.ps1";
    "function" = "WebsiteTest";
  };
}

# Get information about the scale set
$vmss = Get-AzureRmVmss `
                -ResourceGroupName "myResourceGroup" `
                -VMScaleSetName "myScaleSet"

# Add the Desired State Configuration extension to install IIS and configure basic website
$vmss = Add-AzureRmVmssExtension `
    -VirtualMachineScaleSet $vmss `
    -Publisher Microsoft.Powershell `
    -Type DSC `
    -TypeHandlerVersion 2.24 `
    -Name "DSC" `
    -Setting $dscConfig

# Update the scale set and apply the Desired State Configuration extension to the VM instances
Update-AzureRmVmss `
    -ResourceGroupName "myResourceGroup" `
    -Name "myScaleSet"  `
    -VirtualMachineScaleSet $vmss
```

Se a política de atualização no seu conjunto de dimensionamento é *manual*, atualizar as instâncias de VM com [atualização AzureRmVmssInstance](/powershell/module/azurerm.compute/update-azurermvmssinstance). Este cmdlet aplica-se a configuração de conjunto de dimensionamento atualizados para as instâncias de VM e instala a aplicação.


## <a name="install-an-app-to-a-linux-vm-with-cloud-init"></a>Instalar uma aplicação para uma VM com Linux com init de nuvem
[Cloud-init](https://cloudinit.readthedocs.io/latest/) é uma abordagem amplamente utilizada para personalizar uma VM com Linux quando arranca pela primeira vez. Pode utilizar o cloud-init para instalar pacotes e escrever ficheiros ou para configurar utilizadores e segurança. Como o cloud-init é executado durante o processo de arranque inicial, não existem passos adicionais ou agentes necessários a aplicar à configuração.

O cloud-init também funciona em distribuições. Por exemplo, não utiliza **apt-get install** nem **yum install** para instalar um pacote. Em vez disso, pode definir uma lista dos pacotes a instalar. O cloud-init utiliza automaticamente a ferramenta de gestão de pacotes nativa para a distribuição que selecionar.

Para obter mais informações, incluindo um exemplo *nuvem init.txt* de ficheiros, consulte [utilizar nuvem init para personalizar as VMs do Azure](../virtual-machines/linux/using-cloud-init.md).

Para criar um conjunto de dimensionamento e utilizar um ficheiro de nuvem init, adicione o `--custom-data` parâmetro para o [az vmss criar](/cli/azure/vmss#az_vmss_create) comando e especifique o nome de um ficheiro de nuvem init. O exemplo seguinte cria um conjunto nomeado de dimensionamento *myScaleSet* no *myResourceGroup* e configura as instâncias VM com um ficheiro denominado *nuvem init.txt*. Introduza os seus próprios nomes da seguinte forma:

```azurecli
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys
```


### <a name="install-applications-with-os-updates"></a>Instalar aplicações com as atualizações do SO
Quando estiverem disponíveis novas versões de SO, pode utilizar ou criar uma nova imagem personalizada e [implementar atualizações do SO](virtual-machine-scale-sets-upgrade-scale-set.md) para uma escala definido. Cada instância VM é atualizada para a imagem mais recente que especificar. Pode utilizar uma imagem personalizada com a aplicação previamente instalada, a extensão de Script personalizado ou PowerShell DSC ter a sua aplicação automaticamente disponível como efetuar a atualização. Terá de planear para manutenção de aplicação, como efetuar este processo para se certificar de que não existem nenhuma versão de problemas de compatibilidade.

Se utilizar uma imagem VM personalizada com a aplicação previamente instalada, pode integrar as atualizações de aplicação com um pipeline de implementação para as novas imagens de criar e implementar atualizações do SO entre o conjunto de dimensionamento. Esta abordagem permite que o pipeline recolher as compilações de aplicação mais recentes, criar e validar uma imagem de VM, em seguida, atualizar as instâncias de VM no conjunto de dimensionamento. Para executar um pipeline de implementação que cria e implementa atualizações de aplicações nas imagens VM personalizadas, foi [criar uma imagem de Packer e implementar com o Visual Studio Team Services](/vsts/pipelines/apps/cd/azure/deploy-azure-scaleset), ou utilizar outra plataforma como [Spinnaker ](https://www.spinnaker.io/) ou [Jenkins](https://jenkins.io/).


## <a name="next-steps"></a>Passos Seguintes
Como criar e implementar aplicações para os conjuntos de dimensionamento, pode rever o [descrição geral do Design definir dimensionamento](virtual-machine-scale-sets-design-overview.md). Para obter mais informações sobre como gerir o seu conjunto de dimensionamento, consulte [utilize o PowerShell para gerir o seu conjunto de dimensionamento](virtual-machine-scale-sets-windows-manage.md).
