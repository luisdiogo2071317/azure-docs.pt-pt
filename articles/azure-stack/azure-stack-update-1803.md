---
title: Atualização de pilha do Azure 1803 | Microsoft Docs
description: Saiba mais sobre as novidades na atualização 1803 para pilha do Azure integrado sistemas, problemas conhecidos e onde pode transferir a atualização.
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
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: 36d4cd910f841a323dfada49d65f7acb4bdf3138
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="azure-stack-1803-update"></a>Atualização de pilha 1803 do Azure

*Aplica-se a: Azure pilha integrado sistemas*

Este artigo descreve os melhoramentos e corrige no pacote de atualização de 1803, problemas conhecidos para esta versão e onde pode transferir a atualização. Problemas conhecidos são divididos em problemas diretamente relacionada com o processo de atualização e problemas relacionados com a compilação (pós-instalação).

> [!IMPORTANT]        
> Este pacote de atualização é apenas para sistemas de pilha do Azure integrado. Não aplicável este pacote de atualização para o Kit de desenvolvimento de pilha do Azure.

## <a name="build-reference"></a>Referência de compilação    
É o número de compilação de atualização do Azure pilha 1803 **20180329.1**.


## <a name="before-you-begin"></a>Antes de começar    
> [!IMPORTANT]    
> Não tente criar máquinas virtuais durante a instalação desta atualização. Para obter mais informações sobre a gestão de atualizações, consulte [gerir atualizações na descrição geral do Azure pilha](azure-stack-updates.md#plan-for-updates).


### <a name="prerequisites"></a>Pré-requisitos
- Instalar a pilha de Azure [1802 atualizar](azure-stack-update-1802.md) antes de aplicar a atualização 1803 de pilha do Azure.   

- Instalar **AzS correção – 1.0.180312.1-criar 20180222.2** antes de aplicar a atualização 1803 de pilha do Azure. Esta correção atualiza o Windows Defender e está disponível quando transferir atualizações para a pilha do Azure.

  Para instalar a correção, siga os procedimentos normais para [instalar atualizações para o Azure pilha](azure-stack-apply-updates.md). O nome da atualização é apresentado como **AzS correção – 1.0.180312.1**e inclui os seguintes ficheiros: 
    - PUPackageHotFix_20180222.2-1.exe
    - PUPackageHotFix_20180222.2-1.bin
    - Metadata.xml

  Depois de carregar estes ficheiros para uma conta de armazenamento e um contentor, execute a instalação a partir do mosaico de atualização no portal de administração. 
  
  Ao contrário das atualizações à pilha do Azure, instalar esta atualização não se altera a versão da pilha do Azure. Para confirmar esta atualização é instalada, ver a lista de **atualizações instaladas**.

### <a name="post-update-steps"></a>Passos pós-atualização
- Após a instalação de 1803, instale as correções aplicáveis. Para mais informações, veja os seguintes artigos da base de dados de conhecimento, bem como a nossa [manutenção política](azure-stack-servicing-policy.md).

  - [KB 4103348 - falhas de serviço de API do controlador de rede ao tentar instalar uma atualização de pilha do Azure](https://support.microsoft.com/en-us/help/4103348)

- Depois de instalar esta atualização, reveja a configuração de firewall para se certificar [as portas necessárias](azure-stack-integrate-endpoints.md) estão abertas. Por exemplo, esta atualização apresenta Monitor do Azure, que inclui uma alteração de registos de auditoria nos registos de atividade. Com esta alteração, porta 13012 agora é utilizada e também tem de estar aberta.  

### <a name="new-features"></a>Novas funcionalidades 
Esta atualização inclui as seguintes melhorias e correções para pilha do Azure.

- **Atualizar segredos de pilha do Azure** - (contas e certificados). Para obter mais informações sobre a gestão de segredos, consulte [rodar segredos no Azure pilha](azure-stack-rotate-secrets.md). 

- <!-- 1914853 --> **Automatic redirect to HTTPS** when you use HTTP to access the administrator and user portals. This improvement was made based on [UserVoice](https://feedback.azure.com/forums/344565-azure-stack/suggestions/32205385-it-would-be-great-if-there-was-a-automatic-redirec) feedback for Azure Stack. 

- <!-- 2202621  --> **Access the Marketplace** – You can now open the Azure Stack Marketplace by using the [+New](https://ms.portal.azure.com/#create/hub) option from within the admin and user portals the same way you do in the Azure portals.
 
- <!-- 2202621 --> **Azure Monitor** - Azure Stack adds [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) to the admin and user portals. This includes new explorers for metrics and activity logs. To access this Azure Monitor from external networks, port **13012** must be open in firewall configurations. For more information about ports required by Azure Stack, see [Azure Stack datacenter integration - Publish endpoints](azure-stack-integrate-endpoints.md).

   Além disso, como parte deste processo alterar em **mais serviços**, *registos de auditoria* aparece como *registos de atividade*. A funcionalidade agora é consistente com o portal do Azure. 

- <!-- 1664791 --> **Sparse files** -  When you add a New image to Azure Stack, or add an image through marketplace syndication, the image is converted to a sparse file. Images that were added prior to using Azure Stack version 1803 cannot be converted. Instead, you must use marketplace syndication to resubmit those images to take advantage of this feature. 
 
   Ficheiros dispersos são um formato de ficheiro eficiente utilizado para reduzir a utilização do espaço de armazenamento e melhorar a e/s.  Para obter mais informações, consulte [Fsutil dispersa](https://docs.microsoft.com/windows-server/administration/windows-commands/fsutil-sparse) para o Windows Server. 

### <a name="fixed-issues"></a>Problemas fixos

- <!-- 1739988 --> Internal Load Balancing (ILB) now properly handles MAC addresses for back-end VMs, which causes ILB to drop packets to the back-end network when using Linux instances on the back-end network. ILB works fine with Windows instances on the back-end network. 

- <!-- 1805496 --> An issue where VPN Connections between Azure Stack would become disconnected due to Azure Stack using different settings for the IKE policy than Azure.  The values now match the values in Azure. 

- <!-- 2209262 --> The IP issue where VPN Connections was previously visible in the portal; however enabling or toggling IP Forwarding has no effect. The feature is turned on by default and the ability to change this not yet supported.  The control has been removed from the portal. 

- <!-- 1766332 --> Azure Stack does not support Policy Based VPN Gateways, even though the option appears in the Portal.  The option has been removed from the Portal. 

- <!-- 1868283 --> Azure Stack now prevents resizing of a virtual machine that is created with dynamic disks. 

- <!-- 1756324 --> Usage data for virtual machines is now separated at hourly intervals. This is consistent with Azure. 

- <!--  2253274 --> The issue where in the admin and user portals, the Settings blade for vNet Subnets fails to load. As a workaround, use PowerShell and the [Get-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig?view=azurermps-5.5.0) cmdlet to view and manage this information.

- Quando cria uma máquina virtual, a mensagem *não é possível apresentar os preços* deixa de aparecer quando escolher um tamanho para o tamanho da VM.

- Várias correções para o desempenho, estabilidade, segurança e o sistema operativo que é utilizado pela pilha de Azure.


### <a name="changes"></a>Alterações
- A forma de alteração do Estado de uma oferta recentemente criado a partir da *privada* para *pública* ou *desativado* foi alterada. Para obter mais informações, consulte [criar uma oferta](azure-stack-create-offer.md).


### <a name="known-issues-with-the-update-process"></a>Problemas conhecidos relacionados com o processo de atualização    
<!-- 2328416 --> During installation of the 1803 update, there can be downtime of the blob service and internal services that use blob service. This includes some virtual machine operations. This down time can cause failures of tenant operations or alerts from services that can’t access data. This issue resolves itself when the update completes installation. 


### <a name="known-issues-post-installation"></a>Problemas conhecidos (pós-instalação)
As seguintes são problemas conhecidos de pós-instalação para a compilação **20180323.2**.

#### <a name="portal"></a>Portal
- A capacidade [para abrir um novo pedido de suporte na lista pendente](azure-stack-manage-portals.md#quick-access-to-help-and-support) de dentro do administrador do portal não está disponível. Em vez disso, utilize a seguinte hiperligação:     
    - Para a pilha do Azure integrados sistemas, utilize https://aka.ms/newsupportrequest.

- <!-- 2050709 --> In the admin portal, it is not possible to edit storage metrics for Blob service, Table service, or Queue service. When you go to Storage, and then select the blob, table, or queue service tile, a new blade opens that displays a metrics chart for that service. If you then select Edit from the top of the metrics chart tile, the Edit Chart blade opens but does not display options to edit metrics.

- Poderá não ser possível ver os recursos de computação ou o armazenamento no portal do administrador. A causa deste problema é um erro durante a instalação da atualização que faz com que a atualização incorretamente sejam comunicados como concluída com êxito. Se este problema ocorrer, contacte o suporte técnico da Microsoft para obter assistência.

- Poderá ver um dashboard em branco no portal. Para recuperar o dashboard, selecione o ícone de equipamento no canto superior direito do portal e, em seguida, selecione **restaurar predefinições**.

- Ao visualizar as propriedades de um recurso ou grupo de recursos, o **mover** botão está desativado. Este comportamento é esperado. Mover recursos ou grupos de recursos entre grupos de recursos ou subscrições não é atualmente suportado.

- A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos completo e, em seguida, eliminar subscrições de utilizador.

- Não é possível ver as permissões para a sua subscrição utilizando os portais de pilha do Azure. Como solução, utilize o PowerShell para verificar permissões.

- No dashboard do portal de administração, o mosaico de atualização não consegue apresentar informações sobre atualizações. Para resolver este problema, clique no mosaico atualizá-lo.

- No portal de administração, poderá ver um alerta crítico para o *Microsoft.Update.Admin* componente. O nome do alerta, a descrição e remediação todos são apresentadas como:  
    - *ERRO - o modelo para o FaultType ResourceProviderTimeout está em falta.*

  Este alerta pode ser ignorado com segurança. 


<!-- #### Health and monitoring --> 

#### <a name="marketplace"></a>Marketplace
- Os utilizadores podem procurar o mercado completo sem uma subscrição e podem ver itens administrativos como planos e ofertas. Estes itens estão não funcional para os utilizadores.



#### <a name="compute"></a>Computação
- Definições de dimensionamento para conjuntos de dimensionamento de máquina virtual não estão disponíveis no portal. Como solução, pode utilizar [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro em vez de `-VMScaleSetName`.

- Quando cria um conjunto de disponibilidade no portal, acedendo a **novo** > **computação** > **do conjunto de disponibilidade**, só pode criar um conjunto de disponibilidade com um domínio de falhas e o domínio de atualização de 1. Como solução, ao criar uma nova máquina virtual, crie o conjunto com o PowerShell, CLI, ou a partir da disponibilidade do portal.

- Ao criar máquinas virtuais no portal de utilizador de pilha do Azure, o portal apresenta um número incorreto de discos de dados que pode anexar a uma série DS VM. Série DS VMs pode acomodar tantos discos de dados como a configuração do Azure.

- Quando não for possível criar uma imagem de VM, um item de falha que não é possível eliminar pode ser adicionado ao painel de processamento de imagens de VM.

  Como solução, crie uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (novo VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item da falha. Em seguida, 15 minutos depois de criar a imagem fictício, que pode com êxito eliminá-lo.

  Em seguida, pode tentar transfira novamente a imagem VM que tenha falhado anteriormente.

-  Se uma extensão numa implementação de VM de aprovisionamento demora demasiado tempo, os utilizadores devem permitir o aprovisionamento do limite de tempo em vez de tentar interromper o processo de Desalocação ou eliminar a VM.  

- <!-- 1662991 --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings.  


#### <a name="networking"></a>Redes
- Depois de uma VM é criada e associada a um endereço IP público, não é possível desassociar essa VM a partir desse endereço IP. Desassociação aparece funcionar, mas o endereço IP público anteriormente atribuído permanece associado a VM original.

  Atualmente, tem de utilizar apenas novos endereços IP públicos para novas VMs que cria.

  Este comportamento ocorre mesmo reatribuir o endereço IP para uma nova VM (normalmente denominado como um *alternância de VIP*). Todas as futuras tenta estabelecer ligação através deste resultado de endereço IP numa ligação para a VM originalmente associada e não para a nova.



- Pilha do Azure suporta um único *gateway de rede local* por endereço IP. Isto é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local gateway ligação, posterior tentativas criar um recurso de gateway de rede local com o mesmo endereço IP estão bloqueadas.

- Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a alteração a uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas por push para VMs nessa Vnet.

- Pilha do Azure não suporta a adição de interfaces de rede adicionais para uma instância VM após a implementação da VM. Se a VM precisar de mais de uma interface de rede, tem de ser definidos no momento da implementação.

- <!-- 2096388 --> You cannot use the admin portal to update rules for a network security group. 

    Solução para o App Service: Se precisar de ambiente de trabalho remoto para as instâncias de controlador, modificar as regras de segurança dentro os grupos de segurança de rede com o PowerShell.  Seguem-se exemplos de como *permitir*e, em seguida, restaurar a configuração para *negar*:  
    
    - *Permitir:*
 
      ```powershell    
      Add-AzureRmAccount -EnvironmentName AzureStackAdmin
      
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
        
        Add-AzureRmAccount -EnvironmentName AzureStackAdmin
        
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
- Antes de continuar, consulte a nota importante no [antes de começar](#before-you-begin) perto o início destas notas de versão.

- Pode demorar até uma hora antes dos utilizadores podem criar bases de dados numa nova implementação SQL ou MySQL.

- O fornecedor de recursos é suportado para criar itens nos servidores desse anfitrião SQL ou MySQL. Itens criados num servidor anfitrião que não são criados pelo fornecedor de recursos poderão resultar num Estado não correspondentes.  


> [!NOTE]  
> Depois de atualizar para 1803 de pilha do Azure, pode continuar a utilizar os fornecedores de recursos do SQL Server e o MySQL que implementou anteriormente.  Recomendamos que Atualize para o SQL Server e o MySQL quando uma nova versão fica disponível. Como a pilha do Azure, aplica atualizações de fornecedores de recursos do SQL Server e o MySQL sequencialmente.  Por exemplo, se utilizar a versão 1711, aplicar primeiro a versão 1712, em seguida, 1802 e, em seguida, atualize para 1803.      
>   
> A instalação da atualização 1803 não afeta a utilização atual do SQL Server ou MySQL fornecedores de recursos pelos seus utilizadores.
> Independentemente da versão dos fornecedores de recursos que utilizar, os dados de utilizadores as respetivas bases de dados não é touched e permanecem acessíveis.    



#### <a name="app-service"></a>Serviço de Aplicações
- Os utilizadores tem de registar o fornecedor de recursos de armazenamento antes de poderem criarem a sua primeira função do Azure na subscrição.

- Para ampliar a infraestrutura (workers, gestão, funções de front-end), tem de utilizar o PowerShell conforme descrito nas notas de versão da computação.


#### <a name="usage"></a>Utilização  
- Dados de medição de utilização de endereços de IP público de utilização mostram o mesmo *EventDateTime* valor para cada registo em vez do *TimeDate* carimbo que mostra que o registo foi criado. Atualmente, não é possível utilizar estes dados para efetuar exata contabilização da utilização de endereços IP pública.

<!--
#### Identity
-->

#### <a name="downloading-azure-stack-tools-from-github"></a>Transferir as ferramentas de pilha do Azure a partir do GitHub
- Ao utilizar o *webrequest invocar* das ferramentas de cmdlet do PowerShell para transferir a pilha do Azure a partir do Github, receberá um erro:     
    -  *webrequest invocar: O pedido foi abortado: não foi possível criar o canal seguro SSL/TLS.*     

  Este erro ocorre devido a uma descontinuação de suporte do GitHub recente das Tlsv1 Tlsv1.1 criptográficas normas e (a predefinição para o PowerShell). Para obter mais informações, consulte [avisos de remoção de padrões de criptografia fraca](https://githubengineering.com/crypto-removal-notice/).

  Para resolver este problema, adicione `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` na parte superior do script para forçar a consola do PowerShell para utilizar TLSv1.2 quando transferir a partir de repositórios do GitHub.


## <a name="download-the-update"></a>Transferir a atualização
Pode transferir o pacote de atualização de 1803 de pilha do Azure do [aqui](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Consulte também
- Para utilizar o ponto final com privilégios (PEP) para monitorizar e retomar a atualizações, consulte [monitorizar atualizações na pilha do Azure utilizando o ponto final com privilégios](azure-stack-monitor-update.md).
- Para obter uma descrição geral da gestão de atualização na pilha do Azure, consulte [gerir atualizações na descrição geral do Azure pilha](azure-stack-updates.md).
- Para obter mais informações sobre como aplicar atualizações com a pilha do Azure, consulte [aplicar atualizações na pilha de Azure](azure-stack-apply-updates.md).
