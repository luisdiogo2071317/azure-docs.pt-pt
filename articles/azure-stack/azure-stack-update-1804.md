---
title: Atualização de pilha do Azure 1804 | Microsoft Docs
description: Saiba mais sobre as novidades na atualização 1804 para pilha do Azure integrado sistemas, problemas conhecidos e onde pode transferir a atualização.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: 2c2813a7f2d909a23c8f5d4f5ac0280b3f932ba6
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34700129"
---
# <a name="azure-stack-1804-update"></a>Atualização de pilha 1804 do Azure

*Aplica-se a: Azure pilha integrado sistemas*

Este artigo descreve os melhoramentos e corrige no pacote de atualização de 1804, problemas conhecidos para esta versão e onde pode transferir a atualização. Problemas conhecidos são divididos em problemas diretamente relacionada com o processo de atualização e problemas relacionados com a compilação (pós-instalação).

> [!IMPORTANT]        
> Este pacote de atualização é apenas para sistemas de pilha do Azure integrado. Não aplicável este pacote de atualização para o Kit de desenvolvimento de pilha do Azure.

## <a name="build-reference"></a>Referência de compilação    
É o número de compilação de atualização do Azure pilha 1804 **20180513.1**.   

### <a name="new-features"></a>Novas funcionalidades
Esta atualização inclui as seguintes melhorias de pilha do Azure.

- <!-- 15028744 - IS -->  **Visual Studio support for disconnected Azure Stack deployments using AD FS**. Within Visual Studio you now can add subscriptions and authenticate using AD FS federated User credentials. 
 
- <!-- 1779474, 1779458 - IS --> **Use Av2 and F series virtual machines**. Azure Stack can now use virtual machines based on the Av2-series and F-series virtual machine sizes. For more information see [Virtual machine sizes supported in Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes). 

- <!-- 1759172 - IS, ASDK --> **New administrative subscriptions**. With 1804 there are two new subscription types available in the portal. These new subscription types are in addition to the Default Provider subscription and visible with new Azure Stack installations beginning with version 1804. *Do not use these new subscription types with this version of Azure Stack*. We will announce the availability to use these subscription types in with a future update. 

  Se atualizar a pilha do Azure para a versão 1804, os dois novos tipos de subscrição não estão visíveis. No entanto, novas implementações da pilha do Azure integrado sistemas e instalações a versão do Kit de desenvolvimento de pilha do Azure 1804 ou posterior têm acesso a todos os tipos de subscrição de três.  

  Estes novos tipos de subscrição são parte de uma alteração maior para proteger a subscrição do fornecedor predefinida e torna mais fácil de implementar recursos partilhados, como servidores de alojamento do SQL Server. À medida que adiciona mais partes desta alteração maiores com as futuras atualizações à pilha do Azure, os recursos implementados nestes novos tipos de subscrição poderão perder-se. 

  Os tipos de subscrição de três agora visíveis são:  
  - Predefinido a subscrição do fornecedor: continuar a utilizar este tipo de subscrição. 
  - Medição de subscrição: *não utilize este tipo de subscrição.*
  - Subscrição de consumo: *não utilize este tipo de subscrição*

  



## <a name="fixed-issues"></a>Problemas fixos

- <!-- IS, ASDK -->  In the admin portal, you no longer have to refresh the Update tile before it displays information.
 
- <!-- 2050709  -->  You can now use the admin portal to edit storage metrics for Blob service, Table service, and Queue service.
 
- <!-- IS, ASDK --> Under **Networking**, when you click **Connection** to set up a VPN connection, **Site-to-site (IPsec)** is now the only available option.

- **Várias correções** para desempenho, estabilidade, segurança e o sistema operativo que é utilizado pela pilha de Azure.

