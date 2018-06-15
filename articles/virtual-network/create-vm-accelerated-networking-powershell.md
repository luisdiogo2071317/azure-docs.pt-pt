---
title: Criar uma máquina virtual do Azure com acelerados rede | Microsoft Docs
description: Saiba como criar uma máquina virtual Linux com acelerados da rede.
services: virtual-network
documentationcenter: ''
author: gsilva5
manager: gedegrac
editor: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/04/2018
ms.author: gsilva
ms.openlocfilehash: de69cdf69f30639d048dccd7d433c86f6cb9db7b
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
ms.locfileid: "33894190"
---
# <a name="create-a-windows-virtual-machine-with-accelerated-networking"></a>Criar uma máquina virtual do Windows com acelerados da rede

Neste tutorial, irá aprender a criar uma máquina virtual (VM) do Windows com acelerados da rede. Para criar uma VM com Linux com acelerados da rede, consulte [criar uma VM com Linux com acelerados redes](create-vm-accelerated-networking-cli.md). Na melhoria de rede permite que a virtualização de e/s de raiz única (SR-IOV) para uma VM, melhorando em grande medida o desempenho de rede. Este caminho de elevado desempenho ignora o anfitrião datapath, reduzindo a latência, interferência e utilização da CPU, para utilização com cargas de trabalho de rede mais demanding em tipos VM suportados. A imagem seguinte mostra a comunicação entre duas VMs com e sem redes na melhoria:

![Comparação](./media/create-vm-accelerated-networking/accelerated-networking.png)

