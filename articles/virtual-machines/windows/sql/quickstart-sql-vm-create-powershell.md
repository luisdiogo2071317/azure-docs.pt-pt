---
title: Criar VM do SQL Server do Windows com o Azure PowerShell | Microsoft Docs
description: "Este tutorial mostra como criar uma máquina virtual do SQL Server 2017 do Windows com o Azure PowerShell."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 12/12/2017
ms.author: jroth
ms.openlocfilehash: 9af08fd46314ff102eff95e0832f7ce96bc161d6
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-sql-server-windows-virtual-machine-with-azure-powershell"></a>Criar uma máquina virtual do SQL Server do Windows com o Azure PowerShell

Estes passos de início rápido explicam a criação de uma máquina virtual do SQL Server com o Azure PowerShell.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

Este início rápido requer a versão 3.6 ou posterior do módulo Azure PowerShell. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps)(Instalar o módulo do Azure PowerShell).

## <a name="configure-powershell"></a>Configurar o PowerShell

1. Abra o PowerShell e estabeleça o acesso à sua conta do Azure ao executar o comando **Add-AzureRmAccount**.

   ```PowerShell
   Add-AzureRmAccount
   ```

1. Deverá ver um ecrã de início de sessão para introduzir as suas credenciais. Utilize o mesmo e-mail e palavra-passe que utiliza para iniciar sessão no portal do Azure.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

1. Defina uma variável com um nome de grupo de recursos exclusivo. Para simplificar o resto do início rápido, os comandos restantes utilizam este nome como base para outros nomes de recursos.

   ```PowerShell
   $ResourceGroupName = "sqlvm1"
   ```

1. Defina a localização de uma região do Azure de destino para todos os recursos da VM.

   ```PowerShell
   $Location = "East US"
   ```

1. Crie o grupo de recursos.

   ```PowerShell
   New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
   ```

## <a name="configure-network-settings"></a>Configurar as definições de rede

1. Crie uma rede virtual, uma sub-rede e um endereço IP público. Estes recursos são utilizados para fornecer conectividade de rede à máquina virtual e ligá-la à Internet.

   ``` PowerShell
   $SubnetName = $ResourceGroupName + "subnet"
   $VnetName = $ResourceGroupName + "vnet"
   $PipName = $ResourceGroupName + $(Get-Random)

   # Create a subnet configuration
   $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix 192.168.1.0/24

   # Create a virtual network
   $Vnet = New-AzureRmVirtualNetwork -ResourceGroupName $ResourceGroupName -Location $Location `
      -Name VnetName -AddressPrefix 192.168.0.0/16 -Subnet $SubnetConfig

   # Create a public IP address and specify a DNS name
   $Pip = New-AzureRmPublicIpAddress -ResourceGroupName $ResourceGroupName -Location $Location `
      -AllocationMethod Static -IdleTimeoutInMinutes 4 -Name $PipName
   ```

1. Crie um grupo de segurança de rede. Configure regras para permitir ligações de ambiente de trabalho remoto (RDP) e de SQL Server.

   ```PowerShell
   # Rule to allow remote desktop (RDP)
   $NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp `
      -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow

   #Rule to allow SQL Server connections on port 1433
   $NsgRuleSQL = New-AzureRmNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp `
      -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow

   # Create the network security group
   $NsgName = $ResourceGroupName + "nsg"
   $Nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName `
      -Location $Location -Name $NsgName `
      -SecurityRules $NsgRuleRDP,$NsgRuleSQL
   ```

1. Crie a interface de rede.

   ```PowerShell
   $InterfaceName = $ResourceGroupName + "int"
   $Interface = New-AzureRmNetworkInterface -Name $InterfaceName `
      -ResourceGroupName $ResourceGroupName -Location $Location `
      -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $Pip.Id `
      -NetworkSecurityGroupId $Nsg.Id
   ```

## <a name="create-the-sql-vm"></a>Criar a VM do SQL

1. Defina as suas credenciais para iniciar sessão na VM. O nome de utilizador é "azureadmin". Certifique-se de que altera a palavra-passe antes de executar o comando.

   ``` PowerShell
   # Define a credential object
   $SecurePassword = ConvertTo-SecureString 'Change.This!000' `
      -AsPlainText -Force
   $Cred = New-Object System.Management.Automation.PSCredential ("azureadmin", $securePassword)
   ```

