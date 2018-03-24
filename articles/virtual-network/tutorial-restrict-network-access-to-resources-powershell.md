---
title: Restringir o acesso de rede para recursos de PaaS - Azure PowerShell | Microsoft Docs
description: Saiba como limitar e restringir o acesso de rede para recursos do Azure, tais como o Storage do Azure e SQL Database do Azure, com pontos finais de serviço de rede virtual com o PowerShell.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 7e402af74babda2ce32d4a1597c61d71aba89b9e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-powershell"></a>Restringir o acesso de rede para recursos de PaaS com pontos finais de serviço de rede virtual com o PowerShell

Pontos finais do serviço de rede virtual permitem-lhe limitar o acesso de rede a alguns recursos do serviço do Azure para uma sub-rede de rede virtual. Também pode remover o acesso à internet para os recursos. Pontos finais de serviço fornecem ligação direta a partir da sua rede virtual para os serviços do Azure suportadas, permitindo-lhe utilizar o espaço de endereços privados da sua rede virtual para aceder aos serviços do Azure. O tráfego destinado aos recursos do Azure através de pontos finais de serviço sempre permanece da rede principal do Microsoft Azure. Neste artigo, saiba como:

> [!div class="checklist"]
> * Criar uma rede virtual com uma sub-rede
> * Adicionar uma sub-rede e ativar um ponto final de serviço
> * Criar um recurso do Azure e permitir o acesso de rede ao mesmo de apenas uma sub-rede
> * Implementar uma máquina virtual (VM) em cada sub-rede
> * Confirmar o acesso a um recurso de sub-rede
> * Confirmar o acesso é negado a um recurso de uma sub-rede e a internet

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este artigo requer o Azure PowerShell versão do módulo 5.4.1 ou posterior. Execute ` Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

Antes de criar uma rede virtual, tem de criar um grupo de recursos para a rede virtual e todos os outros recursos criados neste artigo. Criar um grupo de recursos com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). O exemplo seguinte cria um grupo de recursos denominado *myResourceGroup*: 

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

Criar uma configuração de sub-rede com [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). O exemplo seguinte cria uma configuração de sub-rede para uma sub-rede designada *pública*:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Criar a sub-rede na rede virtual ao escrever a configuração de sub-rede para a rede virtual com [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork):

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="enable-a-service-endpoint"></a>Ativar um ponto final de serviço 

Pode ativar a pontos finais do serviço apenas para serviços que suportam pontos finais de serviço. Ver os serviços de ativado o ponto final de serviço disponíveis numa localização do Azure com [Get-AzureRmVirtualNetworkAvailableEndpointService](/powershell/module/azurerm.network/get-azurermvirtualnetworkavailableendpointservice). O exemplo seguinte devolve uma lista de serviços ativados de ponto final de serviço disponíveis no *eastus* região. A lista de serviços devolvidos irá aumentar ao longo do tempo, como serviços mais Azure tornam-se o ponto final de serviço ativado.

```azurepowershell-interactive
Get-AzureRmVirtualNetworkAvailableEndpointService -Location eastus | Select Name
``` 

Crie uma sub-rede adicional na rede virtual. Neste exemplo, uma sub-rede denominada *privada* é criada com um ponto final de serviço para *Microsoft*: 

```azurepowershell-interactive
$subnetConfigPrivate = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork `
  -ServiceEndpoint Microsoft.Storage

$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="restrict-network-access-to-and-from-a-subnet"></a>Restringir o acesso de rede de e para uma sub-rede

Criar regras de segurança de grupo com a segurança de rede [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). A seguinte regra permite o acesso de saída para os endereços IP públicos atribuída para este serviço de armazenamento do Azure: 

```azurepowershell-interactive
$rule1 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Allow-Storage-All `
  -Access Allow `
  -DestinationAddressPrefix Storage `
  -DestinationPortRange * `
  -Direction Outbound `
  -Priority 100 `
  -Protocol * `
  -SourceAddressPrefix VirtualNetwork `
  -SourcePortRange *
