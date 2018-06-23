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
ms.date: 06/22/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: a74e77f84aa70519015a589cbc6e7478c0c41592
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36318814"
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



### <a name="new-features"></a>Novas funcionalidades 
Esta atualização inclui as seguintes melhorias e correções para pilha do Azure.

- **Atualizar segredos de pilha do Azure** - (contas e certificados). Para obter mais informações sobre a gestão de segredos, consulte [rodar segredos no Azure pilha](azure-stack-rotate-secrets.md). 

- <!-- 1914853 --> **Redirecionamento automático para HTTPS** quando utiliza HTTP para aceder de portais de administrador e utilizador. Este melhoramento foi efetuado com base no [UserVoice](https://feedback.azure.com/forums/344565-azure-stack/suggestions/32205385-it-would-be-great-if-there-was-a-automatic-redirec) comentários para a pilha do Azure. 

- <!-- 2202621  --> **Aceder o Marketplace** – pode agora abrir a pilha do Azure Marketplace utilizando o [+ novo](https://ms.portal.azure.com/#create/hub) opção de portais de administrador e utilizador da mesma forma, fazê-lo nos portais do Azure.
 
- <!-- 2202621 --> **Monitor do Azure** -Azure pilha adiciona [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) para os portais de administrador e utilizador. Isto inclui o novo exploradores para os registos de métricas e atividade. Para aceder a este Monitor de Azure de redes externas, porta **13012** tem de estar aberta em configurações de firewall. Para obter mais informações sobre as portas necessárias pela pilha do Azure, consulte [integração do Centro de dados de pilha do Azure - publicar pontos finais](azure-stack-integrate-endpoints.md).

   Além disso, como parte deste processo alterar em **mais serviços**, *registos de auditoria* aparece como *registos de atividade*. A funcionalidade agora é consistente com o portal do Azure. 

- <!-- 1664791 --> **Ficheiros dispersos** - quando adicionar uma nova imagem para a pilha do Azure, ou adicionar uma imagem através de sindicação do marketplace, a imagem é convertida para um ficheiro disperso. Não não possível converter imagens que foram adicionadas antes de utilizar a versão de pilha do Azure 1803. Em vez disso, tem de utilizar sindicação marketplace reenviar essas imagens para tirar partido desta funcionalidade. 
 
   Ficheiros dispersos são um formato de ficheiro eficiente utilizado para reduzir a utilização do espaço de armazenamento e melhorar a e/s.  Para obter mais informações, consulte [Fsutil dispersa](https://docs.microsoft.com/windows-server/administration/windows-commands/fsutil-sparse) para o Windows Server. 

### <a name="fixed-issues"></a>Problemas fixos

- <!-- 1739988 --> Balanceamento de carga interno (ILB) agora corretamente processa os endereços MAC para VMs do back-end, que faz com que o ILB remover pacotes para a rede de back-end ao utilizar instâncias do Linux na rede de back-end. ILB funciona bem com instâncias de Windows na rede de back-end. 

- <!-- 1805496 --> Um problema onde seria desligam ligações de VPN entre pilha do Azure devido a pilha do Azure, utilizando definições diferentes para a política de IKE ao Azure. Os valores para SALifetime (hora) e SALiftetime (Bytes) não foram compatíveis com o Azure e foram alterados no 1803 para corresponder às definições do Azure. O valor para SALifetime (segundos) antes de 1803 tinham 14,400 e agora as alterações ao 27,000 1803. O valor para SALifetime (Bytes) antes de 1803 foi 819,200 e as alterações ao 33,553,408 no 1803.

- <!-- 2209262 --> O problema IP onde foi anteriormente visível no portal; ligações VPN No entanto ativando ou ativando ou desativando o reencaminhamento IP não tem efeito. A funcionalidade está ativada por predefinição e a capacidade de alterar esta ainda não suportado.  O controlo foi removido do portal. 

- <!-- 1766332 --> Pilha do Azure não suporta a política com base dos Gateways de VPN, apesar da opção é apresentada no Portal.  A opção foi removida do Portal. 

- <!-- 1868283 --> Pilha do Azure agora impede o redimensionamento de uma máquina virtual que é criada com discos dinâmicos. 

- <!-- 1756324 --> Dados de utilização por máquinas virtuais é agora separados em intervalos por hora. Isto é consistente com o Azure. 

- <!--  2253274 --> O problema onde em portais administrador e utilizador, o painel de definições para a vNet sub-redes não consegue carregar. Como solução, utilize o PowerShell e o [Get-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig?view=azurermps-5.5.0) cmdlet para ver e gerir estas informações.

- Quando cria uma máquina virtual, a mensagem *não é possível apresentar os preços* deixa de aparecer quando escolher um tamanho para o tamanho da VM.

- Várias correções para o desempenho, estabilidade, segurança e o sistema operativo que é utilizado pela pilha de Azure.


### <a name="changes"></a>Alterações
- A forma de alteração do Estado de uma oferta recentemente criado a partir da *privada* para *pública* ou *desativado* foi alterada. Para obter mais informações, consulte [criar uma oferta](azure-stack-create-offer.md).


### <a name="known-issues-with-the-update-process"></a>Problemas conhecidos relacionados com o processo de atualização    
<!-- 2328416 --> Durante a instalação da atualização 1803, pode ser período de indisponibilidade do serviço blob e serviços internos que utilizam o serviço blob. Isto inclui algumas operações de máquina virtual. Este tempo pode provocar falhas de inquilino de operações ou alertas dos serviços de que não é possível aceder a dados. Este problema é resolvido em si quando a atualização estiver concluída a instalação. 



### <a name="post-update-steps"></a>Passos pós-atualização
- Após a instalação de 1803, instale as correções aplicáveis. Para mais informações, veja os seguintes artigos da base de dados de conhecimento, bem como a nossa [manutenção política](azure-stack-servicing-policy.md).

  - [KB 4341390 - correção de pilha do Azure 1.0.180424.12](https://support.microsoft.com/en-us/help/4341390).

- Depois de instalar esta atualização, reveja a configuração de firewall para se certificar [as portas necessárias](azure-stack-integrate-endpoints.md) estão abertas. Por exemplo, esta atualização apresenta *Azure Monitor* que inclui uma alteração de registos de auditoria nos registos de atividade. Com esta alteração, porta 13012 agora é utilizada e também tem de estar aberta.  


### <a name="known-issues-post-installation"></a>Problemas conhecidos (pós-instalação)
As seguintes são problemas conhecidos de pós-instalação para a compilação **20180323.2**.

#### <a name="portal"></a>Portal
- <!-- 2332636 - IS -->  Quando utilizar o AD FS para o seu sistema de identidade de pilha do Azure e a atualização para esta versão da pilha do Azure, o proprietário de predefinida da subscrição de fornecedor predefinido é reposto para o incorporado **CloudAdmin** utilizador.  
  Solução: Para resolver este problema, depois de instalar esta atualização, utilize o passo 3 do [automatização de Acionador para configurar afirmações de confiança do fornecedor na pilha de Azure](azure-stack-integrate-identity.md#trigger-automation-to-configure-claims-provider-trust-in-azure-stack-1) procedimento para repor o proprietário da subscrição de fornecedor predefinido.   

- A capacidade [para abrir um novo pedido de suporte na lista pendente](azure-stack-manage-portals.md#quick-access-to-help-and-support) de dentro do administrador do portal não está disponível. Em vez disso, utilize a seguinte hiperligação:     
    - Para a pilha do Azure integrados sistemas, utilize https://aka.ms/newsupportrequest.

- <!-- 2050709 --> No portal de administração, não é possível editar as métricas do storage para o serviço Blob, tabela serviço ou serviço fila. Quando aceder ao armazenamento e, em seguida, selecione o blob, tabela ou mosaico do serviço de fila, um novo painel abre-se que apresenta um gráfico de métricas para esse serviço. Se, em seguida, selecione Editar da parte superior do mosaico de gráfico de métricas, o painel de editar gráfico abre, mas não apresenta as opções para editar as métricas.

- Poderá não ser possível ver os recursos de computação ou o armazenamento no portal do administrador. A causa deste problema é um erro durante a instalação da atualização que faz com que a atualização incorretamente sejam comunicados como concluída com êxito. Se este problema ocorrer, contacte o suporte técnico da Microsoft para obter assistência.

- Poderá ver um dashboard em branco no portal. Para recuperar o dashboard, selecione o ícone de equipamento no canto superior direito do portal e, em seguida, selecione **restaurar predefinições**.

- A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos completo e, em seguida, eliminar subscrições de utilizador.

- Não é possível ver as permissões para a sua subscrição utilizando os portais de pilha do Azure. Como solução, utilize o PowerShell para verificar permissões.

- No dashboard do portal de administração, o mosaico de atualização não consegue apresentar informações sobre atualizações. Para resolver este problema, clique no mosaico atualizá-lo.

- No portal de administração, poderá ver um alerta crítico para o *Microsoft.Update.Admin* componente. O nome do alerta, a descrição e remediação todos são apresentadas como:  
    - *ERRO - o modelo para o FaultType ResourceProviderTimeout está em falta.*

  Este alerta pode ser ignorado com segurança. 


#### <a name="health-and-monitoring"></a>Estado de funcionamento e a monitorização
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

  Ambos os alertas podem ser ignoradas. Irá fechar automaticamente ao longo do tempo.  


#### <a name="marketplace"></a>Marketplace
- Os utilizadores podem procurar o mercado completo sem uma subscrição e podem ver itens administrativos como planos e ofertas. Estes itens estão não funcional para os utilizadores.



#### <a name="compute"></a>Computação
- Definições de dimensionamento para conjuntos de dimensionamento de máquina virtual não estão disponíveis no portal. Como solução, pode utilizar [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro em vez de `-VMScaleSetName`.

- Quando cria um conjunto de disponibilidade no portal, acedendo a **novo** > **computação** > **do conjunto de disponibilidade**, só pode criar um conjunto de disponibilidade com um domínio de falhas e o domínio de atualização de 1. Como solução, ao criar uma nova máquina virtual, crie o conjunto com o PowerShell, CLI, ou a partir da disponibilidade do portal.

- Ao criar máquinas virtuais no portal de utilizador de pilha do Azure, o portal apresenta um número incorreto de discos de dados que pode anexar a uma série de D VM. Todas as séries de D suportadas VMs podem acomodar tantos discos de dados como a configuração do Azure.

- Quando não for possível criar uma imagem de VM, um item de falha que não é possível eliminar pode ser adicionado ao painel de processamento de imagens de VM.

  Como solução, crie uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (novo VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item da falha. Em seguida, 15 minutos depois de criar a imagem fictício, que pode com êxito eliminá-lo.

  Em seguida, pode tentar transfira novamente a imagem VM que tenha falhado anteriormente.

-  Se uma extensão numa implementação de VM de aprovisionamento demora demasiado tempo, os utilizadores devem permitir o aprovisionamento do limite de tempo em vez de tentar interromper o processo de Desalocação ou eliminar a VM.  

- <!-- 1662991 --> Diagnóstico de VM com Linux não é suportado na pilha do Azure. Quando implementa uma VM com Linux com diagnósticos da VM ativados, a implementação falhará. A implementação falha também se ativar as métricas de básicas da VM com Linux através das definições de diagnóstico.  


#### <a name="networking"></a>Redes
- Depois de uma VM é criada e associada a um endereço IP público, não é possível desassociar essa VM a partir desse endereço IP. Desassociação aparece funcionar, mas o endereço IP público anteriormente atribuído permanece associado a VM original.

  Atualmente, tem de utilizar apenas novos endereços IP públicos para novas VMs que cria.

  Este comportamento ocorre mesmo reatribuir o endereço IP para uma nova VM (normalmente denominado como um *alternância de VIP*). Todas as futuras tenta estabelecer ligação através deste resultado de endereço IP numa ligação para a VM originalmente associada e não para a nova.



- Pilha do Azure suporta um único *gateway de rede local* por endereço IP. Isto é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local gateway ligação, posterior tentativas criar um recurso de gateway de rede local com o mesmo endereço IP estão bloqueadas.

- Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a alteração a uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas por push para VMs nessa Vnet.

- Pilha do Azure não suporta a adição de interfaces de rede adicionais para uma instância VM após a implementação da VM. Se a VM precisar de mais de uma interface de rede, tem de ser definidos no momento da implementação.

- <!-- 2096388 --> Não é possível utilizar o portal de administração para atualizar as regras para um grupo de segurança de rede. 

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

- <!-- IS, ASDK --> Os carateres especiais, incluindo espaços e períodos, não são suportados no **família** nome quando cria um SKU para os fornecedores de recursos do SQL Server e o MySQL.

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
