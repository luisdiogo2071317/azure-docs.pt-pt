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
ms.date: 01/11/2018
ms.author: iainfou
ms.openlocfilehash: 397afc28b5f4c4f7f84afde13b6d031d83aaced4
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones-preview"></a>Criar um conjunto de dimensionamento de máquina virtual que utiliza zonas de disponibilidade (pré-visualização)
Para proteger os conjuntos de dimensionamento de máquina virtual de falhas de nível de centro de dados, pode criar um conjunto através de zonas de disponibilidade de dimensionamento. Regiões do Azure que suportam zonas de disponibilidade tem um mínimo de três zonas separadas, cada um com os seus próprios independente de origem, rede e arrefecimento de energia. Para obter mais informações, consulte [zonas de descrição geral da disponibilidade](../availability-zones/az-overview.md).

[!INCLUDE [availability-zones-preview-statement.md](../../includes/availability-zones-preview-statement.md)]


## <a name="single-zone-and-zone-redundant-scale-sets"></a>Conjuntos de dimensionamento de zona único e com redundância de zona
Quando implementa um conjunto de dimensionamento de máquina virtual, pode optar por utilizar uma única zona de disponibilidade numa região ou várias zonas.

Quando cria um conjunto numa única zona, controlo que zona todas essas instâncias de VM executam no, e o conjunto de dimensionamento é gerido e autoscales apenas dentro desse horário de dimensionamento. O diagrama seguinte mostra um exemplo de como pode criar várias única zona escala define com um balanceador de carga com redundância de zona que distribui o tráfego:

![Conjunto de dimensionamento de zona único de implementação com o Balanceador de carga com redundância de zona](media/virtual-machine-scale-sets-use-availability-zones/zonal-vmss.png)

Um conjunto de dimensionamento com redundância de zona permite-lhe criar um conjunto de dimensionamento única que abrange várias zonas. Como são criadas instâncias de VM, por predefinição, são uniformemente equilibrados nos zonas. Uma interrupção deve ocorrer das zonas de, um conjunto de dimensionamento não automaticamente aumentar horizontalmente para aumentar a capacidade. Uma melhor prática, seria possível configurar regras de dimensionamento automático com base na utilização de CPU ou memória. As regras de dimensionamento automático permitiria que o conjunto para responder a uma perda das instâncias de VM em que uma zona ao aumentar horizontalmente novas instâncias nas restantes zonas operacionais de dimensionamento. O diagrama seguinte mostra um exemplo de um conjunto de dimensionamento única que é implementado em várias zonas:

![Dimensionamento zonal redundante definir implementação e o Balanceador de carga](media/virtual-machine-scale-sets-use-availability-zones/zone-redundant-vmss.png)

