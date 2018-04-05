---
title: Criar um conjunto de dimensionamento do Azure que utiliza disponibilidade zonas | Microsoft Docs
description: Saiba como criar conjuntos de dimensionamento de máquina virtual do Azure que utilizam zonas de disponibilidade para a redundância de aumento contra falhas
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: iainfou
ms.openlocfilehash: dee06eee045bc24c2864333a66a6d145a771b3ad
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones"></a>Criar um conjunto de dimensionamento de máquina virtual que utiliza as zonas de disponibilidade
Para proteger os conjuntos de dimensionamento de máquina virtual de falhas de nível de centro de dados, pode criar um conjunto através de zonas de disponibilidade de dimensionamento. Regiões do Azure que suportam zonas de disponibilidade tem um mínimo de três zonas separadas, cada um com os seus próprios independente de origem, rede e arrefecimento de energia. Para obter mais informações, consulte [zonas de descrição geral da disponibilidade](../availability-zones/az-overview.md).


## <a name="single-zone-and-zone-redundant-scale-sets"></a>Conjuntos de dimensionamento de zona único e com redundância de zona
Quando implementa um conjunto de dimensionamento de máquina virtual, pode optar por utilizar uma única zona de disponibilidade numa região ou várias zonas.

Quando cria um conjunto numa única zona, controlo que zona todas essas instâncias de VM executam no, e o conjunto de dimensionamento é gerido e autoscales apenas dentro desse horário de dimensionamento. Um conjunto de dimensionamento com redundância de zona permite-lhe criar um conjunto de dimensionamento única que abrange várias zonas. Como são criadas instâncias de VM, por predefinição, são uniformemente equilibrados nos zonas. Uma interrupção deve ocorrer das zonas de, um conjunto de dimensionamento não automaticamente aumentar horizontalmente para aumentar a capacidade. Uma melhor prática, seria possível configurar regras de dimensionamento automático com base na utilização de CPU ou memória. As regras de dimensionamento automático permitiria que o conjunto para responder a uma perda das instâncias de VM em que uma zona ao aumentar horizontalmente novas instâncias nas restantes zonas operacionais de dimensionamento.

