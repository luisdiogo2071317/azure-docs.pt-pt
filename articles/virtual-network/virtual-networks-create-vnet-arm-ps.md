---
title: Criar uma rede Virtual do Azure com várias sub-redes - PowerShell | Microsoft Docs
description: Saiba como criar uma rede virtual com várias sub-redes através do PowerShell.
services: virtual-network
documentationcenter: ''
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: c90bdc9381bf98e5c1457e8e28f74105227d8f8d
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/13/2018
ms.locfileid: "29880564"
---
# <a name="create-a-virtual-network-with-multiple-subnets-using-powershell"></a>Criar uma rede virtual com várias sub-redes através do PowerShell

Uma rede virtual permite que vários tipos de recursos do Azure comuniquem com a Internet e modo privado entre si. Criar várias sub-redes numa rede virtual permite-lhe segmentar a sua rede para que pode filtrar ou controlar o fluxo de tráfego entre sub-redes. Neste artigo, saiba como:

> [!div class="checklist"]
> * Criar uma rede virtual
> * Criar uma sub-rede
> * Comunicação de rede entre as máquinas virtuais de teste

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer o Azure PowerShell versão do módulo 3,6 ou posterior. Executar ` Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure. 

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

Criar um grupo de recursos com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). O exemplo seguinte cria um grupo de recursos denominado *myResourceGroup* no *EastUS* localização.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Crie uma rede virtual com [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). O exemplo seguinte cria uma rede virtual denominada *myVirtualNetwork* com o prefixo de endereço *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

O **AddressPrefix** é especificada em notação CIDR. O prefixo de endereço especificado inclui 10.0.0.0-10.0.255.254 de endereços IP.

## <a name="create-a-subnet"></a>Criar uma sub-rede

Uma sub-rede é criada ao primeiro criar uma configuração de sub-rede e, em seguida, atualizar a rede virtual com a configuração de sub-rede. Criar uma configuração de sub-rede com [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). O exemplo seguinte cria duas configurações de sub-rede para *pública* e *privada* sub-redes:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigPrivate = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork
```

O **AddressPrefix** especificado para uma sub-rede tem de estar dentro do prefixo de endereço definido para a rede virtual. Azure DHCP atribui endereços IP de um prefixo de endereço de sub-rede para recursos implementados numa sub-rede. Azure atribui apenas o 10.0.0.4-10.0.0.254 endereços a recursos implementados no **pública** sub-rede, porque o Azure reserva de quatro primeiras endereços (10.0.0.0-10.0.0.3 para a sub-rede, neste exemplo) e o último endereço ( 10.0.0.255 para a sub-rede, neste exemplo) em cada sub-rede.

Escrever as configurações de sub-rede da rede virtual com [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), que cria as sub-redes na rede virtual:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

Antes de implementar redes virtuais do Azure e sub-redes para utilização em produção, recomenda-se que lhe exaustivamente familiarizar-se com o espaço de endereços [considerações](manage-virtual-network.md#create-a-virtual-network) e [limites de rede virtual](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Depois de recursos são implementados em sub-redes, algumas rede virtual e alterações de sub-rede, tal como alterar os intervalos de endereços, podem exigir a reimplementação de recursos do Azure existentes implementadas em sub-redes.

## <a name="test-network-communication"></a>Comunicação de rede de teste

Uma rede virtual permite que vários tipos de recursos do Azure comuniquem com a Internet e modo privado entre si. Um tipo de recurso que pode implementar numa rede virtual é uma máquina virtual. Crie duas máquinas virtuais na rede virtual para que possa testá comunicação de rede entre eles e a Internet num passo posterior. 

### <a name="create-virtual-machines"></a>Criar máquinas virtuais

Criar uma máquina virtual com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). O exemplo seguinte cria uma máquina virtual denominada *myVmWeb* no *pública* sub-rede do *myVirtualNetwork* rede virtual. O `-AsJob` opção cria a máquina virtual em segundo plano, para poder continuar para o passo seguinte. Quando lhe for pedido, introduza o nome de utilizador e palavra-passe que pretende iniciar sessão na máquina virtual com o.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Public" `
    -ImageName "Win2016Datacenter" `
    -Name "myVmWeb" `
    -AsJob