1. Crie um objeto de configuração de máquina virtual e, em seguida, crie a VM. O comando seguinte cria uma VM do SQL Server 2017 Developer Edition no Windows Server 2016.

   ```PowerShell
   # Create a virtual machine configuration
   $VMName = $ResourceGroupName + "VM"
   $VMConfig = New-AzureRmVMConfig -VMName $VMName -VMSize Standard_DS13 | `
      Set-AzureRmVMOperatingSystem -Windows -ComputerName $VMName -Credential $Cred -ProvisionVMAgent -EnableAutoUpdate | `
      Set-AzureRmVMSourceImage -PublisherName "MicrosoftSQLServer" -Offer "SQL2017-WS2016" -Skus "SQLDEV" -Version "latest" | `
      Add-AzureRmVMNetworkInterface -Id $Interface.Id
   
   # Create the VM
   New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VMConfig
   ```

   > [!TIP]
   > Demora alguns minutos a criar a VM.

## <a name="install-the-sql-iaas-agent"></a>Instalar o Agente Iaas do SQL

Para obter a integração do portal e as funcionalidades da VM do SQL, tem de instalar a [Extensão do Agente IaaS do SQL Server](virtual-machines-windows-sql-server-agent-extension.md). Para instalar o agente da nova VM, execute o seguinte comando depois de ser criado.

   ```PowerShell
   Set-AzureRmVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
   ```

## <a name="remote-desktop-into-the-vm"></a>Ambiente de trabalho remoto na VM

1. Utilize o seguinte comando para obter o endereço IP Público para a nova VM.

   ```PowerShell
   Get-AzureRmPublicIpAddress -ResourceGroupName $ResourceGroupName | Select IpAddress
   ```

1. Em seguida, transmita o endereço IP devolvido como um parâmetro da linha de comandos para **mstsc** para iniciar uma sessão de Ambiente de Trabalho Remoto na nova VM.

   ```
   mstsc /v:<publicIpAddress>
   ```

1. Quando lhe forem pedidas as credenciais, introduza credenciais para uma conta diferente. Introduza o nome de utilizador com uma barra invertida precedente (por exemplo, `\azureadmin`) e a palavra-passe definida anteriormente neste início rápido.

## <a name="connect-to-sql-server"></a>Ligar ao SQL Server

1. Depois de iniciar sessão no Ambiente de Trabalho Remoto, inicie o **SQL Server Management Studio 2017** a partir do menu Iniciar.

1. Na caixa de diálogo **Ligar ao Servidor**, mantenha as predefinições. O nome do servidor é o nome da VM. A autenticação está definida como **Autenticação do Windows**. Clique em **Ligar**.

Está agora ligado ao SQL Server localmente. Se quiser ligar remotamente, tem de [configurar a conectividade](virtual-machines-windows-sql-connect.md) a partir do portal ou manualmente.

## <a name="clean-up-resources"></a>Limpar recursos

Se não precisar que a VM seja executada continuamente, pode evitar despesas desnecessárias ao pará-la quando não estiver em utilização. O comando seguinte para a VM, mas deixa-a disponível para utilização futura.

```PowerShell
Stop-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
```

Também pode eliminar permanentemente todos os recursos associados à máquina virtual com o comando **Remove-AzureRmResourceGroup**. Isto também elimina permanentemente a máquina virtual, pelo que utilize este comando com cuidado.

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, criou uma máquina virtual do SQL Server 2017 com o Azure PowerShell. Para saber mais sobre como migrar os seus dados para o novo SQL Server, consulte o seguinte artigo.

> [!div class="nextstepaction"]
> [Migrar uma base de dados para uma VM de SQL](virtual-machines-windows-migrate-sql.md)