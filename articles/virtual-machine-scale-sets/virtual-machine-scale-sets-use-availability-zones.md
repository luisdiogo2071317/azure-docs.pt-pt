---
title: "Criar um conjunto de dimensionamento do Azure que utiliza zonas de disponibilidade (pré-visualização) | Microsoft Docs"
description: "Saiba como criar conjuntos de dimensionamento de máquina virtual do Azure que utilizam zonas de disponibilidade para a redundância de aumento contra falhas"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: iainfou
ms.openlocfilehash: 310fdc68d3eb662906053d2bc0c45e6cfa18d4da
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2018
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones-preview"></a>Criar um conjunto de dimensionamento de máquina virtual que utiliza zonas de disponibilidade (pré-visualização)
Para proteger os conjuntos de dimensionamento de máquina virtual de falhas de nível de centro de dados, pode criar uma escala definida para um horário de disponibilidade. Regiões do Azure que suportam zonas de disponibilidade tem um mínimo de três zonas separadas, cada um com os seus próprios independente de origem, rede e arrefecimento de energia. Para obter mais informações, consulte [zonas de descrição geral da disponibilidade](../availability-zones/az-overview.md).

Para utilizar zonas disponibilidade, o conjunto de dimensionamento tem de ser criado num [suportado região do Azure](../availability-zones/az-overview.md#regions-that-support-availability-zones). Pode criar um conjunto de dimensionamento que utiliza as zonas de disponibilidade com um dos seguintes métodos:

- [Portal do Azure](#use-the-azure-portal)
- [CLI 2.0 do Azure](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Modelos Azure Resource Manager](#use-azure-resource-manager-templates)


## <a name="use-the-azure-portal"></a>Utilizar o portal do Azure
O processo de criação de um conjunto de dimensionamento que utiliza uma zona de disponibilidade é o mesmo, conforme detalhado no [introdução artigo](virtual-machine-scale-sets-create-portal.md). Quando seleciona uma região do Azure suportada, pode criar uma escala definida das zonas disponíveis, conforme mostrado no exemplo seguinte:

![Criar um conjunto numa única zona disponibilidade de dimensionamento](media/virtual-machine-scale-sets-use-availability-zones/create-portal-single-az.png)

O conjunto de dimensionamento e o suporte de recursos, tais como o Balanceador de carga do Azure e o endereço IP público, são criados na zona único que especificar.


## <a name="use-the-azure-cli-20"></a>Utilizar a CLI do Azure 2.0
O processo de criação de um conjunto de dimensionamento que utiliza uma zona de disponibilidade é o mesmo, conforme detalhado no [introdução artigo](virtual-machine-scale-sets-create-cli.md). Para utilizar zonas disponibilidade, tem de criar o conjunto numa região do Azure suportada de dimensionamento. Adicionar o `--zones` parâmetro para o [az vmss criar](/cli/azure/vmss#az_vmss_create) de comandos e especificar qual horário a utilizar (tais como zona *1*, *2*, ou *3*). O exemplo seguinte cria um conjunto nomeado de dimensionamento *myScaleSet* na zona *1*:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --zones 1
```

Demora alguns minutos para criar e configurar todos os do conjunto de dimensionamento de VMs e de recursos na zona que especificar.


## <a name="use-azure-powershell"></a>Utilizar o Azure PowerShell
O processo de criação de um conjunto de dimensionamento que utiliza uma zona de disponibilidade é o mesmo, conforme detalhado no [introdução artigo](virtual-machine-scale-sets-create-powershell.md). Para utilizar zonas disponibilidade, tem de criar o conjunto numa região do Azure suportada de dimensionamento. Adicionar o `-Zone` parâmetro para o [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) de comandos e especificar qual horário a utilizar (como zona *1*, *2*, ou *3*). O exemplo seguinte cria uma configuração de conjunto de dimensionamento com o nome *vmssConfig* no *EUA Leste 2* zona *1*:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Zone "1"
```

Para criar o conjunto de dimensionamento real, siga os passos adicionais, conforme detalhado no [introdução artigo](virtual-machine-scale-sets-create-powershell.md).


## <a name="use-azure-resource-manager-templates"></a>Use Azure Resource Manager templates (Utilizar modelos do Azure Resource Manager)
O processo de criação de um conjunto de dimensionamento que utiliza uma zona de disponibilidade é o mesmo, conforme detalhado no artigo Introdução ao obter para [Linux](virtual-machine-scale-sets-create-template-linux.md) ou [Windows](virtual-machine-scale-sets-create-template-windows.md). Para utilizar zonas disponibilidade, tem de criar o conjunto numa região do Azure suportada de dimensionamento. Adicionar o `zones` propriedade para o *Microsoft.Compute/virtualMachineScaleSets* recursos escreva no seu modelo e especifique que horário a utilizar (como zona *1*, *2*, ou *3*). O exemplo seguinte cria um conjunto nomeado de dimensionamento de Linux *myScaleSet* no *EUA Leste 2* zona *1*:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2017-12-01",
  "zones": ["1"],
  "sku": {
    "name": "Standard_A1",
    "capacity": "2"
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
      "storageProfile": {
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage"
        },
        "imageReference":  {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "16.04-LTS",
          "version": "latest"
        }
      },
      "osProfile": {
        "computerNamePrefix": "myvmss",
        "adminUsername": "azureuser",
        "adminPassword": "P@ssw0rd!"
      }
    }
  }
}
```

Para criar o conjunto de dimensionamento real, siga os passos adicionais, conforme detalhado no artigo Introdução ao obter para [Linux](virtual-machine-scale-sets-create-template-linux.md) ou [Windows](virtual-machine-scale-sets-create-template-windows.md)


## <a name="next-steps"></a>Passos Seguintes
Agora que criou uma escala definida para um horário de disponibilidade, pode saber como [conjuntos de dimensionamento de implementar aplicações na máquina virtual](virtual-machine-scale-sets-deploy-app.md) ou [utilizar dimensionamento automático com conjuntos de dimensionamento de máquina virtual](virtual-machine-scale-sets-autoscale-overview.md).
