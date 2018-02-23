---
title: Criar uma rede virtual - CLI do Azure | Microsoft Docs
description: "Saiba mais rapidamente criar uma rede virtual com a CLI do Azure. Uma rede virtual permite que vários tipos de recursos do Azure em privado comuniquem entre si."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 01/25/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 2cb32ddc67060d9860d172b90cc399622c52b04b
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-virtual-network-using-the-azure-cli"></a>Criar uma rede virtual com a CLI do Azure

Neste artigo, irá aprender a criar uma rede virtual. Depois de criar uma rede virtual, implementar duas máquinas virtuais numa rede virtual e comunicar em privado entre eles.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0.4 ou posterior da CLI do Azure. Para localizar a versão instalada, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az_group_create). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos. 

O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*. Todos os recursos do Azure são criados dentro de uma localização do Azure (ou região).

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

Criar uma rede virtual com o [az rede vnet criar](/cli/azure/network/vnet#az_network_vnet_create) comando. O exemplo seguinte cria uma rede virtual da predefinição denominada *myVirtualNetwork* com uma sub-rede designada *predefinido*. Uma vez que uma localização não está especificada, o Azure cria a rede virtual na mesma localização que o grupo de recursos.

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --subnet-name default
```

Depois de criar a rede virtual, uma parte de informação devolvida é a seguinte.

```azurecli
"newVNet": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/16"
```

Todas as redes virtuais têm um ou mais os prefixos de endereço atribuídos aos mesmos. Uma vez que um prefixo de endereço não foi especificado ao criar a rede virtual, o Azure definido o espaço de endereços 10.0.0.0/16, por predefinição. Foi especificado o espaço de endereços em notação CIDR. O endereço espaço 10.0.0.0/16 abrange 10.0.0.0-10.0.255.254.

Outra parte das informações devolvido é o **addressPrefix** de *10.0.0.0/24* para o *predefinido* sub-rede especificada no comando. Uma rede virtual contém zero ou mais sub-redes. O comando criou uma única sub-rede com o nome *predefinido*, mas não foi especificado nenhum prefixo de endereço para a sub-rede. Quando um prefixo de endereço não está especificado para uma rede virtual ou uma sub-rede, Azure define 10.0.0.0/24 como o prefixo de endereço para a primeira sub-rede, por predefinição. Como resultado, a sub-rede abrange 10.0.0.0-10.0.0.254, mas 10.0.0.4-10.0.0.254 só estão disponíveis, porque o Azure reserva-se os endereços de quatro primeiras (0-3) e o último endereço em cada sub-rede.

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Uma rede virtual permite que vários tipos de recursos do Azure em privado comuniquem entre si. Um tipo de recurso que pode implementar numa rede virtual é uma máquina virtual. Crie duas máquinas virtuais na rede virtual para que possa validar e compreender como funciona a comunicação entre as máquinas virtuais numa rede virtual num passo posterior.

Crie uma máquina virtual com o comando [az vm create](/cli/azure/vm#az_vm_create). O exemplo seguinte cria uma máquina virtual denominada *myVm1*. Se as chaves SSH ainda não existir numa localização chaves predefinido, o comando cria-los. Para utilizar um conjunto específico de chaves, utilize a opção `--ssh-key-value`. O `--no-wait` opção cria a máquina virtual em segundo plano, para poder continuar para o passo seguinte.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --generate-ssh-keys \
  --no-wait
```

Azure cria automaticamente a máquina virtual no *predefinido* sub-rede do *myVirtualNetwork* virtual de rede, porque a rede virtual existe no grupo de recursos e a nenhuma rede virtual ou sub-rede foi especificado no comando. Azure DHCP atribuído automaticamente 10.0.0.4 à máquina virtual durante a criação, porque é o primeiro endereço disponível no *predefinido* sub-rede. A localização em que uma máquina virtual é criada na tem de ser a mesma localização que da rede virtual existe. A máquina virtual não é necessário para estar no mesmo grupo de recursos de rede virtual, embora seja neste artigo.

Crie uma segunda máquina virtual. Por predefinição, o Azure cria também esta máquina virtual no *predefinido* sub-rede.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --generate-ssh-keys
```

A máquina virtual demora alguns minutos a criar. Depois da máquina virtual é criada, a CLI do Azure devolve um resultado semelhante ao seguinte exemplo: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm1",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

No exemplo, verá que o **privateIpAddress** é *10.0.0.5*. DHCP Azure atribuída automaticamente *10.0.0.5* à máquina virtual porque foi o endereço disponível seguinte no *predefinido* sub-rede. Tome nota do **publicIpAddress**. Este endereço é utilizado para aceder a máquina virtual a partir da Internet num passo posterior. O endereço IP público não está atribuído partir de dentro da rede virtual ou prefixos de endereço de sub-rede. Endereços IP públicos são atribuídos de um [conjunto de endereços atribuídos a cada região do Azure](https://www.microsoft.com/download/details.aspx?id=41653). Enquanto o Azure sabe qual o endereço IP público é atribuído a uma máquina virtual, o sistema operativo executado numa máquina virtual tem não conhecimento de qualquer endereço IP público atribuído.

## <a name="connect-to-a-virtual-machine"></a>Ligar a uma máquina virtual

Utilize o seguinte comando para criar uma sessão SSH com o *myVm2* máquina virtual. Substitua `<publicIpAddress>` com o endereço IP público da sua máquina virtual. No exemplo acima, o endereço IP é *40.68.254.142*.

```bash 
ssh <publicIpAddress>
```

## <a name="validate-communication"></a>Validar a comunicação

Utilize o seguinte comando para confirmar a comunicação com *myVm1* de *myVm2*:

```bash
ping myVm1 -c 4
```

Receberá quatro respostas de *10.0.0.4*. Pode comunicar com *myVm1* de *myVm2*, porque as duas máquinas virtuais têm endereços IP privados atribuídos a partir do *predefinido* sub-rede. É capaz de executar o ping pelo nome de anfitrião porque o Azure oferece automaticamente resolução do nome DNS de todos os anfitriões numa rede virtual.

Utilize o seguinte comando para confirmar a comunicação de saída à Internet:

```bash
ping bing.com -c 4
```

Receber quatro respostas bing.com. Por predefinição, qualquer máquina virtual numa rede virtual podem comunicar saída à Internet.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, pode utilizar o [eliminação do grupo de az](/cli/azure/group#az_group_delete) comando para remover o grupo de recursos e todos os recursos nele contidos. Sair sessão SSH para a VM, em seguida, elimine os recursos.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, implementou uma predefinição a rede virtual com uma sub-rede e duas máquinas virtuais. Para saber como criar uma rede virtual personalizada com várias sub-redes e efetuar tarefas de gestão básicas, avance para o tutorial para criar uma rede virtual personalizada e geri-lo.


> [!div class="nextstepaction"]
> [Criar uma rede virtual personalizada e geri-lo](virtual-networks-create-vnet-arm-pportal.md#azure-cli)