## <a name="additional-releases-timed-with-this-update"></a>Versões adicionais com esta atualização excedeu o tempo limite  
Os seguintes estão agora disponíveis, mas não necessitam de atualização de pilha do Azure 1804.
- **Atualizar para o pacote de monitorização do Azure pilha do Microsoft System Center Operations Manager**. Uma nova versão (1.0.3.0) o Microsoft System Center Operations Manager monitorização Pack de pilha do Azure está disponível para [transferir](https://www.microsoft.com/download/details.aspx?id=55184). Com esta versão, pode utilizar principais de serviço ao adicionar uma implementação de pilha do Azure ligada. Esta versão inclui também uma experiência de gestão de atualizações que lhe permite efetuar a ação de remediação diretamente a partir do Operations Manager. Também existem novos dashboards que apresentam fornecedores de recursos, unidades de escala e nós de unidade de escala.

- **Novo Azure da pilha administração PowerShell versão 1.3.0**.  O Azure PowerShell pilha 1.3.0 está agora disponível para instalação. Esta versão fornece comandos para todos os fornecedores de recursos de administrador gerir a pilha do Azure.  Com esta versão, algum conteúdo vai ser preterido a partir do GitHub de ferramentas de pilha do Azure [repositório](https://github.com/Azure/AzureStack-Tools). 

   Para obter detalhes de instalação, siga o [instruções](azure-stack-powershell-install.md) ou [ajudar](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) conteúdo para o módulo de pilha do Azure 1.3.0. 

- **Inicial de versão de referência da API Rest do Azure pilha**. O [referência da API para todos os fornecedores de recursos do Azure pilha Admin](https://docs.microsoft.com/rest/api/azure-stack/) agora é publicado. 


## <a name="before-you-begin"></a>Antes de começar    

### <a name="prerequisites"></a>Pré-requisitos
- Instalar a pilha de Azure [1803 atualizar](azure-stack-update-1803.md) antes de aplicar a atualização 1804 de pilha do Azure.    

### <a name="known-issues-with-the-update-process"></a>Problemas conhecidos relacionados com o processo de atualização   
- Durante a instalação da atualização 1804, poderá ver alertas com o título *erro – o modelo para o FaultType UserAccounts.New está em falta.*  Pode ignorar com segurança estes alertas. Estes alertas irão fechar automaticamente depois de concluída a atualização a 1804.   
 
- <!-- TBD - IS --> Do not attempt to create virtual machines during the installation of this update. For more information about managing updates, see [Manage updates in Azure Stack overview](azure-stack-updates.md#plan-for-updates).


### <a name="post-update-steps"></a>Passos pós-atualização
*Existem não existem passos pós-atualização para atualização 1804.*



### <a name="known-issues-post-installation"></a>Problemas conhecidos (pós-instalação)
As seguintes são problemas conhecidos de pós-instalação para a compilação **20180513.1**.

#### <a name="portal"></a>Portal
- <!-- 1272111 - IS --> After you install or update to this version of Azure Stack, you might not be able to view Azure Stack scale units in the Admin portal.  
  Solução: Utilize o PowerShell para ver informações sobre unidades de escala. Para obter mais informações, consulte o [ajudar](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) conteúdo para o módulo de pilha do Azure 1.3.0. 

- <!-- 2332636 - IS -->  When you use AD FS for your Azure Stack identity system and update to this version of Azure Stack, the default owner of the default provider subscription is reset to the built-in **CloudAdmin** user.  
  Solução: Para resolver este problema, depois de instalar esta atualização, utilize o passo 3 do [automatização de Acionador para configurar afirmações de confiança do fornecedor na pilha de Azure](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) procedimento para repor o proprietário da subscrição de fornecedor predefinido.   

- <!-- TBD - IS ASDK --> Some administrative subscription types are not available.  When you upgrade Azure Stack to this version, the two subscription types that were [introduced with version 1804](#new-features) are not visible in the console. This is expected. The unavailable subscription types are *Metering subscription*, and *Consumption subscription*. These subscription types are visible in new Azure Stack environments beginning with version 1804 but are not yet ready for use. You should continue to use the *Default Provider* subscription type.  


- <!-- TBD -  IS ASDK -->The ability [to open a new support request from the dropdown](azure-stack-manage-portals.md#quick-access-to-help-and-support) from within the administrator portal isn’t available. Instead, use the following link:     
    - Para a pilha do Azure integrados sistemas, utilize https://aka.ms/newsupportrequest.

- <!-- 2403291 - IS ASDK --> You might not have use of the horizontal scroll bar along the bottom of the admin and user portals. If you can’t access the horizontal scroll bar, use the breadcrumbs to navigate to a previous blade in the portal by selecting the name of the blade you want to view from the breadcrumb list found at the top left of the portal.
  ![Trilho](media/azure-stack-update-1804/breadcrumb.png) 

- <!-- TBD - IS --> It might not be possible to view compute or storage resources in the administrator portal. The cause of this issue is an error during the installation of the update that causes the update to be incorrectly reported as successful. If this issue occurs, contact Microsoft Customer Support Services for assistance.

- <!-- TBD - IS --> You might see a blank dashboard in the portal. To recover the dashboard, select the gear icon in the upper right corner of the portal, and then select **Restore default settings**.

- <!-- TBD - IS ASDK --> Deleting user subscriptions results in orphaned resources. As a workaround, first delete user resources or the entire resource group, and then delete user subscriptions.

- <!-- TBD - IS ASDK --> You cannot view permissions to your subscription using the Azure Stack portals. As a workaround, use PowerShell to verify permissions.

- <!-- TBD - IS ASDK --> In the admin portal, you might see a critical alert for the *Microsoft.Update.Admin* component. The Alert name, description, and remediation all display as:  
    - *ERRO - o modelo para o FaultType ResourceProviderTimeout está em falta.*

  Este alerta pode ser ignorado com segurança. 


#### <a name="health-and-monitoring"></a>Estado de funcionamento e a monitorização
- <!-- 1264761 - IS ASDK -->  You might see alerts for the *Health controller* component that have the following details:  

   Alerta #1:
   - NOME: Mau estado de funcionamento da função de infraestrutura
   - GRAVIDADE: aviso
   - COMPONENTE: Controlador de estado de funcionamento
   - Descrição: O controlador de estado de funcionamento de Heartbeat Scanner não está disponível. Isto pode afetar métricas e relatórios de estado de funcionamento.  

  Alerta #2:
   - NOME: Mau estado de funcionamento da função de infraestrutura
   - GRAVIDADE: aviso
   - COMPONENTE: Controlador de estado de funcionamento
   - Descrição: O controlador de estado de funcionamento Scanner de falhas não está disponível. Isto pode afetar métricas e relatórios de estado de funcionamento.

  Ambos os alertas podem ser ignoradas. Irá fechar automaticamente ao longo do tempo.  
 

#### <a name="compute"></a>Computação
- <!-- TBD - IS --> When selecting a virtual machine size for a virtual machine deployment, some F-Series VM sizes are not visible as part of the size selector when you create a VM. The following VM sizes do not appear in the selector: *F8s_v2*, *F16s_v2*, *F32s_v2*, and *F64s_v2*.  
  Como solução, utilize um dos seguintes métodos para implementar uma VM. Cada método, tem de especificar o tamanho da VM que pretende utilizar.

  - **Modelo Azure Resource Manager:** quando utilizar um modelo, defina o *vmSize* no modelo para igualar o tamanho da VM pretendido. Por exemplo, o seguinte procedimento é utilizado para implementar uma VM que utiliza o *F32s_v2* tamanho:  

    ```
        "properties": {
        "hardwareProfile": {
                "vmSize": "Standard_F32s_v2"
        },
    ```  
  - **CLI do Azure:** pode utilizar o [az vm criar](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) de comandos e especificar o tamanho da VM como um parâmetro semelhante `--size "Standard_F32s_v2"`.

  - **PowerShell:** com o PowerShell, pode utilizar [New-AzureRMVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-6.0.0) com o parâmetro que especifica o tamanho da VM, semelhante a `-VMSize "Standard_F32s_v2"`.


- <!-- TBD - IS ASDK --> Scaling settings for virtual machine scale sets are not available in the portal. As a workaround, you can use [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Because of PowerShell version differences, you must use the `-Name` parameter instead of `-VMScaleSetName`.

- <!-- TBD - IS --> When you create an availability set in the portal by going to **New** > **Compute** > **Availability set**, you can only create an availability set with a fault domain and update domain of 1. As a workaround, when creating a new virtual machine, create the availability set by using PowerShell, CLI, or from within the portal.

- <!-- TBD - IS ASDK --> When you create virtual machines on the Azure Stack user portal, the portal displays an incorrect number of data disks that can attach to a D series VM. All supported D series VMs can accommodate as many data disks as the Azure configuration.

- <!-- TBD - IS ASDK --> When a VM image fails to be created, a failed item that you cannot delete might be added to the VM images compute blade.

  Como solução, crie uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (novo VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item da falha. Em seguida, 15 minutos depois de criar a imagem fictício, que pode com êxito eliminá-lo.

  Em seguida, pode tentar transfira novamente a imagem VM que tenha falhado anteriormente.

- <!-- TBD - IS ASDK --> If provisioning an extension on a VM deployment takes too long, users should let the provisioning time-out instead of trying to stop the process to deallocate or delete the VM.  

- <!-- 1662991 IS ASDK --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings.  


#### <a name="networking"></a>Redes
- <!-- 1766332 - IS ASDK --> Under **Networking**, if you click **Create VPN Gateway** to set up a VPN connection, **Policy Based** is listed as a VPN type. Do not select this option. Only the **Route Based** option is supported in Azure Stack.

- <!-- 2388980 - IS ASDK --> After a VM is created and associated with a public IP address, you can't disassociate that VM from that IP address. Disassociation appears to work, but the previously assigned public IP address remains associated with the original VM.

  Atualmente, tem de utilizar apenas novos endereços IP públicos para novas VMs que cria.

  Este comportamento ocorre mesmo reatribuir o endereço IP para uma nova VM (normalmente denominado como um *alternância de VIP*). Todas as futuras tenta estabelecer ligação através deste resultado de endereço IP numa ligação para a VM originalmente associada e não para a nova.

- <!-- 2292271 - IS ASDK --> If you raise a Quota limit for a Network resource that is part of an Offer and Plan that is associated with a tenant subscription, the new limit is not applied to that subscription. However, the new limit does apply to new subscriptions that are created after the quota is increased. 

  Para contornar este problema, utilize um plano de suplemento para aumentar uma Quota de rede quando o plano já está associado uma subscrição. Para obter mais informações, consulte como [disponibilizar um plano de suplemento](azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> You cannot delete a subscription that has DNS Zone resources or Route Table resources associated with it. To successfully delete the subscription, you must first delete DNS Zone and Route Table resources from the tenant subscription. 
  

- <!-- 1902460 - IS ASDK --> Azure Stack supports a single *local network gateway* per IP address. This is true across all tenant subscriptions. After the creation of the first local network gateway connection, subsequent attempts to create a local network gateway resource with the same IP address are blocked.

- <!-- 16309153 - IS ASDK --> On a Virtual Network that was created with a DNS Server setting of *Automatic*, changing to a custom DNS Server fails. The updated settings are not pushed to VMs in that Vnet.

- <!-- TBD - IS ASDK --> Azure Stack does not support adding additional network interfaces to a VM instance after the VM is deployed. If the VM requires more than one network interface, they must be defined at deployment time.

- <!-- 2096388 IS --> You cannot use the admin portal to update rules for a network security group. 

    Solução para o App Service: Se precisar de ambiente de trabalho remoto para as instâncias de controlador, modificar as regras de segurança dentro os grupos de segurança de rede com o PowerShell.  Seguem-se exemplos de como *permitir*e, em seguida, restaurar a configuração para *negar*:  
    
    - *Permitir:*
 
      ```powershell    
      Connect-AzureRmAccount -EnvironmentName AzureStackAdmin
      
      $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"
      
      $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"
      
      ##This doesn’t work. Need to set properties again even in case of edit
      
      #Set-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389" -NetworkSecurityGroup $nsg -Access Allow  
      
      Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
        -Name $RuleConfig_Inbound_Rdp_3389.Name `
        -Description "Inbound_Rdp_3389" `
        -Access Allow `
        -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
        -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
        -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
        -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
        -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
        -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
        -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange
      
      # Commit the changes back to NSG
      Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
      ```

    - *Negar:*

        ```powershell
        
        Connect-AzureRmAccount -EnvironmentName AzureStackAdmin
        
        $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"
        
        $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"
        
        ##This doesn’t work. Need to set properties again even in case of edit
    
        #Set-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389" -NetworkSecurityGroup $nsg -Access Allow  
    
        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
          -Name $RuleConfig_Inbound_Rdp_3389.Name `
          -Description "Inbound_Rdp_3389" `
          -Access Deny `
          -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
          -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
          -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
          -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
          -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
          -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
          -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange
          
        # Commit the changes back to NSG
        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg 
        ```

#### <a name="sql-and-mysql"></a>SQL Server e o MySQL

- <!-- TBD - IS --> Only the resource provider is supported to create items on servers that host SQL or MySQL. Items created on a host server that are not created by the resource provider might result in a mismatched state.  

- <!-- IS, ASDK --> Special characters, including spaces and periods, are not supported in the **Family** or **Tier** names when you create a SKU for the SQL and MySQL resource providers.


> [!NOTE]  
> <!-- TBD - IS --> After you update to Azure Stack 1804, you can continue to use the SQL and MySQL resource providers that you previously deployed.  We recommend you update SQL and MySQL when a new release becomes available. Like Azure Stack, apply updates to SQL and MySQL resource providers sequentially.  For example, if you use version 1802, first apply version 1803, and then update to 1804.      
>   
> A instalação da atualização 1804 não afeta a utilização atual do SQL Server ou MySQL fornecedores de recursos pelos seus utilizadores.
> Independentemente da versão dos fornecedores de recursos que utilizar, os dados de utilizadores as respetivas bases de dados não é touched e permanecem acessíveis.    



#### <a name="app-service"></a>Serviço de Aplicações
- <!-- 2352906 - IS ASDK --> Users must register the storage resource provider before they create their first Azure Function in the subscription.

- <!-- TBD - IS ASDK --> In order to scale out infrastructure (workers, management, front-end roles), you must use PowerShell as described in the release notes for Compute.

- <!-- TBD - IS ASDK --> App Service can only be deployed into the **Default Provider Subscription** at this time.  In a future update App Service will deploy into the new Metering Subscription introduced in Azure Stack 1804 and all existing deployments will be migrated to this new subscription also.

#### <a name="usage"></a>Utilização  
- <!-- TBD - IS ASDK --> Usage Public IP address usage meter data shows the same *EventDateTime* value for each record instead of the *TimeDate* stamp that shows when the record was created. Currently, you can’t use this data to perform accurate accounting of public IP address usage.


<!-- #### Identity -->
<!-- #### Marketplace --> 


## <a name="download-the-update"></a>Transferir a atualização
Pode transferir o pacote de atualização de 1804 de pilha do Azure do [aqui](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Consulte também
- Para utilizar o ponto final com privilégios (PEP) para monitorizar e retomar a atualizações, consulte [monitorizar atualizações na pilha do Azure utilizando o ponto final com privilégios](azure-stack-monitor-update.md).
- Para obter uma descrição geral da gestão de atualização na pilha do Azure, consulte [gerir atualizações na descrição geral do Azure pilha](azure-stack-updates.md).
- Para obter mais informações sobre como aplicar atualizações com a pilha do Azure, consulte [aplicar atualizações na pilha de Azure](azure-stack-apply-updates.md).
