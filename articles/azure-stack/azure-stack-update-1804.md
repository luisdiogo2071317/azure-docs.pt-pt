---
title: O Azure Stack 1804 atualização | Documentos da Microsoft
description: Saiba mais sobre as novidades na atualização para o Azure Stack 1804 integrada sistemas, os problemas conhecidos e onde pode transferir a atualização.
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
ms.date: 09/26/2018
ms.author: sethm
ms.reviewer: justini
ms.openlocfilehash: 16f12d8119a14e668a7502d99fa2d9c976d23833
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47393524"
---
# <a name="azure-stack-1804-update"></a>Atualização de 1804 de pilha do Azure

*Aplica-se a: sistemas integrados do Azure Stack*

Este artigo descreve as melhorias e correções no pacote de atualização de 1804, problemas conhecidos para esta versão e onde pode transferir a atualização. Problemas conhecidos são divididos em problemas diretamente relacionados com o processo de atualização e problemas com a compilação (após a instalação).

> [!IMPORTANT]        
> Este pacote de atualização é apenas para sistemas integrados do Azure Stack. Não é aplicável este pacote de atualização para o Azure Stack Development Kit.

## <a name="build-reference"></a>Criar a referência    
É o número de compilação de atualização do Azure Stack 1804 **20180513.1**.   

### <a name="new-features"></a>Novos recursos
Esta atualização inclui os seguintes aprimoramentos para o Azure Stack.

- <!-- 15028744 - IS -->  **Suporte do Visual Studio para implementações do Azure Stack desconectados através do AD FS**. No Visual Studio agora pode adicionar subscrições e autenticar com o AD FS federadas as credenciais do utilizador. 
 
- <!-- 1779474, 1779458 - IS --> **Utilize máquinas virtuais da série Av2 e F**. O Azure Stack pode agora utilizar máquinas virtuais com base nos tamanhos de máquina virtual da série Av2 e da série F. Para obter mais informações, consulte [tamanhos de máquinas virtuais suportados no Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes). 

- <!-- 1759172 - IS, ASDK --> **Novas subscrições administrativas**. Com 1804 há dois novos tipos de subscrição disponíveis no portal. Esses novos tipos de subscrição são, além da subscrição do fornecedor predefinida e visíveis com novas instalações do Azure Stack a partir da versão 1804. *Não utilize esses novos tipos de subscrição com esta versão do Azure Stack*. Anunciaremos a disponibilidade para utilizar estes tipos de subscrição com uma atualização futura. 

  Se atualizar o Azure Stack para a versão 1804, dois novos tipos de subscrição não estão visíveis. No entanto, os sistemas integrados de novas implementações do Azure Stack e as instalações da versão do Kit de desenvolvimento do Azure Stack 1804 ou posterior tem acesso a todos os tipos de subscrição de três.  

  Esses novos tipos de subscrição fazem parte de uma alteração maior para proteger a subscrição do fornecedor predefinida e, para que seja mais fácil de implantar recursos compartilhados, como servidores SQL que aloja. À medida que adicionamos mais partes dessa alteração maiores com as futuras atualizações para o Azure Stack, recursos implementados nesses novos tipos de subscrição poderão perder-se. 

  Os tipos de subscrição de três agora visíveis são:  
  - Padrão de subscrição do fornecedor: continuar a utilizar este tipo de subscrição. 
  - Medição de subscrição: *não utilize este tipo de subscrição.*
  - Subscrição de consumo: *não utilize este tipo de subscrição*

  



## <a name="fixed-issues"></a>Problemas corrigidos

- <!-- IS, ASDK -->  No portal de administração, já não tem de atualizar o mosaico de atualização antes de apresenta informações.
 
- <!-- 2050709  -->  Agora, pode utilizar o portal de administração para editar as métricas de armazenamento para o serviço Blob, serviço tabela e o serviço de fila.
 
- <!-- IS, ASDK --> Sob **Networking**, quando clicar em **ligação** para configurar uma ligação de VPN, **Site-site (IPsec)** agora é a única opção disponível.

- **Várias correções** de desempenho, estabilidade, segurança e o sistema operativo que é utilizado pelo Azure Stack.

