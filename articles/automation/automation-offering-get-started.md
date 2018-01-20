---
title: "Introdução à automatização do Azure | Microsoft Docs"
description: "Este artigo fornece uma descrição geral do serviço de automatização do Azure. -Revê os detalhes de design e implementação durante a preparação para a integração de oferta do Azure Marketplace."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: magoedte
ms.openlocfilehash: d6ee5c35ce9866f6106c7b5dbc51599b666c3eb1
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/20/2018
---
# <a name="get-started-with-azure-automation"></a>Introdução à Automatização do Azure

Este artigo apresenta conceitos principais relacionadas com a implementação da automatização do Azure. Se estiver familiarizado com automatização no Azure ou ter experiência com o software de fluxo de trabalho de automatização como o System Center Orchestrator, pode saber como preparar e automatização carregar. Depois de ler este artigo, estará pronto para começar a desenvolver runbooks para suportar as suas necessidades de automatização de processos. 


## <a name="automation-architecture-overview"></a>Descrição geral da arquitetura de automatização

![Descrição geral da Automatização do Azure](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

A automatização do Azure é um software como uma aplicação de serviço (SaaS) que fornece um ambiente multi-inquilino escalável e fiável em que pode utilizar runbooks para automatizar os processos. Pode utilizar a configuração de estado pretendido (DSC) no Azure, outros serviços em nuvem, ou no local ambiente para gerir a configuração é alterado para sistemas Windows e Linux. Entidades na sua conta de automatização, incluindo os runbooks, recursos e contas Run as, estão isoladas de outras contas de automatização na sua subscrição e a partir de outras subscrições.  

Os Runbooks que executar no Azure são executados no sandboxes da automatização. Sandboxes estão alojados na plataforma do Azure como máquinas de virtuais do serviço (PaaS). 

Sandboxes da automatização proporcionam o isolamento de inquilino para todos os aspetos de execução do runbook, incluindo módulos, armazenamento, memória, comunicação de rede e fluxos de trabalho. Esta função é gerida pelo serviço. Não é possível aceder ou gerir a função da sua conta do Azure ou de automatização.         

Para automatizar a implementação e gestão de recursos no seu centro de dados local ou outros serviços em nuvem, depois de criar uma conta de automatização, pode designar um ou mais VMs para executar o [Runbook Worker híbrido](automation-hybrid-runbook-worker.md) função. Cada trabalho de Runbook híbrida requer o agente de gestão da Microsoft para ser instalada e uma conta de automatização. O agente tem de ter uma ligação para uma área de trabalho do Log Analytics do Azure. Pode utilizar a análise do registo de arranque a instalação, a manter o agente de gestão da Microsoft e monitorizar a funcionalidade do Runbook Worker híbrido. A automatização do Azure efetua o fornecimento de runbooks e as instruções para executá-los.

Pode implementar vários Runbook Workers híbridos. Utilize os Runbook Workers híbridos para fornecer elevada disponibilidade para os runbooks e balanceamento de carga de tarefas de runbook. Em alguns casos, pode dedicar tarefas de runbook para cargas de trabalho específicas ou ambientes. Microsoft Monitoring Agent no Runbook Worker híbrido inicia a comunicação com o serviço de automatização através da porta TCP 443. Os Runbook Workers híbridos ter não existem requisitos de firewall de entrada.  

Poderá pretender que um runbook que está a executar um Runbook Worker híbrido para efetuar tarefas de gestão contra outras máquinas ou serviços no seu ambiente. Nesse cenário, o runbook pode também devem ter acesso outras portas. Se as políticas de segurança de TI não permitir que os computadores na sua rede para ligar à internet, reveja [OMS Gateway](../log-analytics/log-analytics-oms-gateway.md). O Gateway do Operations Management Suite (OMS) atua como um proxy para o Runbook Worker híbrido. Recolhe o estado da tarefa e recebe informações de configuração da sua conta de automatização.

Os Runbooks que executar um Runbook Worker híbrido executados no contexto da conta do sistema local no computador. Recomendamos um contexto de segurança quando efetuar ações administrativas no computador local do Windows. Se pretender que o runbook para executar tarefas relativamente aos recursos que estão fora do computador local, poderá ter de definir ativos de credenciais seguras na conta de automatização. Pode aceder a recursos de credenciais seguras do runbook e utilizá-los para autenticar com o recurso externo. Pode utilizar [credencial](automation-credentials.md), [certificado](automation-certificates.md), e [ligação](automation-connections.md) ativos no runbook. Utilize os recursos com os cmdlets que pode utilizar para especificar as credenciais para autenticá-los.

Pode aplicar configurações de DSC são armazenadas na automatização do Azure para máquinas virtuais. Outras máquinas virtuais e físicas pode solicitar as configurações do servidor de solicitação do Automation DSC. Não precisa de implementar qualquer infra-estrutura para suportar o servidor de solicitação do Automation DSC para gerir as configurações do seu local físicos ou virtuais sistemas Windows e Linux. Só precisa de acesso de internet de saída de cada sistema que irá gerir utilizando o DSC de automatização. Ocorrerá a comunicação através da porta TCP 443 para o serviço do OMS.   

## <a name="prerequisites"></a>Pré-requisitos

### <a name="automation-dsc"></a>Automation DSC
Pode utilizar o DSC de automatização para gerir estas máquinas:

* Virtual machines do Azure (clássica) que executem Windows ou Linux.
* Máquinas virtuais do Azure com o Windows ou Linux.
* Máquinas virtuais do Amazon Web Services (AWS) que executem Windows ou Linux.
* Computadores com Windows físicas e virtuais estão no local ou numa nuvem que não sejam do Azure ou AWS.
* Computadores com Linux físicas e virtuais que estão no local ou numa nuvem que não sejam do Azure ou AWS.

Para máquinas do Windows, a versão mais recente do Windows Management Framework (WMF) 5 tem de estar instalada. Para computadores Linux, a versão mais recente do [agente DSC do PowerShell para Linux](https://www.microsoft.com/en-us/download/details.aspx?id=49150) tem de estar instalado. O agente do PowerShell DSC utiliza WMF 5 para comunicar com a automatização. 

### <a name="hybrid-runbook-worker"></a>Função de Trabalho de Runbook Híbrida  
Ao designar um computador para executar tarefas de runbook híbridas, o computador tem de cumprir os seguintes pré-requisitos:

* Windows Server 2012 ou posterior.
* Windows PowerShell 4.0 ou posterior. Para uma maior fiabilidade, recomendamos o Windows PowerShell 5.0. Pode [transferir a versão nova](https://www.microsoft.com/download/details.aspx?id=50395) do Microsoft Download Center.
* .NET framework 4.6.2 ou posterior.
* Um mínimo de dois núcleos.
* Um mínimo de 4 GB de RAM.

### <a name="permissions-required-to-create-an-automation-account"></a>Permissões necessárias para criar uma conta de automatização
Criar ou atualizar uma conta de automatização e concluir as tarefas descritas neste artigo, tem de ter os seguintes privilégios e permissões:   
 
* Para criar uma conta de automatização, a conta de utilizador do Azure Active Directory (Azure AD) tem de ser adicionada a uma função com permissões equivalentes à função de proprietário para **Microsoft** recursos. Para obter mais informações, consulte [controlo de acesso baseado em funções na automatização do Azure](automation-role-based-access-control.md).  
* No portal do Azure, em **do Azure Active Directory** > **GERIR** > **registos de aplicação**, se **registos de aplicação**  está definido como **Sim**, os utilizadores de não administrador no inquilino do Azure AD podem [registar aplicações do Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions). Se **registos de aplicação** está definido como **não**, o utilizador que executa esta ação tem de ser um administrador global no Azure AD. 

Se não um membro de instância do Active Directory da subscrição antes de é adicionado à função de administrador global/coadministrador da subscrição, são adicionados ao Active Directory como convidado. Neste cenário, verá esta mensagem no **adicionar conta de automatização** página: "Não tem permissões para criar." 

Se um utilizador é adicionado à função de administrador/coadministrador global em primeiro lugar, pode removê-los a partir de instância de Active Directory da subscrição e, em seguida, adicione-os à função de utilizador completa no Active Directory.

Para verificar as funções de utilizador:
1. No portal do Azure, visite o **do Azure Active Directory** painel.
2. Selecione **utilizadores e grupos**.
3. Selecione **todos os utilizadores**. 
4. Depois de selecionar um utilizador específico, selecione **perfil**. O valor da **tipo de utilizador** atributo sob o perfil do utilizador não deve ser **convidado**.

## <a name="authentication-planning"></a>Plano para autenticação
Na automatização do Azure, pode automatizar tarefas relativamente aos recursos no Azure, no local, e em outros serviços em nuvem. Para um runbook efetuar as ações necessárias, tem de ter permissões para aceder de forma segura os recursos. Tem de ter os direitos mínimos necessários dentro da subscrição.  

### <a name="what-is-an-automation-account"></a>O que é uma conta de automatização 
Todas as tarefas de automatização que executa relativamente aos recursos utilizando os cmdlets na automatização do Azure autenticados no Azure utilizando a autenticação de com base em credenciais de identidade organizacional do Azure AD.  Uma conta de automatização é diferente da conta que utiliza para iniciar sessão no portal para configurar e utilizar recursos do Azure. 

Os seguintes recursos estão incluídos com uma conta de automatização:

* **Certificados**. Contém um certificado que é utilizado para autenticação de um runbook ou a configuração de DSC. Também pode adicionar certificados.
* **Ligações**. Contém informações de autenticação e a configuração necessária para ligar a uma aplicação ou serviço externo a partir de um runbook ou a configuração de DSC.
* **Credenciais**. Contém um **PSCredential** objeto, que tenha credenciais de segurança, tais como um nome de utilizador e palavra-passe. As credenciais são necessárias para autenticar a partir de um runbook ou a configuração de DSC.
* **Módulos de integração**. Módulos do PowerShell que estão incluídos com uma conta de automatização. Utilize os módulos do PowerShell para executar os cmdlets nos runbooks e configurações de DSC.
* **As agendas**. Contém as agendas que iniciar ou parar um runbook a uma determinada hora, incluindo frequências periódicas.
* **Variáveis**. Contém valores que estão disponíveis a partir de um runbook ou a configuração de DSC.
* **Configurações de DSC**. Scripts do PowerShell que descrevem como configurar uma definição ou uma funcionalidade do sistema operativo ou como instalar uma aplicação num computador Windows ou Linux.  
* **Os Runbooks**. Um conjunto de tarefas que executam um processo automatizado na automatização, com base no Windows PowerShell.    

Recursos de automatização para cada conta de automatização estão associados uma única região do Azure. No entanto, pode utilizar as contas de automatização para gerir todos os recursos na sua subscrição. Crie contas de Automatização em diferentes regiões se tiver políticas que exigem que os dados e os recursos estejam isolados numa região específica.

Quando cria uma conta de automatização no portal do Azure, duas entidades de autenticação são criadas automaticamente:

* **Conta Run As**. Esta conta faz as seguintes tarefas:
  - Cria um principal de serviço no Azure AD.
  - Cria um certificado.
  - Atribui o Contributor Role-Based controlo de acesso (RBAC), que gere os recursos do Azure Resource Manager utilizando runbooks.
* **Conta Run As clássica**. Esta conta carrega um certificado de gestão. O certificado é utilizado para gerir os recursos clássicos através de runbooks.

RBAC está disponível com o Gestor de recursos para conceder ações permitidas a uma conta de utilizador do Azure AD e a conta Run As. Também pode utilizar o RBAC para autenticar esse principal de serviço. Para obter mais informações e para ajudar a desenvolver um modelo para a gestão de permissões de automatização, consulte [controlo de acesso baseado em funções no artigo de automatização do Azure](automation-role-based-access-control.md).  

#### <a name="authentication-methods"></a>Métodos de autenticação
A tabela seguinte resume os métodos de autenticação que pode utilizar para cada ambiente que suporta a automatização do Azure.

| Método | Ambiente 
| --- | --- | 
| Conta Run As e Run As Clássica |Implementação clássica do Azure e Azure Resource Manager |  
| Conta de Utilizador do Azure AD |Implementação clássica do Azure e Azure Resource Manager |  
| Autenticação do Windows |Centro de dados local ou de outro fornecedor de serviços na nuvem utilizando a função de trabalho de Runbook híbrida |  
| Credenciais do Amazon Web Services |Amazon Web Services |  

Os artigos seguintes fornecem passos de descrição geral e implementação para configurar a autenticação para os ambientes. Os artigos descrevem a utilizar um existente e utilizar uma nova conta, dedique nesse ambiente. 
* [Criar uma conta de automatização do Azure de autónoma](automation-create-standalone-account.md)
* [Autenticar Runbooks com a implementação clássica do Azure e o Resource Manager](automation-create-aduser-account.md)
* [Autenticar Runbooks com Amazon Web Services](automation-config-aws-account.md)
* [Atualizar a conta de automatização Run As](automation-create-runas-account.md)

Para as contas Run As do Azure e Run As clássica, [atualizar automatização conta Run As](automation-create-runas-account.md) descreve como atualizar a sua conta de automatização existente com as contas Run As do portal. Também descreve como utilizar o PowerShell se a conta de automatização não foi originalmente configurada com uma conta Run As ou Run As clássica. Pode criar uma conta Run As e uma conta Run As clássica através da utilização de um certificado emitido por sua autoridade de certificação empresarial (AC). Reveja [atualização automatização conta Run As](automation-create-runas-account.md) para aprender a criar as contas utilizando esta configuração.     
 
## <a name="network-planning"></a>Planear a sua rede
Para o Runbook Worker híbrido ligar a e registar com o OMS, tem de ter acesso para o número de porta e os URLs descritos nesta secção. Isto é, para além de [portas e URLs necessários para o Microsoft Monitoring Agent](../log-analytics/log-analytics-windows-agent.md) para ligar ao OMS. 

Se utilizar um servidor proxy para comunicação entre o agente e o serviço do OMS, certifique-se de que os recursos adequados estão acessíveis. Se utilizar uma firewall para restringir o acesso à internet, tem de configurar a firewall para permitir o acesso.

A porta e URLs seguintes são necessários para a função Runbook Worker híbrido comunicar com a automatização:

* : Apenas TCP 443 é necessária a porta De saída acesso à internet.
* Global URL: *.azure-automation.net.

Se tiver uma conta de automatização que esteja definida para uma região específica, pode restringir a comunicação para esse datacenter regional. A tabela seguinte fornece o registo DNS para cada região.

| **Região** | **Registo de DNS** |
| --- | --- |
| EUA Centro-Sul |scus-jobruntimedata-prod-su1.azure-automation.net |
| EUA Leste 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| EUA Centro-Oeste | wcus-jobruntimedata-prod-su1.azure-automation.net |
| Europa Ocidental |we-jobruntimedata-prod-su1.azure-automation.net |
| Europa do Norte |ne-jobruntimedata-prod-su1.azure-automation.net |
| Canadá Central |cc-jobruntimedata-prod-su1.azure-automation.net |
| Sudeste Asiático |sea-jobruntimedata-prod-su1.azure-automation.net |
| Índia Central |cid-jobruntimedata-prod-su1.azure-automation.net |
| Leste do Japão |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Sudeste da Austrália |ase-jobruntimedata-prod-su1.azure-automation.net |
| Reino Unido Sul | uks-jobruntimedata-prod-su1.azure-automation.net |
| Gov (US) - Virginia | usge-jobruntimedata-prod-su1.azure-automation.us |

Para obter uma lista de endereços IP da região em vez de nomes de região, transfira o [endereço IP de Datacenter do Azure](https://www.microsoft.com/download/details.aspx?id=41653) ficheiro XML do Microsoft Download Center. 

> [!NOTE]
> O ficheiro XML do endereço IP de Datacenter do Azure lista os intervalos de endereços IP que são utilizados nos centros de dados dos Microsoft Azure. Intervalos de computação, o SQL Server e o armazenamento são incluídos no ficheiro. 
>
>Um ficheiro atualizado é publicado semanalmente. O ficheiro reflete os intervalos de implementado e as alterações futuras para os intervalos de IP. Novo intervalos que aparecem no ficheiro de não são utilizados em dos centros de dados de, pelo menos, uma semana. 
>
> É uma boa ideia para transferir o ficheiro XML novo todas as semanas. Em seguida, atualize o site para identificar corretamente os serviços em execução no Azure. Utilizadores do Azure ExpressRoute devem ter em consideração que este ficheiro utilizado para atualizar o anúncio Border Gateway Protocol (BGP) de espaço do Azure a primeira semana de cada mês. 
> 

## <a name="creating-an-automation-account"></a>Criar uma conta de automatização

A tabela seguinte apresenta os métodos para criar uma conta de automatização no portal do Azure. A tabela descreve cada tipo de experiência de implementação e as diferenças entre os mesmos.  

|Método | Descrição |
|-------|-------------|
| Selecione **automatização e controlo** no Azure Marketplace | Uma oferta Azure Marketplace cria uma conta de automatização e a área de trabalho OMS que estão ligadas e no mesmo grupo de recursos e região. Integração com o OMS também inclui a vantagem de utilizar a análise de registos para monitorizar e analisar fluxos de trabalho e o estado de tarefa de runbook ao longo do tempo. Também pode utilizar as funcionalidades avançadas na análise de registos para escalar ou investigar problemas. A oferta implementa o **alterações** e **gestão de atualizações** soluções, que estão ativadas por predefinição. |
| Selecione **automatização** no Marketplace | Este método cria uma conta de automatização num grupo de recursos novo ou existente que não está ligado a uma área de trabalho do OMS. Não inclui qualquer soluções disponíveis do **automatização e controlo** oferta. Este método é uma configuração básica que apresente automatização. Pode ajudar a aprender a escrever runbooks e configurações de DSC e saiba como utilizar as capacidades do serviço. |
| Selecione **gestão** soluções | Se selecionar um **gestão** solução, incluindo [gestão de atualizações](../operations-management-suite/oms-solution-update-management.md), [VMs de início/paragem durante as horas de inatividade](automation-solution-vm-management.md), ou [alterações](../log-analytics/log-analytics-change-tracking.md), a solução pede-lhe para selecionar uma conta de automatização existente e a área de trabalho do OMS. A solução oferece a opção de criar uma conta de automatização e a área de trabalho do OMS conforme necessário para a solução ser implementada na sua subscrição. |

### <a name="create-an-automation-account-thats-integrated-with-oms"></a>Criar uma conta de automatização que esteja integrada com o OMS
Para carregar automatização, recomendamos que selecione o **automatização e controlo** oferta no Marketplace. Ao utilizar este método cria uma conta de automatização e estabelece a integração com uma área de trabalho do OMS. Quando utiliza este método, tem também a opção para instalar as soluções de gestão que estão disponíveis com a oferta.  

[Criar uma conta de automatização de autónoma](automation-create-standalone-account.md) explica o processo de criação de uma conta de automatização e a área de trabalho OMS através da integração do **automatização e controlo** oferta. Pode saber como criar um conta de automatização para fins de teste de autónomo ou o serviço de pré-visualização.  

Para criar uma conta de automatização e a área de trabalho do OMS, utilizando o **automatização e controlo** oferta do Marketplace:

1. Inicie sessão no portal do Azure com uma conta que seja um membro da função de administradores da subscrição e um coadministrador da subscrição.
2. Selecione **novo**.<br><br> ![Selecionar novo no portal do Azure](media/automation-offering-get-started/automation-portal-martketplacestart.png)<br>  
3. Procurar **automatização**. Nos resultados da pesquisa, selecione **automatização e controlo**.<br><br> ![Procure e selecione a automatização e controlo no Azure Marketplace](media/automation-offering-get-started/automation-portal-martketplace-select-automationandcontrol.png).<br>   
4. Reveja a descrição para a oferta e, em seguida, selecione **criar**.  
5. Em **automatização e controlo**, selecione **área de trabalho OMS**. Em **áreas de trabalho do OMS**, selecione uma área de trabalho do OMS que está ligada à subscrição do Azure que está a ser a conta de automatização. Se não tiver uma área de trabalho do OMS, selecione **criar nova área de trabalho**. Em **área de trabalho OMS**: 
  1. Para **área de trabalho OMS**, introduza um nome para a nova área de trabalho.
  2. Para **subscrição**, selecione uma subscrição para ligar a. Se a seleção predefinida não estiver a subscrição que pretende utilizar, selecione a subscrição da lista pendente.
  3. Para **grupo de recursos**, pode criar um grupo de recursos ou selecione um grupo de recursos existente.  
  4. Para **localização**, selecione uma região. Para obter mais informações, consulte [em que regiões da automatização do Azure está disponível no](https://azure.microsoft.com/regions/services/). Soluções são disponibilizadas em duas camadas: camada livre e por nó (OMS). O escalão gratuito tem um limite, a quantidade de dados que recolheu diariamente, o período de retenção e minutos de tempo de execução de tarefa de runbook. O por nó (OMS) camada não tem um limite na quantidade de dados recolhidos diariamente.  
  5. Selecione **Conta de Automatização**.  Se criar uma nova área de trabalho do OMS, também tem de criar uma conta de automatização que está associada a nova área de trabalho do OMS. Incluem a sua subscrição do Azure, o grupo de recursos e a região. 
    1. Selecione **criar uma conta de automatização**.
    2. Em **conta de automatização**, além de **nome** campo, introduza o nome da conta de automatização.
    Todas as outras opções automaticamente são povoadas com base na área de trabalho OMS selecionada. Não é possível modificar estas opções. 
    3. O método de autenticação predefinido para a oferta é a conta Run As do Azure. Depois de selecionar **OK**, as opções de configuração são validadas e a conta de automatização é criada. Para controlar o progresso da mesma, no menu, selecione **notificações**. 
    4. Caso contrário, selecione uma conta Run As de Automatização existente. A conta que selecionou já não pode ser ligada a outra área de trabalho do OMS. Se estiver, é apresentada uma mensagem de notificação. Se a conta já está associada a uma área de trabalho do OMS, selecione uma conta Run As de automatização diferente ou crie uma.
    5. Depois de introduzir ou selecionar as informações necessárias, selecione **criar**. Verificar as informações e as contas Run As e conta de automatização são criadas. É automaticamente reencaminhado para o **área de trabalho OMS** painel.  
6. Depois de introduzir ou selecionar as informações necessárias no **área de trabalho OMS** painel, selecione **criar**.  Verificar as informações e a área de trabalho é criada. Para controlar o progresso da mesma, no menu, selecione **notificações**. É encaminhado para o **Adicionar solução** painel.  
7. Em **automatização e controlo** definições, confirme que pretende instalar as soluções pré-selecionadas recomendadas. Se alterar qualquer uma das opções predefinidas, pode instalar as soluções individualmente mais tarde.  
8. Para continuar com a integração da automatização e uma área de trabalho do OMS, selecione **criar**. Todas as definições são validadas e, em seguida, tenta implementar a oferta na sua subscrição do Azure. Este processo poderá demorar vários segundos. Para controlar o progresso da mesma, no menu, selecione **notificações**. 

Depois da oferta é integrado, pode efetuar as seguintes tarefas:
* Iniciar criação de runbooks.
* Trabalhar com as soluções de gestão ativada.
* Implementar um [Runbook Worker híbrido](automation-hybrid-runbook-worker.md) função.
* Começar a trabalhar com [Log Analytics](https://docs.microsoft.com/azure/log-analytics) para recolher dados que são gerados pelo recursos no seu ambiente na nuvem ou no local.   

## <a name="next-steps"></a>Passos Seguintes
* Certifique-se de que a sua nova conta de automatização pode autenticar face a recursos do Azure. Para obter mais informações, consulte [teste automatização Run As do Azure autenticação da conta](automation-verify-runas-authentication.md).
* Saiba como começar a utilizar com a criação de runbooks e as considerações relacionadas antes de iniciar a criação. Para obter mais informações, consulte [tipos de runbook de automatização](automation-runbook-types.md).