Para utilizar zonas disponibilidade, o conjunto de dimensionamento tem de ser criado num [suportado região do Azure](../availability-zones/az-overview.md#regions-that-support-availability-zones). Terá também de [registar para as zonas de disponibilidade de pré-visualização](http://aka.ms/azenroll). Pode criar um conjunto de dimensionamento que utiliza as zonas de disponibilidade com um dos seguintes métodos:

- [Portal do Azure](#use-the-azure-portal)
- [CLI 2.0 do Azure](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Modelos Azure Resource Manager](#use-azure-resource-manager-templates)


## <a name="use-the-azure-portal"></a>Utilizar o portal do Azure
O processo de criação de um conjunto de dimensionamento que utiliza uma zona de disponibilidade é o mesmo, conforme detalhado no [introdução artigo](virtual-machine-scale-sets-create-portal.md). Certifique-se de que tem [registado para as zonas de disponibilidade de pré-visualização](http://aka.ms/azenroll). Quando seleciona uma região do Azure suportada, pode criar uma escala definida das zonas disponíveis, conforme mostrado no exemplo seguinte:

![Criar um conjunto numa única zona disponibilidade de dimensionamento](media/virtual-machine-scale-sets-use-availability-zones/create-portal-single-az.png)

O conjunto de dimensionamento e o suporte de recursos, tais como o Balanceador de carga do Azure e o endereço IP público, são criados na zona único que especificar.


## <a name="use-the-azure-cli-20"></a>Utilizar a CLI do Azure 2.0
O processo de criação de um conjunto de dimensionamento que utiliza uma zona de disponibilidade é o mesmo, conforme detalhado no [introdução artigo](virtual-machine-scale-sets-create-cli.md). Para utilizar zonas disponibilidade, tem de criar o conjunto de dimensionamento numa região do Azure suportada e ter [registado para as zonas de disponibilidade de pré-visualização](http://aka.ms/azenroll).

Adicionar o `--zones` parâmetro para o [az vmss criar](/cli/azure/vmss#az_vmss_create) de comandos e especificar qual horário a utilizar (tais como zona *1*, *2*, ou *3*). O exemplo seguinte cria um conjunto nomeado de dimensionamento de zona único *myScaleSet* na zona *1*:

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
Para obter um exemplo completo da escala de uma única zona definido e recursos de rede, consulte [este script de exemplo do CLI](https://github.com/Azure/azure-docs-cli-python-samples/blob/master/virtual-machine-scale-sets/create-single-availability-zone/create-single-availability-zone.sh.)

### <a name="zone-redundant-scale-set"></a>Conjunto de dimensionamento com redundância de zona
Para criar uma escala com redundância de zona definido, que utiliza um *padrão* SKU IP endereço e a carga balanceador público. Para fins de redundância melhorado, o *padrão* SKU cria recursos de rede com redundância de zona. Para obter mais informações, consulte [descrição geral do Balanceador de carga do Azure Standard](../load-balancer/load-balancer-standard-overview.md). Na primeira vez que cria uma escala com redundância de zona definida ou o Balanceador de carga, tem de concluir os seguintes passos para registar a sua conta para estas funcionalidades de pré-visualização.

1. Registar a sua conta para o conjunto de dimensionamento com redundância de zona e a rede funcionalidades com [o registo de funcionalidade az](/cli/azure/feature#az_feature_register) da seguinte forma:

    ```azurecli
    az feature register --name MultipleAvailabilityZones --namespace Microsoft.Compute
    az feature register --name AllowLBPreview --namespace Microsoft.Network
    ```
    
2. Pode demorar alguns minutos para se registar para as funcionalidades. Pode verificar o estado da operação com [mostrar de funcionalidade az](/cli/azure/feature#az_feature_show):

    ```azurecli
    az feature show --name MultipleAvailabilityZones --namespace Microsoft.Compute
    az feature show --name AllowLBPreview --namespace Microsoft.Network
    ```

    O exemplo seguinte mostra o estado pretendido da funcionalidade como *registada*:
    
    ```json
    "properties": {
          "state": "Registered"
       },
    ```

3. Depois de definir de escala com redundância de zona e de rede de recursos de relatórios como *registada*, volte a registar o *computação* e *rede* fornecedores com [az o registo de fornecedor](/cli/azure/provider#az_provider_register) da seguinte forma:

    ```azurecli
    az provider register --namespace Microsoft.Compute
    az provider register --namespace Microsoft.Network
    ```

Para criar um conjunto de dimensionamento com redundância de zona, especificar várias zonas com o `--zones` parâmetro. O exemplo seguinte cria um conjunto nomeado de dimensionamento com redundância de zona *myScaleSet* em horários *1,2,3*:

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --admin-username azureuser \
    --generate-ssh-keys \
    --zones {1,2,3}
```

Demora alguns minutos para criar e configurar todos os o conjunto de dimensionamento de VMs e de recursos no zone(s) que especificar. Para obter um exemplo completo de uma escala com redundância de zona definido e recursos de rede, consulte [este script de exemplo do CLI](https://github.com/Azure/azure-docs-cli-python-samples/blob/master/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.sh)


## <a name="use-azure-powershell"></a>Utilizar o Azure PowerShell
O processo de criação de um conjunto de dimensionamento que utiliza uma zona de disponibilidade é o mesmo, conforme detalhado no [introdução artigo](virtual-machine-scale-sets-create-powershell.md). Para utilizar zonas disponibilidade, tem de criar o conjunto de dimensionamento numa região do Azure suportada e ter [registado para as zonas de disponibilidade de pré-visualização](http://aka.ms/azenroll). Adicionar o `-Zone` parâmetro para o [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) de comandos e especificar qual horário a utilizar (como zona *1*, *2*, ou *3*). 

O exemplo seguinte cria uma configuração de conjunto de dimensionamento de zona único com o nome *vmssConfig* no *EUA Leste 2* zona *1*:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Zone "1"
```

Para obter um exemplo completo da escala de uma única zona definido e recursos de rede, consulte [este script do PowerShell de exemplo](https://github.com/Azure/azure-docs-powershell-samples/blob/master/virtual-machine-scale-sets/create-single-availability-zone/create-single-availability-zone.ps1)

### <a name="zone-redundant-scale-set"></a>Conjunto de dimensionamento com redundância de zona
Para criar uma escala com redundância de zona definido, que utiliza um *padrão* SKU IP endereço e a carga balanceador público. Para fins de redundância melhorado, o *padrão* SKU cria recursos de rede com redundância de zona. Para obter mais informações, consulte [descrição geral do Balanceador de carga do Azure Standard](../load-balancer/load-balancer-standard-overview.md). Na primeira vez que cria uma escala com redundância de zona definida ou o Balanceador de carga, tem de concluir os seguintes passos para registar a sua conta para estas funcionalidades de pré-visualização.

1. Registar a sua conta para o conjunto de dimensionamento com redundância de zona e a rede funcionalidades com [Register-AzureRmProviderFeature](/powershell/module/azurerm.resources/register-azurermproviderfeature) da seguinte forma:

    ```powershell
    Register-AzureRmProviderFeature -FeatureName MultipleAvailabilityZones -ProviderNamespace Microsoft.Compute
    Register-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```
    
2. Pode demorar alguns minutos para se registar para as funcionalidades. Pode verificar o estado da operação com [Get-AzureRmProviderFeature](/powershell/module/AzureRM.Resources/Get-AzureRmProviderFeature):

    ```powershell
    Get-AzureRmProviderFeature -FeatureName MultipleAvailabilityZones -ProviderNamespace Microsoft.Compute 
    Get-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

    O exemplo seguinte mostra o estado pretendido da funcionalidade como *registada*:
    
    ```powershell
    RegistrationState
    -----------------
    Registered
    ```

3. Depois de definir de escala com redundância de zona e de rede de recursos de relatórios como *registada*, volte a registar o *computação* e *rede* fornecedores com [ Registar AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider) da seguinte forma:

    ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ```

Para criar um conjunto de dimensionamento com redundância de zona, especificar várias zonas com o `-Zone` parâmetro. O exemplo seguinte cria uma configuração de conjunto de dimensionamento com redundância de zona com o nome *myScaleSet* em *EUA Leste 2* zonas *1, 2, 3*:

```powershell
$vmssConfig = New-AzureRmVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Zone "1", "2", "3"
```

Se criar um endereço IP público com [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) ou um balanceador de carga com [New-AzureRmLoadBalancer](/powershell/module/AzureRM.Network/New-AzureRmLoadBalancer), especifique o *- SKU "Standard"* para criar recursos de rede com redundância de zona. Também terá de criar um grupo de segurança de rede e as regras para permitir qualquer tráfego. Para obter mais informações, consulte [descrição geral do Balanceador de carga do Azure Standard](../load-balancer/load-balancer-standard-overview.md).

Para obter um exemplo completo de uma escala com redundância de zona definido e recursos de rede, consulte [este script do PowerShell de exemplo](https://github.com/Azure/azure-docs-powershell-samples/blob/master/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.ps1)


## <a name="use-azure-resource-manager-templates"></a>Use Azure Resource Manager templates (Utilizar modelos do Azure Resource Manager)
O processo de criação de um conjunto de dimensionamento que utiliza uma zona de disponibilidade é o mesmo, conforme detalhado no artigo Introdução ao obter para [Linux](virtual-machine-scale-sets-create-template-linux.md) ou [Windows](virtual-machine-scale-sets-create-template-windows.md). Para utilizar zonas disponibilidade, tem de criar o conjunto de dimensionamento numa região do Azure suportada e ter [registado para as zonas de disponibilidade de pré-visualização](http://aka.ms/azenroll). Adicionar o `zones` propriedade para o *Microsoft.Compute/virtualMachineScaleSets* recursos escreva no seu modelo e especifique que horário a utilizar (como zona *1*, *2*, ou *3*).

O exemplo seguinte cria um conjunto nomeado de dimensionamento do única zona Linux *myScaleSet* no *EUA Leste 2* zona *1*:

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

Para obter um exemplo completo da escala de uma única zona definido e recursos de rede, consulte [este modelo de Gestor de recursos de exemplo](https://github.com/Azure/vm-scale-sets/blob/master/preview/zones/singlezone.json)

### <a name="zone-redundant-scale-set"></a>Conjunto de dimensionamento com redundância de zona
Para criar um conjunto de dimensionamento com redundância de zona, especificar vários valores no `zones` propriedade para o *Microsoft.Compute/virtualMachineScaleSets* tipo de recurso. O exemplo seguinte cria um conjunto nomeado de dimensionamento com redundância de zona *myScaleSet* em *EUA Leste 2* zonas *1,2,3*:

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US 2",
  "apiVersion": "2017-12-01",
  "zones": [
        "1",
        "2",
        "3"
      ]
}
```

Se criar um endereço IP público ou um balanceador de carga, especifique o *"sku": {"name": "Padrão"} "* propriedade para criar recursos de rede com redundância de zona. Também terá de criar um grupo de segurança de rede e as regras para permitir qualquer tráfego. Para obter mais informações, consulte [descrição geral do Balanceador de carga do Azure Standard](../load-balancer/load-balancer-standard-overview.md).

Para obter um exemplo completo de uma escala com redundância de zona definido e recursos de rede, consulte [este modelo de Gestor de recursos de exemplo](https://github.com/Azure/vm-scale-sets/blob/master/preview/zones/multizone.json)


## <a name="next-steps"></a>Passos Seguintes
Agora que criou uma escala definida para um horário de disponibilidade, pode saber como [conjuntos de dimensionamento de implementar aplicações na máquina virtual](virtual-machine-scale-sets-deploy-app.md) ou [utilizar dimensionamento automático com conjuntos de dimensionamento de máquina virtual](virtual-machine-scale-sets-autoscale-overview.md).