Para utilizar zonas disponibilidade, o conjunto de dimensionamento tem de ser criado num [suportado região do Azure](../availability-zones/az-overview.md#regions-that-support-availability-zones). Pode criar um conjunto de dimensionamento que utiliza as zonas de disponibilidade com um dos seguintes métodos:

- [Portal do Azure](#use-the-azure-portal)
- [CLI 2.0 do Azure](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Modelos Azure Resource Manager](#use-azure-resource-manager-templates)

## <a name="availability-considerations"></a>Considerações de disponibilidade
A partir da API versão 2017-12-01, quando implementa um conjunto num ou mais zonas de dimensionamento, tem a opção de implementação com "propagando-se máximo" ou "estático 5 falhas domínio propagando-se". Com propagando-se máximo, o conjunto de dimensionamento propaga as suas VMs em como muitas domínios de falha possível dentro de cada zona. Este propagando-se dever-se em superior ou inferior a cinco domínios por zona de falha. Por outro lado, com "estático 5 falhas domínio propagando-se", o conjunto de dimensionamento propaga as suas VMs em domínios de falhas exatamente 5 por zona. Se o conjunto de dimensionamento não é possível localizar 5 domínios de falhas distinct por zona para satisfazer o pedido de atribuição, o pedido falha.

**Recomendamos a implementação com máximo propagando-se para a maioria das cargas de trabalho** porque propagando-se máximo fornece a melhor propagando-se na maioria dos casos. Se precisar de réplicas para serem distribuídos por unidades de isolamento de hardware distintos, recomendamos propagando-se em zonas de disponibilidade e utilização máxima propagando-se dentro de cada zona. Tenha em atenção que com propagando-se máximo, apenas verá um domínio de falhas na vista de instância VM de conjunto de escala e nos metadados da instância, independentemente de quantas domínios de falhas de VMs, na verdade, são distribuídas por; o propagando-se dentro de cada zona é implícita.

Para utilizar propagando-se máximo, defina "platformFaultDomainCount" para 1. Para utilizar estático 5 falhas domínio propagando-se, defina "platformFaultDomainCount" 5. Na API versão 2017-12-01, "platformFaultDomainCount" assume como 1 para conjuntos de dimensionamento única zona e a zona em vários locais. Atualmente, apenas estático 5 falhas domínio propagando-se é suportada para conjuntos de dimensionamento regional.

Além disso, quando implementa um conjunto de dimensionamento, tem a opção de implementação com um único [grupo colocação](./virtual-machine-scale-sets-placement-groups.md) por zona de disponibilidade ou com múltiplos por zona (para conjuntos de dimensionamento regionais, a escolha é ter um grupo de posicionamento único o região ou ter vários na região). Para a maioria das cargas de trabalho, recomendamos que utilize vários grupos de posicionamento, que permite uma escala maior. Na API versão 2017-12-01, conjuntos de dimensionamento predefinido a vários grupos de posicionamento para conjuntos de dimensionamento única zona e a zona em vários locais, mas estes predefinido para o grupo de posicionamento único para conjuntos de dimensionamento regional.

>[!NOTE]
> Se utilizar propagando-se máximo, tem de utilizar vários grupos de colocação.

Por fim, para conjuntos de dimensionamento implementados em várias zonas, também tem a opção de escolher "balance zona de melhor esforço" ou "balance zona strict". Um conjunto de dimensionamento é considerado "balanceamento" se o número de VMs em cada zona estiver dentro do número de VMs em todas as outras zonas para o conjunto de dimensionamento. Para a instância, um conjunto com 2 VMs na zona 1, 3 VMs na zona 2 e 3 VMs na zona 3 de dimensionamento é considerada balanceamento. No entanto, uma escala definida com 1 VM na zona 1, 3 VMs na zona é considerado VMs de 2 e 3 na zona 3 desequilibradam. É possível que as VMs do conjunto de dimensionamento são criadas com êxito, enquanto a extensão dessas VMs falhar. Estas VMs com falhas de extensão ainda são contadas ao determinar se um conjunto de dimensionamento é com balanceamento de. Por exemplo, um conjunto com 3 VMs na zona 1, 3 VMs na zona 2 e 3 VMs na zona 3 de dimensionamento é considerado balanceamento mesmo que todas as extensões de falha na zona 1 e todas as extensões foi concluída com êxito em zonas 2 e 3. Com balanceamento de zona melhor esforço, o conjunto de dimensionamento de tentativas para reduzir e ampliar mantendo balancear. No entanto, se por alguma razão não for possível (por exemplo, uma zona fica inativo, pelo que o conjunto de dimensionamento não é possível criar uma nova VM em que zona), em seguida, o conjunto de dimensionamento permitirá unbalance temporário para dimensionar com êxito a entrada ou saída. No subsequente ampliação tentativas, o conjunto de dimensionamento adiciona VMs zonas que necessitam de mais VMs para o conjunto equilibrada de dimensionamento. Da mesma forma, numa escala subsequente tentativas, o conjunto de dimensionamento remove VMs zonas que necessitam de menos de VMs para o conjunto equilibrada de dimensionamento. Por outro lado, com "balance zona strict", o conjunto de dimensionamento não consegue todas as tentativas para aumentar ou reduzir se tal poderia fazer unbalance.

Para utilizar o balanceamento de zona do melhor esforço, defina "zoneBalance" como FALSO (predefinição na API versão 2017-12-01). Para utilizar o balanceamento de zona strict, defina "zoneBalance" como verdadeiro.

## <a name="use-the-azure-portal"></a>Utilizar o portal do Azure
O processo de criação de um conjunto de dimensionamento que utiliza uma zona de disponibilidade é o mesmo, conforme detalhado no [introdução artigo](quick-create-portal.md). Certifique-se de que tem [registado para as zonas de disponibilidade de pré-visualização](http://aka.ms/azenroll). Quando seleciona uma região do Azure suportada, pode criar uma escala definida das zonas disponíveis, conforme mostrado no exemplo seguinte:

![Criar um conjunto numa única zona disponibilidade de dimensionamento](media/virtual-machine-scale-sets-use-availability-zones/create-portal-single-az.png)

O conjunto de dimensionamento e o suporte de recursos, tais como o Balanceador de carga do Azure e o endereço IP público, são criados na zona único que especificar.


## <a name="use-the-azure-cli-20"></a>Utilizar a CLI do Azure 2.0
O processo de criação de um conjunto de dimensionamento que utiliza uma zona de disponibilidade é o mesmo, conforme detalhado no [introdução artigo](quick-create-cli.md). Para utilizar zonas disponibilidade, tem de criar o conjunto de dimensionamento numa região do Azure suportada e ter [registado para as zonas de disponibilidade de pré-visualização](http://aka.ms/azenroll).

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
Para criar uma escala com redundância de zona definido, que utiliza um *padrão* SKU IP endereço e a carga balanceador público. Para fins de redundância melhorado, o *padrão* SKU cria recursos de rede com redundância de zona. Para obter mais informações, consulte [descrição geral do Balanceador de carga do Azure Standard](../load-balancer/load-balancer-standard-overview.md). 

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
O processo de criação de um conjunto de dimensionamento que utiliza uma zona de disponibilidade é o mesmo, conforme detalhado no [introdução artigo](quick-create-powershell.md). Para utilizar zonas disponibilidade, tem de criar o conjunto de dimensionamento numa região do Azure suportada e ter [registado para as zonas de disponibilidade de pré-visualização](http://aka.ms/azenroll). Adicionar o `-Zone` parâmetro para o [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) de comandos e especificar qual horário a utilizar (como zona *1*, *2*, ou *3*). 

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
Para criar uma escala com redundância de zona definido, que utiliza um *padrão* SKU IP endereço e a carga balanceador público. Para fins de redundância melhorado, o *padrão* SKU cria recursos de rede com redundância de zona. Para obter mais informações, consulte [descrição geral do Balanceador de carga do Azure Standard](../load-balancer/load-balancer-standard-overview.md).

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
O processo de criação de um conjunto de dimensionamento que utiliza uma zona de disponibilidade é o mesmo, conforme detalhado no artigo Introdução ao obter para [Linux](quick-create-template-linux.md) ou [Windows](quick-create-template-windows.md). Para utilizar zonas disponibilidade, tem de criar o conjunto de dimensionamento numa região do Azure suportada e ter [registado para as zonas de disponibilidade de pré-visualização](http://aka.ms/azenroll). Adicionar o `zones` propriedade para o *Microsoft.Compute/virtualMachineScaleSets* recursos escreva no seu modelo e especifique que horário a utilizar (como zona *1*, *2*, ou *3*).

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

Para obter um exemplo completo da escala de uma única zona definido e recursos de rede, consulte [este modelo de Gestor de recursos de exemplo](https://github.com/Azure/vm-scale-sets/blob/master/zones/singlezone.json)

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

Para obter um exemplo completo de uma escala com redundância de zona definido e recursos de rede, consulte [este modelo de Gestor de recursos de exemplo](https://github.com/Azure/vm-scale-sets/blob/master/zones/multizone.json)


## <a name="next-steps"></a>Passos seguintes
Agora que criou uma escala definida para um horário de disponibilidade, pode saber como [conjuntos de dimensionamento de implementar aplicações na máquina virtual](virtual-machine-scale-sets-deploy-app.md) ou [utilizar dimensionamento automático com conjuntos de dimensionamento de máquina virtual](virtual-machine-scale-sets-autoscale-overview.md).
