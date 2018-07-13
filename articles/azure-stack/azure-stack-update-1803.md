---
title: Atualização de versão 1803 do Azure Stack | Documentos da Microsoft
description: Saiba mais sobre as novidades na versão 1803 atualização para o Azure Stack integrada sistemas, os problemas conhecidos e onde pode transferir a atualização.
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
ms.date: 07/11/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: 11430a0d194a722c0c0520c936db3c08b1a6b863
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989576"
---
# <a name="azure-stack-1803-update"></a>Atualização de versão 1803 de pilha do Azure

*Aplica-se a: sistemas integrados do Azure Stack*

Este artigo descreve as melhorias e correções no pacote de atualização de versão 1803, problemas conhecidos para esta versão e onde pode transferir a atualização. Problemas conhecidos são divididos em problemas diretamente relacionados com o processo de atualização e problemas com a compilação (após a instalação).

> [!IMPORTANT]        
> Este pacote de atualização é apenas para sistemas integrados do Azure Stack. Não é aplicável este pacote de atualização para o Azure Stack Development Kit.

## <a name="build-reference"></a>Criar a referência    
É o número de compilação de atualização de versão 1803 do Azure Stack **20180329.1**.


## <a name="before-you-begin"></a>Antes de começar    
> [!IMPORTANT]    
> Não tente criar máquinas virtuais durante a instalação desta atualização. Para obter mais informações sobre a gestão de atualizações, consulte [gerir atualizações na descrição geral do Azure Stack](azure-stack-updates.md#plan-for-updates).


### <a name="prerequisites"></a>Pré-requisitos
- Instalar o Azure Stack [1802 atualizar](azure-stack-update-1802.md) antes de aplicar a atualização de versão 1803 de pilha do Azure.   

- Instale **AzS correção – 20180222.2 de criar 1.0.180312.1-** antes de aplicar a atualização de versão 1803 de pilha do Azure. Esta correção atualiza o Windows Defender e está disponível quando transferir atualizações para o Azure Stack.

  Para instalar a correção, siga os procedimentos normais para [instalação de atualizações para o Azure Stack](azure-stack-apply-updates.md). O nome da atualização é apresentado como **AzS correção – 1.0.180312.1**e inclui os seguintes ficheiros: 
    - PUPackageHotFix_20180222.2-1.exe
    - PUPackageHotFix_20180222.2-1.bin
    - Metadata.xml

  Depois de carregar estes ficheiros para uma conta de armazenamento e um contentor, execute a instalação a partir do mosaico de atualização no portal de administração. 
  
  Ao contrário das atualizações para o Azure Stack, instalação desta atualização não altera a versão do Azure Stack. Para confirmar esta atualização é instalada, ver a lista de **atualizações instaladas**.



### <a name="new-features"></a>Novos recursos 
Esta atualização inclui as seguintes melhorias e correções para o Azure Stack.

- **Atualizar segredos do Azure Stack** - (contas e certificados). Para obter mais informações sobre a gestão de segredos, consulte [rodar segredos no Azure Stack](azure-stack-rotate-secrets.md). 

- <!-- 1914853 --> **Redirecionamento automático para HTTPS** quando usar HTTP para acessar os portais de administrador e usuário. Esta melhoria foi efetuada com base na [UserVoice](https://feedback.azure.com/forums/344565-azure-stack/suggestions/32205385-it-would-be-great-if-there-was-a-automatic-redirec) comentários para o Azure Stack. 

- <!-- 2202621  --> **Aceder ao Marketplace** – agora pode abrir a pilha do Azure Marketplace utilizando o [+ novo](https://ms.portal.azure.com/#create/hub) opção de portais de administrador e utilizador da mesma forma que nos portais do Azure.
 
- <!-- 2202621 --> **O Azure Monitor** -adiciona do Azure Stack [do Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) nos portais de administrador e utilizador. Isto inclui novos gerenciadores de métricas e registos de atividades. Para aceder a este Monitor do Azure a partir de redes externas, da porta **13012** devem estar abertas em configurações de firewall. Para obter mais informações sobre portas exigidas pelo Azure Stack, veja [integração no datacenter do Azure Stack - publicar pontos de extremidade](azure-stack-integrate-endpoints.md).

   Além disso, como parte deste alterar em **mais serviços**, *registos de auditoria* agora é apresentada como *registos de atividades*. A funcionalidade agora é consistente com o portal do Azure. 

- <!-- 1664791 --> **Arquivos esparsos** - quando adiciona uma nova imagem para o Azure Stack ou adicionar uma imagem por meio de distribuição de mercado, a imagem é convertida para um ficheiro disperso. Não não possível converter imagens que foram adicionadas antes de utilizar a versão do Azure Stack versão 1803. Em vez disso, tem de utilizar a distribuição de mercado para submeter novamente essas imagens para tirar partido desta funcionalidade. 
 
   Arquivos esparsos são um formato de arquivo eficiente usado para reduzir a utilização do espaço de armazenamento e melhorar a e/s.  Para obter mais informações, consulte [Fsutil dispersa](https://docs.microsoft.com/windows-server/administration/windows-commands/fsutil-sparse) para o Windows Server. 

### <a name="fixed-issues"></a>Problemas de fixos

- <!-- 1739988 --> Balanceamento de carga interno (ILB) agora apropriada com endereços MAC para VMs de back-end, que faz com que o ILB remover pacotes para a rede de back-end, quando utilizar instâncias de Linux na rede de back-end. ILB funciona bem com instâncias do Windows na rede de back-end. 

- <!-- 1805496 --> Um problema em que as ligações de VPN entre o Azure Stack seria ser desligadas devido ao Azure Stack com definições diferentes para a política de IKE que o Azure. Os valores para SALifetime (hora) e SALiftetime (Bytes) não eram compatíveis com o Azure e foram alterados na versão 1803 de acordo com as definições do Azure. O valor para SALifetime (segundos) antes da versão 1803 foi 14 400 e agora as alterações ao 27,000 na versão 1803. O valor para SALifetime (Bytes) antes da versão 1803 foi 819,200 e as alterações ao 33,553,408 na versão 1803.

- <!-- 2209262 --> O problema IP em que estava anteriormente visível no portal; ligações VPN No entanto ativar ou alternar o reencaminhamento IP não tem qualquer efeito. A funcionalidade está ativada por predefinição e a capacidade de alterar esta ainda não é suportado.  O controle foi removido do portal. 

- <!-- 1766332 --> O Azure Stack não suporta a política com base em VPN Gateways, mesmo que a opção é apresentada no Portal do.  A opção foi removida do Portal. 

- <!-- 1868283 --> O Azure Stack agora impede o redimensionamento de uma máquina virtual que é criada com discos dinâmicos. 

- <!-- 1756324 --> Dados de utilização de máquinas virtuais está agora divididos em intervalos por hora. Isso é consistente com o Azure. 

- <!--  2253274 --> O problema em que nos portais de administrador e utilizador, o painel de definições para a vNet, sub-redes não consegue carregar. Como solução, utilize o PowerShell e o [Get-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig?view=azurermps-5.5.0) cmdlet para ver e gerir estas informações.

- Quando cria uma máquina virtual, a mensagem *não foi possível apresentar preços* deixa de aparecer ao escolher um tamanho para o tamanho da VM.

- Várias correções para o desempenho, estabilidade, segurança e o sistema operativo que é utilizado pelo Azure Stack.


### <a name="changes"></a>Alterações
- A forma de alterar o estado de uma oferta recém-criado desde *privada* para *público* ou *desativados* foi alterado. Para obter mais informações, consulte [criar uma oferta](azure-stack-create-offer.md).


### <a name="known-issues-with-the-update-process"></a>Problemas conhecidos com o processo de atualização    
<!-- 2328416 --> Durante a instalação da atualização de versão 1803, pode haver um período de indisponibilidade do serviço de BLOBs e serviços internos que utilizam o serviço de Blobs. Isto inclui algumas operações de máquina virtual. Este período de indisponibilidade pode provocar falhas de inquilino de operações ou alertas dos serviços que não é possível aceder aos dados. Este problema é resolvido em si quando a atualização é concluída a instalação. 



### <a name="post-update-steps"></a>Passos de pós-atualização
- Após a instalação da versão 1803, instale as correções aplicáveis. Para obter mais informações, consulte os seguintes artigos da base de dados de conhecimento, bem como nossos [política de manutenção](azure-stack-servicing-policy.md).

  - [KB 4344115 - o Azure Stack correção 1.0.180427.15](https://support.microsoft.com/help/4344115).

- Depois de instalar esta atualização, reveja a configuração de firewall para garantir [portas necessárias](azure-stack-integrate-endpoints.md) estão abertas. Por exemplo, esta atualização introduz *do Azure Monitor* que inclui uma alteração de registos de auditoria para os registos de atividades. Com esta alteração, a porta 13012 agora é utilizada e também tem de estar aberta.  


### <a name="known-issues-post-installation"></a>Problemas conhecidos (após a instalação)
Seguem-se após a instalação problemas conhecidos para compilação **20180323.2**.

#### <a name="portal"></a>Portal
- <!-- 2332636 - IS -->  Quando utilizar o AD FS para o seu sistema de identidade do Azure Stack e a atualização para esta versão do Azure Stack, o proprietário de predefinição da subscrição do fornecedor de padrão é reposto incorporada **CloudAdmin** utilizador.  
  Solução: Para resolver este problema depois de instalar esta atualização, utilize o passo 3 dos [acionar a automatização para configurar afirmações fidedignidade de fornecedor no Azure Stack](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) procedimento para repor o proprietário da subscrição do fornecedor de predefinição.   

- A capacidade [para abrir um novo pedido de suporte na lista pendente](azure-stack-manage-portals.md#quick-access-to-help-and-support) de dentro do administrador do portal não está disponível. Em vez disso, utilize a seguinte hiperligação:     
    - Para o Azure Stack sistemas integrados, utilize https://aka.ms/newsupportrequest.

- <!-- 2050709 --> No portal de administração, não é possível editar as métricas de armazenamento para o serviço Blob, serviço de tabela ou o serviço de fila. Quando passe para o armazenamento e, em seguida, selecione o blob, tabela ou mosaico de serviço de fila, abrirá um novo painel que apresenta um gráfico de métricas para esse serviço. Se, em seguida, selecione editar na parte superior do mosaico de gráfico de métricas, o painel de editar gráfico é aberto, mas não apresenta as opções para editar as métricas.

- Pode não ser possível ver os recursos de computação ou o armazenamento no portal do administrador. A causa deste problema é um erro durante a instalação da atualização que faz com que a atualização para ser comunicadas incorretamente conclusão com êxito. Se este problema ocorrer, contacte o suporte técnico da Microsoft para obter assistência.

- Poderá ver um dashboard em branco no portal. Para recuperar o dashboard, selecione o ícone de engrenagem no canto superior direito do portal e, em seguida, selecione **restaurar predefinições**.

- A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos inteiro e, em seguida, eliminar subscrições de utilizador.

- Não é possível ver as permissões à sua subscrição a utilizar os portais do Azure Stack. Como solução, utilize o PowerShell para verificar as permissões.

- No dashboard do portal de administração, o mosaico de atualização não apresenta informações sobre atualizações. Para resolver este problema, clique no mosaico para atualizá-la.

- No portal de administração, poderá ver um alerta crítico para o *Microsoft.Update.Admin* componente. O nome do alerta, descrição e remediação todos são apresentadas como:  
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


#### <a name="marketplace"></a>Marketplace
- Os utilizadores podem procurar o marketplace completo sem uma subscrição e podem ver itens administrativos, como os planos e ofertas. Esses itens estão não funcional para os utilizadores.



#### <a name="compute"></a>Computação
- Definições de dimensionamento para conjuntos de dimensionamento de máquinas virtuais não estão disponíveis no portal. Como solução, pode usar [do Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro, em vez de `-VMScaleSetName`.

- Quando cria um conjunto de disponibilidade no portal, vai **New** > **computação** > **do conjunto de disponibilidade**, só pode criar um conjunto de disponibilidade com um domínio de falha e o domínio de atualização de 1. Como solução, ao criar uma nova máquina virtual, criar o conjunto com o PowerShell, CLI, ou a partir de disponibilidade do portal.

- Ao criar máquinas virtuais no portal de utilizador do Azure Stack, o portal apresenta um número incorreto de discos de dados que pode anexar a uma VM da série D. Série de D suportada todas as VMs pode acomodar os discos de dados como a configuração do Azure.

- Quando não for possível criar uma imagem de VM, pode ser adicionado um item com falha que não é possível eliminar para o painel de computação de imagens VM.

  Como solução, criar uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (New-VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item falhado. Em seguida, 15 minutos depois de criar a imagem fictícia, pode com êxito eliminá-la.

  Em seguida, pode tentar transfira a imagem VM que falhou anteriormente.

-  Se uma extensão numa implantação de VM o aprovisionamento demora demasiado tempo, os utilizadores devem permitir que o limite de tempo de aprovisionamento em vez de tentar interromper o processo para desaloque ou elimine a VM.  

- <!-- 1662991 --> Diagnóstico de VM do Linux não é suportado no Azure Stack. Quando implementa uma VM do Linux com o diagnóstico VM ativado, a implementação falhar. A implementação falhar, também, se ativar as métricas básicas de VM do Linux através das definições de diagnóstico.  


#### <a name="networking"></a>Redes
- Depois de uma VM é criada e associada a um endereço IP público, não é possível desassociar VM desse endereço IP. Desassociação parece funcionar, mas o endereço IP público atribuído anteriormente que permanece associado com a VM original.

  Atualmente, tem de utilizar apenas novos endereços IP públicos para novas VMs que criar.

  Este comportamento ocorre mesmo que a reatribuir o endereço IP a uma VM nova (geralmente conhecida como uma *alternância de VIP*). Todos os futuro tenta se conectar por meio deste resultado de endereço IP numa ligação para a VM associada originalmente e não para a nova.



- O Azure Stack oferece suporte a uma única *gateway de rede local* por endereço IP. Isso é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local ligação de gateway, subsequente tenta criar um recurso de gateway de rede local com o mesmo endereço IP são bloqueados.

- Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a mudança para uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas para as VMs nessa vnet.

- O Azure Stack não suporta a adição de interfaces de rede adicionais para uma instância VM depois de implementada a VM. Se a VM exigir mais de uma interface de rede, tem de ser definidos no momento da implementação.

- <!-- 2096388 --> Não é possível utilizar o portal de administração ao atualizar as regras para um grupo de segurança de rede. 

    Solução alternativa para o serviço de aplicações: Se tiver de ambiente de trabalho remoto para as instâncias de controlador, modificar as regras de segurança dentro de grupos de segurança de rede com o PowerShell.  Seguem-se exemplos de como *permitem*e, em seguida, restaurar a configuração para *negar*:  
    
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


#### <a name="sql-and-mysql"></a>SQL e o MySQL
- Antes de prosseguir, verifique a observação importante [antes de começar](#before-you-begin) perto o início destas notas de versão.

- Pode demorar até uma hora antes dos utilizadores podem criar bases de dados numa nova implementação do SQL ou MySQL.

- O fornecedor de recursos é suportado para criar itens nos servidores desse host SQL ou MySQL. Itens criados num servidor de anfitrião que não são criados pelo fornecedor de recursos podem resultar num estado sem correspondência.  

- <!-- IS, ASDK --> Caracteres especiais, incluindo espaços e períodos, não são suportadas no **família** dê um nome ao criar um SKU para os fornecedores de recursos do SQL e o MySQL.

> [!NOTE]  
> Depois de atualizar para versão 1803 de pilha do Azure, pode continuar a utilizar os fornecedores de recursos do SQL e o MySQL que implementou anteriormente.  Recomendamos que Atualize para o SQL e o MySQL quando uma nova versão estiver disponível. Como o Azure Stack, aplica atualizações de fornecedores de recursos do SQL e o MySQL sequencialmente.  Por exemplo, se utilizar a versão 1711, aplicar primeiro a versão versão 1712, em seguida, 1802 e, em seguida, atualize para versão 1803.      
>   
> A instalação da atualização de versão 1803 não afeta a utilização atual de fornecedores de recursos SQL ou MySQL pelos seus utilizadores.
> Independentemente da versão dos fornecedores de recursos que utilizar, os dados de usuários em seus bancos de dados não é tocados e permanecem acessíveis.    



#### <a name="app-service"></a>Serviço de Aplicações
- Os utilizadores tem de registar o fornecedor de recursos de armazenamento antes de ser criar sua primeira função do Azure na subscrição.

- Para ampliar a infraestrutura (funções de trabalho, gerenciamento, funções de front-end), tem de utilizar o PowerShell, tal como descrito nas notas de versão para computação.


#### <a name="usage"></a>Utilização  
- Dados de medidor de utilização de endereços de IP público de utilização mostram o mesmo *EventDateTime* valor para cada registo, em vez do *TimeDate* carimbo que mostra quando o registo foi criado. Atualmente, não pode utilizar estes dados para efetuar gestão de contas preciso de utilização de endereços IP pública.

<!--
#### Identity
-->



#### <a name="downloading-azure-stack-tools-from-github"></a>Baixar ferramentas de pilha do Azure a partir do GitHub
- Ao utilizar o *invoke-webrequest* cmdlet do PowerShell para transferir o Azure Stack de ferramentas do Github, recebe um erro:     
    -  *Invoke-webrequest: O pedido foi abortado: não foi possível criar o canal seguro SSL/TLS.*     

  Este erro ocorre devido a uma recente descontinuação de suporte do GitHub dos Tlsv1 e Tlsv1.1 padrões de criptografia (o padrão para o PowerShell). Para obter mais informações, consulte [aviso de remoção de padrões de criptografia fraca](https://githubengineering.com/crypto-removal-notice/).

  Para resolver este problema, adicione `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` na parte superior do script para forçar a consola do PowerShell para utilizar TLSv1.2 ao transferir a partir de repositórios do GitHub.


## <a name="download-the-update"></a>Transferir a atualização
Pode transferir o pacote de atualização de versão 1803 de pilha do Azure partir [aqui](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Consulte também
- Para utilizar o ponto final com privilégios (PEP) para monitorizar e retomar as atualizações, consulte [monitorizar atualizações no Azure Stack, utilizando o ponto final com privilégios](azure-stack-monitor-update.md).
- Para uma descrição geral da gestão de atualizações no Azure Stack, veja [gerir atualizações na descrição geral do Azure Stack](azure-stack-updates.md).
- Para obter mais informações sobre como aplicar as atualizações com o Azure Stack, veja [aplicar atualizações no Azure Stack](azure-stack-apply-updates.md).
