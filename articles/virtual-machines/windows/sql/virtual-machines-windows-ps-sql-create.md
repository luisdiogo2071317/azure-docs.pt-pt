---
title: Guia de aprovisionamento para VMs do SQL Server com o Azure PowerShell | Documentos da Microsoft
description: Fornece passos e comandos do PowerShell para criar uma VM do Azure com imagens de Galeria de máquinas virtuais do SQL Server.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
editor: ''
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/15/2018
ms.author: jroth
ms.openlocfilehash: bb7a0b8c2d0511088282e180a108f8d925f0e4e8
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/10/2018
ms.locfileid: "42056927"
---
# <a name="how-to-provision-sql-server-virtual-machines-with-azure-powershell"></a>Como aprovisionar máquinas virtuais do SQL Server com o Azure PowerShell

Este guia explica as opções para criar VMs do SQL Server do Windows com o Azure PowerShell. Para obter um exemplo do Azure PowerShell simplificado com valores de predefinição mais, consulte a [início rápido de VM de SQL do Azure PowerShell](quickstart-sql-vm-create-powershell.md).

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

Este artigo requer a versão do módulo 3.6 ou posterior do Azure PowerShell. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps)(Instalar o módulo do Azure PowerShell).

## <a name="configure-your-subscription"></a>Configurar a sua subscrição

1. Abra o PowerShell e estabeleça o acesso à sua conta do Azure ao executar o comando **Connect-AzureRmAccount**.

   ```PowerShell
   Connect-AzureRmAccount
   ```

1. Deverá ver um ecrã de início de sessão para introduzir as suas credenciais. Utilize o mesmo e-mail e palavra-passe que utiliza para iniciar sessão no portal do Azure.

## <a name="define-image-variables"></a>Definir variáveis de imagem
Para simplificar as criações de reutilização e script, comece definindo um número de variáveis. Altere os valores de parâmetro como julgar, mas tome cuidado com a nomenclatura restrições relacionadas com comprimentos de nome e carateres especiais ao modificar os valores fornecidos.

### <a name="location-and-resource-group"></a>Localização e grupo de recursos
Utilize duas variáveis para definir a região de dados e o grupo de recursos no qual criar os outros recursos para a máquina virtual.

Modifique conforme pretendido e, em seguida, execute os seguintes cmdlets para inicializar estas variáveis.

```PowerShell
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm2"
```

### <a name="storage-properties"></a>Propriedades de armazenamento
Utilize as seguintes variáveis para definir a conta de armazenamento e o tipo de armazenamento a ser utilizado pela máquina virtual.

Modifique conforme pretendido e, em seguida, execute o seguinte cmdlet para inicializar estas variáveis. Tenha em atenção que neste exemplo, estamos a utilizar [o armazenamento Premium](../premium-storage.md), que é recomendado para cargas de trabalho de produção.

```PowerShell
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"
```

### <a name="network-properties"></a>Propriedades de rede
Utilize as seguintes variáveis para definir a interface de rede, o método de alocação de TCP/IP, o nome de rede virtual, o nome de sub-rede virtual, o intervalo de endereços IP para a rede virtual, o intervalo de endereços IP para a sub-rede e a etiqueta de nome de domínio público para ser utilizado pela rede na máquina virtual.

Modifique conforme pretendido e, em seguida, execute o seguinte cmdlet para inicializar estas variáveis.

```PowerShell
$InterfaceName = $ResourceGroupName + "ServerInterface"
$NsgName = $ResourceGroupName + "nsg"
$TCPIPAllocationMethod = "Dynamic"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$DomainName = $ResourceGroupName
```

### <a name="virtual-machine-properties"></a>Propriedades da máquina virtual
Utilize as seguintes variáveis para definir o nome da máquina virtual, o nome do computador, o tamanho de máquina virtual e o nome do disco de sistema operativo da máquina virtual.

Modifique conforme pretendido e, em seguida, execute o seguinte cmdlet para inicializar estas variáveis.

```PowerShell
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"
```

### <a name="choose-a-sql-server-image"></a>Escolha uma imagem do SQL Server
Utilize as seguintes variáveis para definir a imagem do SQL Server a utilizar para a máquina virtual.

1. Em primeiro lugar, listar todas as ofertas de imagem do SQL Server com o **Get-AzureRmVMImageOffer** comando:

   ```PowerShell
   Get-AzureRmVMImageOffer -Location $Location -Publisher 'MicrosoftSQLServer'
   ```

1. Para este tutorial, utilize as seguintes variáveis para especificar o SQL Server 2017 no Windows Server 2016.

   ```PowerShell
   $OfferName = "SQL2017-WS2016"
   $PublisherName = "MicrosoftSQLServer"
   $Version = "latest"
   ```