## <a name="additional-releases-timed-with-this-update"></a>Versões adicionais excedeu o tempo limite com esta atualização  
A seguir está agora disponível, mas não precisam de atualização do Azure Stack 1804.
- **Atualizar para o pacote de monitorização do Microsoft Azure Stack System Center Operations Manager**. Uma nova versão (1.0.3.0) do Microsoft System Center Operations Manager monitorização Pack para o Azure Stack está disponível para [transferir](https://www.microsoft.com/download/details.aspx?id=55184). Com esta versão, pode utilizar principais de serviço ao adicionar uma implementação do Azure Stack ligada. Esta versão também apresenta uma experiência de gestão de atualizações permite-lhe tomar medidas de remediação diretamente a partir de dentro do Operations Manager. Há também novos dashboards que apresentam os fornecedores de recursos, unidades de escala e dimensionar nós de unidade.

- **Versão do PowerShell de administrador 1.3.0 de pilha do Azure novo**.  O Azure Stack do PowerShell 1.3.0 está agora disponível para instalação. Esta versão fornece comandos para todos os fornecedores de recursos de administração gerir o Azure Stack.  Com esta versão, algum conteúdo irá ser preterido a partir do GitHub de ferramentas do Azure Stack [repositório](https://github.com/Azure/AzureStack-Tools). 

   Para obter detalhes de instalação, siga os [instruções](azure-stack-powershell-install.md) ou o [ajudar](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) conteúdo para o módulo do Azure Stack 1.3.0. 

- **Lançamento de referência da Rest API do Azure Stack inicial**. O [referência da API para todos os fornecedores de recursos de administração do Azure Stack](https://docs.microsoft.com/rest/api/azure-stack/) agora está publicada. 


## <a name="before-you-begin"></a>Antes de começar    

### <a name="prerequisites"></a>Pré-requisitos
- Instalar o Azure Stack [atualizar versão 1803](azure-stack-update-1803.md) antes de aplicar a atualização do Azure Stack 1804.  
  
- Instalar o mais recente disponível [atualização ou correção para a versão 1803](azure-stack-update-1803.md#post-update-steps). 


### <a name="known-issues-with-the-update-process"></a>Problemas conhecidos com o processo de atualização   
- Durante a instalação da atualização 1804, poderá ver alertas com o título *erro – o modelo para typu FaultType UserAccounts.New está em falta.*  Pode ignorar com segurança estes alertas. Estes alertas fechará automaticamente depois de concluída a atualização para 1804.   
 
- <!-- TBD - IS --> Não tente criar máquinas virtuais durante a instalação desta atualização. Para obter mais informações sobre a gestão de atualizações, consulte [gerir atualizações na descrição geral do Azure Stack](azure-stack-updates.md#plan-for-updates).


### <a name="post-update-steps"></a>Passos de pós-atualização
Após a instalação de 1804, instale as correções aplicáveis. Para obter mais informações, consulte os seguintes artigos da base de dados de conhecimento, bem como nossos [política de manutenção](azure-stack-servicing-policy.md).  
 - [KB 4344114 - o Azure Stack correção 1.0.180527.15](https://support.microsoft.com/help/4344114).




### <a name="known-issues-post-installation"></a>Problemas conhecidos (após a instalação)
Seguem-se após a instalação problemas conhecidos para compilação **20180513.1**.

#### <a name="portal"></a>Portal

- A documentação técnica do Azure Stack enfoca a versão mais recente. Devido a alterações de portais entre versões, o que vê quando utilizar os portais do Azure Stack pode variar do que vê na documentação. 

- <!-- TBD - IS ASDK --> Não é possível aplicar atualizações de controladores mediante a utilização de um pacote de extensão de OEM com esta versão do Azure Stack.  Não é uma solução para este problema.

- <!-- 1272111 - IS --> Depois de instalar ou atualizar para esta versão do Azure Stack, poderá não conseguir ver as unidades de escala do Azure Stack no portal de administração.  
  Solução: Utilize o PowerShell para ver informações sobre unidades de escala. Para obter mais informações, consulte a [ajudar](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) conteúdo para o módulo do Azure Stack 1.3.0. 

- <!-- 2332636 - IS -->  Quando utilizar o AD FS para o seu sistema de identidade do Azure Stack e a atualização para esta versão do Azure Stack, o proprietário de predefinição da subscrição do fornecedor de padrão é reposto incorporada **CloudAdmin** utilizador.  
  Solução: Para resolver este problema depois de instalar esta atualização, utilize o passo 3 dos [acionar a automatização para configurar afirmações fidedignidade de fornecedor no Azure Stack](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) procedimento para repor o proprietário da subscrição do fornecedor de predefinição.   

- <!-- TBD - IS ASDK --> Alguns tipos de subscrição administrativos não estão disponíveis.  Quando atualizar o Azure Stack para esta versão, os tipos de duas subscrição que eram [introduzido com a versão 1804](#new-features) não estão visíveis na consola do. Isto era esperado. São os tipos de subscrição indisponível *medição subscrição*, e *subscrição de consumo*. Esses tipos de subscrição são visíveis no novos ambientes do Azure Stack a partir da versão 1804, mas ainda não estão prontos a utilizar. Deve continuar a utilizar o *fornecedor predefinido* tipo de subscrição.  


- <!-- TBD -  IS ASDK -->A capacidade [para abrir um novo pedido de suporte na lista pendente](azure-stack-manage-portals.md#quick-access-to-help-and-support) de dentro do administrador do portal não está disponível. Em vez disso, utilize a seguinte hiperligação:     
    - Para o Azure Stack sistemas integrados, utilize https://aka.ms/newsupportrequest.

- <!-- 2403291 - IS ASDK --> Poderá não ter o uso da barra de deslocamento horizontal na parte inferior de portais de administrador e utilizador. Se não conseguir aceder a barra de deslocamento horizontal, utilize a estrutura de ligações para navegar até um painel anterior no portal ao selecionar o nome do painel pretende visualizar na lista de trilha encontrado na parte superior esquerda do portal.
  ![Navegação estrutural](media/azure-stack-update-1804/breadcrumb.png) 

- <!-- TBD - IS --> Pode não ser possível ver os recursos de computação ou o armazenamento no portal do administrador. A causa deste problema é um erro durante a instalação da atualização que faz com que a atualização para ser comunicadas incorretamente conclusão com êxito. Se este problema ocorrer, contacte o suporte técnico da Microsoft para obter assistência.

- <!-- TBD - IS --> Poderá ver um dashboard em branco no portal. Para recuperar o dashboard, selecione o ícone de engrenagem no canto superior direito do portal e, em seguida, selecione **restaurar predefinições**.

- <!-- TBD - IS ASDK --> A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos inteiro e, em seguida, eliminar subscrições de utilizador.

- <!-- TBD - IS ASDK --> Não é possível ver as permissões à sua subscrição a utilizar os portais do Azure Stack. Como solução, utilize o PowerShell para verificar as permissões.

- <!-- TBD - IS ASDK --> No portal de administração, poderá ver um alerta crítico para o *Microsoft.Update.Admin* componente. O nome do alerta, descrição e remediação todos são apresentadas como:  
    - *ERRO - o modelo para typu FaultType ResourceProviderTimeout está em falta.*

  Este alerta pode ser ignorado com segurança. 


#### <a name="health-and-monitoring"></a>Estado de funcionamento e monitorização
- <!-- 1264761 - IS ASDK -->  Poderá ver alertas para o *controlador de estado de funcionamento* componente que tem os seguintes detalhes:  

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

  Ambos os alertas podem ser ignoradas com segurança. Será fechada automaticamente ao longo do tempo.  
 

#### <a name="compute"></a>Computação
- <!-- TBD - IS --> Ao selecionar um tamanho de máquina virtual para uma implementação da máquina virtual, alguns tamanhos de VM da série F não estão visíveis como parte do Seletor de tamanho, ao criar uma VM. Os seguintes tamanhos VM não aparecem no Seletor de: *F8s_v2*, *F16s_v2*, *F32s_v2*, e *F64s_v2*.  
  Como solução, utilize um dos seguintes métodos para implementar uma VM. Cada método, tem de especificar o tamanho da VM que pretende utilizar.

  - **Modelo de Gestor de recursos do Azure:** quando utiliza um modelo, defina o *vmSize* no modelo para o tamanho desejado da VM de igual. Por exemplo, o seguinte é utilizado para implementar uma VM que utiliza a *F32s_v2* tamanho:  

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

- <!-- TBD - IS ASDK --> Ao criar máquinas virtuais no portal de utilizador do Azure Stack, o portal apresenta um número incorreto de discos de dados que pode anexar a uma VM da série D. Série de D suportada todas as VMs pode acomodar os discos de dados como a configuração do Azure.

- <!-- TBD - IS ASDK --> Quando não for possível criar uma imagem de VM, pode ser adicionado um item com falha que não é possível eliminar para o painel de computação de imagens VM.

  Como solução, criar uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (New-VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item falhado. Em seguida, 15 minutos depois de criar a imagem fictícia, pode com êxito eliminá-la.

  Em seguida, pode tentar transfira a imagem VM que falhou anteriormente.

- <!-- TBD - IS ASDK --> Se uma extensão numa implantação de VM o aprovisionamento demora demasiado tempo, os utilizadores devem permitir que o limite de tempo de aprovisionamento em vez de tentar interromper o processo para desaloque ou elimine a VM.  

- <!-- 1662991 IS ASDK --> Diagnóstico de VM do Linux não é suportado no Azure Stack. Quando implementa uma VM do Linux com o diagnóstico VM ativado, a implementação falhar. A implementação falhar, também, se ativar as métricas básicas de VM do Linux através das definições de diagnóstico.  


#### <a name="networking"></a>Redes
- <!-- 1766332 - IS ASDK --> Sob **Networking**, se clicar em **criar Gateway de VPN** para configurar uma ligação de VPN, **baseado em política** está listado como um tipo de VPN. Não selecione esta opção. Apenas os **rota com base** opção é suportada no Azure Stack.

- <!-- 2388980 - IS ASDK --> Depois de uma VM é criada e associada a um endereço IP público, não é possível desassociar VM desse endereço IP. Desassociação parece funcionar, mas o endereço IP público atribuído anteriormente que permanece associado com a VM original.

  Atualmente, tem de utilizar apenas novos endereços IP públicos para novas VMs que criar.

  Este comportamento ocorre mesmo que a reatribuir o endereço IP a uma VM nova (geralmente conhecida como uma *alternância de VIP*). Todos os futuro tenta se conectar por meio deste resultado de endereço IP numa ligação para a VM associada originalmente e não para a nova.

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

#### <a name="sql-and-mysql"></a>SQL e o MySQL

- <!-- TBD - IS --> O fornecedor de recursos é suportado para criar itens nos servidores desse host SQL ou MySQL. Itens criados num servidor de anfitrião que não são criados pelo fornecedor de recursos podem resultar num estado sem correspondência.  

- <!-- IS, ASDK --> Caracteres especiais, incluindo espaços e períodos, não são suportadas no **família** ou **escalão** nomes ao criar um SKU para os fornecedores de recursos do SQL e o MySQL.


> [!NOTE]  
> <!-- TBD - IS --> Depois de atualizar para o Azure Stack 1804, pode continuar a utilizar os fornecedores de recursos do SQL e o MySQL que implementou anteriormente.  Recomendamos que Atualize para o SQL e o MySQL quando uma nova versão estiver disponível. Como o Azure Stack, aplica atualizações de fornecedores de recursos do SQL e o MySQL sequencialmente.  Por exemplo, se utilizar a versão 1802, aplicar primeiro a versão 1803 e, em seguida, atualize para 1804.      
>   
> A instalação da atualização 1804 não afeta a utilização atual de fornecedores de recursos SQL ou MySQL pelos seus utilizadores.
> Independentemente da versão dos fornecedores de recursos que utilizar, os dados de usuários em seus bancos de dados não é tocados e permanecem acessíveis.    



#### <a name="app-service"></a>Serviço de Aplicações
- <!-- 2352906 - IS ASDK --> Os utilizadores tem de registar o fornecedor de recursos de armazenamento antes de ser criar sua primeira função do Azure na subscrição.

- <!-- TBD - IS ASDK --> Para ampliar a infraestrutura (funções de trabalho, gerenciamento, funções de front-end), tem de utilizar o PowerShell, tal como descrito nas notas de versão para computação.

- <!-- TBD - IS ASDK --> Serviço de aplicações só pode ser implementado para o **subscrição do fornecedor predefinido** neste momento.  Numa atualização futura o serviço de aplicações irá implementar para a nova subscrição de medição introduzida no Azure Stack 1804 e todas as implementações existentes serão migradas para esta nova subscrição também.

#### <a name="usage"></a>Utilização  
- <!-- TBD - IS ASDK --> Dados de medidor de utilização de endereços de IP público de utilização mostram o mesmo *EventDateTime* valor para cada registo, em vez do *TimeDate* carimbo que mostra quando o registo foi criado. Atualmente, não pode utilizar estes dados para efetuar gestão de contas preciso de utilização de endereços IP pública.


<!-- #### Identity -->
<!-- #### Marketplace --> 


## <a name="download-the-update"></a>Transferir a atualização
Pode transferir o pacote de atualização do Azure Stack 1804 partir [aqui](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Consulte também
- Para utilizar o ponto final com privilégios (PEP) para monitorizar e retomar as atualizações, consulte [monitorizar atualizações no Azure Stack, utilizando o ponto final com privilégios](azure-stack-monitor-update.md).
- Para uma descrição geral da gestão de atualizações no Azure Stack, veja [gerir atualizações na descrição geral do Azure Stack](azure-stack-updates.md).
- Para obter mais informações sobre como aplicar as atualizações com o Azure Stack, veja [aplicar atualizações no Azure Stack](azure-stack-apply-updates.md).
