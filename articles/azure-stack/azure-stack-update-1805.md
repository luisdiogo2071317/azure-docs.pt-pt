---
title: Atualização de pilha do Azure 1805 | Microsoft Docs
description: Saiba mais sobre as novidades na atualização 1805 para sistemas de pilha do Azure integrado, incluindo problemas conhecidos e onde pode transferir a atualização.
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
ms.date: 06/20/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: 80ed0d2353fc6ea3a515c0d05475c713920abe46
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295748"
---
# <a name="azure-stack-1805-update"></a>Atualização de pilha 1805 do Azure

*Aplica-se a: Azure pilha integrado sistemas*

Este artigo descreve os melhoramentos e corrige no pacote de atualização de 1805, problemas conhecidos para esta versão e onde pode transferir a atualização. Problemas conhecidos são divididos em problemas diretamente relacionada com o processo de atualização e problemas relacionados com a compilação (pós-instalação).

> [!IMPORTANT]        
> Este pacote de atualização é apenas para sistemas de pilha do Azure integrado. Não aplicável este pacote de atualização para o Kit de desenvolvimento de pilha do Azure.

## <a name="build-reference"></a>Referência de compilação    
É o número de compilação de atualização do Azure pilha 1805 **1.1805.1.47**.  

> [!TIP]  
> Com base nos comentários de clientes, não há uma atualização para o esquema versão utilizada para a pilha do Microsoft Azure.  Começando com esta atualização, 1805, o novo esquema representa melhor a versão atual da nuvem.  
> 
> O esquema de versão é agora *Version.YearYearMonthMonth.MinorVersion.BuildNumber* onde os conjuntos de segundo e terceiro indicam a versão e a versão. Por exemplo, 1805.1 representa o *lançamento para fabrico* versão (RTM) do 1805.  


### <a name="new-features"></a>Novas funcionalidades
Esta atualização inclui as seguintes melhorias de pilha do Azure.
<!-- 2297790 - IS, ASDK --> 
- **Pilha do Azure inclui agora um *Syslog* cliente** como um *funcionalidade de pré-visualização*. Este tipo de cliente permite que o reencaminhamento de registos de auditoria e segurança relacionados com a infraestrutura de pilha do Azure para um Syslog segurança ou de servidor de informações e eventos management (SIEM) software que é externo à pilha do Azure. Atualmente, o cliente de Syslog só suporta ligações não autenticadas de UDP através da porta predefinida 514. O payload de cada mensagem do Syslog é formatado em comum evento formato (CEF). 

  Para configurar o cliente de Syslog, utilize o **conjunto SyslogServer** cmdlet exposto no ponto final com privilégios. 

  Com esta pré-visualização, poderá ver os seguintes três alertas. Quando apresentado pela pilha do Azure, estes alertas incluem *descrições* e *remediação* orientações. 
  - Título: Integridade do código desativado  
  - Título: Integridade do código no modo de auditoria 
  - Título: Conta de utilizador criada

  Enquanto esta funcionalidade está em pré-visualização, este não deve ser confiada em ambientes de produção.   




### <a name="fixed-issues"></a>Problemas fixos