```

Azure automaticamente atribuído 10.0.0.4 como o endereço IP privado da máquina virtual, porque 10.0.0.4 é o primeiro endereço IP disponível na *pública* sub-rede. 

Criar uma máquina virtual no *privada* sub-rede.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Private" `
    -ImageName "Win2016Datacenter" `
    -Name "myVmMgmt"
```

A máquina virtual demora alguns minutos a criar. Apesar de não na saída devolvida, Azure atribuído 10.0.1.4 como o endereço IP privado da máquina virtual, porque 10.0.1.4 é o primeiro endereço IP disponível na *privada* sub-rede de *myVirtualNetwork*. 

Não continue com os restantes passos até que a máquina virtual está criada e PowerShell devolve um resultado.

As máquinas virtuais criadas neste artigo tem um [interface de rede](virtual-network-network-interface.md) com um endereço IP dinamicamente atribuído à interface de rede. Depois de implementar a VM, pode [adicionar vários endereços IP públicos e privados, ou alterar o método de atribuição de endereços IP para estático](virtual-network-network-interface-addresses.md#add-ip-addresses). Pode [adicionar interfaces de rede](virtual-network-network-interface-vm.md#vm-add-nic), até ao limite suportado pelo [tamanho da VM](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) que selecionar quando criar uma máquina virtual. Também pode [ativar a virtualização de e/s de raiz única (SR-IOV)](create-vm-accelerated-networking-powershell.md) para uma VM, mas apenas quando criar uma VM com um tamanho VM que suporta a capacidade.

### <a name="communicate-between-virtual-machines-and-with-the-internet"></a>Comunicar entre máquinas virtuais e com a internet

Pode ligar ao endereço IP público de uma máquina virtual a partir da Internet. Quando o Azure criado o *myVmMgmt* máquina virtual, um endereço IP público com o nome *myVmMgmt* também foi criada e atribuída à máquina virtual. Apesar de uma máquina virtual não é necessário ter um endereço IP público atribuído ao mesmo, o Azure atribui um endereço IP público para cada máquina virtual que cria, por predefinição. Para comunicar a partir da Internet para uma máquina virtual, um endereço IP público tem de ser atribuído à máquina virtual. Todas as máquinas virtuais podem comunicar saída com a Internet, quer tenha ou não está atribuído um endereço IP público para a máquina virtual. Para saber mais sobre ligações de Internet de saída no Azure, consulte o artigo [ligações de saída no Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Utilize [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) para devolver o endereço IP público de uma máquina virtual. O exemplo seguinte devolve o endereço IP público do *myVmMgmt* máquina virtual:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

Utilize o seguinte comando para criar uma sessão de ambiente de trabalho remota com o *myVmMgmt* máquina virtual do seu computador local. Substitua `<publicIpAddress>` com o endereço IP devolvido do comando anterior.

```
mstsc /v:<publicIpAddress>
```

Um ficheiro de protocolo de ambiente de trabalho remoto (RDP) é criado, transferido para o computador e aberto. Introduza o nome de utilizador e palavra-passe que especificou ao criar a máquina virtual (poderá ter de selecionar **mais opções**, em seguida, **utilizar uma conta diferente**, para especificar as credenciais introduzidas quando a criar a máquina virtual) e, em seguida, clique em **OK**. Poderá receber um aviso de certificado durante o processo de início de sessão. Clique em **Sim** ou **Continuar** para continuar com a ligação. 

Num passo posterior, ping é utilizado para comunicar com o *myVmMgmt* máquina virtual a partir de *myVmWeb* máquina virtual. Utiliza o ping ICMP, que é negado através da Firewall do Windows, por predefinição. Ative o ICMP através da firewall do Windows, introduzindo o seguinte comando numa linha de comandos:

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

Apesar de ping é utilizado neste artigo, permitir ICMP através da Firewall do Windows para implementações de produção não é recomendada.

Por motivos de segurança, é comum para limitar o número de máquinas virtuais que podem ser ligados remotamente numa rede virtual. Neste tutorial, o *myVmMgmt* máquina virtual é utilizada para gerir o *myVmWeb* máquina virtual na rede virtual. Utilize o seguinte comando para ambiente de trabalho remoto para o *myVmWeb* máquina virtual a partir de *myVmMgmt* máquina virtual:

``` 
mstsc /v:myVmWeb
```

Para comunicar com o *myVmMgmt* máquina virtual a partir de *myVmWeb* máquina virtual, introduza o seguinte comando numa linha de comandos:

```
ping myvmmgmt
```

Poderá recebe um resultado semelhante ao seguinte exemplo de saída:
    
```
Pinging myvmmgmt.dar5p44cif3ulfq00wxznl3i3f.bx.internal.cloudapp.net [10.0.1.4] with 32 bytes of data:
Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    
Ping statistics for 10.0.1.4:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```
      
Pode ver que o endereço do *myVmMgmt* máquina virtual é 10.0.1.4. 10.0.1.4 foi o primeiro endereço IP disponível no intervalo de endereços do *privada* sub-rede que implementou o *myVmMgmt* máquina virtual num passo anterior.  Verá que o nome de domínio completamente qualificado da máquina virtual é *myvmmgmt.dar5p44cif3ulfq00wxznl3i3f.bx.internal.cloudapp.net*. Embora o *dar5p44cif3ulfq00wxznl3i3f* parte do nome de domínio é diferente para a máquina virtual, o restante do nome de domínio são os mesmos. Por predefinição, todas as máquinas virtuais do Azure utilizam o serviço de DNS do Azure predefinido. Todas as máquinas virtuais dentro de uma rede virtual pode resolver os nomes de todas as outras máquinas virtuais na mesma rede virtual com o serviço DNS do Azure predefinido. Em vez de utilizar o serviço DNS predefinido do Azure, pode utilizar o seu próprio servidor DNS ou a capacidade de domínio privada do serviço DNS do Azure. Para obter mais informações, consulte [resolução de nomes utilizando o seu próprio servidor DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) ou [utilizando o DNS do Azure para domínios privados](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Para instalar serviços de informação Internet (IIS) para o Windows Server no *myVmWeb* máquina virtual, introduza o comando seguinte a partir de uma sessão do PowerShell:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Depois de concluída a instalação do IIS, desligue o *myVmWeb* sessões de ambiente de trabalho remoto, deixando-na *myVmMgmt* sessões de ambiente de trabalho remoto. Abra um browser e navegue para http://myvmweb. Consulte o página de boas-vindas do IIS.

Desligar o *myVmMgmt* sessões de ambiente de trabalho remoto.

Obter o endereço público Azure atribuído para o *myVmWeb* máquina virtual:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

No seu computador, navegue para o endereço IP público do *myVmWeb* máquina virtual. Falha ao tentar ver a página de boas-vindas do IIS do seu computador. A tentativa falhar porque quando as máquinas virtuais foram implementadas, o Azure criado um grupo de segurança de rede para cada máquina virtual, por predefinição. 

Um grupo de segurança de rede contém regras de segurança que permitem ou negam o tráfego de rede de entrada e saída pela porta e endereço IP. O grupo de segurança de rede predefinido criado do Azure permite a comunicação através de todas as portas entre os recursos na mesma rede virtual. Para máquinas virtuais do Windows, o grupo de segurança de rede predefinido nega todo o tráfego de entrada da Internet através de todas as portas, aceite a porta TCP 3389 (RDP). Como consequência, por predefinição, também pode RDP diretamente para o *myVmWeb* máquina virtual a partir da Internet, apesar de não poderá porta 3389 aberto a um servidor web. Uma vez que a navegação na web comunica através da porta 80, falha de comunicação da Internet porque não há nenhuma regra num grupo de segurança de rede predefinido a permitir o tráfego através da porta 80.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário utilizar [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos e todos os recursos nele contidos.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a implementar uma rede virtual com várias sub-redes. Também aprendeu que quando cria uma máquina virtual do Windows, o Azure cria uma interface de rede que liga à máquina virtual e cria um grupo de segurança de rede que permita que apenas o tráfego através da porta 3389, através da Internet. Avançar para o próximo tutorial para saber como filtrar o tráfego de rede a sub-redes, em vez de para máquinas virtuais individuais.

> [!div class="nextstepaction"]
> [Filtrar o tráfego de rede para sub-redes](./virtual-networks-create-nsg-arm-ps.md)
