---
title: Como alterar o modelo de licenciamento de uma VM do SQL no Azure | Documentos da Microsoft
description: Saiba como mudar o licenciamento para uma máquina virtual do SQL no Azure.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/14/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 3bcbfc876ed27d16180ca03801f2054ad804e148
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52500403"
---
# <a name="how-to-change-the-licensing-model-for-a-sql-virtual-machine-in-azure"></a>Como alterar o modelo de licenciamento para uma máquina virtual do SQL no Azure
Este artigo descreve como alterar o modelo de licenciamento para uma máquina virtual do SQL Server no Azure com o novo fornecedor de recursos do SQL - **Microsoft.SqlVirtualMachine**. Há dois modelos para uma máquina virtual (VM) a alojar o SQL Server - pagamento por utilização, de licenciamento e traga a sua própria licença (BYOL). E agora, com o Powershell ou a CLI do Azure, pode modificar o modelo de licenciamento utiliza a sua VM do SQL. 

O **pagamento por utilização** modelo significa que o custo por segundo de execução da VM do Azure inclui o custo da licença do SQL Server.

O **Bring-your-own-license** modelo também é conhecido como o [benefício híbrido do Azure](https://azure.microsoft.com/pricing/hybrid-benefit/), e permite que use sua própria licença do SQL Server com uma VM a executar o SQL Server. Para obter mais informações sobre os preços, consulte [guia de preços de VM do SQL](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-pricing-guidance).

Alternar entre os dois modelos de licença incorre **sem período de indisponibilidade**, não reinicia a VM, adiciona **sem custos adicionais** (na verdade, ativar AHB reduz o custo) e é **efeitos imediatos**. 


## <a name="register-legacy-sql-vm-with-new-resource-provider"></a>Registar herdados VM do SQL Server com o novo fornecedor de recursos
A capacidade de alternar entre modelos de licenciamento é um recurso fornecido pelo fornecedor de recursos de nova VM do SQL (Microsoft.SqlVirtualMachine). Neste momento, ser capaz de alternar o seu modelo de licenciamento, primeiro terá de registar o novo fornecedor para a sua subscrição e, em seguida, registe-se a VM legada com o novo fornecedor de recursos de VM do SQL Server. 

  >[!IMPORTANT]
  > Se remover o seu recurso de VM do SQL Server, irá voltar para a definição de licença hard-coded da imagem. 

### <a name="powershell"></a>PowerShell

O código a seguir primeiramente se registra o novo fornecedor de recursos do SQL com a sua subscrição e, em seguida, regista a VM do SQL legado com o fornecedor de recursos novo. 

```powershell
# Connect to Azure
Connect-AzureRmAccount
Account: <account_name>

# Verify your subscription ID
Get-AzureRmContext

# Set the correct Azure Subscription ID
Set-AzureRmContext -SubscriptionId <Subscription_ID>

# Register the new SQL resource provider for your subscription
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine

# Register your legacy SQL VM with the new resource provider
# example: $vm=Get-AzureRmVm -ResourceGroupName AHBTest -Name AHBTest
$vm=Get-AzureRmVm -ResourceGroupName <ResourceGroupName> -Name <VMName>
New-AzureRmResource -ResourceName $vm.Name ResourceGroupName $vm.ResourceGroupName -Location $vm.Location -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines -Properties @{virtualMachineResourceId=$vm.Id}
```

### <a name="portal"></a>Portal
Também pode registar o fornecedor de recursos de VM do SQL nova com o portal. Para tal, siga estes passos:
1. Abra o portal do Azure e navegue para **todos os serviços**. 
1. Navegue para **subscrições** e selecione a subscrição de interesse.  

  ![Selecione a sua subscrição](media/virtual-machines-windows-sql-ahb/open-subscriptions.png)

1. Na **subscrições** painel, navegue até à **fornecedor de recursos**. 
1. Tipo de `sql` no filtro para apresentar os fornecedores de recursos relacionados com o SQL. 
1. Selecione *registar*, *voltar a registar*, ou *Unregister* para o **Microsoft.SqlVirtualMachine** fornecedor consoante sua ação desejada. 

  ![Modificar o fornecedor](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)


## <a name="use-powershell"></a>Utilizar o PowerShell 
Pode utilizar o Powershell para alterar o seu modelo de licenciamento.  Certifique-se de que a VM do SQL já foi registrada com o novo fornecedor de recursos do SQL antes de mudar o modelo de licenciamento. 

Este fragmento de código muda o seu modelo de licença de pagamento por utilização para BYOL (ou utilizar o benefício híbrido do Azure): 
```powershell
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="AHUB"
$SqlVm | Set-AzureRmResource -Force 
``` 

Este fragmento de código muda o seu modelo BYOL para pagamento por utilização:
```powershell
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName AHBTest -ResourceName AHBTest
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType="PAYG"
$SqlVm | Set-AzureRmResource -Force 
```

  >[!NOTE]
  > Para alternar entre licenças, tem de utilizar o novo fornecedor de recursos de VM do SQL Server. Se tentar executar estes comandos antes de registar a VM do SQL com o novo fornecedor, pode encontrar este erro: `Get-AzureRmResource : The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. The property 'sqlServerLicenseType' cannot be found on this object. Verify that the property exists and can be set. ` se vir este erro, consulte [registar a VM do SQL com o fornecedor de recursos novo](#register-legacy-SQL-vm-with-new-resource-provider). 
 

## <a name="use-azure-cli"></a>Utilizar a CLI do Azure
Pode utilizar a CLI do Azure para alterar o seu modelo de licenciamento.  Certifique-se de que a VM do SQL já foi registrada com o novo fornecedor de recursos do SQL antes de mudar o modelo de licenciamento. 

Este fragmento de código muda o seu modelo de licença de pagamento por utilização para BYOL (ou utilizar o benefício híbrido do Azure):
```azurecli
az resource update -g <resource_group_name> -n <sql_virtual_machine_name> --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=AHUB
# example: az resource update -g AHBTest -n AHBTest --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=AHUB
```

Este fragmento de código muda o seu modelo BYOL para pagamento por utilização: 
```azurecli
az resource update -g <resource_group_name> -n <sql_virtual_machine_name> --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=PAYG
# example: az resource update -g AHBTest -n AHBTest --resource-type "Microsoft.SqlVirtualMachine/SqlVirtualMachines" --set properties.sqlServerLicenseType=PAYG
```

  >[!NOTE]
  >Para alternar entre licenças, tem de utilizar o novo fornecedor de recursos de VM do SQL Server. Se tentar executar estes comandos antes de registar a VM do SQL com o novo fornecedor, pode encontrar este erro: `The Resource 'Microsoft.SqlVirtualMachine/SqlVirtualMachines/AHBTest' under resource group 'AHBTest' was not found. ` se vir este erro, consulte [registar a VM do SQL com o fornecedor de recursos novo](#register-legacy-SQL-vm-with-new-resource-provider). 

## <a name="view-current-licensing"></a>Vista atual licenciamento 

O fragmento de código seguinte permite-lhe ver o seu modelo de licenciamento atual para a sua VM de SQL. 

```powershell
# View current licensing model for your SQL VM
#example: $SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm = Get-AzureRmResource -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines -ResourceGroupName <resource_group_name> -ResourceName <VM_name>
$SqlVm.Properties.sqlServerLicenseType
```

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações, veja os artigos seguintes: 

* [Descrição geral do SQL Server num VM do Windows](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server num FAQ de VM do Windows](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server numa orientação de preços de VM windows](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server num notas de versão de VM do Windows](virtual-machines-windows-sql-server-iaas-release-notes.md)


