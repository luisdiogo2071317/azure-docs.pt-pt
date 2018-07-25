---
title: Notas de versão do Kit de desenvolvimento do Microsoft Azure Stack | Documentos da Microsoft
description: Melhorias, correções e problemas conhecidos ao Kit de desenvolvimento do Azure Stack.
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
ms.date: 06/04/2018
ms.author: brenduns
ms.reviewer: misainat
ms.openlocfilehash: d86f0e96f64a56644b3161725f7f6a430568a72f
ms.sourcegitcommit: d76d9e9d7749849f098b17712f5e327a76f8b95c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242704"
---
# <a name="azure-stack-development-kit-release-notes"></a>Notas de versão do Azure Stack Development Kit
Estas notas de versão fornecem informações sobre problemas conhecidos no Kit de desenvolvimento do Azure Stack, correções e melhorias. Se não tiver a certeza qual é a versão que está a executar, pode [utilizar o portal para verificar](.\.\azure-stack-updates.md#determine-the-current-version).

> Mantenha-se atualizado com o que há de novo no ASDK inscrevendo-se para o [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-11805142"></a>Compilação 1.1805.1.42

<!-- ### New features 
This build includes the following improvements and fixes for Azure Stack.  
-->


### <a name="fixed-issues"></a>Problemas de fixos

- **Várias correções** de desempenho, estabilidade, segurança e o sistema operativo que é utilizado pelo Azure Stack


<!-- ### Changes  --> 


<!--   ### Additional releases timed with this update  -->


### <a name="known-issues"></a>Problemas conhecidos
 
#### <a name="portal"></a>Portal
- <!-- TBD - IS ASDK --> Não utilize os novos tipos de subscrição administrativos do *medição subscrição*, e *subscrição de consumo*. Esses novos tipos de subscrição foram introduzidos com versão 1804, mas ainda não estão prontos a utilizar. Deve continuar a utilizar o *fornecedor predefinido* tipo de subscrição.  

- <!-- 2403291 - IS ASDK --> Poderá não ter o uso da barra de deslocamento horizontal na parte inferior de portais de administrador e utilizador. Se não conseguir aceder a barra de deslocamento horizontal, utilize a estrutura de ligações para navegar até um painel anterior no portal ao selecionar o nome do painel pretende visualizar na lista de trilha encontrado na parte superior esquerda do portal.
  ![Navegação estrutural](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos inteiro e, em seguida, eliminar subscrições de utilizador.

- <!-- TBD -  IS ASDK --> Não é possível ver as permissões à sua subscrição a utilizar os portais do Azure Stack. Como solução, utilize o PowerShell para verificar as permissões.


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
- <!-- TBD -  IS ASDK --> Definições de dimensionamento para conjuntos de dimensionamento de máquinas virtuais não estão disponíveis no portal. Como solução, pode usar [do Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro, em vez de `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Ao criar máquinas virtuais no portal de utilizador do Azure Stack, o portal apresenta um número incorreto de discos de dados que pode anexar uma VM da série D. Série de D suportada todas as VMs pode acomodar os discos de dados como a configuração do Azure.

- <!-- TBD -  IS ASDK --> Quando não for possível criar uma imagem de VM, pode ser adicionado um item com falha que não é possível eliminar para o painel de computação de imagens VM.

  Como solução, criar uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (New-VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item falhado. Em seguida, 15 minutos depois de criar a imagem fictícia, pode com êxito eliminá-la.

  Em seguida, pode tentar transfira a imagem VM que falhou anteriormente.

- <!-- TBD -  IS ASDK --> Se uma extensão numa implantação de VM o aprovisionamento demora demasiado tempo, os utilizadores devem permitir que o limite de tempo de aprovisionamento em vez de tentar interromper o processo para desaloque ou elimine a VM.  

- <!-- 1662991 - IS ASDK --> Diagnóstico de VM do Linux não é suportado no Azure Stack. Quando implementa uma VM do Linux com o diagnóstico VM ativado, a implementação falhar. A implementação falhar, também, se ativar as métricas básicas de VM do Linux através das definições de diagnóstico. 

#### <a name="networking"></a>Redes
- <!-- 1766332 - IS, ASDK --> Sob **Networking**, se clicar em **criar Gateway de VPN** para configurar uma ligação de VPN, **baseado em política** está listado como um tipo de VPN. Não selecione esta opção. Apenas os **rota com base** opção é suportada no Azure Stack.

- <!-- 2388980 -  IS ASDK --> Depois de uma VM é criada e associada a um endereço IP público, não é possível desassociar VM desse endereço IP. Desassociação parece funcionar, mas o endereço IP público atribuído anteriormente que permanece associado com a VM original.

  Atualmente, tem de utilizar apenas novos endereços IP públicos para novas VMs que criar.

  Este comportamento ocorre mesmo que a reatribuir o endereço IP a uma VM nova (geralmente conhecida como uma *alternância de VIP*). Todos os futuro tenta se conectar por meio deste resultado de endereço IP numa ligação para a VM associada originalmente e não para a nova.

- <!-- 2292271 - IS ASDK --> Se aumentar um limite de Quota para um recurso de rede que faz parte de uma oferta e o plano que está associado uma subscrição de inquilino, o novo limite não se aplica a essa subscrição. No entanto, o novo limite é aplicável às novas assinaturas que são criadas após a quota for aumentada. 

  Para contornar este problema, utilize um plano de suplemento para aumentar uma Quota de rede quando o plano já está associado uma subscrição. Para obter mais informações, consulte como [disponibilizar um plano de suplemento](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> Não é possível eliminar uma subscrição que tem recursos de zona DNS ou os recursos de tabela de rotas associados ao mesmo. Para eliminar com êxito a subscrição, tem primeiro de eliminar os recursos de zona DNS e a tabela de rotas da subscrição do inquilino. 


- <!-- 1902460 -  IS ASDK --> O Azure Stack oferece suporte a uma única *gateway de rede local* por endereço IP. Isso é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local ligação de gateway, subsequente tenta criar um recurso de gateway de rede local com o mesmo endereço IP são bloqueados.

- <!-- 16309153 -  IS ASDK --> Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a mudança para uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas para as VMs nessa vnet.
 
- <!-- TBD -  IS ASDK --> O Azure Stack não suporta a adição de interfaces de rede adicionais para uma instância VM depois de implementada a VM. Se a VM exigir mais de uma interface de rede, tem de ser definidos no momento da implementação.


#### <a name="sql-and-mysql"></a>SQL e o MySQL 
- <!-- TBD - ASDK --> Servidores a alojar a base de dados tem de estar dedicada para utilização pelas cargas de trabalho do utilizador e o fornecedor de recursos. Não é possível utilizar uma instância que está a ser utilizada por outros consumidores, incluindo serviços de aplicações.

- <!-- IS, ASDK --> Caracteres especiais, incluindo espaços e períodos, não são suportadas no **família** dê um nome ao criar um SKU para os fornecedores de recursos do SQL e o MySQL. 

#### <a name="app-service"></a>Serviço de Aplicações
- <!-- 2352906 - IS ASDK --> Os utilizadores tem de registar o fornecedor de recursos de armazenamento antes de ser criar sua primeira função do Azure na subscrição.

- <!-- TBD - IS ASDK --> Para ampliar a infraestrutura (funções de trabalho, gerenciamento, funções de front-end), tem de utilizar o PowerShell, tal como descrito nas notas de versão para computação.  

- <!-- TBD - IS ASDK --> Serviço de aplicações só pode ser implementado para o *subscrição do fornecedor predefinido* neste momento. Numa atualização futura, o serviço de aplicações irá implementar para a nova *medição subscrição* que foi introduzida em 1804 de pilha do Azure. Quando a medição é suportada para utilização, todas as implementações existentes serão migradas para este novo tipo de subscrição.

#### <a name="usage"></a>Utilização  
- <!-- TBD -  IS ASDK --> Dados de medidor de utilização de endereços de IP público de utilização mostram o mesmo *EventDateTime* valor para cada registo, em vez do *TimeDate* carimbo que mostra quando o registo foi criado. Atualmente, não pode utilizar estes dados para efetuar gestão de contas preciso de utilização de endereços IP pública.

<!-- #### Identity -->



## <a name="build-201805131"></a>Compilação 20180513.1

### <a name="new-features"></a>Novos recursos 
Esta compilação inclui as seguintes melhorias e correções para o Azure Stack.  

- <!-- 1759172 - IS, ASDK --> **Novas subscrições administrativas**. Com 1804 há dois novos tipos de subscrição disponíveis no portal. Esses novos tipos de subscrição são, além da subscrição do fornecedor predefinida e visíveis com novas instalações do Azure Stack a partir da versão 1804. *Não utilize esses novos tipos de subscrição com esta versão do Azure Stack*. Anunciaremos a disponibilidade para utilizar estes tipos de subscrição com uma atualização futura. 

  Esses novos tipos de subscrição são visíveis, mas faz parte de uma alteração maior para proteger a subscrição do fornecedor predefinida e, para que seja mais fácil de implantar recursos compartilhados, como servidores SQL que aloja. 

  Os tipos de subscrição de três agora disponíveis são:  
  - Padrão de subscrição do fornecedor: continuar a utilizar este tipo de subscrição. 
  - Medição de subscrição: *não utilize este tipo de subscrição.*
  - Subscrição de consumo: *não utilize este tipo de subscrição*

### <a name="fixed-issues"></a>Problemas de fixos
- <!-- IS, ASDK -->  No portal de administração, já não tem de atualizar o mosaico de atualização antes de apresenta informações. 

- <!-- 2050709 - IS, ASDK -->  Agora, pode utilizar o portal de administração para editar as métricas de armazenamento para o serviço Blob, serviço tabela e o serviço de fila.

- <!-- IS, ASDK --> Sob **Networking**, quando clicar em **ligação** para configurar uma ligação de VPN, **Site-site (IPsec)** agora é a única opção disponível. 

- **Várias correções** de desempenho, estabilidade, segurança e o sistema operativo que é utilizado pelo Azure Stack

<!-- ### Changes  --> 
### <a name="additional-releases-timed-with-this-update"></a>Versões adicionais excedeu o tempo limite com esta atualização  
A seguir está agora disponível, mas não precisam de atualização do Azure Stack 1804.
- **Atualizar para o pacote de monitorização do Microsoft Azure Stack System Center Operations Manager**. Uma nova versão (1.0.3.0) do Microsoft System Center Operations Manager monitorização Pack para o Azure Stack está disponível para [transferir](https://www.microsoft.com/download/details.aspx?id=55184). Com esta versão, pode utilizar principais de serviço ao adicionar uma implementação do Azure Stack ligada. Esta versão também apresenta uma experiência de gestão de atualizações permite-lhe tomar medidas de remediação diretamente a partir de dentro do Operations Manager. Há também novos dashboards que apresentam os fornecedores de recursos, unidades de escala e dimensionar nós de unidade.

- **Versão do PowerShell de administrador 1.3.0 de pilha do Azure novo**.  O Azure Stack do PowerShell 1.3.0 está agora disponível para instalação. Esta versão fornece comandos para todos os fornecedores de recursos de administração gerir o Azure Stack.  Com esta versão, algum conteúdo irá ser preterido a partir do GitHub de ferramentas do Azure Stack [repositório](https://github.com/Azure/AzureStack-Tools). 

   Para obter detalhes de instalação, siga os [instruções](.\.\azure-stack-powershell-install.md) ou o [ajudar](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) conteúdo para o módulo do Azure Stack 1.3.0. 

- **Lançamento de referência da Rest API do Azure Stack inicial**. O [referência da API para todos os fornecedores de recursos de administração do Azure Stack](https://docs.microsoft.com/rest/api/azure-stack/) agora está publicada. 

### <a name="known-issues"></a>Problemas conhecidos
 
#### <a name="portal"></a>Portal
- <!-- TBD - IS ASDK --> A capacidade [para abrir um novo pedido de suporte na lista pendente](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) de dentro do administrador do portal não está disponível. Em vez disso, utilize a seguinte hiperligação:     
    - Para o Azure Stack Development Kit, utilize https://aka.ms/azurestackforum.    

- <!-- 2403291 - IS ASDK --> Poderá não ter o uso da barra de deslocamento horizontal na parte inferior de portais de administrador e utilizador. Se não conseguir aceder a barra de deslocamento horizontal, utilize a estrutura de ligações para navegar até um painel anterior no portal ao selecionar o nome do painel pretende visualizar na lista de trilha encontrado na parte superior esquerda do portal.
  ![Navegação estrutural](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos inteiro e, em seguida, eliminar subscrições de utilizador.

- <!-- TBD -  IS ASDK --> Não é possível ver as permissões à sua subscrição a utilizar os portais do Azure Stack. Como solução, utilize o PowerShell para verificar as permissões.

-   <!-- TBD -  IS ASDK --> No portal de administração, poderá ver um alerta crítico para o componente de Microsoft.Update.Admin. O nome do alerta, descrição e remediação todos são apresentadas como:  
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
- <!-- TBD -  IS ASDK --> Definições de dimensionamento para conjuntos de dimensionamento de máquinas virtuais não estão disponíveis no portal. Como solução, pode usar [do Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro, em vez de `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Ao criar máquinas virtuais no portal de utilizador do Azure Stack, o portal apresenta um número incorreto de discos de dados que pode anexar a uma VM da série DS. Séries de DS VMs capaz de acomodar os discos de dados como a configuração do Azure.

- <!-- TBD -  IS ASDK --> Quando não for possível criar uma imagem de VM, pode ser adicionado um item com falha que não é possível eliminar para o painel de computação de imagens VM.

  Como solução, criar uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (New-VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item falhado. Em seguida, 15 minutos depois de criar a imagem fictícia, pode com êxito eliminá-la.

  Em seguida, pode tentar transfira a imagem VM que falhou anteriormente.

- <!-- TBD -  IS ASDK --> Se uma extensão numa implantação de VM o aprovisionamento demora demasiado tempo, os utilizadores devem permitir que o limite de tempo de aprovisionamento em vez de tentar interromper o processo para desaloque ou elimine a VM.  

- <!-- 1662991 - IS ASDK --> Diagnóstico de VM do Linux não é suportado no Azure Stack. Quando implementa uma VM do Linux com o diagnóstico VM ativado, a implementação falhar. A implementação falhar, também, se ativar as métricas básicas de VM do Linux através das definições de diagnóstico. 

#### <a name="networking"></a>Redes
- <!-- 1766332 - IS, ASDK --> Sob **Networking**, se clicar em **criar Gateway de VPN** para configurar uma ligação de VPN, **baseado em política** está listado como um tipo de VPN. Não selecione esta opção. Apenas os **rota com base** opção é suportada no Azure Stack.

- <!-- 2388980 -  IS ASDK --> Depois de uma VM é criada e associada a um endereço IP público, não é possível desassociar VM desse endereço IP. Desassociação parece funcionar, mas o endereço IP público atribuído anteriormente que permanece associado com a VM original.

  Atualmente, tem de utilizar apenas novos endereços IP públicos para novas VMs que criar.

  Este comportamento ocorre mesmo que a reatribuir o endereço IP a uma VM nova (geralmente conhecida como uma *alternância de VIP*). Todos os futuro tenta se conectar por meio deste resultado de endereço IP numa ligação para a VM associada originalmente e não para a nova.

- <!-- 2292271 - IS ASDK --> Se aumentar um limite de Quota para um recurso de rede que faz parte de uma oferta e o plano que está associado uma subscrição de inquilino, o novo limite não se aplica a essa subscrição. No entanto, o novo limite é aplicável às novas assinaturas que são criadas após a quota for aumentada. 

  Para contornar este problema, utilize um plano de suplemento para aumentar uma Quota de rede quando o plano já está associado uma subscrição. Para obter mais informações, consulte como [disponibilizar um plano de suplemento](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> Não é possível eliminar uma subscrição que tem recursos de zona DNS ou os recursos de tabela de rotas associados ao mesmo. Para eliminar com êxito a subscrição, tem primeiro de eliminar os recursos de zona DNS e a tabela de rotas da subscrição do inquilino. 


- <!-- 1902460 -  IS ASDK --> O Azure Stack oferece suporte a uma única *gateway de rede local* por endereço IP. Isso é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local ligação de gateway, subsequente tenta criar um recurso de gateway de rede local com o mesmo endereço IP são bloqueados.

- <!-- 16309153 -  IS ASDK --> Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a mudança para uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas para as VMs nessa vnet.
 
- <!-- TBD -  IS ASDK --> O Azure Stack não suporta a adição de interfaces de rede adicionais para uma instância VM depois de implementada a VM. Se a VM exigir mais de uma interface de rede, tem de ser definidos no momento da implementação.


#### <a name="sql-and-mysql"></a>SQL e o MySQL 
- <!-- TBD - ASDK --> Servidores a alojar a base de dados tem de estar dedicada para utilização pelas cargas de trabalho do utilizador e o fornecedor de recursos. Não é possível utilizar uma instância que está a ser utilizada por outros consumidores, incluindo serviços de aplicações.

- <!-- IS, ASDK --> Caracteres especiais, incluindo espaços e períodos, não são suportadas no **família** dê um nome ao criar um SKU para os fornecedores de recursos do SQL e o MySQL. 

#### <a name="app-service"></a>Serviço de Aplicações
- <!-- TBD -  IS ASDK --> Os utilizadores tem de registar o fornecedor de recursos de armazenamento antes de ser criar sua primeira função do Azure na subscrição.

- <!-- TBD -  IS ASDK --> Para ampliar a infraestrutura (funções de trabalho, gerenciamento, funções de front-end), tem de utilizar o PowerShell, tal como descrito nas notas de versão para computação.
 
#### <a name="usage"></a>Utilização  
- <!-- TBD -  IS ASDK --> Dados de medidor de utilização de endereços de IP público de utilização mostram o mesmo *EventDateTime* valor para cada registo, em vez do *TimeDate* carimbo que mostra quando o registo foi criado. Atualmente, não pode utilizar estes dados para efetuar gestão de contas preciso de utilização de endereços IP pública.

<!--
#### Identity
-->



#### <a name="downloading-azure-stack-tools-from-github"></a>Baixar ferramentas de pilha do Azure a partir do GitHub
- Ao utilizar o *invoke-webrequest* cmdlet do PowerShell para transferir o Azure Stack de ferramentas do Github, recebe um erro:     
    -  *Invoke-webrequest: O pedido foi abortado: não foi possível criar o canal seguro SSL/TLS.*     

  Este erro ocorre devido a uma recente descontinuação de suporte do GitHub dos Tlsv1 e Tlsv1.1 padrões de criptografia (o padrão para o PowerShell). Para obter mais informações, consulte [aviso de remoção de padrões de criptografia fraca](https://githubengineering.com/crypto-removal-notice/).

<!-- #### Identity -->



## <a name="build-201803021"></a>Compilação 20180302.1

### <a name="new-features-and-fixes"></a>Novas funcionalidades e correções
As novas funcionalidades e correções lançadas para a versão de sistemas integrados versão 1803 aplicam-se para o Azure Stack Development Kit do Azure Stack. Consulte a [novas funcionalidades](.\.\azure-stack-update-1803.md#new-features) e [foram corrigidos erros](.\.\azure-stack-update-1803.md#fixed-issues) notas de versão para obter detalhes de atualização de secções de 1803 de pilha do Azure.  
> [!IMPORTANT]    
> Alguns dos itens listados na **novas funcionalidades** e **foram corrigidos erros** secções são relevantes apenas para os sistemas integrados do Azure Stack.

### <a name="changes"></a>Alterações
- A forma de alterar o estado de uma oferta recém-criado desde *privada* para *público* ou *desativados* foi alterado. Para obter mais informações, consulte [criar uma oferta](.\.\azure-stack-create-offer.md). 


### <a name="known-issues"></a>Problemas conhecidos
 
#### <a name="portal"></a>Portal
- A capacidade [para abrir um novo pedido de suporte na lista pendente](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) de dentro do administrador do portal não está disponível. Em vez disso, utilize a seguinte hiperligação:     
    - Para o Azure Stack Development Kit, utilize https://aka.ms/azurestackforum.    

- <!-- 2050709 --> No portal de administração, não é possível editar as métricas de armazenamento para o serviço Blob, serviço de tabela ou o serviço de fila. Quando passe para o armazenamento e, em seguida, selecione o blob, tabela ou mosaico de serviço de fila, abrirá um novo painel que apresenta um gráfico de métricas para esse serviço. Se, em seguida, selecione editar na parte superior do mosaico de gráfico de métricas, o painel de editar gráfico é aberto, mas não apresenta as opções para editar as métricas.  

- Verá uma **ativação necessária** alerta de aviso que indica que se Registre seu Kit de desenvolvimento do Azure Stack. Este comportamento é esperado.

- A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos inteiro e, em seguida, eliminar subscrições de utilizador.

- Não é possível ver as permissões à sua subscrição a utilizar os portais do Azure Stack. Como solução, utilize o PowerShell para verificar as permissões.

- No dashboard do portal de administração, o mosaico de atualização não apresenta informações sobre atualizações. Para resolver este problema, clique no mosaico para atualizá-la.

-   No portal de administração, poderá ver um alerta crítico para o componente de Microsoft.Update.Admin. O nome do alerta, descrição e remediação todos são apresentadas como:  
    - *ERRO - o modelo para typu FaultType ResourceProviderTimeout está em falta.*

    Este alerta pode ser ignorado com segurança. 

- O portal de administração tanto no portal de utilizador, o painel de descrição geral Falha ao carregar ao selecionar o painel de descrição geral das contas de armazenamento que foram criadas com uma versão de API anterior (exemplo: 2015-06-15). 

  Como solução, utilize o PowerShell para executar o **Start-ResourceSynchronization.ps1** script para restaurar o acesso para os detalhes da conta de armazenamento. [O script está disponível a partir do GitHub]( https://github.com/Azure/AzureStack-Tools/tree/master/Support/scripts)e deve ser executado com credenciais de administrador de serviço no anfitrião do kit de desenvolvimento se usar o ASDK.  

- O **estado de funcionamento do serviço** painel não consegue carregar. Ao abrir o painel de estado de funcionamento do serviço no portal do administrador ou utilizador, Azure Stack exibirá uma mensagem de erro e não carrega informações. Este comportamento está previsto. Embora pode selecionar e abrir o estado de funcionamento do serviço, esta funcionalidade ainda não está disponível, mas será implementada numa versão futura do Azure Stack.


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

- No portal de administração do Azure Stack, poderá ver um alerta crítico com o nome **pendentes expiração do certificado externo**.  Este alerta pode ser ignorado com segurança e afetar as operações do Development Kit do Azure Stack. 


#### <a name="marketplace"></a>Marketplace
- Os utilizadores podem procurar o marketplace completo sem uma subscrição e podem ver itens administrativos, como os planos e ofertas. Esses itens estão não funcional para os utilizadores.
 
#### <a name="compute"></a>Computação
- Definições de dimensionamento para conjuntos de dimensionamento de máquinas virtuais não estão disponíveis no portal. Como solução, pode usar [do Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro, em vez de `-VMScaleSetName`.

- Ao criar máquinas virtuais no portal de utilizador do Azure Stack, o portal apresenta um número incorreto de discos de dados que pode anexar a uma VM da série DS. Séries de DS VMs capaz de acomodar os discos de dados como a configuração do Azure.

- Quando não for possível criar uma imagem de VM, pode ser adicionado um item com falha que não é possível eliminar para o painel de computação de imagens VM.

  Como solução, criar uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (New-VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item falhado. Em seguida, 15 minutos depois de criar a imagem fictícia, pode com êxito eliminá-la.

  Em seguida, pode tentar transfira a imagem VM que falhou anteriormente.

-  Se uma extensão numa implantação de VM o aprovisionamento demora demasiado tempo, os utilizadores devem permitir que o limite de tempo de aprovisionamento em vez de tentar interromper o processo para desaloque ou elimine a VM.  

- <!-- 1662991 --> Diagnóstico de VM do Linux não é suportado no Azure Stack. Quando implementa uma VM do Linux com o diagnóstico VM ativado, a implementação falhar. A implementação falhar, também, se ativar as métricas básicas de VM do Linux através das definições de diagnóstico. 


#### <a name="networking"></a>Redes
- Sob **Networking**, se clicar em **ligação** para configurar uma ligação de VPN, **VNet-para-VNet** está listado como um tipo de ligação possíveis. Não selecione esta opção. Atualmente, apenas os **Site a site (IPsec)** opção é suportada.

- Depois de uma VM é criada e associada a um endereço IP público, não é possível desassociar VM desse endereço IP. Desassociação parece funcionar, mas o endereço IP público atribuído anteriormente que permanece associado com a VM original.

  Atualmente, tem de utilizar apenas novos endereços IP públicos para novas VMs que criar.

  Este comportamento ocorre mesmo que a reatribuir o endereço IP a uma VM nova (geralmente conhecida como uma *alternância de VIP*). Todos os futuro tenta se conectar por meio deste resultado de endereço IP numa ligação para a VM associada originalmente e não para a nova.



- O Azure Stack oferece suporte a uma única *gateway de rede local* por endereço IP. Isso é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local ligação de gateway, subsequente tenta criar um recurso de gateway de rede local com o mesmo endereço IP são bloqueados.

- Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a mudança para uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas para as VMs nessa vnet.
 
- O Azure Stack não suporta a adição de interfaces de rede adicionais para uma instância VM depois de implementada a VM. Se a VM exigir mais de uma interface de rede, tem de ser definidos no momento da implementação.



#### <a name="sql-and-mysql"></a>SQL e o MySQL 
- Pode demorar até uma hora antes dos utilizadores podem criar bases de dados num novo SQL ou MySQL SKU.

- Servidores a alojar a base de dados tem de estar dedicada para utilização pelas cargas de trabalho do utilizador e o fornecedor de recursos. Não é possível utilizar uma instância que está a ser utilizada por outros consumidores, incluindo serviços de aplicações.

- <!-- IS, ASDK --> Caracteres especiais, incluindo espaços e períodos, não são suportadas no **família** ou **escalão** nomes ao criar um SKU para os fornecedores de recursos do SQL e o MySQL.

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
