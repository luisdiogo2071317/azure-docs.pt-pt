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
ms.date: 04/05/2018
ms.author: iainfou
ms.openlocfilehash: 9093fcf008f199cc1a78d50979f6387a2e1b5262
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="create-a-virtual-machine-scale-set-that-uses-availability-zones"></a>Criar um conjunto de dimensionamento de máquina virtual que utiliza as zonas de disponibilidade

Para proteger os conjuntos de dimensionamento de máquina virtual de falhas de nível de centro de dados, pode criar um conjunto através de zonas de disponibilidade de dimensionamento. Regiões do Azure que suportam zonas de disponibilidade tem um mínimo de três zonas separadas, cada um com os seus próprios independente de origem, rede e arrefecimento de energia. Para obter mais informações, consulte [zonas de descrição geral da disponibilidade](../availability-zones/az-overview.md).

## <a name="availability-considerations"></a>Considerações de disponibilidade

Quando implementa um conjunto num ou mais zonas a partir da versão da API de dimensionamento *2017-12-01*, tem a opção para implementar com "propagando-se máximo" ou "estático 5 falhas domínio propagando-se". Com propagando-se máximo, o conjunto de dimensionamento propaga as suas VMs em como muitas domínios de falha possível dentro de cada zona. Este propagando-se dever-se em superior ou inferior a cinco domínios por zona de falha. Com "estático 5 falhas domínio propagando-se", o conjunto de dimensionamento propaga as suas VMs em domínios de falhas de cinco exatamente por zona. Se o conjunto de dimensionamento não é possível localizar a cinco domínios de falhas distinct por zona para satisfazer o pedido de atribuição, o pedido falha.

**Recomendamos a implementação com máximo propagando-se para a maioria das cargas de trabalho**, como esta abordagem fornece a melhor propagando-se na maioria dos casos. Se precisar de réplicas para serem distribuídos por unidades de isolamento de hardware diferente, é recomendável propagando-se em zonas de disponibilidade e utilizar máximo propagando-se dentro de cada zona.

Com propagando-se máximo, verá apenas um domínio de falhas na vista de instância VM de conjunto de escala e nos metadados, independentemente de quantas domínios de falhas as VMs que são distribuídas por instância. O propagando-se dentro de cada zona é implícita.

Para utilizar propagando-se máximo, definidas *platformFaultDomainCount* para *1*. Para utilizar estático falhas cinco domínio propagando-se, defina *platformFaultDomainCount* para *5*. Na versão de API *2017-12-01*, *platformFaultDomainCount* assume a predefinição *1* para escala única zona e zona entre define. Atualmente, apenas estático falhas cinco domínio propagando-se é suportada para conjuntos de dimensionamento regional.

### <a name="placement-groups"></a>Grupos de colocação

Quando implementa um conjunto de dimensionamento, também tem a opção para implementar com um único [grupo colocação](./virtual-machine-scale-sets-placement-groups.md) por zona de disponibilidade ou com múltiplos por zona. Para conjuntos de dimensionamento regionais, a escolha é ter um grupo de posicionamento único na região ou ter vários na região. Para a maioria das cargas de trabalho, recomendamos que vários grupos de posicionamento, que permite uma escala maior. Na versão de API *2017-12-01*, conjuntos de dimensionamento predefinido a vários grupos de posicionamento para conjuntos de dimensionamento única zona e a zona em vários locais, mas estes predefinido para o grupo de posicionamento único para conjuntos de dimensionamento regional.

> [!NOTE]
> Se utilizar propagando-se máximo, tem de utilizar vários grupos de colocação.

### <a name="zone-balancing"></a>Balanceamento de zona

Por fim, para conjuntos de dimensionamento implementados em várias zonas, também tem a opção de escolher "balance zona de melhor esforço" ou "balance zona strict". Um conjunto de dimensionamento é considerado "balanceamento" se o número de VMs em cada zona estiver dentro do número de VMs em todas as outras zonas para o conjunto de dimensionamento. Por exemplo:

