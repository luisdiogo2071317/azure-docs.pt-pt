---
title: Notas de versão do Kit de desenvolvimento de pilha do Microsoft Azure | Microsoft Docs
description: Melhoramentos, correções e problemas conhecidos do Kit de desenvolvimento de pilha do Azure.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 3335fdd3a1bb20c378bf36307d742491de0e46ad
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="azure-stack-development-kit-release-notes"></a>Notas de versão do Kit de desenvolvimento de pilha do Azure
Estas notas de versão fornecem informações sobre os melhoramentos, correções e problemas conhecidos no Kit de desenvolvimento de pilha do Azure. Se não tiver a certeza de qual é a versão que está a executar, pode [utilizar o portal para verificar](.\.\azure-stack-updates.md#determine-the-current-version).

> Se mantêm atualizados com Novidades o ASDK ao subscrever o [ ![RSS](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [feed](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-201803291"></a>Compilação 20180329.1

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

- <!-- 2050709 --> In the admin portal, it is not possible to edit storage metrics for Blob service, Table service, or Queue service. When you go to Storage, and then select the blob, table, or queue service tile, a new blade opens that displays a metrics chart for that service. If you then select Edit from the top of the metrics chart tile, the Edit Chart blade opens but does not display options to edit metrics.  

- Ao visualizar as propriedades de um recurso ou grupo de recursos, o **mover** botão está desativado. Este comportamento é esperado. Mover recursos ou grupos de recursos entre grupos de recursos ou subscrições não é atualmente suportado.
 
- Verá uma **ativação necessária** alerta de aviso que indica a registar o Kit de desenvolvimento de pilha do Azure. Este comportamento é esperado.

- A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos completo e, em seguida, eliminar subscrições de utilizador.

- Não é possível ver as permissões para a sua subscrição utilizando os portais de pilha do Azure. Como solução, utilize o PowerShell para verificar permissões.

- No dashboard do portal de administração, o mosaico de atualização não consegue apresentar informações sobre atualizações. Para resolver este problema, clique no mosaico atualizá-lo.

-   No portal de administração, poderá ver um alerta crítico para o componente de Microsoft.Update.Admin. O nome do alerta, a descrição e remediação todos são apresentadas como:  
    - *ERRO - o modelo para o FaultType ResourceProviderTimeout está em falta.*

    Este alerta pode ser ignorado com segurança. 



#### <a name="marketplace"></a>Marketplace
- Os utilizadores podem procurar o mercado completo sem uma subscrição e podem ver itens administrativos como planos e ofertas. Estes itens estão não funcional para os utilizadores.
 
#### <a name="compute"></a>Computação
- Definições de dimensionamento para conjuntos de dimensionamento de máquina virtual não estão disponíveis no portal. Como solução, pode utilizar [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro em vez de `-VMScaleSetName`.

- Ao criar máquinas virtuais no portal de utilizador de pilha do Azure, o portal apresenta um número incorreto de discos de dados que pode anexar a uma série DS VM. Série DS VMs pode acomodar tantos discos de dados como a configuração do Azure.

- Quando não for possível criar uma imagem de VM, um item de falha que não é possível eliminar pode ser adicionado ao painel de processamento de imagens de VM.

  Como solução, crie uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (novo VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item da falha. Em seguida, 15 minutos depois de criar a imagem fictício, que pode com êxito eliminá-lo.

  Em seguida, pode tentar transfira novamente a imagem VM que tenha falhado anteriormente.

-  Se uma extensão numa implementação de VM de aprovisionamento demora demasiado tempo, os utilizadores devem permitir o aprovisionamento do limite de tempo em vez de tentar interromper o processo de Desalocação ou eliminar a VM.  

- <!-- 1662991 --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings. 


#### <a name="networking"></a>Redes
- Em **redes**, se clicar em **ligação** para configurar uma ligação VPN, **VNet a VNet** está listado como um tipo de ligação possíveis. Não selecione esta opção. Atualmente, apenas o **Site a site (IPsec)** opção é suportada.

- Depois de uma VM é criada e associada a um endereço IP público, não é possível desassociar essa VM a partir desse endereço IP. Desassociação aparece funcionar, mas o endereço IP público anteriormente atribuído permanece associado a VM original.

  Atualmente, tem de utilizar apenas novos endereços IP públicos para novas VMs que cria.

  Este comportamento ocorre mesmo reatribuir o endereço IP para uma nova VM (normalmente denominado como um *alternância de VIP*). Todas as futuras tenta estabelecer ligação através deste resultado de endereço IP numa ligação para a VM originalmente associada e não para a nova.



- Pilha do Azure suporta um único *gateway de rede local* por endereço IP. Isto é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local gateway ligação, posterior tentativas criar um recurso de gateway de rede local com o mesmo endereço IP estão bloqueadas.

- Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a alteração a uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas por push para VMs nessa Vnet.
 
- Pilha do Azure não suporta a adição de interfaces de rede adicionais para uma instância VM após a implementação da VM. Se a VM precisar de mais de uma interface de rede, tem de ser definidos no momento da implementação.



#### <a name="sql-and-mysql"></a>SQL Server e o MySQL 
- Pode demorar até uma hora antes dos utilizadores podem criar bases de dados num novo SQL Server ou MySQL SKU.

- A base de dados que aloja os servidores têm de estar dedicada para utilização pelas cargas de trabalho do utilizador e o fornecedor de recursos. Não é possível utilizar uma instância que está a ser utilizada por outros consumidores, incluindo os serviços de aplicação.

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






## <a name="build-201803021"></a>Compilação 20180302.1

### <a name="new-features-and-fixes"></a>Novas funcionalidades e correções
Consulte o [novas funcionalidades e correções](.\.\azure-stack-update-1802.md#new-features-and-fixes) secção as notas de versão de atualização de 1802 de pilha do Azure para a pilha do Azure integrado sistemas.

> [!IMPORTANT]    
> Alguns dos itens listados no **novas funcionalidades e correções** secção são relevantes apenas para os sistemas de pilha do Azure integrado.


### <a name="known-issues"></a>Problemas conhecidos
 
#### <a name="portal"></a>Portal
- A capacidade [para abrir um novo pedido de suporte na lista pendente](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) de dentro do administrador do portal não está disponível. Em vez disso, utilize a seguinte hiperligação:     
    - Para o Kit de desenvolvimento de pilha do Azure, utilize https://aka.ms/azurestackforum.    

- <!-- 2050709 --> In the admin portal, it is not possible to edit storage metrics for Blob service, Table service, or Queue service. When you go to Storage, and then select the blob, table, or queue service tile, a new blade opens that displays a metrics chart for that service. If you then select Edit from the top of the metrics chart tile, the Edit Chart blade opens but does not display options to edit metrics.  

- Ao visualizar as propriedades de um recurso ou grupo de recursos, o **mover** botão está desativado. Este comportamento é esperado. Mover recursos ou grupos de recursos entre grupos de recursos ou subscrições não é atualmente suportado.
 
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
No portal de administração de pilha do Azure, poderá ver um alerta crítico com o nome **pendentes expiração de certificado externo**.  Este alerta pode ser ignorado com segurança e afetem as operações do Kit de desenvolvimento de pilha do Azure. 


#### <a name="marketplace"></a>Marketplace
- Os utilizadores podem procurar o mercado completo sem uma subscrição e podem ver itens administrativos como planos e ofertas. Estes itens estão não funcional para os utilizadores.
 
#### <a name="compute"></a>Computação
- Definições de dimensionamento para conjuntos de dimensionamento de máquina virtual não estão disponíveis no portal. Como solução, pode utilizar [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro em vez de `-VMScaleSetName`.

- Pilha do Azure suporta a utilização de apenas de tipo fixo VHDs. Algumas imagens disponibilizadas através do marketplace na pilha do Azure utilizam VHDs dinâmicos, mas os que são removidos. O redimensionamento de uma máquina virtual (VM) com um disco dinâmico anexado deixa a VM em estado de falha.

  Para atenuar este problema, elimine a VM sem eliminar o disco da VM, um blob VHD numa conta do storage. Em seguida, converter o VHD de um disco dinâmico num disco fixo e, em seguida, voltar a criar a máquina virtual.

- Ao criar máquinas virtuais no portal de utilizador de pilha do Azure, o portal apresenta um número incorreto de discos de dados que pode anexar a uma série DS VM. Série DS VMs pode acomodar tantos discos de dados como a configuração do Azure.

- Quando não for possível criar uma imagem de VM, um item de falha que não é possível eliminar pode ser adicionado ao painel de processamento de imagens de VM.

  Como solução, crie uma nova imagem VM com um VHD fictício que pode ser criado através de Hyper-V (novo VHD-caminho C:\dummy.vhd-fixo - SizeBytes 1 GB). Este processo deve resolver o problema que impede a eliminar o item da falha. Em seguida, 15 minutos depois de criar a imagem fictício, que pode com êxito eliminá-lo.

  Em seguida, pode tentar transfira novamente a imagem VM que tenha falhado anteriormente.

-  Se uma extensão numa implementação de VM de aprovisionamento demora demasiado tempo, os utilizadores devem permitir o aprovisionamento do limite de tempo em vez de tentar interromper o processo de Desalocação ou eliminar a VM.  

- <!-- 1662991 --> Linux VM diagnostics is not supported in Azure Stack. When you deploy a Linux VM with VM diagnostics enabled, the deployment fails. The deployment also fails if you enable the Linux VM basic metrics through diagnostic settings. 


#### <a name="networking"></a>Redes
- Em **redes**, se clicar em **ligação** para configurar uma ligação VPN, **VNet a VNet** está listado como um tipo de ligação possíveis. Não selecione esta opção. Atualmente, apenas o **Site a site (IPsec)** opção é suportada.

- Depois de uma VM é criada e associada a um endereço IP público, não é possível desassociar essa VM a partir desse endereço IP. Desassociação aparece funcionar, mas o endereço IP público anteriormente atribuído permanece associado a VM original.

  Atualmente, tem de utilizar apenas novos endereços IP públicos para novas VMs que cria.

  Este comportamento ocorre mesmo reatribuir o endereço IP para uma nova VM (normalmente denominado como um *alternância de VIP*). Todas as futuras tenta estabelecer ligação através deste resultado de endereço IP numa ligação para a VM originalmente associada e não para a nova.

-   A funcionalidade de reencaminhamento IP é visível no portal, no entanto, ativar o reencaminhamento IP não tem qualquer efeito. Esta funcionalidade ainda não é suportada.

- Pilha do Azure suporta um único *gateway de rede local* por endereço IP. Isto é verdadeiro em todas as subscrições de inquilino. Após a criação da primeira rede local gateway ligação, posterior tentativas criar um recurso de gateway de rede local com o mesmo endereço IP estão bloqueadas.

- Numa rede Virtual que foi criado com uma definição de servidor DNS de *automática*, a alteração a uma falha de servidor DNS personalizado. As definições atualizadas não são enviadas por push para VMs nessa Vnet.
 
- Pilha do Azure não suporta a adição de interfaces de rede adicionais para uma instância VM após a implementação da VM. Se a VM precisar de mais de uma interface de rede, tem de ser definidos no momento da implementação.



#### <a name="sql-and-mysql"></a>SQL Server e o MySQL 
- Pode demorar até uma hora antes dos utilizadores podem criar bases de dados num novo SQL Server ou MySQL SKU.

- A base de dados que aloja os servidores têm de estar dedicada para utilização pelas cargas de trabalho do utilizador e o fornecedor de recursos. Não é possível utilizar uma instância que está a ser utilizada por outros consumidores, incluindo os serviços de aplicação.

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




## <a name="build-201801032"></a>Compilação 20180103.2

### <a name="new-features-and-fixes"></a>Novas funcionalidades e correções

- Consulte o [novas funcionalidades e correções](.\.\azure-stack-update-1712.md#new-features-and-fixes) secção as notas de versão de atualização de 1712 de pilha do Azure para a pilha do Azure integrado sistemas.

    > [!IMPORTANT]
    > Alguns dos itens listados no **novas funcionalidades e correções** secção são relevantes apenas para os sistemas de pilha do Azure integrado.

### <a name="known-issues"></a>Problemas conhecidos
 
#### <a name="deployment"></a>Implementação
- Tem de especificar um servidor de tempo por endereço IP durante a implementação.

#### <a name="infrastructure-management"></a>Gestão de infraestrutura
- Não ative a cópia de segurança de infraestrutura no **cópia de segurança da infraestrutura** painel.
- O endereço IP do bmc gestão controlador BMC e o modelo não são apresentadas as informações essenciais de um nó de unidade de escala. Este comportamento é esperado no Kit de desenvolvimento de pilha do Azure.

#### <a name="portal"></a>Portal
- Poderá ver um dashboard em branco no portal. Para recuperar o dashboard, selecione o ícone de equipamento no canto superior direito do portal e, em seguida, selecione **restaurar predefinições**.
- Ao visualizar as propriedades de um grupo de recursos, o **mover** botão está desativado. Este comportamento é esperado. Mover grupos de recursos entre subscrições não é atualmente suportado.
-  Para qualquer fluxo de trabalho onde selecionou uma subscrição, o grupo de recursos ou a localização numa lista pendente, pode deparar-se um ou mais dos seguintes problemas:

   - Poderá ver uma linha em branco no topo da lista. Deve ainda poderá selecionar um item conforme esperado.
   - Se a lista de itens na lista pendente é pequeno, poderá não conseguir ver algum dos nomes de item.
   - Se tiver várias subscrições do utilizador, a lista de lista pendente do grupo de recursos pode estar vazia. 

   Para contornar os dois últimos problemas, pode escrever o nome da subscrição ou grupo de recursos (se sabe que este) ou pode utilizar em vez disso, o PowerShell.

- Verá uma **ativação necessária** alerta de aviso que indica a registar o Kit de desenvolvimento de pilha do Azure. Este comportamento é esperado.
- Se o **componente** clica na hiperligação de qualquer **função de infraestrutura** de alerta, resultante **descrição geral** painel tentar carregar e não falha. Além do * * descrição geral * * painel não o tempo limite.
- A eliminar os resultados de subscrições do utilizador em recursos órfãos. Como solução, primeiro eliminar recursos de utilizador ou grupo de recursos completo e, em seguida, eliminar subscrições de utilizador.
- Não é possível ver permissões à sua subscrição através da utilização de portais de pilha do Azure. Como solução, pode verificar as permissões com o PowerShell.
- O **estado de funcionamento do serviço** painel Falha ao carregar. Quando abrir o painel de estado de funcionamento do serviço no portal de administrador ou utilizador, a pilha de Azure apresenta um erro e não carregar informações. Este comportamento está previsto. Apesar de poder selecione e abra o estado de funcionamento do serviço, esta funcionalidade ainda não está disponível, mas irá ser implementada numa versão futura da pilha do Azure.
#### <a name="marketplace"></a>Marketplace
- Alguns itens do marketplace estão a ser removidos nesta versão devido a problemas de compatibilidade. Estes serão novamente ativados após a validação adicional.
- Os utilizadores podem procurar o mercado completo sem uma subscrição e podem ver itens administrativos como planos e ofertas. Estes itens estão não funcional para os utilizadores.
 
#### <a name="compute"></a>Computação
- Os utilizadores recebem a opção para criar uma máquina virtual com armazenamento georredundante. Esta configuração provoca a falha da criação máquina virtual. 
- Pode configurar uma conjunto apenas com um domínio de falhas de um e um domínio de atualização de um de disponibilidade de máquina virtual.
- Não há nenhum experiência marketplace para criar conjuntos de dimensionamento de máquina virtual. Pode criar um conjunto, utilizando um modelo de dimensionamento.
- Definições de dimensionamento para conjuntos de dimensionamento de máquina virtual não estão disponíveis no portal. Como solução, pode utilizar [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Devido às diferenças de versão do PowerShell, tem de utilizar o `-Name` parâmetro em vez de `-VMScaleSetName`.

#### <a name="networking"></a>Redes
- Não é possível criar um balanceador de carga com um endereço IP público utilizando o portal. Como solução, pode utilizar o PowerShell para criar o Balanceador de carga.
- Tem de criar uma regra de tradução (NAT) de endereço de rede quando cria um balanceador de carga de rede. Caso contrário, receberá um erro ao tentar adicionar uma regra NAT após a criação do Balanceador de carga.
- Em **redes**, se clicar em **ligação** para configurar uma ligação VPN, **VNet a VNet** está listado como um tipo de ligação possíveis. Não selecione esta opção. Atualmente, apenas o **Site a site (IPsec)** opção é suportada.
- Não é possível desassociar um endereço IP público de uma máquina virtual (VM) depois da VM foi criada e associada a esse endereço IP. Desassociação irá aparecer funcionar, mas o endereço IP público anteriormente atribuído permanece associado a VM original. Este comportamento ocorre mesmo reatribuir o endereço IP para uma nova VM (normalmente denominado como um *alternância de VIP*). Todas as futuras tenta estabelecer ligação através deste resultado de endereço IP numa ligação para a VM originalmente associada e não para a nova. Atualmente, tem de utilizar os novos endereços IP públicos apenas para a criação de nova VM.
- Os operadores do Azure da pilha poderão não ser possível implementar, eliminar, modificar VNETs ou grupos de segurança de rede. Este problema é principalmente utilizado em tentativas de atualização subsequentes do mesmo pacote. Isto é causado por um problema de empacotamento com uma atualização que está atualmente a ser investigação.
- Balanceamento de carga interno (ILB) processa incorretamente endereços MAC para VMs do back-end que remover pacotes para a rede de back-end quando utilizando as instâncias de Linux.
 
#### <a name="sqlmysql"></a>SQL/MySQL 
- Pode demorar até uma hora até os inquilinos podem criar bases de dados num novo SQL Server ou MySQL SKU. 
- Criação de itens diretamente no SQL Server e o MySQL servidores que não são executadas pelo fornecedor de recursos de alojamento não é suportada e pode resultar num Estado não correspondentes.

#### <a name="app-service"></a>Serviço de Aplicações
- Um utilizador tem de registar o fornecedor de recursos de armazenamento antes de poderem criarem a sua primeira função do Azure na subscrição.
 
#### <a name="usage"></a>Utilização  
- Dados de medição de utilização de endereços de IP público de utilização mostram o mesmo *EventDateTime* valor para cada registo em vez do *TimeDate* carimbo que mostra que o registo foi criado. Atualmente, não é possível utilizar estes dados para efetuar exata contabilização da utilização de endereços IP pública.

#### <a name="identity"></a>Identidade

No Azure Active Directory serviços de Federação (ADFS) implementar ambientes, o **azurestack\azurestackadmin** conta já não é o proprietário da subscrição de fornecedor predefinido. Em vez de iniciar sessão no **portal de administração / adminmanagement endpoint** com o **azurestack\azurestackadmin**, pode utilizar o **azurestack\cloudadmin** conta, por isso que pode gerir e utilizar a subscrição do fornecedor predefinido.

> [!IMPORTANT]
> Mesmo as **azurestack\cloudadmin** conta é o proprietário da subscrição de fornecedor predefinido em ambientes de ADFS implementada, não tem permissões para RDP num anfitrião. Continuar a utilizar o **azurestack\azurestackadmin** conta ou a conta de administrador local para iniciar sessão, aceder e gerir o anfitrião, conforme necessário.


