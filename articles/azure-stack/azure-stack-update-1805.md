---
title: O Azure Stack 1805 atualização | Documentos da Microsoft
description: Saiba mais sobre o que há de novo na atualização 1805 para os sistemas integrados do Azure Stack, incluindo os problemas conhecidos e onde pode transferir a atualização.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/27/2018
ms.author: sethm
ms.reviewer: justini
ms.openlocfilehash: 60b4658d18d39797d30055e86cb21689cfb661fa
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/15/2018
ms.locfileid: "45631669"
---
# <a name="azure-stack-1805-update"></a>Atualização de 1805 de pilha do Azure

*Aplica-se a: sistemas integrados do Azure Stack*

Este artigo descreve as melhorias e correções no pacote de atualização de 1805, problemas conhecidos para esta versão e onde pode transferir a atualização. Problemas conhecidos são divididos em problemas diretamente relacionados com o processo de atualização e problemas com a compilação (após a instalação).

> [!IMPORTANT]        
> Este pacote de atualização é apenas para sistemas integrados do Azure Stack. Não é aplicável este pacote de atualização para o Azure Stack Development Kit.

## <a name="build-reference"></a>Criar a referência    
É o número de compilação de atualização do Azure Stack 1805 **1.1805.1.47**.  

> [!TIP]  
> Com base nos comentários dos clientes, há uma atualização para o esquema de versão em utilização para o Microsoft Azure Stack.  Começando com esta atualização, 1805, o novo esquema representa melhor a versão atual da cloud.  
> 
> O esquema versão já está *Version.YearYearMonthMonth.MinorVersion.BuildNumber* em que o segundo e terceiro conjuntos de indicam a versão e a versão. Por exemplo, 1805.1 representa a *de versão para produção* versão (RTM) do 1805.  


### <a name="new-features"></a>Novos recursos
Esta atualização inclui os seguintes aprimoramentos para o Azure Stack.
<!-- 2297790 - IS, ASDK --> 
- **O Azure Stack inclui agora uma *Syslog* cliente** como uma *funcionalidade de pré-visualização*. Este cliente permite que o reencaminhamento de registos de auditoria e segurança relacionados com a infraestrutura do Azure Stack para um Syslog servidor ou segurança informações e eventos (SIEM) software de gestão que é externo ao Azure Stack. Atualmente, o cliente de Syslog só suporta ligações não autenticadas de UDP através da porta de predefinição 514. O payload de cada mensagem do Syslog é formatado em comum o formato de evento (CEF). 

  Para configurar o cliente de Syslog, utilize o **Set-SyslogServer** cmdlet exposto o ponto final com privilégios. 

  Com esta pré-visualização, poderá ver os alertas de três seguintes. Quando apresentado ao Azure Stack, estes alertas incluem *descrições* e *remediação* orientações. 
  - Título: Desativar a integridade do código  
  - Título: A integridade do código no modo de auditoria 
  - Título: Conta de utilizador criada

  Enquanto esta funcionalidade está em pré-visualização, ele deve não é confiável em ambientes de produção.   




### <a name="fixed-issues"></a>Problemas corrigidos

