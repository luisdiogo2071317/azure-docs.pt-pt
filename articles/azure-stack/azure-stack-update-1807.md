---
title: O Azure Stack 1807 atualização | Documentos da Microsoft
description: Saiba mais sobre o que há de novo na atualização 1807 para os sistemas integrados do Azure Stack, incluindo os problemas conhecidos e onde pode transferir a atualização.
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
ms.date: 07/24/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: fe37a62d364d53d03a232b6fdc41e38a9ae2e30f
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228113"
---
# <a name="azure-stack-1807-update"></a>Atualização de 1807 de pilha do Azure

*Aplica-se a: sistemas integrados do Azure Stack*

Este artigo descreve o conteúdo do pacote de atualização de 1807. Esta atualização inclui melhorias, correções e problemas conhecidos para esta versão do Azure Stack e onde pode transferir a atualização. Problemas conhecidos são divididos em problemas diretamente relacionados com o processo de atualização e problemas com a compilação (após a instalação).

> [!IMPORTANT]        
> Este pacote de atualização é apenas para sistemas integrados do Azure Stack. Não é aplicável este pacote de atualização para o Azure Stack Development Kit.

## <a name="build-reference"></a>Criar a referência    
É o número de compilação de atualização do Azure Stack 1807 **1.1807.XX.X**.  


### <a name="new-features"></a>Novos recursos
Esta atualização inclui os seguintes aprimoramentos para o Azure Stack.

- <!-- 1658937 | ASDK, IS --> **Iniciar as cópias de segurança através de um agendamento predefinido** -como uma aplicação do Azure Stack agora podem disparar automaticamente cópias de segurança de infra-estrutura periodicamente para eliminar a intervenção humana. O Azure Stack irá limpar também automaticamente a partilha externa para cópias de segurança que sejam mais antigas do que o período de retenção definida. 

- <!-- 2496385 | ASDK, IS --> **Tempo para o tempo total de cópia de segurança de transferência de dados foi adicionados.**

-   <!-- 1702130 | ASDK, IS --> **Cópia de segurança capacidade externa mostra agora a capacidade correta da partilha externa.** (Anteriormente era isso codificar a 10 GB.)

- <!-- 2508488 |  IS   -->   Expandir a capacidade por [adicionar nós de unidade de escala adicionais](azure-stack-add-scale-node.md).



### <a name="fixed-issues"></a>Problemas de fixos

- <!-- 448955 | IS ASDK --> Agora pode consultar os registos de atividades para sistemas que são implementados num fuso horário UTC + N com êxito.    

- <!-- 2319627 |  ASDK, IS --> Pré-verificação de parâmetros de configuração de cópia de segurança (chave de caminho/nome de utilizador/palavra-passe/encriptação) já não define definições incorretas para a configuração de cópia de segurança. (Anteriormente, definições incorretas tiverem sido definidas para a cópia de segurança e a cópia de segurança seria seria, em seguida, falhar quando tirggered.)

- <!-- 2215948 |  ASDK, IS --> Agora atualiza a lista de cópia de segurança ao eliminar manualmente a cópia de segurança da partilha externa.

- <!-- 2332636 - IS -->  Atualização para esta versão já não repõe o proprietário do padrão da subscrição do fornecedor predefinido para o usuário interno de CloudAdmin quando implantado com o AD FS.