1. Em seguida, listar as edições disponíveis para a sua oferta.

   ```PowerShell
   Get-AzureRmVMImageSku -Location $Location -Publisher 'MicrosoftSQLServer' -Offer $OfferName | Select Skus
   ```

1. Neste tutorial, utilize a edição do SQL Server 2017 Developer (**SQLDEV**). A edição de programador livremente está licenciada para o teste e desenvolvimento e paga apenas o custo de execução da VM.

   ```PowerShell
   $Sku = "SQLDEV"
   ```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos
Com o modelo de implementação do Resource Manager, o primeiro objeto criado por si, é o grupo de recursos. Utilize o [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet para criar um grupo de recursos do Azure e os respetivos recursos com o nome do grupo de recursos e a localização definido pelas variáveis que anteriormente inicializado.

Execute o seguinte cmdlet para criar o seu novo grupo de recursos.

```PowerShell
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
```

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento
A máquina virtual requer recursos de armazenamento para o disco do sistema operativo e para os ficheiros de dados e de registo do SQL Server. Para simplificar, vamos criar um único disco para ambos. Pode anexar discos adicionais posteriormente utilizando o [Azure-adicionar disco](/powershell/module/servicemanagement/azure/add-azuredisk) cmdlet para poder colocar os seus dados do SQL Server e o registo de ficheiros em discos dedicados. Utilize o [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet para criar uma conta de armazenamento standard em seu novo grupo de recursos e com o nome da conta de armazenamento, o nome de Sku de armazenamento e a localização definido usando as variáveis que anteriormente inicializado .

Execute o seguinte cmdlet para criar a sua nova conta de armazenamento.

```PowerShell
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName `
   -Name $StorageName -SkuName $StorageSku `
   -Kind "Storage" -Location $Location
```

> [!TIP]
> Criar a conta de armazenamento, pode demorar alguns minutos.

## <a name="create-network-resources"></a>Criar recursos de rede
A máquina virtual requer um número de recursos de rede para conectividade de rede.

* Cada máquina virtual requer uma rede virtual.
* Uma rede virtual tem de ter pelo menos uma subrede definida.
* Uma interface de rede tem de ser definida com um público ou um endereço IP privado.

### <a name="create-a-virtual-network-subnet-configuration"></a>Criar uma configuração de sub-rede de rede virtual
Comece por criar uma configuração de sub-rede para a nossa rede virtual. Para o nosso tutorial, vamos criar uma sub-rede de predefinição com o [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) cmdlet. Vamos criar nossa configuração de sub-rede de rede virtual com o prefixo de nome e endereço de sub-rede definido usando as variáveis que anteriormente inicializado.

> [!NOTE]
> Pode definir propriedades adicionais da configuração de sub-rede de rede virtual com este cmdlet, mas que está além do escopo deste tutorial.

Execute o seguinte cmdlet para criar a configuração de sub-rede virtual.

```PowerShell
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
```

### <a name="create-a-virtual-network"></a>Criar uma rede virtual
Em seguida, crie a sua rede virtual com o [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) cmdlet. Criar a rede virtual no seu novo grupo de recursos, com o nome, a localização e o endereço de prefixo definido usando as variáveis que anteriormente inicializado e utilizar a configuração de sub-rede que definiu no passo anterior.

Execute o seguinte cmdlet para criar a rede virtual.

```PowerShell
$VNet = New-AzureRmVirtualNetwork -Name $VNetName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
```

### <a name="create-the-public-ip-address"></a>Criar o endereço IP público
Agora que temos nossa rede virtual definido, é necessário configurar um endereço IP para a conectividade para a máquina virtual. Para este tutorial, vamos criar um endereço IP público a utilizar para suportar a conectividade à Internet de endereçamento de IP dinâmico. Utilize o [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) cmdlet para criar o IP público de endereços no grupo de recursos que criou anteriormente e com o nome, a localização, o método de alocação e a etiqueta de nome de domínio DNS definido usando as variáveis que anteriormente inicializado.

> [!NOTE]
> Pode definir propriedades adicionais do endereço IP público utilizando este cmdlet, mas que está além do escopo deste tutorial inicial. Também é possível criar um endereço privado ou um endereço com um endereço estático, mas que também está além do escopo deste tutorial.

Execute o seguinte cmdlet para criar o seu endereço IP público.

```PowerShell
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
```

### <a name="create-the-network-security-group"></a>Criar o grupo de segurança de rede
Para proteger o tráfego VM e o SQL Server, crie um grupo de segurança de rede.

1. Primeiro, crie uma regra de grupo de segurança de rede para RDP permitir ligações de ambiente de trabalho remotas.

   ```PowerShell
   $NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp `
      -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
   ```
1. Configure uma regra de grupo de segurança de rede que permita o tráfego na porta TCP 1433. Isto permite ligações ao SQL Server através da internet.

   ```PowerShell
   $NsgRuleSQL = New-AzureRmNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp `
      -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
   ```

1. Em seguida, crie o grupo de segurança de rede.

   ```PowerShell
   $Nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName `
      -Location $Location -Name $NsgName `
      -SecurityRules $NsgRuleRDP,$NsgRuleSQL
   ```

### <a name="create-the-network-interface"></a>Criar a interface de rede
Agora estamos prontos para criar a interface de rede que irá utilizar a nossa máquina virtual. Chamamos a [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) cmdlet para criar nossa interface de rede no grupo de recursos que criou anteriormente e com o nome, a localização, a sub-rede e o endereço IP público definido anteriormente.

Execute o seguinte cmdlet para criar sua interface de rede.

```PowerShell
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName `
   -ResourceGroupName $ResourceGroupName -Location $Location `
   -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id `
   -NetworkSecurityGroupId $Nsg.Id
```

## <a name="configure-a-vm-object"></a>Configurar um objeto VM
Agora que temos os recursos de armazenamento e de rede definidos, estamos prontos para definir os recursos de computação para a máquina virtual. Para o nosso tutorial, vamos especificar o tamanho de máquina virtual e várias propriedades do sistema operativo, especifique a interface de rede que criámos anteriormente, definir o armazenamento de BLOBs e, em seguida, especifique o disco do sistema operativo.

### <a name="create-the-vm-object"></a>Criar o objeto VM
Comece por especificar o tamanho da máquina virtual. Para este tutorial, estamos especificando um DS13. Chamamos a [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) cmdlet para criar um objeto de máquinas virtuais configuráveis com o nome e o tamanho definido usando as variáveis que anteriormente inicializado.

Execute o seguinte cmdlet para criar o objeto de máquina virtual.

```PowerShell
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
```

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Criar um objeto de credencial para conter o nome e a palavra-passe para as credenciais de administrador local
Antes, pode definir as propriedades do sistema operativo da máquina virtual, é necessário fornecer as credenciais da conta de administrador local como uma cadeia segura. Para tal, utilize o [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.

Execute o seguinte cmdlet e, na janela de pedido de credencial do PowerShell, escreva o nome e a palavra-passe para a conta de administrador local na máquina virtual.

```PowerShell
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
```

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Defina as propriedades do sistema operativo da máquina virtual
Agora estamos prontos para definir as propriedades do sistema operativo da máquina virtual com [conjunto AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) cmdlet para definir o tipo de sistema operativo como Windows, requerem a [agente da máquina virtual](../../extensions/agent-windows.md) para ser instalado, especificar que o cmdlet permite a atualização automática e defina o nome da máquina virtual, o nome do computador e a credencial a utilizar as variáveis que anteriormente inicializado.

Execute o seguinte cmdlet para definir as propriedades do sistema operativo para a máquina virtual.

```PowerShell
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine `
   -Windows -ComputerName $ComputerName -Credential $Credential `
   -ProvisionVMAgent -EnableAutoUpdate
```

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Adicionar a interface de rede para a máquina virtual
Em seguida, adicionamos a interface de rede que criamos anteriormente para a máquina virtual. Chamar o [Add-AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) cmdlet para adicionar a interface de rede usando a variável de interface de rede que definiu anteriormente.

Execute o seguinte cmdlet para definir a interface de rede para a máquina virtual.

```PowerShell
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
```

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Defina a localização de armazenamento de BLOBs para o disco a ser utilizado pela máquina virtual
Em seguida, defina a localização de armazenamento de BLOBs para o disco a ser utilizado pela máquina virtual com as variáveis que definiu anteriormente.

Execute o seguinte cmdlet para definir o local de armazenamento de Blobs.

```PowerShell
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
```

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Definir o sistema operativo as propriedades do disco para a máquina virtual
Em seguida, configure o sistema operativo as propriedades do disco para a máquina virtual. Utilize o [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk) cmdlet para especificar que o sistema operativo da máquina virtual será proveniente de uma imagem, para definir a colocação em cache para ler apenas (porque o SQL Server está a ser instalado no mesmo disco) e definir a máquina virtual o nome e o disco do sistema operativo definido usando as variáveis que definimos anteriormente.

Execute o seguinte cmdlet para definir o sistema operativo as propriedades do disco para a máquina virtual.

```PowerShell
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name `
   $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage
```

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Especifique a imagem de plataforma para a máquina virtual
Nossa última etapa de configuração é especificar a imagem de plataforma para a nossa máquina virtual. Para o nosso tutorial, estamos a utilizar a imagem mais recente do CTP do SQL Server 2016. Utilizar o [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage) cmdlet para utilizar esta imagem, conforme definido pelas variáveis que definiu anteriormente.

Execute o seguinte cmdlet para especificar a imagem de plataforma para a máquina virtual.

```PowerShell
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine `
   -PublisherName $PublisherName -Offer $OfferName `
   -Skus $Sku -Version $Version
```

## <a name="create-the-sql-vm"></a>Criar a VM do SQL
Agora que concluiu os passos de configuração, está pronto para criar a máquina virtual. Utilize o [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) cmdlet para criar a máquina virtual com as variáveis que definimos.

Execute o seguinte cmdlet para criar a sua máquina virtual.

```PowerShell
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine
```

A máquina virtual é criada.

> [!NOTE]
> Pode ignorar o erro sobre o diagnóstico de arranque. É criada uma conta de armazenamento standard para o diagnóstico de arranque, porque a conta de armazenamento especificada para o disco da máquina virtual é uma conta de armazenamento premium.

## <a name="install-the-sql-iaas-agent"></a>Instalar o Agente Iaas do SQL
As máquinas de virtuais do SQL Server suportam funcionalidades de gestão automatizadas com o [extensão de agente IaaS do SQL Server](virtual-machines-windows-sql-server-agent-extension.md). Para instalar o agente da nova VM, execute o seguinte comando depois de ser criado.

   ```PowerShell
   Set-AzureRmVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
   ```

## <a name="remove-a-test-vm"></a>Remover uma VM de teste

Se não precisar que a VM seja executada continuamente, pode evitar despesas desnecessárias ao pará-la quando não estiver em utilização. O comando seguinte para a VM, mas deixa-a disponível para utilização futura.

```PowerShell
Stop-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
```

Também pode eliminar permanentemente todos os recursos associados à máquina virtual com o comando **Remove-AzureRmResourceGroup**. Isto também elimina permanentemente a máquina virtual, pelo que utilize este comando com cuidado.

## <a name="example-script"></a>Script de exemplo
O script seguinte contém o script de PowerShell completo para este tutorial. Parte do princípio de que já a configuração subscrição do Azure para utilizar com o **Connect-AzureRmAccount** e **Select-AzureRmSubscription** comandos.

```PowerShell
# Variables

## Global
$Location = "SouthCentralUS"
$ResourceGroupName = "sqlvm2"

## Storage
$StorageName = $ResourceGroupName + "storage"
$StorageSku = "Premium_LRS"

## Network
$InterfaceName = $ResourceGroupName + "ServerInterface"
$NsgName = $ResourceGroupName + "nsg"
$VNetName = $ResourceGroupName + "VNet"
$SubnetName = "Default"
$VNetAddressPrefix = "10.0.0.0/16"
$VNetSubnetAddressPrefix = "10.0.0.0/24"
$TCPIPAllocationMethod = "Dynamic"
$DomainName = $ResourceGroupName

##Compute
$VMName = $ResourceGroupName + "VM"
$ComputerName = $ResourceGroupName + "Server"
$VMSize = "Standard_DS13"
$OSDiskName = $VMName + "OSDisk"

##Image
$PublisherName = "MicrosoftSQLServer"
$OfferName = "SQL2017-WS2016"
$Sku = "SQLDEV"
$Version = "latest"

# Resource Group
New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

# Storage
$StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

# Network
$SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
$VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
$PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
$NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow
$NsgRuleSQL = New-AzureRmNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow
$Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id -NetworkSecurityGroupId $Nsg.Id

# Compute
$VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
$Credential = Get-Credential -Message "Type the name and password of the local administrator account."
$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
$OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

# Image
$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

# Create the VM in Azure
New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

# Add the SQL IaaS Extension
Set-AzureRmVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
```

## <a name="next-steps"></a>Passos Seguintes
Depois da máquina virtual é criada, pode:

- Ligar à máquina virtual utilizando o ambiente de trabalho remoto (RDP).
- Configurar definições do SQL Server no portal para a sua VM, incluindo:
   - [Definições de armazenamento](virtual-machines-windows-sql-server-storage-configuration.md) 
   - [Tarefas de gerenciamento automatizado](virtual-machines-windows-sql-server-agent-extension.md)
- [Configurar a conectividade](virtual-machines-windows-sql-connect.md).
- Ligue-se de que os clientes e aplicações para a nova instância do SQL Server.