<!-- # - applicability -->
- Podemos corrigir o problema que bloqueados [abrir um novo pedido de suporte na lista pendente](azure-stack-manage-portals.md#quick-access-to-help-and-support) no portal de administração. Esta opção agora funciona como esperado. 

- **Várias correções** de desempenho, estabilidade, segurança e o sistema operativo que é utilizado pelo Azure Stack.


<!-- # Additional releases timed with this update    -->



## <a name="before-you-begin"></a>Antes de começar    

### <a name="prerequisites"></a>Pré-requisitos
- Instalar o Azure Stack [1804 atualizar](azure-stack-update-1804.md) antes de aplicar a atualização do Azure Stack 1805.  
- Instalar o mais recente disponível [atualização ou correção para a versão 1804](azure-stack-update-1804.md#post-update-steps).   
- Antes de iniciar a instalação da atualização 1805, execute [AzureStack teste](azure-stack-diagnostic-test.md) para validar o status do seu Azure Stack e resolver quaisquer problemas operacionais encontrados. Também rever alertas ativos e resolver qualquer um que requerem uma ação. 

### <a name="known-issues-with-the-update-process"></a>Problemas conhecidos com o processo de atualização   
- Durante a instalação da atualização 1805, poderá ver alertas com o título *erro – o modelo para typu FaultType UserAccounts.New está em falta.*  Pode ignorar com segurança estes alertas. Estes alertas fechará automaticamente depois de concluída a atualização para 1805.   

- <!-- 2489559 - IS --> Não tente criar máquinas virtuais durante a instalação desta atualização. Para obter mais informações sobre a gestão de atualizações, consulte [gerir atualizações na descrição geral do Azure Stack](azure-stack-updates.md#plan-for-updates).


### <a name="post-update-steps"></a>Passos de pós-atualização
Após a instalação de 1805, instale as correções aplicáveis. Para obter mais informações, consulte os seguintes artigos da base de dados de conhecimento, bem como nossos [política de manutenção](azure-stack-servicing-policy.md).  
 - [KB 4344102 - o Azure Stack correção 1.1805.7.57](https://support.microsoft.com/help/4344102).


## <a name="known-issues-post-installation"></a>Problemas conhecidos (após a instalação)
Seguem-se após a instalação problemas conhecidos para esta versão de compilação.

### <a name="portal"></a>Portal  
- <!-- 2931230 – IS  ASDK --> Não não possível eliminar os planos que são adicionados a uma subscrição de utilizador como um plano de suplemento, mesmo quando remover o plano de subscrição de utilizador. O plano irá permanecer até que as subscrições que referenciam o plano de suplemento também são eliminadas. 

- <!-- TBD - IS ASDK --> Não é possível aplicar atualizações de controladores mediante a utilização de um pacote de extensão de OEM com esta versão do Azure Stack.  Não é uma solução para este problema.

- <!-- 2551834 - IS, ASDK --> Quando seleciona **descrição geral** para uma conta de armazenamento em portais do administrador ou utilizador, as informações a partir do *Essentials* não apresentar o painel.  Painel Essentials apresenta informações sobre a conta, como o respetivo *grupo de recursos*, *localização*, e *ID de subscrição*.  Outras opções de descrição geral são acessíveis, como *serviços* e *monitorização*, bem como opções para *abrir no Explorador de* ou *eliminar conta de armazenamento* . 

  Para ver as informações disponíveis, utilize o [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) cmdlet do PowerShell. 

- <!-- 2551834 - IS, ASDK --> Quando seleciona **etiquetas** para uma conta de armazenamento em portais do administrador ou utilizador, as informações de falha ao carregar e não apresenta.  

  Para ver as informações disponíveis, utilize o [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) cmdlet do PowerShell.


- <!-- 2332636 - IS -->  Quando utilizar o AD FS para o seu sistema de identidade do Azure Stack e a atualização para esta versão do Azure Stack, o proprietário de predefinição da subscrição do fornecedor de padrão é reposto incorporada **CloudAdmin** utilizador.  
  Solução: Para resolver este problema depois de instalar esta atualização, utilize o passo 3 dos [acionar a automatização para configurar afirmações fidedignidade de fornecedor no Azure Stack](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) procedimento para repor o proprietário da subscrição do fornecedor de predefinição.   

- <!-- TBD - IS ASDK --> Alguns tipos de subscrição administrativos não estão disponíveis.  Quando atualizar o Azure Stack para esta versão, os tipos de duas subscrição que eram [introduzido com a versão 1804](azure-stack-update-1804.md#new-features) não estão visíveis na consola do. Isto era esperado. São os tipos de subscrição indisponível *medição subscrição*, e *subscrição de consumo*. Esses tipos de subscrição são visíveis no novos ambientes do Azure Stack a partir da versão 1804, mas ainda não estão prontos a utilizar. Deve continuar a utilizar o *fornecedor predefinido* tipo de subscrição.  

- <!-- 2403291 - IS ASDK --> Poderá não ter o uso da barra de deslocamento horizontal na parte inferior de portais de administrador e utilizador. Se não conseguir aceder a barra de deslocamento horizontal, utilize a estrutura de ligações para navegar até um painel anterior no portal ao selecionar o nome do painel pretende visualizar na lista de trilha encontrado na parte superior esquerda do portal.
  ![Navegação estrutural](media/azure-stack-update-1804/breadcrumb.png)

- <!-- TBD - IS --> Pode não ser possível ver os recursos de computação ou o armazenamento no portal do administrador. A causa deste problema é um erro durante a instalação da atualização que faz com que a atualização para ser comunicadas incorretamente conclusão com êxito. Se este problema ocorrer, contacte o suporte técnico da Microsoft para obter assistência.

- <!-- TBD - IS --> Poderá ver um dashboard em branco no portal. Para recuperar o dashboard, selecione o ícone de engrenagem no canto superior direito do portal e, em seguida, selecione **restaurar predefinições**.

- <!-- TBD - IS ASDK --> A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos inteiro e, em seguida, eliminar subscrições de utilizador.

- <!-- TBD - IS ASDK --> Não é possível ver as permissões à sua subscrição a utilizar os portais do Azure Stack. Como solução, utilize o PowerShell para verificar as permissões.


### <a name="health-and-monitoring"></a>Estado de funcionamento e monitorização
- <!-- 1264761 - IS ASDK -->  Poderá ver alertas para o *controlador de estado de funcionamento* componente que tem os seguintes detalhes:  
- 
   #1 do alerta:
   - NOME: Função de infraestrutura mau estado de funcionamento
   - GRAVIDADE: aviso
   - COMPONENTE: Controlador de estado de funcionamento
   - Descrição: O Scanner de Heartbeat do controlador de estado de funcionamento não está disponível. Isto pode afetar as métricas e relatórios de estado de funcionamento.  

  Alerta #2:
   - NOME: Função de infraestrutura mau estado de funcionamento
   - GRAVIDADE: aviso
   - COMPONENTE: Controlador de estado de funcionamento
   - Descrição: O Scanner de falhas de controlador do Estado de funcionamento não está disponível. Isto pode afetar as métricas e relatórios de estado de funcionamento.

  Os dois alertas #1 e #2 podem ser ignoradas com segurança e vai ser fechado automaticamente ao longo do tempo. 

  Também poderá ver o seguinte alerta para *capacidade*. Para este alerta, pode variar a percentagem de memória disponível identificada na descrição:  

  #3 do alerta:
   - NOME: Capacidade de memória baixa
   - GRAVIDADE: crítico
   - COMPONENTE: capacidade
   - Descrição: A região consumiu 80.00 superior a % de memória disponível. Criação de máquinas virtuais com grandes quantidades de memória pode falhar.  

  Nesta versão do Azure Stack, este alerta pode ser acionados incorretamente. Se as máquinas virtuais inquilinas continuar a implementar com êxito, pode ignorar este alerta. 
  
  Alerta #3 não fecha automaticamente. Se fechar este alerta do Azure Stack irá criar o mesmo alerta em 15 minutos.  

- <!-- 2368581 - IS. ASDK --> Como um operador do Azure Stack, se receber um alerta de pouca memória e máquinas de virtuais de inquilino não for possível implementar com um *erro de criação de recursos de infraestrutura de VM*, é possível que o carimbo de data / Azure Stack está disponível memória esgotada. Utilize o [Planeador de capacidade do Azure Stack](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) para melhor compreender a capacidade disponível para as cargas de trabalho. 


### <a name="compute"></a>Computação
- <!-- TBD - IS, ASDK --> Ao selecionar um tamanho de máquina virtual para uma implementação da máquina virtual, alguns tamanhos de VM da série F não estão visíveis como parte do Seletor de tamanho, ao criar uma VM. Os seguintes tamanhos VM não aparecem no Seletor de: *F8s_v2*, *F16s_v2*, *F32s_v2*, e *F64s_v2*.  
  Como solução, utilize um dos seguintes métodos para implementar uma VM. Cada método, tem de especificar o tamanho da VM que pretende utilizar.

  - **Modelo de Gestor de recursos do Azure:** quando utiliza um modelo, defina o *vmSize* no modelo para igual o tamanho da VM que pretende utilizar. Por exemplo, a entrada seguinte é utilizada para implementar uma VM que utiliza a *F32s_v2* tamanho:  

    ```
        "properties": {
        "hardwareProfile": {
                "vmSize": "Standard_F32s_v2"
        },
    ```  
  - **CLI do Azure:** pode utilizar o [az vm crie](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) comando e especifique o tamanho da VM como um parâmetro, semelhante a `--size "Standard_F32s_v2"`.

  - **PowerShell:** com o PowerShell, pode usar [New-AzureRMVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-6.0.0) com o parâmetro que especifica o tamanho da VM, semelhante às `-VMSize "Standard_F32s_v2"`.


- <!-- TBD - IS ASDK --> Definições de dimensionamento para conjuntos de dimensionamento de máquinas virtuais não estão disponíveis no portal. Como solução, pode usar [do Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro, em vez de `-VMScaleSetName`.

- <!-- TBD - IS --> Quando cria um conjunto de disponibilidade no portal, vai **New** > **computação** > **do conjunto de disponibilidade**, só pode criar um conjunto de disponibilidade com um domínio de falha e o domínio de atualização de 1. Como solução, ao criar uma nova máquina virtual, criar o conjunto com o PowerShell, CLI, ou a partir de disponibilidade do portal.

- <!-- TBD - IS ASDK --> Ao criar máquinas virtuais no portal de utilizador do Azure Stack, o portal apresenta um número incorreto de discos de dados que pode anexar a uma VM da série DS. Séries de DS VMs capaz de acomodar os discos de dados como a configuração do Azure.

- <!-- TBD - IS ASDK --> Quando não for possível criar uma imagem de VM, pode ser adicionado um item com falha que não é possível eliminar para o painel de computação de imagens VM.

  Como solução, criar uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (New-VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item falhado. Em seguida, 15 minutos depois de criar a imagem fictícia, pode com êxito eliminá-la.

  Em seguida, pode tentar transfira a imagem VM que falhou anteriormente.

- <!-- TBD - IS ASDK --> Se uma extensão numa implantação de VM o aprovisionamento demora demasiado tempo, os utilizadores devem permitir que o limite de tempo de aprovisionamento em vez de tentar interromper o processo para desaloque ou elimine a VM.  

- <!-- 1662991 IS ASDK --> Diagnóstico de VM do Linux não é suportado no Azure Stack. Quando implementa uma VM do Linux com o diagnóstico VM ativado, a implementação falhar. A implementação falhar, também, se ativar as métricas básicas de VM do Linux através das definições de diagnóstico.  


### <a name="networking"></a>Redes
- <!-- TBD - IS ASDK --> Não é possível criar rotas definidas pelo utilizador no portal do administrador ou utilizador. Como solução, utilize [do Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell).

- <!-- 1766332 - IS ASDK --> Sob **Networking**, se clicar em **criar Gateway de VPN** para configurar uma ligação de VPN, **baseado em política** está listado como um tipo de VPN. Não selecione esta opção. Apenas os **rota com base** opção é suportada no Azure Stack.

- <!-- 2388980 - IS ASDK --> Depois de uma VM é criada e associada a um endereço IP público, não é possível desassociar VM desse endereço IP. Desassociação parece funcionar, mas o endereço IP público atribuído anteriormente que permanece associado com a VM original.

  Atualmente, tem de utilizar apenas novos endereços IP públicos para novas VMs que criar.

  Este comportamento ocorre mesmo que a reatribuir o endereço IP a uma VM nova (geralmente conhecida como uma *alternância de VIP*). Todos os futuro tenta se conectar por meio deste resultado de endereço IP numa ligação para a VM original e não para a nova.

- <!-- 2292271 - IS ASDK --> Se aumentar um limite de Quota para um recurso de rede que faz parte de uma oferta e o plano que está associado uma subscrição de inquilino, o novo limite não se aplica a essa subscrição. No entanto, o novo limite é aplicável às novas assinaturas que são criadas após a quota for aumentada.

  Para contornar este problema, utilize um plano de suplemento para aumentar uma Quota de rede quando o plano já está associado uma subscrição. Para obter mais informações, consulte como [disponibilizar um plano de suplemento](azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> Não é possível eliminar uma subscrição que tem recursos de zona DNS ou os recursos de tabela de rotas associados ao mesmo. Para eliminar com êxito a subscrição, tem primeiro de eliminar os recursos de zona DNS e a tabela de rotas da subscrição do inquilino.


- <!-- 1902460 - IS ASDK --> O Azure Stack oferece suporte a uma única *gateway de rede local* por endereço IP. Isso é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local ligação de gateway, subsequente tenta criar um recurso de gateway de rede local com o mesmo endereço IP são bloqueados.

- <!-- 16309153 - IS ASDK --> Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a mudança para uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas para as VMs nessa vnet.

- <!-- TBD - IS ASDK --> O Azure Stack não suporta a adição de interfaces de rede adicionais para uma instância VM depois de implementada a VM. Se a VM exigir mais de uma interface de rede, tem de ser definidos no momento da implementação.

- <!-- 2096388 IS --> Não é possível utilizar o portal de administração ao atualizar as regras para um grupo de segurança de rede.

    Solução alternativa para o serviço de aplicações: Se tiver de ambiente de trabalho remoto para as instâncias de controlador, modificar as regras de segurança dentro de grupos de segurança de rede com o PowerShell.  Seguem-se exemplos de como *permitem*e, em seguida, restaurar a configuração para *negar*:  

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


### <a name="sql-and-mysql"></a>SQL e o MySQL

- <!-- TBD - IS --> O fornecedor de recursos é suportado para criar itens nos servidores desse host SQL ou MySQL. Itens criados num servidor de anfitrião que não são criados pelo fornecedor de recursos podem resultar num estado sem correspondência.  

- <!-- IS, ASDK --> Caracteres especiais, incluindo espaços e períodos, não são suportadas no **família** ou **escalão** nomes ao criar um SKU para os fornecedores de recursos do SQL e o MySQL.


> [!NOTE]  
> <!-- TBD - IS --> Depois de atualizar para o Azure Stack 1805, pode continuar a utilizar os fornecedores de recursos do SQL e o MySQL que implementou anteriormente.  Recomendamos que Atualize para o SQL e o MySQL quando uma nova versão estiver disponível. Como o Azure Stack, aplica atualizações de fornecedores de recursos do SQL e o MySQL sequencialmente. Por exemplo, se utilizar a versão 1803, aplicar primeiro a versão 1804 e, em seguida, atualize para 1805.      
>   
> A instalação da atualização 1805 não afeta a utilização atual de fornecedores de recursos SQL ou MySQL pelos seus utilizadores.
> Independentemente da versão dos fornecedores de recursos que utilizar, os dados de usuários em seus bancos de dados não é tocados e permanecem acessíveis.    



### <a name="app-service"></a>Serviço de Aplicações
- <!-- 2352906 - IS ASDK --> Os utilizadores tem de registar o fornecedor de recursos de armazenamento antes de ser criar sua primeira função do Azure na subscrição.

- <!-- 2489178 - IS ASDK --> Para ampliar a infraestrutura (funções de trabalho, gerenciamento, funções de front-end), tem de utilizar o PowerShell, tal como descrito nas notas de versão para computação.

- <!-- TBD - IS ASDK --> Serviço de aplicações só pode ser implementado para o *subscrição do fornecedor predefinido* neste momento. Numa atualização futura, o serviço de aplicações irá implementar para a nova *medição subscrição* que foi introduzida em 1804 de pilha do Azure. Quando a medição é suportada para utilização, todas as implementações existentes serão migradas para este novo tipo de subscrição.


### <a name="usage"></a>Utilização  
- <!-- TBD - IS ASDK --> Dados de medidor de utilização de endereços de IP público de utilização mostram o mesmo *EventDateTime* valor para cada registo, em vez do *TimeDate* carimbo que mostra quando o registo foi criado. Atualmente, não pode utilizar estes dados para efetuar gestão de contas preciso de utilização de endereços IP pública.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>Transferir a atualização
Pode transferir o pacote de atualização do Azure Stack 1805 partir [aqui](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Consulte também
- Para utilizar o ponto final com privilégios (PEP) para monitorizar e retomar as atualizações, consulte [monitorizar atualizações no Azure Stack, utilizando o ponto final com privilégios](azure-stack-monitor-update.md).
- Para uma descrição geral da gestão de atualizações no Azure Stack, veja [gerir atualizações na descrição geral do Azure Stack](azure-stack-updates.md).
- Para obter mais informações sobre como aplicar as atualizações com o Azure Stack, veja [aplicar atualizações no Azure Stack](azure-stack-apply-updates.md).