Sem redes na melhoria, todo o tráfego de rede que entra e sai da VM tem atravessar o anfitrião e o comutador virtual. O comutador virtual fornece todos os imposição de política, tais como grupos de segurança de rede, listas de controlo de acesso, isolamento e outros serviços de rede virtualizado para tráfego de rede. Para obter mais informações sobre comutadores virtuais, leia o [Virtualização de rede do Hyper-V e do comutador virtual](https://technet.microsoft.com/library/jj945275.aspx) artigo.

Com redes na melhoria, o tráfego de rede chega à interface de rede da VM (NIC) e, em seguida, é reencaminhado para a VM. Todas as políticas de rede que o comutador virtual aplica-se agora são descarregadas sendo aplicadas no hardware. Aplicar a política de hardware permite que o NIC para reencaminhar o tráfego de rede diretamente para a VM, ignorando o anfitrião e o comutador virtual, enquanto mantém todos os a política que aplicada no anfitrião.

As vantagens do funcionamento em rede na melhoria só se aplicam à VM que está ativada no. Para obter os melhores resultados, é ideal ativar esta funcionalidade em, pelo menos, duas VMs ligadas à mesma rede de Virtual do Azure (VNet). Quando a comunicação entre VNets ou ligação no local, esta funcionalidade tem um impacto mínimo para geral de latência.

## <a name="benefits"></a>Benefícios
* **Reduzir a latência / superiores pacotes por segundo (pps):** removendo o comutador virtual do datapath remove o tempo de pacotes gastam no anfitrião para o processamento da política e aumenta o número de pacotes que podem ser processados dentro da VM.
* **Reduzido interferência:** comutador Virtual de processamento depende da quantidade de política que tem de ser aplicadas e a carga de trabalho da CPU que está a fazer o processamento. Descarregar a imposição de política para o hardware remove esse variabilidade fornecendo pacotes diretamente para a VM, a remoção do anfitrião para comunicação de VM e todas as interrupções de software e comutadores de contexto.
* **Diminuir a utilização da CPU:** ignorar o comutador virtual no anfitrião leva a menor utilização da CPU para processar o tráfego de rede.

## <a name="limitations-and-constraints"></a>Limitações e restrições

### <a name="supported-operating-systems"></a>Sistemas operativos suportados
São suportadas as seguintes distribuições Box na galeria do Azure: 
* **O Centro de dados do Windows Server 2016** 
* **O Centro de dados do Windows Server 2012 R2** 

### <a name="supported-vm-instances"></a>Instâncias VM suportadas
É suportada na melhoria de redes objetivo mais comum e tamanhos de instância com otimização de computação com vCPUs 2 ou mais.  Este série suportado é: D/série DSv2 e F/Fs

As instâncias que suportem Hyper-Threading, acelerados redes é suportada em instâncias VM com vCPUs 4 ou mais. Série suportado é: D/DSv3, I/ESv3, Fsv2 e Ms/Mms

Para obter mais informações sobre as instâncias VM, consulte [tamanhos de Windows VM](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="regions"></a>Regiões
Disponível em todas as regiões do Azure públicas e na nuvem do Azure Government.

### <a name="enabling-accelerated-networking-on-a-running-vm"></a>Ativar acelerados rede numa VM em execução
Um tamanho VM suportado sem redes na melhoria ativada só pode ter a funcionalidade ativada quando está parado e desalocada.

### <a name="deployment-through-azure-resource-manager"></a>Implementação através do Azure Resource Manager
Não é possível implementar máquinas virtuais (clássicas) com acelerados da rede.

## <a name="create-a-windows-vm-with-azure-accelerated-networking"></a>Criar uma VM do Windows com o funcionamento em rede na melhoria do Azure

Embora este artigo fornece os passos para criar uma máquina virtual com redes na melhoria com o Azure PowerShell, também pode [criar uma máquina virtual com redes na melhoria no portal do Azure](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Quando criar uma máquina virtual no portal, em **definições**, selecione **ativado**, em **acelerados redes**. A opção para ativar na melhoria de redes não aparece no portal, salvo se tiver selecionado um [sistema operativo suportado](#supported-operating-systems) e [tamanho da VM](#supported-vm-instances). Depois de criar a máquina virtual, tem de concluir as instruções em [confirmar que o controlador está instalado no sistema operativo](#confirm-the-driver-is-installed-in-the-operating-system).

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

Instalar [Azure PowerShell](/powershell/azure/install-azurerm-ps) versão 5.1.1 ou posterior. Para localizar a versão atualmente instalada, execute `Get-Module -ListAvailable AzureRM`. Se precisar de instalar ou atualizar, instale a versão mais recente do módulo de AzureRM o [galeria do PowerShell](https://www.powershellgallery.com/packages/AzureRM). Numa sessão do PowerShell, iniciar sessão uma conta do Azure utilizando [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount).

Nos exemplos a seguir, substitua os nomes dos parâmetros de exemplo com os seus próprios valores. Os nomes de parâmetros de exemplo incluídos *myResourceGroup*, *myNic*, e *myVM*.

Crie um grupo de recursos com [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). O exemplo seguinte cria um grupo de recursos denominado *myResourceGroup* no *centralus* localização:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "centralus"
```

Em primeiro lugar, crie uma configuração de sub-rede com [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/AzureRM.Network/New-AzureRmVirtualNetworkSubnetConfig). O exemplo seguinte cria uma sub-rede designada *mySubnet*:

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
    -Name "mySubnet" `
    -AddressPrefix "192.168.1.0/24"
```

Criar uma rede virtual com [New-AzureRmVirtualNetwork](/powershell/module/AzureRM.Network/New-AzureRmVirtualNetwork), com o *mySubnet* sub-rede.

```powershell
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "centralus" `
    -Name "myVnet" `
    -AddressPrefix "192.168.0.0/16" `
    -Subnet $Subnet
```

## <a name="create-a-network-security-group"></a>Criar um grupo de segurança de rede

Em primeiro lugar, crie uma regra de grupo de segurança de rede com [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/AzureRM.Network/New-AzureRmNetworkSecurityRuleConfig).

```powershell
$rdp = New-AzureRmNetworkSecurityRuleConfig `
    -Name 'Allow-RDP-All' `
    -Description 'Allow RDP' `
    -Access Allow `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 100 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389
```

Criar um grupo de segurança de rede com [New-AzureRmNetworkSecurityGroup](/powershell/module/AzureRM.Network/New-AzureRmNetworkSecurityGroup) e atribua o *permitir RDP-todos os* regra de segurança ao mesmo. Para além de *permitir RDP-todos os* regra, o grupo de segurança de rede contém várias regras de predefinidas. Uma regra predefinida desativa todos os acessos de entrada da Internet, razão pela qual o *permitir RDP-todos os* regra é atribuída ao grupo de segurança de rede para que possa remotamente ligar à máquina virtual, quando for criado.

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroup `
    -Location centralus `
    -Name "myNsg" `
    -SecurityRules $rdp
```

Associar o grupo de segurança de rede para o *mySubnet* sub-rede com [conjunto AzureRmVirtualNetworkSubnetConfig](/powershell/module/AzureRM.Network/Set-AzureRmVirtualNetworkSubnetConfig). A regra no grupo de segurança de rede é eficaz para todos os recursos implementados na sub-rede.

```powershell
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name 'mySubnet' `
    -AddressPrefix "192.168.1.0/24" `
    -NetworkSecurityGroup $nsg
```

## <a name="create-a-network-interface-with-accelerated-networking"></a>Criar uma interface de rede com redes na melhoria
Crie um endereço IP público com [New-AzureRmPublicIpAddress](/powershell/module/AzureRM.Network/New-AzureRmPublicIpAddress). Um endereço IP público não é necessário se não tiver intenção para aceder a máquina virtual a partir da Internet, mas para concluir os passos neste artigo, é necessário.

```powershell
$publicIp = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroup `
    -Name 'myPublicIp' `
    -location centralus `
    -AllocationMethod Dynamic
```

Criar uma interface de rede com [New-AzureRmNetworkInterface](/powershell/module/AzureRM.Network/New-AzureRmNetworkInterface) com acelerados rede ativada e atribuir o endereço IP público para a interface de rede. O exemplo seguinte cria uma interface de rede com o nome *myNic* no *mySubnet* sub-rede do *myVnet* rede virtual e atribui o *myPublicIp*  endereço IP público para a mesma:

```powershell
$nic = New-AzureRmNetworkInterface `
    -ResourceGroupName "myResourceGroup" `
    -Name "myNic" `
    -Location "centralus" `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIp.Id `
    -EnableAcceleratedNetworking
```

## <a name="create-the-virtual-machine"></a>Criar a máquina virtual

Definir as suas credenciais VM o `$cred` utilizando a variável [Get-Credential](/powershell/module/microsoft.powershell.security/get-credential):

```powershell
$cred = Get-Credential
```

Definir em primeiro lugar, a VM com [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). O exemplo seguinte define uma VM chamada *myVM* com um tamanho VM que suporte redes acelerados (*Standard_DS4_v2*):

```powershell
$vmConfig = New-AzureRmVMConfig -VMName "myVm" -VMSize "Standard_DS4_v2"
```

Para obter uma lista de todos os tamanhos de VM e características, consulte [tamanhos de Windows VM](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Criar o resto da sua configuração de VM com [conjunto AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) e [conjunto AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage). O exemplo seguinte cria uma VM do Windows Server 2016:

```powershell
$vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig `
    -Windows `
    -ComputerName "myVM" `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig `
    -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" `
    -Skus "2016-Datacenter" `
    -Version "latest"
```

Anexar a interface de rede que criou anteriormente com [adicionar AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

```powershell
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```

Por fim, crie a VM com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):

```powershell
New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "centralus"
```

## <a name="confirm-the-driver-is-installed-in-the-operating-system"></a>Confirmar que o controlador está instalado no sistema operativo

Depois de criar a VM no Azure, ligar à VM e confirme que o controlador está instalado no Windows.

1. A partir do browser da Internet, abra o Azure [portal](https://portal.azure.com) e inicie sessão com a sua conta do Azure.
2. Na caixa que contém o texto *procurar recursos* na parte superior do portal do Azure, escreva *myVm*. Quando **myVm** é apresentado nos resultados da pesquisa, clique no mesmo. Se **criar** é visível no **Connect** botão, Azure não ainda acabou de criar a VM. Clique em **Connect** no canto superior esquerdo da descrição geral apenas depois de deixarem de ver **criar** sob o **Connect** botão.
3. Introduza o nome de utilizador e palavra-passe que introduziu no [criar a máquina virtual](#create-the-virtual-machine). Se nunca tiver ligado a uma VM do Windows no Azure, consulte [ligar à máquina virtual](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#connect-to-virtual-machine).
4. Faça duplo clique no botão Iniciar do Windows e clique em **Gestor de dispositivos**. Expanda o **adaptadores de rede** nós. Confirme que o **adaptador de Ethernet de função Virtual Mellanox ConnectX 3** for apresentada, conforme mostrado na imagem seguinte:

    ![Gestor de dispositivos](./media/create-vm-accelerated-networking/device-manager.png)

Na melhoria de redes está agora ativada para a VM.

## <a name="enable-accelerated-networking-on-existing-vms"></a>Ativar acelerados redes em VMs existentes
Se tiver criado uma VM sem acelerados redes, é possível ativar esta funcionalidade numa VM existente.  A VM tem de suportar redes acelerados pelo cumprir os seguintes pré-requisitos que também estão descritos acima:

* A VM tem de ser um tamanho suportado para acelerados redes
* A VM tem de ser uma imagem de galeria do Azure suportada (e a versão de kernel para Linux)
* Todas as VMs num conjunto de disponibilidade ou VMSS tem de ser parado/desalocada antes de ativar acelerados redes em qualquer NIC

### <a name="individual-vms--vms-in-an-availability-set"></a>Individuais VMs & VMs na disponibilidade de um conjunto
Primeiro parar/Desalocação da VM ou, se um conjunto de disponibilidade, todas as VMs no conjunto de:

```azurepowershell
Stop-AzureRmVM -ResourceGroup "myResourceGroup" `
    -Name "myVM"
```

Importante,. tenha em atenção, se a VM foi criada individualmente, sem um conjunto de disponibilidade, que apenas tem de parar/anular atribuição de VM individuais para ativar acelerados redes.  Se a VM foi criada com um conjunto de disponibilidade, todas as VMs incluídas no conjunto de disponibilidade tem de ser parado/desalocada antes de ativar acelerados redes em qualquer um dos NICs. 

Depois de parar, ative acelerados redes no NIC da sua VM:

```azurepowershell
$nic = Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic"

$nic.EnableAcceleratedNetworking = $true

$nic | Set-AzureRmNetworkInterface
```

Reiniciar a VM ou, se num conjunto de disponibilidade, todas as VMs no conjunto e confirme que a rede acelerados está ativado: 

```azurepowershell
Start-AzureRmVM -ResourceGroup "myResourceGroup" `
    -Name "myVM"
```

### <a name="vmss"></a>VMSS
VMSS é ligeiramente diferente, mas segue mesmo fluxo de trabalho.  Em primeiro lugar, pare as VMs:

```azurepowershell
Stop-AzureRmVmss -ResourceGroupName "myResourceGroup" ` 
    -VMScaleSetName "myScaleSet"
```

Assim que as VMs estão paradas, atualize a propriedade acelerados rede sob a interface de rede:

```azurepowershell
$vmss = Get-AzureRmVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet"

$vmss.VirtualMachineProfile.NetworkProfile.NetworkInterfaceConfigurations[0].EnableAcceleratedNetworking = $true

Update-AzureRmVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

. Tenha em atenção, uma VMSS tem as atualizações VM que se aplicam a atualizações com três definições diferentes, graduais manuais e o automáticas.  Nestas instruções, a política estiver definida como automático para que o VMSS selecionará as alterações imediatamente após o reinício.  Para o definir como automático para que as alterações são imediatamente captadas: 

```azurepowershell
$vmss.UpgradePolicy.AutomaticOSUpgrade = $true

Update-AzureRmVmss -ResourceGroupName "myResourceGroup" `
    -VMScaleSetName "myScaleSet" `
    -VirtualMachineScaleSet $vmss
```

Finalmente, reinicie o VMSS:

```azurepowershell
Start-AzureRmVmss -ResourceGroupName "myResourceGroup" ` 
    -VMScaleSetName "myScaleSet"
```

Uma vez, reinicia, aguarde que as atualizações concluir, mas uma vez concluído, será apresentado o VF dentro da VM.  (Certifique-se que estiver a utilizar um tamanho de SO e VM suportado)

### <a name="resizing-existing-vms-with-accelerated-networking"></a>Redimensionar VMs existentes com acelerados da rede

As VMs com acelerados rede ativada só podem ser redimensionadas para VMs que suportam acelerados redes.  

Não pode ser redimensionada uma VM com acelerados rede ativada para uma instância VM não suporta acelerados redes utilizando a operação de redimensionamento.  Em vez disso, para redimensionar um estas VMs: 

* Parar/Deallocate a VM ou em caso de um conjunto de disponibilidade/VMSS, pare/anular atribuição de todas as VMs no conjunto/VMSS.
* Tem de ser desativada na melhoria de redes no NIC da VM ou se numa disponibilidade conjunto/VMSS, todas as VMs no conjunto/VMSS.
* Depois de acelerados rede estiver desativado, o conjunto de disponibilidade/VM/VMSS podem ser movidos para um novo tamanho que não suporta acelerados redes e reiniciado.  