```

A seguinte regra nega o acesso a todos os endereços IP públicos. A regra anterior sobrepõe-se esta regra, devido a sua prioridade superior, o que permite o acesso para os endereços IP públicos do Storage do Azure.

```azurepowershell-interactive
$rule2 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Deny-internet-All `
  -Access Deny `
  -DestinationAddressPrefix Internet `
  -DestinationPortRange * `
  -Direction Outbound `
  -Priority 110 `
  -Protocol * `
  -SourceAddressPrefix VirtualNetwork `
  -SourcePortRange *
```

A seguinte regra permite que o tráfego de protocolo RDP (Remote Desktop Protocol) de entrada para a sub-rede partir de qualquer lugar. Ligações de ambiente de trabalho remotas são permitidas para a sub-rede, para que possa confirmar o acesso de rede a um recurso num passo posterior.

```azurepowershell-interactive
$rule3 = New-AzureRmNetworkSecurityRuleConfig `
  -Name Allow-RDP-All `
  -Access Allow `
  -DestinationAddressPrefix VirtualNetwork `
  -DestinationPortRange 3389 `
  -Direction Inbound `
  -Priority 120 `
  -Protocol * `
  -SourceAddressPrefix * `
  -SourcePortRange *
```

Criar um grupo de segurança de rede com [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). O exemplo seguinte cria um grupo de segurança de rede com o nome *myNsgPrivate*.

```azurepowershell-interactive
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myNsgPrivate `
  -SecurityRules $rule1,$rule2,$rule3
```

Associar o grupo de segurança de rede para o *privada* sub-rede com [conjunto AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) e, em seguida, escrever a configuração de sub-rede da rede virtual. O exemplo seguinte associa a *myNsgPrivate* grupo de segurança de rede para o *privada* sub-rede:

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $VirtualNetwork `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -ServiceEndpoint Microsoft.Storage `
  -NetworkSecurityGroup $nsg

$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="restrict-network-access-to-a-resource"></a>Restringir o acesso de rede a um recurso

Os passos necessários para restringir o acesso de rede para recursos criados através de serviços do Azure ativados para pontos finais de serviço varia em serviços. Consulte a documentação para serviços individuais para obter passos específicos para cada serviço. O resto deste artigo inclui os passos para restringir o acesso de rede para uma conta de armazenamento do Azure, como um exemplo.

### <a name="create-a-storage-account"></a>Criar uma conta do Storage

Criar uma conta de armazenamento do Azure com [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount). Substitua `<replace-with-your-unique-storage-account-name>` com um nome que é exclusivo em todas as localizações do Azure, entre 3 e 24 carateres de comprimento, a utilizar apenas números e letras minúsculas.

```azurepowershell-interactive
$storageAcctName = '<replace-with-your-unique-storage-account-name>'

New-AzureRmStorageAccount `
  -Location EastUS `
  -Name $storageAcctName `
  -ResourceGroupName myResourceGroup `
  -SkuName Standard_LRS `
  -Kind StorageV2
```

Depois de criar a conta de armazenamento, obter a chave para a conta de armazenamento para uma variável com [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey):

```azurepowershell-interactive
$storageAcctKey = (Get-AzureRmStorageAccountKey `
  -ResourceGroupName myResourceGroup `
  -AccountName $storageAcctName).Value[0]
```

A chave é utilizada para criar uma partilha de ficheiros num passo posterior. Introduza `$storageAcctKey` e anote o valor, como também terá de introduzi-la manualmente num passo posterior ao mapear a partilha de ficheiros para uma unidade numa VM.

### <a name="create-a-file-share-in-the-storage-account"></a>Criar uma partilha de ficheiros na conta de armazenamento

Criar um contexto para a sua conta de armazenamento e da chave com [New-AzureStorageContext](/powershell/module/azure.storage/new-azurestoragecontext). O contexto contém a chave de conta e o nome da conta de armazenamento:

```azurepowershell-interactive
$storageContext = New-AzureStorageContext $storageAcctName $storageAcctKey
```

Criar uma partilha de ficheiros com [New-AzureStorageShare](/powershell/module/azure.storage/new-azurestorageshare):

$share = New-AzureStorageShare my-file-share -Context $storageContext

### <a name="deny-all-network-access-to-a-storage-account"></a>Negar o acesso de rede de todos os a uma conta de armazenamento

Por predefinição, as contas do storage aceitam ligações de rede de clientes em qualquer rede. Para limitar o acesso a redes selecionadas, altere a ação predefinida para *negar* com [atualização AzureRmStorageAccountNetworkRuleSet](/powershell/module/azurerm.storage/update-azurermstorageaccountnetworkruleset). Quando é negado o acesso à rede, a conta de armazenamento não está acessível a partir de qualquer rede.

```azurepowershell-interactive
Update-AzureRmStorageAccountNetworkRuleSet  `
  -ResourceGroupName "myresourcegroup" `
  -Name $storageAcctName `
  -DefaultAction Deny
```

### <a name="enable-network-access-from-a-subnet"></a>Ativar o acesso de rede de sub-rede

Obter a rede virtual criada com [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork) e, em seguida, obter o objeto de sub-rede privada numa variável com [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$privateSubnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroup" `
  -Name "myVirtualNetwork" `
  | Get-AzureRmVirtualNetworkSubnetConfig `
  -Name "Private"
```

Permitir o acesso de rede para a conta de armazenamento do *privada* sub-rede com [adicionar AzureRmStorageAccountNetworkRule](/powershell/module/azurerm.network/add-azurermnetworksecurityruleconfig).

```azurepowershell-interactive
Add-AzureRmStorageAccountNetworkRule `
  -ResourceGroupName "myresourcegroup" `
  -Name $storageAcctName `
  -VirtualNetworkResourceId $privateSubnet.Id
```

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Para testar o acesso de rede a uma conta de armazenamento, implemente uma VM para cada sub-rede.

### <a name="create-the-first-virtual-machine"></a>Criar primeira máquina virtual

Criar uma máquina virtual no *pública* sub-rede com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Quando executar o comando que se segue, lhe forem pedidas as credenciais. Os valores que introduziu estão configurados como nome de utilizador e palavra-passe para a VM. O `-AsJob` opção cria a VM em segundo plano, para que possa continuar para o passo seguinte.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Public" `
    -Name "myVmPublic" `
    -AsJob
```

É devolvido o resultado semelhante ao seguinte exemplo de saída:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command                  
--     ----            -------------   -----         -----------     --------             -------                  
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM     
```

### <a name="create-the-second-virtual-machine"></a>Criar a máquina virtual segundo

Criar uma máquina virtual no *privada* sub-rede:

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "Private" `
    -Name "myVmPrivate"