<!-- # - applicability -->
- Iremos corrigir o problema que bloqueados [abrir um novo pedido de suporte na lista pendente](azure-stack-manage-portals.md#quick-access-to-help-and-support) de dentro do portal de administração do. Esta opção agora funciona conforme pretendido. 

- **Várias correções** para desempenho, estabilidade, segurança e o sistema operativo que é utilizado pela pilha de Azure.


<!-- # Additional releases timed with this update    -->



## <a name="before-you-begin"></a>Antes de começar    

### <a name="prerequisites"></a>Pré-requisitos
- Instalar a pilha de Azure [1804 atualizar](azure-stack-update-1804.md) antes de aplicar a atualização 1805 de pilha do Azure.    
- Antes de começar a instalação da atualização 1805, execute [teste AzureStack](azure-stack-diagnostic-test.md) para validar o estado da pilha do Azure e resolva quaisquer problemas operacionais encontrados. Também rever os alertas ativos e resolver os que requerem uma ação. 

### <a name="known-issues-with-the-update-process"></a>Problemas conhecidos relacionados com o processo de atualização   
- Durante a instalação da atualização 1805, poderá ver alertas com o título *erro – o modelo para o FaultType UserAccounts.New está em falta.*  Pode ignorar com segurança estes alertas. Estes alertas irão fechar automaticamente depois de concluída a atualização a 1805.   

- <!-- 2489559 - IS --> Não tente criar máquinas virtuais durante a instalação desta atualização. Para obter mais informações sobre a gestão de atualizações, seSe [gerir atualizações na descrição geral do Azure pilha](azure-stack-updates.md#plan-for-updates).


### <a name="post-update-steps"></a>Passos pós-atualização
Após a instalação de 1805, instale as correções aplicáveis. Para mais informações, veja os seguintes artigos da base de dados de conhecimento, bem como a nossa [manutenção política](azure-stack-servicing-policy.md).  
 - [KB 4340474 - correção de pilha do Azure 1.1805.4.53](https://support.microsoft.com/en-us/help/4340474).


## <a name="known-issues-post-installation"></a>Problemas conhecidos (pós-instalação)
As seguintes são problemas conhecidos de pós-instalação para esta versão de compilação.

### <a name="portal"></a>Portal  
- <!-- 2551834 - IS, ASDK --> Quando seleciona **descrição geral** para uma conta de armazenamento em portais de administrador ou utilizador, as informações do *Essentials* não apresentar o painel.  O painel de Essentials apresenta informações sobre a conta como o respetivo *grupo de recursos*, *localização*, e *ID de subscrição*.  Outras opções para descrição geral estão acessíveis, como *serviços* e *monitorização*, bem como opções para *aberta no Explorador de* ou *eliminar conta do storage* . 

  Para ver as informações disponíveis, utilize o [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) cmdlet do PowerShell. 

- <!-- 2551834 - IS, ASDK --> Quando seleciona **etiquetas** para uma conta de armazenamento em portais de administrador ou utilizador, as informações não conseguir carregar e não apresenta.  

  Para ver as informações disponíveis, utilize o [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) cmdlet do PowerShell.


- <!-- 2332636 - IS -->  Quando utilizar o AD FS para o seu sistema de identidade de pilha do Azure e a atualização para esta versão da pilha do Azure, o proprietário de predefinida da subscrição de fornecedor predefinido é reposto para o incorporado **CloudAdmin** utilizador.  
  Solução: Para resolver este problema, depois de instalar esta atualização, utilize o passo 3 do [automatização de Acionador para configurar afirmações de confiança do fornecedor na pilha de Azure](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) procedimento para repor o proprietário da subscrição de fornecedor predefinido.   

- <!-- TBD - IS ASDK --> Alguns tipos de subscrição administrativos não estão disponíveis.  Quando atualizar pilha do Azure para esta versão, os tipos de dois subscrição que foram [introduzido com a versão 1804](azure-stack-update-1804.md#new-features) não estão visíveis na consola do. Isto era esperado. Os tipos de subscrição indisponível são *medição subscrição*, e *subscrição consumo*. Estes tipos de subscrição são visíveis no novos ambientes de pilha do Azure a partir da versão 1804 mas ainda não estão prontos para utilização. Deve continuar a utilizar o *fornecedor predefinido* tipo de subscrição.  

- <!-- 2403291 - IS ASDK --> Poderá não ter a utilização da barra de deslocamento horizontal ao longo da parte inferior dos portais de administrador e utilizador. Se não conseguir aceder a barra de deslocamento horizontal, utilize a navegação estrutural para navegar para um painel anterior no portal, selecionando o nome do painel pretende ver a lista de trilho encontrado na parte superior esquerda do portal.
  ![Trilho](media/azure-stack-update-1804/breadcrumb.png)

- <!-- TBD - IS --> Poderá não ser possível ver os recursos de computação ou o armazenamento no portal do administrador. A causa deste problema é um erro durante a instalação da atualização que faz com que a atualização incorretamente sejam comunicados como concluída com êxito. Se este problema ocorrer, contacte o suporte técnico da Microsoft para obter assistência.

- <!-- TBD - IS --> Poderá ver um dashboard em branco no portal. Para recuperar o dashboard, selecione o ícone de equipamento no canto superior direito do portal e, em seguida, selecione **restaurar predefinições**.

- <!-- TBD - IS ASDK --> A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos completo e, em seguida, eliminar subscrições de utilizador.

- <!-- TBD - IS ASDK --> Não é possível ver as permissões para a sua subscrição utilizando os portais de pilha do Azure. Como solução, utilize o PowerShell para verificar permissões.


### <a name="health-and-monitoring"></a>Estado de funcionamento e a monitorização
- <!-- 1264761 - IS ASDK -->  Poderá ver alertas para o *controlador do Estado de funcionamento* componente que tem os seguintes detalhes:  

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

  Ambos os alertas podem ser ignoradas e irá fechar automaticamente ao longo do tempo.  

- <!-- 2368581 - IS. ASDK --> Um operador de pilha do Azure, se receber um alerta de pouca memória e máquinas virtuais inquilinas não conseguir implementar com um *erro de criação de VM de recursos de infraestrutura*, é possível que o carimbo de pilha do Azure está disponível memória esgotada. Utilize o [Planeador de capacidade de pilha do Azure](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) para melhor compreender a capacidade disponível para as cargas de trabalho. 


### <a name="compute"></a>Computação
- <!-- TBD - IS, ASDK --> Quando selecionar um tamanho de máquina virtual para uma implementação da máquina virtual, alguns dos tamanhos da VM de série F não estão visíveis como parte do Seletor de tamanho quando criar uma VM. Os tamanhos VM seguintes não são apresentados no Seletor de: *F8s_v2*, *F16s_v2*, *F32s_v2*, e *F64s_v2*.  
  Como solução, utilize um dos seguintes métodos para implementar uma VM. Cada método, tem de especificar o tamanho da VM que pretende utilizar.

  - **Modelo Azure Resource Manager:** quando utilizar um modelo, defina o *vmSize* no modelo para igualar o tamanho da VM que pretende utilizar. Por exemplo, a entrada seguinte é utilizada para implementar uma VM que utiliza o *F32s_v2* tamanho:  

    ```
        "properties": {
        "hardwareProfile": {
                "vmSize": "Standard_F32s_v2"
        },
    ```  
  - **CLI do Azure:** pode utilizar o [az vm criar](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) de comandos e especificar o tamanho da VM como um parâmetro semelhante `--size "Standard_F32s_v2"`.

  - **PowerShell:** com o PowerShell, pode utilizar [New-AzureRMVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-6.0.0) com o parâmetro que especifica o tamanho da VM, semelhante a `-VMSize "Standard_F32s_v2"`.


- <!-- TBD - IS ASDK --> Definições de dimensionamento para conjuntos de dimensionamento de máquina virtual não estão disponíveis no portal. Como solução, pode utilizar [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro em vez de `-VMScaleSetName`.

- <!-- TBD - IS --> Quando cria um conjunto de disponibilidade no portal, acedendo a **novo** > **computação** > **do conjunto de disponibilidade**, só pode criar um conjunto de disponibilidade com um domínio de falhas e o domínio de atualização de 1. Como solução, ao criar uma nova máquina virtual, crie o conjunto com o PowerShell, CLI, ou a partir da disponibilidade do portal.

- <!-- TBD - IS ASDK --> Ao criar máquinas virtuais no portal de utilizador de pilha do Azure, o portal apresenta um número incorreto de discos de dados que pode anexar a uma série DS VM. Série DS VMs pode acomodar tantos discos de dados como a configuração do Azure.

- <!-- TBD - IS ASDK --> Quando não for possível criar uma imagem de VM, um item de falha que não é possível eliminar pode ser adicionado ao painel de processamento de imagens de VM.

  Como solução, crie uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (novo VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item da falha. Em seguida, 15 minutos depois de criar a imagem fictício, que pode com êxito eliminá-lo.

  Em seguida, pode tentar transfira novamente a imagem VM que tenha falhado anteriormente.

- <!-- TBD - IS ASDK --> Se uma extensão numa implementação de VM de aprovisionamento demora demasiado tempo, os utilizadores devem permitir o aprovisionamento do limite de tempo em vez de tentar interromper o processo de Desalocação ou eliminar a VM.  

- <!-- 1662991 IS ASDK --> Diagnóstico de VM com Linux não é suportado na pilha do Azure. Quando implementa uma VM com Linux com diagnósticos da VM ativados, a implementação falhará. A implementação falha também se ativar as métricas de básicas da VM com Linux através das definições de diagnóstico.  


### <a name="networking"></a>Redes
- <!-- TBD - IS ASDK --> Não é possível criar rotas definidas pelo utilizador no portal do administrador ou utilizador. Como solução, utilize [Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell).

- <!-- 1766332 - IS ASDK --> Em **redes**, se clicar em **criar Gateway de VPN** para configurar uma ligação VPN, **baseado em política** está listado como um tipo de VPN. Não selecione esta opção. Apenas o **baseada na rota** opção é suportada na pilha do Azure.

- <!-- 2388980 - IS ASDK --> Depois de uma VM é criada e associada a um endereço IP público, não é possível desassociar essa VM a partir desse endereço IP. Desassociação aparece funcionar, mas o endereço IP público anteriormente atribuído permanece associado a VM original.

  Atualmente, tem de utilizar apenas novos endereços IP públicos para novas VMs que cria.

  Este comportamento ocorre mesmo reatribuir o endereço IP para uma nova VM (normalmente denominado como um *alternância de VIP*). Todas as futuras tenta estabelecer ligação através deste resultado de endereço IP numa ligação para a VM original e não para a nova.

- <!-- 2292271 - IS ASDK --> Se aumentar o limite de Quota para um recurso de rede que faça parte de uma oferta e o plano que está associado uma subscrição de inquilino, o novo limite não se aplica a essa subscrição. No entanto, o novo limite é aplicável a novas subscrições que são criadas após a quota é aumentada.

  Para contornar este problema, utilize um plano de suplemento para aumentar uma Quota de rede quando o plano já está associado uma subscrição. Para obter mais informações, consulte como [disponibilizar um plano de suplemento](azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> Não é possível eliminar uma subscrição com recursos de zona DNS ou recursos de tabela de rotas associados. Para eliminar com êxito a subscrição, tem de eliminar os recursos de zona DNS e a tabela de rota da subscrição de inquilino.


- <!-- 1902460 - IS ASDK --> Pilha do Azure suporta um único *gateway de rede local* por endereço IP. Isto é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local gateway ligação, posterior tentativas criar um recurso de gateway de rede local com o mesmo endereço IP estão bloqueadas.

- <!-- 16309153 - IS ASDK --> Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a alteração a uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas por push para VMs nessa Vnet.

- <!-- TBD - IS ASDK --> Pilha do Azure não suporta a adição de interfaces de rede adicionais para uma instância VM após a implementação da VM. Se a VM precisar de mais de uma interface de rede, tem de ser definidos no momento da implementação.

- <!-- 2096388 IS --> Não é possível utilizar o portal de administração para atualizar as regras para um grupo de segurança de rede.

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


### <a name="sql-and-mysql"></a>SQL Server e o MySQL

- <!-- TBD - IS --> O fornecedor de recursos é suportado para criar itens nos servidores desse anfitrião SQL ou MySQL. Itens criados num servidor anfitrião que não são criados pelo fornecedor de recursos poderão resultar num Estado não correspondentes.  

- <!-- IS, ASDK --> Os carateres especiais, incluindo espaços e períodos, não são suportados no **família** ou **camada** nomes quando cria um SKU para os fornecedores de recursos do SQL Server e o MySQL.


> [!NOTE]  
> <!-- TBD - IS --> Depois de atualizar para 1805 de pilha do Azure, pode continuar a utilizar os fornecedores de recursos do SQL Server e o MySQL que implementou anteriormente.  Recomendamos que Atualize para o SQL Server e o MySQL quando uma nova versão fica disponível. Como a pilha do Azure, aplica atualizações de fornecedores de recursos do SQL Server e o MySQL sequencialmente. Por exemplo, se utilizar a versão 1803, aplicar primeiro a versão 1804 e, em seguida, atualize para 1805.      
>   
> A instalação da atualização 1805 não afeta a utilização atual do SQL Server ou MySQL fornecedores de recursos pelos seus utilizadores.
> Independentemente da versão dos fornecedores de recursos que utilizar, os dados de utilizadores as respetivas bases de dados não é touched e permanecem acessíveis.    



### <a name="app-service"></a>Serviço de Aplicações
- <!-- 2352906 - IS ASDK --> Os utilizadores tem de registar o fornecedor de recursos de armazenamento antes de poderem criarem a sua primeira função do Azure na subscrição.

- <!-- 2489178 - IS ASDK --> Para ampliar a infraestrutura (workers, gestão, funções de front-end), tem de utilizar o PowerShell conforme descrito nas notas de versão da computação.

- <!-- TBD - IS ASDK --> Serviço de aplicações só pode ser implementado para o *subscrição do fornecedor predefinido* neste momento. Numa atualização futura, o serviço de aplicações irá implementar para a nova *medição subscrição* que foi introduzida no 1804 de pilha do Azure. Quando medição é suportada para utilização, todas as implementações existentes serão migradas para este novo tipo de subscrição.


### <a name="usage"></a>Utilização  
- <!-- TBD - IS ASDK --> Dados de medição de utilização de endereços de IP público de utilização mostram o mesmo *EventDateTime* valor para cada registo em vez do *TimeDate* carimbo que mostra que o registo foi criado. Atualmente, não é possível utilizar estes dados para efetuar exata contabilização da utilização de endereços IP pública.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>Transferir a atualização
Pode transferir o pacote de atualização de 1805 de pilha do Azure do [aqui](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Consulte também
- Para utilizar o ponto final com privilégios (PEP) para monitorizar e retomar a atualizações, consulte [monitorizar atualizações na pilha do Azure utilizando o ponto final com privilégios](azure-stack-monitor-update.md).
- Para obter uma descrição geral da gestão de atualização na pilha do Azure, consulte [gerir atualizações na descrição geral do Azure pilha](azure-stack-updates.md).
- Para obter mais informações sobre como aplicar atualizações com a pilha do Azure, consulte [aplicar atualizações na pilha de Azure](azure-stack-apply-updates.md).