- Com balanceamento de um conjunto com 2 VMs na zona 1, 3 VMs na zona é considerado VMs de 2 e 3 na zona 3 de dimensionamento.
- Uma escala definida com 1 VM na zona 1, 3 VMs na zona é considerado VMs de 2 e 3 na zona 3 desequilibradam.

É possível que as VMs do conjunto de dimensionamento são criadas com êxito, mas extensões nessas VMs falharem implementar. Estas VMs com falhas de extensão ainda são contadas ao determinar se um conjunto de dimensionamento é com balanceamento de. Por exemplo, um conjunto com 3 VMs na zona 1, 3 VMs na zona 2 e 3 VMs na zona 3 de dimensionamento é considerado balanceamento mesmo que todas as extensões de falha na zona 1 e todas as extensões foi concluída com êxito em zonas 2 e 3.

Com balanceamento de zona de melhor esforço, o conjunto de dimensionamento de tentativas para reduzir e ampliar mantendo balancear. No entanto, se por alguma razão não for possível (por exemplo, se uma zona ficar inativo, o conjunto de dimensionamento não é possível criar uma nova VM em que zona), o conjunto de dimensionamento permite desequilíbrio temporário com êxito aumentar ou reduzir. Em tentativas de escalamento horizontal subsequentes, o conjunto de dimensionamento adiciona VMs zonas que necessitam de mais VMs para o conjunto equilibrada de dimensionamento. Da mesma forma, numa escala subsequente tentativas, o conjunto de dimensionamento remove VMs zonas que necessitam de menos de VMs para o conjunto equilibrada de dimensionamento. Com "balance zona strict", o conjunto de dimensionamento falha todas as tentativas para aumentar ou reduzir se tal poderia fazer unbalance.

Para utilizar o balanceamento de zona de melhor esforço, defina *zoneBalance* para *falso*. Esta é a predefinição na versão de API *2017-12-01*. Para utilizar o balanceamento de zona strict, defina *zoneBalance* para *verdadeiro*.

## <a name="single-zone-and-zone-redundant-scale-sets"></a>Conjuntos de dimensionamento de zona único e com redundância de zona

Quando implementa um conjunto de dimensionamento de máquina virtual, pode optar por utilizar uma única zona de disponibilidade numa região ou várias zonas.

Quando cria um conjunto numa única zona, controlo que zona todas essas instâncias de VM executam no, e o conjunto de dimensionamento é gerido e autoscales apenas dentro desse horário de dimensionamento. Um conjunto de dimensionamento com redundância de zona permite-lhe criar um conjunto de dimensionamento única que abrange várias zonas. Como são criadas instâncias de VM, por predefinição, são uniformemente equilibrados nos zonas. Uma interrupção deve ocorrer das zonas de, um conjunto de dimensionamento não automaticamente aumentar horizontalmente para aumentar a capacidade. Uma melhor prática, seria possível configurar regras de dimensionamento automático com base na utilização de CPU ou memória. As regras de dimensionamento automático permitiria que o conjunto para responder a uma perda das instâncias de VM em que uma zona ao aumentar horizontalmente novas instâncias nas restantes zonas operacionais de dimensionamento.