- <!-- 2360715 |  ASDK, IS -->  Quando configurar a integração no datacenter, já não aceder o ficheiro de metadados do AD FS de um compartilhamento. Para obter mais informações, consulte [configurar a integração do AD FS ao fornecer o ficheiro de metadados de Federação](azure-stack-integrate-identity.md#setting-up-ad-fs-integration-by-providing-federation-metadata-file). 

- <!-- 2388980 | ASDK, IS --> Foi corrigido um problema que impediu que os utilizadores atribuído um endereço IP público existente que tinha sido anteriormente atribuído a uma Interface de rede ou um balanceador de carga para uma nova Interface de rede ou Balanceador de carga.  

-   <!--2292271 | ASDK, IS --> Foi corrigido um problema em que um limite de Quota de modificação não foi aplicada a subscrições existentes.  Agora, quando gerar um limite de Quota para um recurso de rede que faz parte de uma oferta e plano associada a uma subscrição de inquilino, o novo limite aplica-se a subscrições existentes, bem como as novas subscrições.

- **Várias correções** de desempenho, estabilidade, segurança e o sistema operativo que é utilizado pelo Azure Stack.


<!-- ### Additional releases timed with this update    -->



## <a name="before-you-begin"></a>Antes de começar    

### <a name="prerequisites"></a>Pré-requisitos
- Instalar o Azure Stack [1805 atualizar](azure-stack-update-1805.md) antes de aplicar a atualização do Azure Stack 1807.  Não ocorreu nenhuma atualização 1806.  

- Antes de iniciar a instalação da atualização 1807, execute [AzureStack teste](azure-stack-diagnostic-test.md) para validar o status do seu Azure Stack e resolver quaisquer problemas operacionais encontrados. Também rever alertas ativos e resolver qualquer um que requerem uma ação.

### <a name="known-issues-with-the-update-process"></a>Problemas conhecidos com o processo de atualização   
- <!-- 2468613 - IS --> Durante a instalação desta atualização, poderá ver alertas com o título *erro – o modelo para typu FaultType UserAccounts.New está em falta.*  Pode ignorar com segurança estes alertas. Estes alertas serão fechado automaticamente depois de concluída a instalação desta atualização.   

- <!-- 2489559 - IS --> Não tente criar máquinas virtuais durante a instalação desta atualização. Para obter mais informações sobre a gestão de atualizações, consulte [gerir atualizações na descrição geral do Azure Stack](azure-stack-updates.md#plan-for-updates).


### <a name="post-update-steps"></a>Passos de pós-atualização
*Não há nenhuma etapa posterior à atualização para atualização 1807.*

<!-- After the installation of this update, install any applicable Hotfixes. For more information view the following knowledge base articles, as well as our [Servicing Policy](azure-stack-servicing-policy.md).  
 - Link to KB  
 -->

## <a name="known-issues-post-installation"></a>Problemas conhecidos (após a instalação)
Seguem-se após a instalação problemas conhecidos para esta versão de compilação.

### <a name="portal"></a>Portal  
- <!-- 2551834 - IS, ASDK --> Quando seleciona **descrição geral** para uma conta de armazenamento em portais do administrador ou utilizador, as informações a partir do *Essentials* não apresentar o painel.  Painel Essentials apresenta informações sobre a conta, como o respetivo *grupo de recursos*, *localização*, e *ID de subscrição*.  Outras opções de descrição geral são acessíveis, como *serviços* e *monitorização*, bem como opções para *abrir no Explorador de* ou *eliminar conta de armazenamento* .

  Para ver as informações disponíveis, utilize o [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) cmdlet do PowerShell.

- <!-- 2551834 - IS, ASDK --> Quando seleciona **etiquetas** para uma conta de armazenamento em portais do administrador ou utilizador, as informações de falha ao carregar e não apresenta.  

  Para ver as informações disponíveis, utilize o [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) cmdlet do PowerShell. 

- <!-- TBD - IS ASDK --> Alguns tipos de subscrição administrativos não estão disponíveis.  Quando atualizar o Azure Stack para esta versão, os tipos de duas subscrição que eram [introduzido com a versão 1804](azure-stack-update-1804.md#new-features) não estão visíveis na consola do. Isto era esperado. São os tipos de subscrição indisponível *medição subscrição*, e *subscrição de consumo*. Esses tipos de subscrição são visíveis no novos ambientes do Azure Stack a partir da versão 1804, mas ainda não estão prontos a utilizar. Deve continuar a utilizar o *fornecedor predefinido* tipo de subscrição.  

- <!-- 2403291 - IS ASDK --> Poderá não ter o uso da barra de deslocamento horizontal na parte inferior de portais de administrador e utilizador. Se não conseguir aceder a barra de deslocamento horizontal, utilize a estrutura de ligações para navegar até um painel anterior no portal ao selecionar o nome do painel pretende visualizar na lista de trilha encontrado na parte superior esquerda do portal.
  ![Navegação estrutural](media/azure-stack-update-1804/breadcrumb.png)

- <!-- TBD - IS --> Pode não ser possível ver os recursos de computação ou o armazenamento no portal do administrador. A causa deste problema é um erro durante a instalação da atualização que faz com que a atualização para ser comunicadas incorretamente conclusão com êxito. Se este problema ocorrer, contacte o suporte técnico da Microsoft para obter assistência.

- <!-- TBD - IS --> Poderá ver um dashboard em branco no portal. Para recuperar o dashboard, selecione o ícone de engrenagem no canto superior direito do portal e, em seguida, selecione **restaurar predefinições**.

- <!-- TBD - IS ASDK --> A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos inteiro e, em seguida, eliminar subscrições de utilizador.

- <!-- TBD - IS ASDK --> Não é possível ver as permissões à sua subscrição a utilizar os portais do Azure Stack. Como solução, utilize o PowerShell para verificar as permissões.


### <a name="health-and-monitoring"></a>Estado de funcionamento e monitorização
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

  Ambos os alertas podem ser ignoradas com segurança e vai ser fechado automaticamente ao longo do tempo.  

- <!-- 2368581 - IS. ASDK --> Um operador do Azure Stack, se receber um alerta de pouca memória e máquinas de virtuais de inquilino não for possível implementar com um *erro de criação de recursos de infraestrutura de VM*, é possível que o carimbo de data / Azure Stack está disponível memória esgotada. Utilize o [Planeador de capacidade do Azure Stack](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) para melhor compreender a capacidade disponível para as cargas de trabalho.


### <a name="compute"></a>Computação
- <!-- 2724873 - IS --> Ao utilizar os cmdlets do PowerShell **Start-AzsScaleUnitNode** ou **Stop-AzsScaleunitNode** para gerir unidades de escala, a primeira tentativa para iniciar ou parar a unidade de escala poderá falhar. Se o cmdlet falhar na primeira execução, execute o cmdlet uma segunda vez. A segunda execução deve ser bem-sucedida para concluir a operação. 

- <!-- 2494144 - IS, ASDK --> Ao selecionar um tamanho de máquina virtual para uma implementação da máquina virtual, alguns tamanhos de VM da série F não estão visíveis como parte do Seletor de tamanho, ao criar uma VM. Os seguintes tamanhos VM não aparecem no Seletor de: *F8s_v2*, *F16s_v2*, *F32s_v2*, e *F64s_v2*.  
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

  Em seguida, pode repetir a transferência da imagem VM que falhou anteriormente.

- <!-- TBD - IS ASDK --> Se uma extensão numa implantação de VM o aprovisionamento demora demasiado tempo, os utilizadores devem permitir que o limite de tempo de aprovisionamento em vez de tentar interromper o processo para desaloque ou elimine a VM.  

- <!-- 1662991 IS ASDK --> Diagnóstico de VM do Linux não é suportado no Azure Stack. Quando implementa uma VM do Linux com o diagnóstico VM ativado, a implementação falhar. A implementação falhar, também, se ativar as métricas básicas de VM do Linux através das definições de diagnóstico.  


### <a name="networking"></a>Redes  

- <!-- 1766332 - IS ASDK --> Sob **Networking**, se clicar em **criar Gateway de VPN** para configurar uma ligação de VPN, **baseado em política** está listado como um tipo de VPN. Não selecione esta opção. Apenas os **rota com base** opção é suportada no Azure Stack.

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
> <!-- TBD - IS --> Depois de atualizar para esta versão do Azure Stack, pode continuar a utilizar os fornecedores de recursos do SQL e o MySQL que implementou anteriormente.  Recomendamos que Atualize para o SQL e o MySQL quando uma nova versão estiver disponível. Como o Azure Stack, aplica atualizações de fornecedores de recursos do SQL e o MySQL sequencialmente. Por exemplo, se utilizar a versão 1804, aplicar primeiro a versão 1805 e, em seguida, atualize para 1807.      
>   
> A instalação desta atualização não afeta a utilização atual de fornecedores de recursos SQL ou MySQL pelos seus utilizadores.
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
Pode transferir o pacote de atualização do Azure Stack 1807 partir [aqui](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Consulte também
- Para utilizar o ponto final com privilégios (PEP) para monitorizar e retomar as atualizações, consulte [monitorizar atualizações no Azure Stack, utilizando o ponto final com privilégios](azure-stack-monitor-update.md).
- Para uma descrição geral da gestão de atualizações no Azure Stack, veja [gerir atualizações na descrição geral do Azure Stack](azure-stack-updates.md).
- Para obter mais informações sobre como aplicar as atualizações com o Azure Stack, veja [aplicar atualizações no Azure Stack](azure-stack-apply-updates.md).
