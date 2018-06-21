---
title: Notas de versão do Kit de desenvolvimento de pilha do Microsoft Azure | Microsoft Docs
description: Melhoramentos, correções e problemas conhecidos do Kit de desenvolvimento de pilha do Azure.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: brenduns
ms.reviewer: misainat
ms.openlocfilehash: bbd9bb0d56dd61fd0a32531ac425a1dbc1aa8923
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295792"
---
# <a name="azure-stack-development-kit-release-notes"></a>Notas de versão do Kit de desenvolvimento de pilha do Azure  
Estas notas de versão fornecem informações sobre os melhoramentos, correções e problemas conhecidos no Kit de desenvolvimento de pilha do Azure. Se não tiver a certeza de qual é a versão que está a executar, pode [utilizar o portal para verificar](.\.\azure-stack-updates.md#determine-the-current-version).

> Se mantêm atualizados com Novidades o ASDK ao subscrever o [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-11805147"></a>Compilação 1.1805.1.47

> [!TIP]  
> Com base nos comentários de clientes, não há uma atualização para o esquema versão utilizada para a pilha do Microsoft Azure. Começando com esta atualização, 1805, o novo esquema representa melhor a versão atual da nuvem.  
> 
> O esquema de versão é agora *Version.YearYearMonthMonth.MinorVersion.BuildNumber* onde os conjuntos de segundo e terceiro indicam a versão e a versão. Por exemplo, 1805.1 representa o *lançamento para fabrico* versão (RTM) do 1805.  


### <a name="new-features"></a>Novas funcionalidades 
Este compilação inclui as seguintes melhorias e correções para pilha do Azure.  

- <!-- 2297790 - IS, ASDK --> **Pilha do Azure inclui agora um *Syslog* cliente** como um *funcionalidade de pré-visualização*. Este tipo de cliente permite que o reencaminhamento de registos de auditoria e segurança relacionados com a infraestrutura de pilha do Azure para um Syslog segurança ou de servidor de informações e eventos management (SIEM) software que é externo à pilha do Azure. Atualmente, o cliente de Syslog só suporta ligações não autenticadas de UDP através da porta predefinida 514. O payload de cada mensagem do Syslog é formatado em comum evento formato (CEF). 

  Para configurar o cliente de Syslog, utilize o **conjunto SyslogServer** cmdlet exposto no ponto final com privilégios. 

  Com esta pré-visualização, poderá ver os seguintes três alertas. Quando apresentado pela pilha do Azure, estes alertas incluem *descrições* e *remediação* orientações. 
  - Título: Integridade do código desativado  
  - Título: Integridade do código no modo de auditoria 
  - Título: Conta de utilizador criada

  Enquanto esta funcionalidade está em pré-visualização, este não deve ser confiada em ambientes de produção.   


### <a name="fixed-issues"></a>Problemas fixos
- Iremos corrigir o problema que bloqueados [abrir um novo pedido de suporte na lista pendente](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) de dentro do portal de administração do. Esta opção agora funciona conforme pretendido. 

- **Várias correções** para desempenho, estabilidade, segurança e o sistema operativo que é utilizado pela pilha de Azure


<!-- ### Changes  --> 


<!--   ### Additional releases timed with this update  -->


### <a name="known-issues"></a>Problemas conhecidos
 
#### <a name="portal"></a>Portal
- <!-- 2551834 - IS, ASDK --> Quando seleciona **descrição geral** para uma conta de armazenamento em portais de administrador ou utilizador, as informações do *Essentials* não apresentar o painel.  O painel de Essentials apresenta informações sobre a conta como o respetivo *grupo de recursos*, *localização*, e *ID de subscrição*.  Outras opções para descrição geral estão acessíveis, como *serviços* e *monitorização*, bem como opções para *aberta no Explorador de* ou *eliminar conta do storage* .  

  Para ver as informações disponíveis, utilize o [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0) cmdlet do PowerShell. 

- <!-- 2551834 - IS, ASDK --> Quando seleciona **etiquetas** para uma conta de armazenamento em portais de administrador ou utilizador, as informações não conseguir carregar e não apresenta.  

  Para ver as informações disponíveis, utilize o [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0) cmdlet do PowerShell.

- <!-- TBD - IS ASDK --> Não utilize os novos tipos de subscrição administrativas de *medição subscrição*, e *subscrição consumo*. Estes novos tipos de subscrição foram introduzidos com versão 1804 mas ainda não estão prontos para utilização. Deve continuar a utilizar o *fornecedor predefinido* tipo de subscrição.  

- <!-- 2403291 - IS ASDK --> Poderá não ter a utilização da barra de deslocamento horizontal ao longo da parte inferior dos portais de administrador e utilizador. Se não conseguir aceder a barra de deslocamento horizontal, utilize a navegação estrutural para navegar para um painel anterior no portal, selecionando o nome do painel pretende ver a lista de trilho encontrado na parte superior esquerda do portal.
  ![Trilho](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos completo e, em seguida, eliminar subscrições de utilizador.

- <!-- TBD -  IS ASDK --> Não é possível ver as permissões para a sua subscrição utilizando os portais de pilha do Azure. Como solução, utilize o PowerShell para verificar permissões.


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

  Ambos os alertas podem ser ignoradas e serão fechada automaticamente ao longo do tempo.  

- <!-- 2392907 – ASDK -->   Poderá ver um *críticos* alerta **baixa capacidade da memória**. Este alerta com a descrição seguinte: *a região consumiu mais do que 95.00% de memória disponível. A criação de máquinas virtuais com grandes quantidades de memória-la poderá falhar.*

  Este alerta pode ser gerado quando a pilha de Azure incorretamente contas para utilização de memória no kit de desenvolvimento de pilha do Azure.  

  Este alerta pode ser ignorado e o problema não tem qualquer efeito no posicionamento das máquinas virtuais. 

- <!-- 2368581 - IS. ASDK --> Um operador de pilha do Azure, se receber um alerta de pouca memória e máquinas virtuais inquilinas não conseguir implementar com um *erro de criação de VM de recursos de infraestrutura*, é possível que o carimbo de pilha do Azure está disponível memória esgotada. Utilize o [Planne de capacidade de pilha do Azure](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) para melhor compreender a capacidade disponível para as cargas de trabalho. 


#### <a name="compute"></a>Computação
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


- <!-- TBD -  IS ASDK --> Definições de dimensionamento para conjuntos de dimensionamento de máquina virtual não estão disponíveis no portal. Como solução, pode utilizar [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro em vez de `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Ao criar máquinas virtuais no portal de utilizador de pilha do Azure, o portal apresenta um número incorreto de discos de dados que pode anexar uma série de D VM. Todas as séries de D suportadas VMs podem acomodar tantos discos de dados como a configuração do Azure.

- <!-- TBD -  IS ASDK --> Quando não for possível criar uma imagem de VM, um item de falha que não é possível eliminar pode ser adicionado ao painel de processamento de imagens de VM.

  Como solução, crie uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (novo VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item da falha. Em seguida, 15 minutos depois de criar a imagem fictício, que pode com êxito eliminá-lo.

  Em seguida, pode tentar transfira novamente a imagem VM que tenha falhado anteriormente.

- <!-- TBD -  IS ASDK --> Se uma extensão numa implementação de VM de aprovisionamento demora demasiado tempo, os utilizadores devem permitir o aprovisionamento do limite de tempo em vez de tentar interromper o processo de Desalocação ou eliminar a VM.  

- <!-- 1662991 - IS ASDK --> Diagnóstico de VM com Linux não é suportado na pilha do Azure. Quando implementa uma VM com Linux com diagnósticos da VM ativados, a implementação falhará. A implementação falha também se ativar as métricas de básicas da VM com Linux através das definições de diagnóstico. 

#### <a name="networking"></a>Redes
- <!-- TBD - IS ASDK --> Não é possível criar rotas definidas pelo utilizador no portal do administrador ou utilizador. Como solução, utilize [Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell).

- <!-- 1766332 - IS, ASDK --> Em **redes**, se clicar em **criar Gateway de VPN** para configurar uma ligação VPN, **baseado em política** está listado como um tipo de VPN. Não selecione esta opção. Apenas o **baseada na rota** opção é suportada na pilha do Azure.

- <!-- 2388980 -  IS ASDK --> Depois de uma VM é criada e associada a um endereço IP público, não é possível desassociar essa VM a partir desse endereço IP. Desassociação aparece funcionar, mas o endereço IP público anteriormente atribuído permanece associado a VM original.

  Atualmente, tem de utilizar apenas novos endereços IP públicos para novas VMs que cria.

  Este comportamento ocorre mesmo reatribuir o endereço IP para uma nova VM (normalmente denominado como um *alternância de VIP*). Todas as futuras tenta estabelecer ligação através deste resultado de endereço IP numa ligação para a VM original e não para a nova.


- <!-- 2292271 - IS ASDK --> Se aumentar o limite de Quota para um recurso de rede que faça parte de uma oferta e o plano que está associado uma subscrição de inquilino, o novo limite não se aplica a essa subscrição. No entanto, o novo limite é aplicável a novas subscrições que são criadas após a quota é aumentada. 

  Para contornar este problema, utilize um plano de suplemento para aumentar uma Quota de rede quando o plano já está associado uma subscrição. Para obter mais informações, consulte como [disponibilizar um plano de suplemento](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> Não é possível eliminar uma subscrição com recursos de zona DNS ou recursos de tabela de rotas associados. Para eliminar com êxito a subscrição, tem de eliminar os recursos de zona DNS e a tabela de rota da subscrição de inquilino. 


- <!-- 1902460 -  IS ASDK --> Pilha do Azure suporta um único *gateway de rede local* por endereço IP. Isto é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local gateway ligação, posterior tentativas criar um recurso de gateway de rede local com o mesmo endereço IP estão bloqueadas.

- <!-- 16309153 -  IS ASDK --> Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a alteração a uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas por push para VMs nessa Vnet.
 
- <!-- TBD -  IS ASDK --> Pilha do Azure não suporta a adição de interfaces de rede adicionais para uma instância VM após a implementação da VM. Se a VM precisar de mais de uma interface de rede, tem de ser definidos no momento da implementação.


#### <a name="sql-and-mysql"></a>SQL Server e o MySQL 
- <!-- TBD - ASDK --> A base de dados que aloja os servidores têm de estar dedicada para utilização pelas cargas de trabalho do utilizador e o fornecedor de recursos. Não é possível utilizar uma instância que está a ser utilizada por outros consumidores, incluindo os serviços de aplicação.

- <!-- IS, ASDK --> Os carateres especiais, incluindo espaços e períodos, não são suportados no **família** nome quando cria um SKU para os fornecedores de recursos do SQL Server e o MySQL. 

#### <a name="app-service"></a>Serviço de Aplicações
- <!-- 2352906 - IS ASDK --> Os utilizadores tem de registar o fornecedor de recursos de armazenamento antes de poderem criarem a sua primeira função do Azure na subscrição.

- <!-- TBD - IS ASDK --> Para ampliar a infraestrutura (workers, gestão, funções de front-end), tem de utilizar o PowerShell conforme descrito nas notas de versão da computação.  

- <!-- TBD - IS ASDK --> Serviço de aplicações só pode ser implementado para o *subscrição do fornecedor predefinido* neste momento. Numa atualização futura, o serviço de aplicações irá implementar para a nova *medição subscrição* que foi introduzida no 1804 de pilha do Azure. Quando medição é suportada para utilização, todas as implementações existentes serão migradas para este novo tipo de subscrição.

#### <a name="usage"></a>Utilização  
- <!-- TBD -  IS ASDK --> Dados de medição de utilização de endereços de IP público de utilização mostram o mesmo *EventDateTime* valor para cada registo em vez do *TimeDate* carimbo que mostra que o registo foi criado. Atualmente, não é possível utilizar estes dados para efetuar exata contabilização da utilização de endereços IP pública.

<!-- #### Identity -->



## <a name="build-201805131"></a>Compilação 20180513.1

### <a name="new-features"></a>Novas funcionalidades 
Este compilação inclui as seguintes melhorias e correções para pilha do Azure.  

- <!-- 1759172 - IS, ASDK --> **Novas subscrições administrativas**. Com 1804 existem dois novos tipos de subscrição disponíveis no portal. Estes novos tipos de subscrição são além da subscrição do fornecedor predefinida e visíveis com novas instalações de pilha do Azure a partir da versão 1804. *Não utilize estes novos tipos de subscrição com esta versão do Azure pilha*. Iremos irá anunciar a disponibilidade para utilizar estes tipos de subscrição com a atualização futura. 

  Estes novos tipos de subscrição são visíveis, mas a parte de uma alteração maior para proteger a subscrição do fornecedor predefinida e torna mais fácil de implementar recursos partilhados, como servidores de alojamento do SQL Server. 

  Os tipos de subscrição de três agora disponíveis são:  
  - Predefinido a subscrição do fornecedor: continuar a utilizar este tipo de subscrição. 
  - Medição de subscrição: *não utilize este tipo de subscrição.*
  - Subscrição de consumo: *não utilize este tipo de subscrição*

### <a name="fixed-issues"></a>Problemas fixos
- <!-- IS, ASDK -->  No portal de administração, já não terá de atualizar o mosaico de atualização antes de apresenta informações. 

- <!-- 2050709 - IS, ASDK -->  Agora, pode utilizar o portal de administração para editar as métricas do storage para o serviço Blob, o serviço tabela e o serviço de fila.

- <!-- IS, ASDK --> Em **redes**, ao clicar em **ligação** para configurar uma ligação VPN, **Site a site (IPsec)** agora é a única opção disponível. 

- **Várias correções** para desempenho, estabilidade, segurança e o sistema operativo que é utilizado pela pilha de Azure

<!-- ### Changes  --> 
### <a name="additional-releases-timed-with-this-update"></a>Versões adicionais com esta atualização excedeu o tempo limite  
Os seguintes estão agora disponíveis, mas não necessitam de atualização de pilha do Azure 1804.
- **Atualizar para o pacote de monitorização do Azure pilha do Microsoft System Center Operations Manager**. Uma nova versão (1.0.3.0) o Microsoft System Center Operations Manager monitorização Pack de pilha do Azure está disponível para [transferir](https://www.microsoft.com/download/details.aspx?id=55184). Com esta versão, pode utilizar principais de serviço ao adicionar uma implementação de pilha do Azure ligada. Esta versão inclui também uma experiência de gestão de atualizações que lhe permite efetuar a ação de remediação diretamente a partir do Operations Manager. Também existem novos dashboards que apresentam fornecedores de recursos, unidades de escala e nós de unidade de escala.

- **Novo Azure da pilha administração PowerShell versão 1.3.0**.  O Azure PowerShell pilha 1.3.0 está agora disponível para instalação. Esta versão fornece comandos para todos os fornecedores de recursos de administrador gerir a pilha do Azure.  Com esta versão, algum conteúdo vai ser preterido a partir do GitHub de ferramentas de pilha do Azure [repositório](https://github.com/Azure/AzureStack-Tools). 

   Para obter detalhes de instalação, siga o [instruções](.\.\azure-stack-powershell-install.md) ou [ajudar](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) conteúdo para o módulo de pilha do Azure 1.3.0. 

- **Inicial de versão de referência da API Rest do Azure pilha**. O [referência da API para todos os fornecedores de recursos do Azure pilha Admin](https://docs.microsoft.com/rest/api/azure-stack/) agora é publicado. 

### <a name="known-issues"></a>Problemas conhecidos
 
#### <a name="portal"></a>Portal
- <!-- TBD - IS ASDK --> A capacidade [para abrir um novo pedido de suporte na lista pendente](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) de dentro do administrador do portal não está disponível. Em vez disso, utilize a seguinte hiperligação:     
    - Para o Kit de desenvolvimento de pilha do Azure, utilize https://aka.ms/azurestackforum.    

- <!-- 2403291 - IS ASDK --> Poderá não ter a utilização da barra de deslocamento horizontal ao longo da parte inferior dos portais de administrador e utilizador. Se não conseguir aceder a barra de deslocamento horizontal, utilize a navegação estrutural para navegar para um painel anterior no portal, selecionando o nome do painel pretende ver a lista de trilho encontrado na parte superior esquerda do portal.
  ![Trilho](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos completo e, em seguida, eliminar subscrições de utilizador.

- <!-- TBD -  IS ASDK --> Não é possível ver as permissões para a sua subscrição utilizando os portais de pilha do Azure. Como solução, utilize o PowerShell para verificar permissões.

-   <!-- TBD -  IS ASDK --> No portal de administração, poderá ver um alerta crítico para o componente de Microsoft.Update.Admin. O nome do alerta, a descrição e remediação todos são apresentadas como:  
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
- <!-- TBD -  IS ASDK --> Definições de dimensionamento para conjuntos de dimensionamento de máquina virtual não estão disponíveis no portal. Como solução, pode utilizar [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro em vez de `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Ao criar máquinas virtuais no portal de utilizador de pilha do Azure, o portal apresenta um número incorreto de discos de dados que pode anexar a uma série DS VM. Série DS VMs pode acomodar tantos discos de dados como a configuração do Azure.

- <!-- TBD -  IS ASDK --> Quando não for possível criar uma imagem de VM, um item de falha que não é possível eliminar pode ser adicionado ao painel de processamento de imagens de VM.

  Como solução, crie uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (novo VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item da falha. Em seguida, 15 minutos depois de criar a imagem fictício, que pode com êxito eliminá-lo.

  Em seguida, pode tentar transfira novamente a imagem VM que tenha falhado anteriormente.

- <!-- TBD -  IS ASDK --> Se uma extensão numa implementação de VM de aprovisionamento demora demasiado tempo, os utilizadores devem permitir o aprovisionamento do limite de tempo em vez de tentar interromper o processo de Desalocação ou eliminar a VM.  

- <!-- 1662991 - IS ASDK --> Diagnóstico de VM com Linux não é suportado na pilha do Azure. Quando implementa uma VM com Linux com diagnósticos da VM ativados, a implementação falhará. A implementação falha também se ativar as métricas de básicas da VM com Linux através das definições de diagnóstico. 

#### <a name="networking"></a>Redes
- <!-- 1766332 - IS, ASDK --> Em **redes**, se clicar em **criar Gateway de VPN** para configurar uma ligação VPN, **baseado em política** está listado como um tipo de VPN. Não selecione esta opção. Apenas o **baseada na rota** opção é suportada na pilha do Azure.

- <!-- 2388980 -  IS ASDK --> Depois de uma VM é criada e associada a um endereço IP público, não é possível desassociar essa VM a partir desse endereço IP. Desassociação aparece funcionar, mas o endereço IP público anteriormente atribuído permanece associado a VM original.

  Atualmente, tem de utilizar apenas novos endereços IP públicos para novas VMs que cria.

  Este comportamento ocorre mesmo reatribuir o endereço IP para uma nova VM (normalmente denominado como um *alternância de VIP*). Todas as futuras tenta estabelecer ligação através deste resultado de endereço IP numa ligação para a VM original e não para a nova.

- <!-- 2292271 - IS ASDK --> Se aumentar o limite de Quota para um recurso de rede que faça parte de uma oferta e o plano que está associado uma subscrição de inquilino, o novo limite não se aplica a essa subscrição. No entanto, o novo limite é aplicável a novas subscrições que são criadas após a quota é aumentada. 

  Para contornar este problema, utilize um plano de suplemento para aumentar uma Quota de rede quando o plano já está associado uma subscrição. Para obter mais informações, consulte como [disponibilizar um plano de suplemento](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> Não é possível eliminar uma subscrição com recursos de zona DNS ou recursos de tabela de rotas associados. Para eliminar com êxito a subscrição, tem de eliminar os recursos de zona DNS e a tabela de rota da subscrição de inquilino. 


- <!-- 1902460 -  IS ASDK --> Pilha do Azure suporta um único *gateway de rede local* por endereço IP. Isto é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local gateway ligação, posterior tentativas criar um recurso de gateway de rede local com o mesmo endereço IP estão bloqueadas.

- <!-- 16309153 -  IS ASDK --> Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a alteração a uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas por push para VMs nessa Vnet.
 
- <!-- TBD -  IS ASDK --> Pilha do Azure não suporta a adição de interfaces de rede adicionais para uma instância VM após a implementação da VM. Se a VM precisar de mais de uma interface de rede, tem de ser definidos no momento da implementação.


#### <a name="sql-and-mysql"></a>SQL Server e o MySQL 
- <!-- TBD - ASDK --> A base de dados que aloja os servidores têm de estar dedicada para utilização pelas cargas de trabalho do utilizador e o fornecedor de recursos. Não é possível utilizar uma instância que está a ser utilizada por outros consumidores, incluindo os serviços de aplicação.

- <!-- IS, ASDK --> Os carateres especiais, incluindo espaços e períodos, não são suportados no **família** nome quando cria um SKU para os fornecedores de recursos do SQL Server e o MySQL. 

#### <a name="app-service"></a>Serviço de Aplicações
- <!-- TBD -  IS ASDK --> Os utilizadores tem de registar o fornecedor de recursos de armazenamento antes de poderem criarem a sua primeira função do Azure na subscrição.

- <!-- TBD -  IS ASDK --> Para ampliar a infraestrutura (workers, gestão, funções de front-end), tem de utilizar o PowerShell conforme descrito nas notas de versão da computação.
 
#### <a name="usage"></a>Utilização  
- <!-- TBD -  IS ASDK --> Dados de medição de utilização de endereços de IP público de utilização mostram o mesmo *EventDateTime* valor para cada registo em vez do *TimeDate* carimbo que mostra que o registo foi criado. Atualmente, não é possível utilizar estes dados para efetuar exata contabilização da utilização de endereços IP pública.

<!--
#### Identity
-->



#### <a name="downloading-azure-stack-tools-from-github"></a>Transferir as ferramentas de pilha do Azure a partir do GitHub
- Ao utilizar o *webrequest invocar* das ferramentas de cmdlet do PowerShell para transferir a pilha do Azure a partir do Github, receberá um erro:     
    -  *webrequest invocar: O pedido foi abortado: não foi possível criar o canal seguro SSL/TLS.*     

  Este erro ocorre devido a uma descontinuação de suporte do GitHub recente das Tlsv1 Tlsv1.1 criptográficas normas e (a predefinição para o PowerShell). Para obter mais informações, consulte [avisos de remoção de padrões de criptografia fraca](https://githubengineering.com/crypto-removal-notice/).

<!-- #### Identity -->



## <a name="build-201803021"></a>Compilação 20180302.1

### <a name="new-features-and-fixes"></a>Novas funcionalidades e correções
As novas funcionalidades e correções lançadas para pilha do Azure versão sistemas integrada 1803 aplicam-se para o Kit de desenvolvimento de pilha do Azure. Consulte o [novas funcionalidades](.\.\azure-stack-update-1803.md#new-features) e [fixo problemas](.\.\azure-stack-update-1803.md#fixed-issues) notas de versão para obter detalhes de atualização de secções de 1803 de pilha do Azure.  
> [!IMPORTANT]    
> Alguns dos itens listados no **novas funcionalidades** e **fixo problemas** secções são relevantes apenas para os sistemas de pilha do Azure integrado.

### <a name="changes"></a>Alterações
- A forma de alteração do Estado de uma oferta recentemente criado a partir da *privada* para *pública* ou *desativado* foi alterada. Para obter mais informações, consulte [criar uma oferta](.\.\azure-stack-create-offer.md). 


### <a name="known-issues"></a>Problemas conhecidos
 
#### <a name="portal"></a>Portal
- A capacidade [para abrir um novo pedido de suporte na lista pendente](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) de dentro do administrador do portal não está disponível. Em vez disso, utilize a seguinte hiperligação:     
    - Para o Kit de desenvolvimento de pilha do Azure, utilize https://aka.ms/azurestackforum.    

- <!-- 2050709 --> No portal de administração, não é possível editar as métricas do storage para o serviço Blob, tabela serviço ou serviço fila. Quando aceder ao armazenamento e, em seguida, selecione o blob, tabela ou mosaico do serviço de fila, um novo painel abre-se que apresenta um gráfico de métricas para esse serviço. Se, em seguida, selecione Editar da parte superior do mosaico de gráfico de métricas, o painel de editar gráfico abre, mas não apresenta as opções para editar as métricas.  

- Verá uma **ativação necessária** alerta de aviso que indica a registar o Kit de desenvolvimento de pilha do Azure. Este comportamento é esperado.

- A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos completo e, em seguida, eliminar subscrições de utilizador.

- Não é possível ver as permissões para a sua subscrição utilizando os portais de pilha do Azure. Como solução, utilize o PowerShell para verificar permissões.

- No dashboard do portal de administração, o mosaico de atualização não consegue apresentar informações sobre atualizações. Para resolver este problema, clique no mosaico atualizá-lo.

-   No portal de administração, poderá ver um alerta crítico para o componente de Microsoft.Update.Admin. O nome do alerta, a descrição e remediação todos são apresentadas como:  
    - *ERRO - o modelo para o FaultType ResourceProviderTimeout está em falta.*

    Este alerta pode ser ignorado com segurança. 

- No portal de administração tanto portal de utilizador, o painel de descrição geral não é possível carregar quando seleciona o painel de descrição geral de contas de armazenamento que foram criadas com uma versão de API antiga (exemplo: 2015-06-15). 

  Como solução, utilize o PowerShell para executar o **início ResourceSynchronization.ps1** script para restaurar o acesso para os detalhes da conta de armazenamento. [O script está disponível a partir do GitHub]( https://github.com/Azure/AzureStack-Tools/tree/master/Support/scripts)e deve ser executado com credenciais de administrador de serviço no anfitrião do kit de desenvolvimento se utilizar o ASDK.  

- O **estado de funcionamento do serviço** painel Falha ao carregar. Quando abrir o painel de estado de funcionamento do serviço no portal de administrador ou utilizador, a pilha de Azure apresenta um erro e não carregar informações. Este comportamento está previsto. Apesar de poder selecione e abra o estado de funcionamento do serviço, esta funcionalidade ainda não está disponível, mas irá ser implementada numa versão futura da pilha do Azure.


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

- No portal de administração de pilha do Azure, poderá ver um alerta crítico com o nome **pendentes expiração de certificado externo**.  Este alerta pode ser ignorado com segurança e afetem as operações do Kit de desenvolvimento de pilha do Azure. 


#### <a name="marketplace"></a>Marketplace
- Os utilizadores podem procurar o mercado completo sem uma subscrição e podem ver itens administrativos como planos e ofertas. Estes itens estão não funcional para os utilizadores.
 
#### <a name="compute"></a>Computação
- Definições de dimensionamento para conjuntos de dimensionamento de máquina virtual não estão disponíveis no portal. Como solução, pode utilizar [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro em vez de `-VMScaleSetName`.

- Ao criar máquinas virtuais no portal de utilizador de pilha do Azure, o portal apresenta um número incorreto de discos de dados que pode anexar a uma série DS VM. Série DS VMs pode acomodar tantos discos de dados como a configuração do Azure.

- Quando não for possível criar uma imagem de VM, um item de falha que não é possível eliminar pode ser adicionado ao painel de processamento de imagens de VM.

  Como solução, crie uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (novo VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item da falha. Em seguida, 15 minutos depois de criar a imagem fictício, que pode com êxito eliminá-lo.

  Em seguida, pode tentar transfira novamente a imagem VM que tenha falhado anteriormente.

-  Se uma extensão numa implementação de VM de aprovisionamento demora demasiado tempo, os utilizadores devem permitir o aprovisionamento do limite de tempo em vez de tentar interromper o processo de Desalocação ou eliminar a VM.  

- <!-- 1662991 --> Diagnóstico de VM com Linux não é suportado na pilha do Azure. Quando implementa uma VM com Linux com diagnósticos da VM ativados, a implementação falhará. A implementação falha também se ativar as métricas de básicas da VM com Linux através das definições de diagnóstico. 


#### <a name="networking"></a>Redes
- Em **redes**, se clicar em **ligação** para configurar uma ligação VPN, **VNet a VNet** está listado como um tipo de ligação possíveis. Não selecione esta opção. Atualmente, apenas o **Site a site (IPsec)** opção é suportada.

- Depois de uma VM é criada e associada a um endereço IP público, não é possível desassociar essa VM a partir desse endereço IP. Desassociação aparece funcionar, mas o endereço IP público anteriormente atribuído permanece associado a VM original.

  Atualmente, tem de utilizar apenas novos endereços IP públicos para novas VMs que cria.

  Este comportamento ocorre mesmo reatribuir o endereço IP para uma nova VM (normalmente denominado como um *alternância de VIP*). Todas as futuras tenta estabelecer ligação através deste resultado de endereço IP numa ligação para a VM original e não para a nova.



- Pilha do Azure suporta um único *gateway de rede local* por endereço IP. Isto é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local gateway ligação, posterior tentativas criar um recurso de gateway de rede local com o mesmo endereço IP estão bloqueadas.

- Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a alteração a uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas por push para VMs nessa Vnet.
 
- Pilha do Azure não suporta a adição de interfaces de rede adicionais para uma instância VM após a implementação da VM. Se a VM precisar de mais de uma interface de rede, tem de ser definidos no momento da implementação.



#### <a name="sql-and-mysql"></a>SQL Server e o MySQL 
- Pode demorar até uma hora antes dos utilizadores podem criar bases de dados num novo SQL Server ou MySQL SKU.

- A base de dados que aloja os servidores têm de estar dedicada para utilização pelas cargas de trabalho do utilizador e o fornecedor de recursos. Não é possível utilizar uma instância que está a ser utilizada por outros consumidores, incluindo os serviços de aplicação.

- <!-- IS, ASDK --> Os carateres especiais, incluindo espaços e períodos, não são suportados no **família** ou **camada** nomes quando cria um SKU para os fornecedores de recursos do SQL Server e o MySQL.

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