Para utilizar zonas disponibilidade, o conjunto de dimensionamento tem de ser criado num [suportado região do Azure](../availability-zones/az-overview.md#regions-that-support-availability-zones). Pode criar um conjunto de dimensionamento que utiliza as zonas de disponibilidade com um dos seguintes métodos:

- [Portal do Azure](#use-the-azure-portal)
- [CLI 2.0 do Azure](#use-the-azure-cli-20)
- [Azure PowerShell](#use-azure-powershell)
- [Modelos Azure Resource Manager](#use-azure-resource-manager-templates)

## <a name="use-the-azure-portal"></a>Utilizar o portal do Azure

O processo de criação de um conjunto de dimensionamento que utiliza uma zona de disponibilidade é o mesmo, conforme detalhado no [introdução artigo](quick-create-portal.md). Quando seleciona uma região do Azure suportada, pode criar uma escala definida num ou mais zonas disponíveis, conforme mostrado no exemplo seguinte:

![Criar um conjunto numa única zona disponibilidade de dimensionamento](media/virtual-machine-scale-sets-use-availability-zones/vmss-az-portal.png)

O conjunto de dimensionamento e o suporte de recursos, tais como o Balanceador de carga do Azure e o endereço IP público, são criados na zona único que especificar.

## <a name="use-the-azure-cli-20"></a>Utilizar a CLI do Azure 2.0

O processo de criação de um conjunto de dimensionamento que utiliza uma zona de disponibilidade é o mesmo, conforme detalhado no [introdução artigo](quick-create-cli.md). Para utilizar zonas disponibilidade, tem de criar o conjunto numa região do Azure suportada de dimensionamento.

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
    --zones 1 2 3
```

Demora alguns minutos para criar e configurar todos os o conjunto de dimensionamento de VMs e de recursos no zone(s) que especificar. Para obter um exemplo completo de uma escala com redundância de zona definido e recursos de rede, consulte [este script de exemplo do CLI](https://github.com/Azure/azure-docs-cli-python-samples/blob/master/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.sh)

## <a name="use-azure-powershell"></a>Utilizar o Azure PowerShell

Para utilizar zonas disponibilidade, tem de criar o conjunto numa região do Azure suportada de dimensionamento. Adicionar o `-Zone` parâmetro para o [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) de comandos e especificar qual horário a utilizar (como zona *1*, *2*, ou *3*).

O exemplo seguinte cria um conjunto nomeado de dimensionamento de zona único *myScaleSet* no *EUA Leste 2* zona *1*. Os recursos de rede do Azure para a rede virtual, o endereço IP público e o balanceador de carga são criados automaticamente. Quando solicitado, forneça as suas próprias credenciais administrativas pretendidas para as instâncias de VM no conjunto de dimensionamento:

```powershell
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS2" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicy "Automatic" `
  -Zone "1"
```

### <a name="zone-redundant-scale-set"></a>Conjunto de dimensionamento com redundância de zona

Para criar um conjunto de dimensionamento com redundância de zona, especificar várias zonas com o `-Zone` parâmetro. O exemplo seguinte cria um conjunto nomeado de dimensionamento com redundância de zona *myScaleSet* em *EUA Leste 2* zonas *1, 2, 3*. Os recursos de rede do Azure com redundância de zona de rede virtual, o endereço IP público e o Balanceador de carga são criados automaticamente. Quando solicitado, forneça as suas próprias credenciais administrativas pretendidas para as instâncias de VM no conjunto de dimensionamento:

```powershell
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS2" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicy "Automatic" `
  -Zone "1", "2", "3"
```

## <a name="use-azure-resource-manager-templates"></a>Use Azure Resource Manager templates (Utilizar modelos do Azure Resource Manager)

O processo de criação de um conjunto de dimensionamento que utiliza uma zona de disponibilidade é o mesmo, conforme detalhado no artigo Introdução ao obter para [Linux](quick-create-template-linux.md) ou [Windows](quick-create-template-windows.md). Para utilizar zonas disponibilidade, tem de criar o conjunto numa região do Azure suportada de dimensionamento. Adicionar o `zones` propriedade para o *Microsoft.Compute/virtualMachineScaleSets* recursos escreva no seu modelo e especifique que horário a utilizar (como zona *1*, *2*, ou *3*).

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

## <a name="next-steps"></a>Passos Seguintes

Agora que criou uma escala definida para um horário de disponibilidade, pode saber como [conjuntos de dimensionamento de implementar aplicações na máquina virtual](tutorial-install-apps-cli.md) ou [utilizar dimensionamento automático com conjuntos de dimensionamento de máquina virtual](tutorial-autoscale-cli.md).
