---
title: "Criar uma rede Virtual do Azure com várias sub-redes - CLI do Azure | Microsoft Docs"
description: "Saiba como criar uma rede virtual com várias sub-redes utilizando a CLI do Azure."
services: virtual-network
documentationcenter: 
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: feb8fb218b8bde88ba7fbdc3419e32c9313c7885
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
# <a name="create-a-virtual-network-with-multiple-subnets-using-the-azure-cli"></a>Criar uma rede virtual com várias sub-redes utilizando a CLI do Azure

Uma rede virtual permite que vários tipos de recursos do Azure comuniquem com a Internet e modo privado entre si. Criar várias sub-redes numa rede virtual permite-lhe segmentar a sua rede para que pode filtrar ou controlar o fluxo de tráfego entre sub-redes. Neste artigo, saiba como:

> [!div class="checklist"]
> * Criar uma rede virtual
> * Criar uma sub-rede
> * Comunicação de rede entre as máquinas virtuais de teste

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0.4 ou posterior da CLI do Azure. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli.md). 

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

Crie um grupo de recursos com [az group create](/cli/azure/group#az_group_create). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Crie uma rede virtual com [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create). O exemplo seguinte cria uma rede virtual denominada *myVirtualNetwork* com o prefixo de endereço *10.0.0.0/16*. O comando cria uma sub-rede designada *pública*, com o prefixo de endereço *10.0.0.0/24*.

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

Uma vez que não foi especificada uma localização no comando anterior, o Azure cria a rede virtual na mesma localização que o *myResourceGroup* grupo de recursos existe. O **prefixos de endereço** e **prefixo de sub-rede** estão especificados na notação CIDR. O prefixo de endereço especificado inclui 10.0.0.0-10.0.255.254 de endereços IP. O prefixo especificado para a sub-rede tem de estar no prefixo de endereço definido para a rede virtual. Azure DHCP atribui endereços IP de um prefixo de endereço de sub-rede para recursos implementados numa sub-rede. Azure atribui apenas o 10.0.0.4-10.0.0.254 endereços a recursos implementados no **pública** sub-rede, porque o Azure reserva de quatro primeiras endereços (10.0.0.0-10.0.0.3 para a sub-rede, neste exemplo) e o último endereço ( 10.0.0.255 para a sub-rede, neste exemplo) em cada sub-rede.

## <a name="create-a-subnet"></a>Criar uma sub-rede

Criar uma sub-rede com [az rede vnet sub-rede](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). O exemplo seguinte cria uma sub-rede designada *privada* dentro de *myVirtualNetwork* rede virtual com o prefixo de endereço *10.0.1.0/24*. O prefixo do endereço tem de estar dentro do prefixo de endereço definido para a rede virtual e não se podem sobrepor o prefixo do endereço de quaisquer outras sub-redes na rede virtual.

```azurecli-interactive 
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.1.0/24
```

Antes de implementar redes virtuais do Azure e sub-redes para utilização em produção, recomendamos que lhe exaustivamente familiarizar-se com o espaço de endereços [considerações](virtual-network-manage-network.md#create-a-virtual-network) e [limites de rede virtual](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Depois de recursos são implementados em sub-redes, algumas rede virtual e alterações de sub-rede, tal como alterar os intervalos de endereços, podem exigir a reimplementação de recursos do Azure existentes implementadas em sub-redes.

## <a name="test-network-communication"></a>Comunicação de rede de teste

Uma rede virtual permite que vários tipos de recursos do Azure comuniquem com a Internet e modo privado entre si. Um tipo de recurso que pode implementar numa rede virtual é uma máquina virtual. Crie duas máquinas virtuais na rede virtual para que possa testá comunicação de rede entre eles e a Internet num passo posterior.

### <a name="create-virtual-machines"></a>Criar máquinas virtuais

Criar uma máquina virtual com [az vm criar](/cli/azure/vm#az_vm_create). O exemplo seguinte cria uma máquina virtual denominada *myVmWeb* no *pública* sub-rede. O `--no-wait` parâmetro ativa do Azure ao executar o comando em segundo plano, para poder continuar para o comando seguinte. Para simplificar este tutorial, é utilizada uma palavra-passe. As chaves são normalmente utilizadas nas implementações de produção. Se utilizar chaves, também tem de configurar o reencaminhamento para concluir os passos restantes de agente SSH. Para obter mais informações sobre o reencaminhamento de agente SSH, consulte a documentação para o cliente SSH. Substitua `<replace-with-your-password>` no seguinte comando com uma palavra-passe da sua escolha.

```azurecli-interactive
adminPassword="<replace-with-your-password>"

az vm create \
  --resource-group myResourceGroup \
  --name myVmWeb \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --admin-username azureuser \
  --admin-password $adminPassword \
  --no-wait
```

Azure automaticamente atribuído 10.0.0.4 como o endereço IP privado da máquina virtual, porque 10.0.0.4 é o primeiro endereço IP disponível na *pública* sub-rede. 

Criar uma máquina virtual no *privada* sub-rede.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVmMgmt \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --admin-username azureuser \
  --admin-password $adminPassword
```
A máquina virtual demora alguns minutos a criar. Depois da máquina virtual é criada, a CLI do Azure devolve um resultado semelhante ao seguinte exemplo: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmMgmt",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.1.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

No exemplo de saída, verá que o **privateIpAddress** é *10.0.1.4*. Azure criado um [interface de rede](virtual-network-network-interface.md), ligado-lo para a máquina virtual, atribuído um endereço IP privado, a interface de rede e um **macAddress**. Azure DHCP automaticamente atribuído 10.0.1.4 à interface de rede porque é o primeiro endereço IP disponível na *privada* sub-rede. Os endereços IP e MAC privados permanecem atribuídos à interface de rede até que a interface de rede é eliminada. 

Tome nota do **publicIpAddress**. Este endereço é utilizado para aceder a máquina virtual a partir da Internet num passo posterior. Apesar de uma máquina virtual não é necessário ter um endereço IP público atribuído ao mesmo, o Azure atribui um endereço IP público para cada máquina virtual que cria, por predefinição. Para comunicar a partir da Internet para uma máquina virtual, um endereço IP público tem de ser atribuído à máquina virtual. Todas as máquinas virtuais podem comunicar saída com a Internet, quer tenha ou não está atribuído um endereço IP público para a máquina virtual. Para saber mais sobre ligações de Internet de saída no Azure, consulte o artigo [ligações de saída no Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

As máquinas virtuais criadas neste artigo tem um [interface de rede](virtual-network-network-interface.md) com um endereço IP dinamicamente atribuído à interface de rede. Depois de implementar a VM, pode [adicionar vários endereços IP públicos e privados, ou alterar o método de atribuição de endereços IP para estático](virtual-network-network-interface-addresses.md#add-ip-addresses). Pode [adicionar interfaces de rede](virtual-network-network-interface-vm.md#vm-add-nic), até ao limite suportado pelo [tamanho da VM](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) que selecionar quando criar uma máquina virtual. Também pode [ativar a virtualização de e/s de raiz única (SR-IOV)](create-vm-accelerated-networking-cli.md) para uma VM, mas apenas quando criar uma VM com um tamanho VM que suporta a capacidade.

### <a name="communicate-between-virtual-machines-and-with-the-internet"></a>Comunicar entre máquinas virtuais e com a internet

Utilize o seguinte comando para criar uma sessão SSH com o *myVmMgmt* máquina virtual. Substitua `<publicIpAddress>` com o endereço IP público da sua máquina virtual. No exemplo anterior, o endereço IP público é *13.90.242.231*. Quando lhe for pedido para uma palavra-passe, introduza a palavra-passe que introduziu no [criar máquinas virtuais](#create-virtual-machines).

```bash 
ssh azureuser@<publicIpAddress>
```

Por motivos de segurança, é comum para limitar o número de máquinas virtuais que podem ser ligados remotamente numa rede virtual. Neste tutorial, o *myVmMgmt* máquina virtual é utilizada para gerir o *myVmWeb* máquina virtual na rede virtual. Utilize o seguinte comando para SSH para a *myVmWeb* máquina virtual a partir de *myVmMgmt* máquina virtual:

```bash 
ssh azureuser@myVmWeb
```

Para comunicar com o *myVmMgmt* máquina virtual a partir de *myVmWeb* máquina virtual, introduza o seguinte comando numa linha de comandos:

```
ping -c 4 myvmmgmt
```

Poderá recebe um resultado semelhante ao seguinte exemplo de saída:
    
```
PING myvmmgmt.hxehizax3z1udjnrx1r4gr30pg.bx.internal.cloudapp.net (10.0.1.4) 56(84) bytes of data.
64 bytes from 10.0.1.4: icmp_seq=1 ttl=64 time=1.45 ms
64 bytes from 10.0.1.4: icmp_seq=2 ttl=64 time=0.628 ms
64 bytes from 10.0.1.4: icmp_seq=3 ttl=64 time=0.529 ms
64 bytes from 10.0.1.4: icmp_seq=4 ttl=64 time=0.674 ms

--- myvmmgmt.hxehizax3z1udjnrx1r4gr30pg.bx.internal.cloudapp.net ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3029ms
rtt min/avg/max/mdev = 0.529/0.821/1.453/0.368 ms
```
      
Pode ver que o endereço do *myVmMgmt* máquina virtual é 10.0.1.4. 10.0.1.4 foi o primeiro endereço IP disponível no intervalo de endereços do *privada* sub-rede que implementou o *myVmMgmt* máquina virtual num passo anterior.  Verá que o nome de domínio completamente qualificado da máquina virtual é *myvmmgmt.hxehizax3z1udjnrx1r4gr30pg.bx.internal.cloudapp.net*. Embora o *hxehizax3z1udjnrx1r4gr30pg* parte do nome de domínio é diferente para a máquina virtual, o restante do nome de domínio são os mesmos. Por predefinição, todas as máquinas virtuais do Azure utilizam o serviço de DNS do Azure predefinido. Todas as máquinas virtuais dentro de uma rede virtual pode resolver os nomes de todas as outras máquinas virtuais na mesma rede virtual com o serviço DNS do Azure predefinido. Em vez de utilizar o serviço DNS predefinido do Azure, pode utilizar o seu próprio servidor DNS ou a capacidade de domínio privada do serviço DNS do Azure. Para obter mais informações, consulte [resolução de nomes utilizando o seu próprio servidor DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) ou [utilizando o DNS do Azure para domínios privados](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Utilize os seguintes comandos para instalar o servidor de web nginx no *myVmWeb* máquina virtual:

```bash 
# Update package source
sudo apt-get -y update

# Install NGINX
sudo apt-get -y install nginx
```

Assim que nginx estiver instalado, feche o *myVmWeb* sessão SSH, deixando-o na linha de comandos para o *myVmMgmt* máquina virtual. Introduza o seguinte comando para obter o ecrã de boas-vindas nginx do *myVmWeb* máquina virtual.

```bash
curl myVmWeb
```

O ecrã de boas-vindas nginx é devolvido.

Feche a sessão SSH com o *myVmMgmt* máquina virtual.

Quando o Azure criado o *myVmWeb* máquina virtual, um endereço IP público com o nome *myVmWebPublicIP* também foi criada e atribuída à máquina virtual. Obter o endereço público Azure atribuído com [mostrar de ip público de rede az](/cli/azure/network/public-ip#az_network_public_ip_show).

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroup \
  --name myVmWebPublicIP \
  --query ipAddress
```

No seu computador, introduza o seguinte comando, substituindo `<publicIpAddress>` com o endereço devolvido pelo comando anterior:

```bash
curl <publicIpAddress>
```

Falha ao tentar curl o ecrã de boas-vindas nginx do seu computador. A tentativa falhar porque quando as máquinas virtuais foram implementadas, o Azure criado um grupo de segurança de rede para cada máquina virtual, por predefinição. 

Um grupo de segurança de rede contém regras de segurança que permitem ou negam o tráfego de rede de entrada e saída pela porta e endereço IP. O grupo de segurança de rede predefinido criado do Azure permite a comunicação através de todas as portas entre os recursos na mesma rede virtual. Para máquinas virtuais do Linux, o grupo de segurança de rede predefinido nega todo o tráfego de entrada da Internet através de todas as portas, aceite a porta TCP 22 (SSH). Como consequência, por predefinição, também pode criar uma sessão SSH diretamente para o *myVmWeb* máquina virtual a partir da Internet, mesmo que não queira porta 22 aberto a um servidor web. Uma vez que o `curl` comando comunica através da porta 80, falha de comunicação da Internet porque não há nenhuma regra num grupo de segurança de rede predefinido a permitir o tráfego através da porta 80.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário utilizar [eliminação do grupo de az](/cli/azure/group#az_group_delete) para remover o grupo de recursos e todos os recursos nele contidos.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a implementar uma rede virtual com várias sub-redes. Também aprendeu que quando cria uma máquina virtual Linux, o Azure cria uma interface de rede que liga à máquina virtual e cria um grupo de segurança de rede que permita que apenas o tráfego através da porta 22, através da Internet. Avançar para o próximo tutorial para saber como filtrar o tráfego de rede a sub-redes, em vez de para máquinas virtuais individuais.

> [!div class="nextstepaction"]
> [Filtrar o tráfego de rede para sub-redes](./virtual-networks-create-nsg-arm-cli.md)