```

Demora alguns minutos para o Azure criar a VM. Continue para o passo seguinte até que o Azure acaba de criar a VM e devolve um resultado para o PowerShell. 

## <a name="confirm-access-to-storage-account"></a>Confirmar o acesso à conta de armazenamento

Utilize [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) para devolver o endereço IP público de uma VM. O exemplo seguinte devolve o endereço IP público do *myVmPrivate* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Substitua `<publicIpAddress>` no comando seguinte, com o IP público endereço devolvido pelo comando anterior e, em seguida, introduza o seguinte comando: 

```powershell
mstsc /v:<publicIpAddress>
```

Um ficheiro de protocolo de ambiente de trabalho remoto (RDP) é criado e transferido para o seu computador. Abra o ficheiro rdp transferido. Se lhe for pedido, selecione **Connect**. Introduza o nome de utilizador e palavra-passe que especificou ao criar a VM. Poderá ter de selecionar **mais opções**, em seguida, **utilizar uma conta diferente**, para especificar as credenciais que introduziu quando criou a VM. Selecione **OK**. Poderá receber um aviso de certificado durante o processo de início de sessão. Se receber o aviso, selecione **Sim** ou **continuar**, prossiga com a ligação.

No *myVmPrivate* VM, a partilha de ficheiros do Azure para a unidade Z através do PowerShell do mapa. Antes de executar os comandos que se seguem, substitua `<storage-account-key>` e `<storage-account-name>` com valores do utilizador fornecidas ou obtidos no [criar uma conta de armazenamento](#create-a-storage-account).

```powershell
$acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
```
PowerShell devolve o resultado semelhante ao seguinte exemplo de saída:

```powershell
Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
Z                                      FileSystem    \\vnt.file.core.windows.net\my-f...
```

A partilha de ficheiros do Azure mapeada com êxito para a unidade Z.

Confirme que a VM possui sem conectividade de saída para outros endereços IP públicos:

```powershell
ping bing.com
```

Receberá não existem respostas, porque o grupo de segurança de rede associados para o *privada* sub-rede não permite o acesso de saída para endereços IP públicos que não sejam endereços atribuídos para este serviço de armazenamento do Azure.

Fechar a sessão de ambiente de trabalho remoto para o *myVmPrivate* VM.

## <a name="confirm-access-is-denied-to-storage-account"></a>Confirmar o acesso foi negado à conta de armazenamento

Obter o endereço IP público do *myVmPublic* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPublic `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Substitua `<publicIpAddress>` no comando seguinte, com o IP público endereço devolvido pelo comando anterior e, em seguida, introduza o seguinte comando: 

```powershell
mstsc /v:<publicIpAddress>
```

No *myVmPublic* VM, tentar mapear a partilha de ficheiros do Azure para a unidade Z. Antes de executar os comandos que se seguem, substitua `<storage-account-key>` e `<storage-account-name>` com valores do utilizador fornecidas ou obtidos no [criar uma conta de armazenamento](#create-a-storage-account).

```powershell
$acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
```

É negado o acesso à partilha e recebe um `New-PSDrive : Access is denied` erro. O acesso é negado porque o *myVmPublic* VM for implementada no *pública* sub-rede. O *pública* sub-rede não tem um ponto final de serviço ativado para o Storage do Azure e a conta de armazenamento só permite o acesso de rede do *privada* sub-rede, não o *público*sub-rede.

Fechar a sessão de ambiente de trabalho remoto para o *myVmPublic* VM.

Do seu computador, tente ver as partilhas de ficheiros na conta de armazenamento com o seguinte comando:

```powershell-interactive
Get-AzureStorageFile `
  -ShareName my-file-share `
  -Context $storageContext
```

Acesso negado e recebe um *Get-AzureStorageFile: O servidor remoto devolveu um erro: proibido (403). Código de estado HTTP: 403 - mensagem de erro de HTTP: este pedido não está autorizado a efetuar esta operação* erro, porque o computador não está a ser o *privada* sub-rede do *MyVirtualNetwork* rede virtual.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, pode utilizar [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos e todos os recursos que contém:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ativar um ponto final de serviço para uma sub-rede de rede virtual. Aprendeu a que os pontos finais de serviço podem ser ativados para recursos implementados com vários serviços do Azure. Criou uma conta de armazenamento do Azure e o acesso limitado à rede para a conta de armazenamento para apenas os recursos dentro de uma sub-rede de rede virtual. Antes de criar pontos finais de serviço na produção redes virtuais, é recomendado que lhe exaustivamente familiarizar-se com [pontos finais de serviço](virtual-network-service-endpoints-overview.md).

Se tiver várias redes virtuais na sua conta, poderá pretender ligar duas redes virtuais em conjunto para que recursos dentro de cada rede virtual podem comunicar entre si. Avançar para o próximo tutorial para saber como ligar redes virtuais.

> [!div class="nextstepaction"]
> [Ligar redes virtuais](./tutorial-connect-virtual-networks-powershell.md)
