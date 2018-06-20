---
title: Realojamento uma Contoso Linux para o suporte técnico do app service do Azure e MySQL do Azure | Microsoft Docs
description: Saiba como Contoso realojar uma aplicação do Linux no local ao migrar para as VMs do Azure e MySQL do Azure.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: 15a429c033cfd1598dd01b5c8cd2743c397dacdb
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36225522"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms-and-azure-mysql"></a>Migração de Contoso: realojamento uma aplicação do Linux no local para as VMs do Azure e MySQL do Azure

Este artigo mostra como Contoso são realojar o respetivo no local de duas camadas Linux para o suporte técnico do app service (osTicket), ao migrar para o Azure e MySQL do Azure.

Este ocument é oitavo numa série de artigos que mostram como a empresa fictícia com Contoso migra respetivos recursos no local para a nuvem do Microsoft Azure. A série inclui cenários que mostram como configurar uma infraestrutura de migração e executar diferentes tipos de migrações e informações de fundo. Cenários de crescimento em complexidade e será adicionado artigos adicionais ao longo do tempo.

**Artigo** | **Detalhes** | **Estado**
--- | --- | ---
[Artigo 1: Descrição geral](contoso-migration-overview.md) | Fornece uma descrição geral da estratégia de migração da Contoso, a série de artigo e as aplicações de exemplo, que vamos utilizar. | Disponível
[Artigo 2: Implementar uma infraestrutura do Azure](contoso-migration-infrastructure.md) | Descreve como Contoso prepara a infraestrutura do Azure e no local para a migração. A mesma infraestrutura é utilizada para todos os cenários de migração de Contoso. | Disponível
[Artigo 3: Avaliar a recursos no local](contoso-migration-assessment.md)  | Mostra como Contoso executa uma avaliação da respetiva aplicação de SmartHotel de duas camadas no local em execução no VMware. Avaliar a VMs de aplicação com o [Azure migrar](migrate-overview.md) serviço e a base de dados de SQL Server de aplicação com o [Assistente de migração de base de dados do Azure](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponível
[Artigo 4: Realojamento para as VMs do Azure e uma instância do SQL gerida](contoso-migration-rehost-vm-sql-managed-instance.md) | Demonstra como Contoso migra a aplicação de SmartHotel para o Azure. Se migrar a aplicação web VM através de [do Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)e a base de dados de aplicação através de [migração de base de dados do Azure](https://docs.microsoft.com/azure/dms/dms-overview) serviço, para migrar para uma instância do SQL Server geridos. | Disponível
[Artigo 5: Realojamento para as VMs do Azure](contoso-migration-rehost-vm.md) | Mostra como Contoso migrar as respetivas SmartHotel para VMs do IaaS do Azure, utilizando o serviço de recuperação de sites.
[Artigo 6: Realojamento para as VMs do Azure e grupos de disponibilidade do SQL Server](contoso-migration-rehost-vm-sql-ag.md) | Mostra como Contoso migra a aplicação de SmartHotel. Podem utilizar a recuperação de sites para migrar a aplicação VMs e o serviço de base de dados de migração para migrar a base de dados de aplicação para um grupo de disponibilidade do SQL Server. | Disponível
[Artigo 7: Realojamento uma aplicação do Linux para as VMs do Azure](contoso-migration-rehost-linux-vm.md) | Mostra como Contoso migra a respetiva aplicação de Linux osTicket para VMs de IaaS do Azure utilizando o Azure Site Recovery.
Artigo 8: Realojamento uma aplicação do Linux para as VMs do Azure e o servidor de MySQL do Azure (Este artigo) | Demonstra como Contoso migra a aplicação do Linux osTicket. Utilizam o Site de recuperação para a migração de VM e o MySQL Workbench para migrar para uma instância de servidor de MySQL do Azure. | Disponível

Neste artigo, Contoso migra uma de duas camadas Linux Apache MySQL PHP (LAMP) para o suporte técnico do app service (osTicket) para o Azure. Se gostaria de utilizar esta aplicação de open source, poderá transferi-lo de [GitHub](https://github.com/osTicket/osTicket).



## <a name="business-drivers"></a>Controladores de negócio

A equipa de TI liderança trabalhou coincida com os respetivos parceiros de negócios para compreender o que pretende alcançar:

- **Crescimento de negócio de endereços**: Contoso está a crescer e, como resultado existe pressão na sistemas no local e infraestrutura.
- **Limitar o risco**: O suporte técnico do serviço de aplicações é fundamental para as empresas de Contoso. Pretende movê-la para o Azure com o risco de zero.
- **Expandir**: Contoso não pretende alterar agora a aplicação. Basta pretendem manter estável.


## <a name="migration-goals"></a>Objetivos de migração

A equipa de nuvem Contoso tiver afixado baixo objetivos para migração, para determinar o melhor método de migração:

- Após a migração, a aplicação no Azure deve ter as mesmas capacidades de desempenho, como sucede no respetivo ambiente de VMWare no local.  A aplicação irá permanecer como crítica na nuvem porque está no local. 
- Contoso não quer investir nesta aplicação.  É importante para o negócio, mas o respetivo formato atual simplesmente pretendem movê-lo em segurança para a nuvem.
- Ter concluído alguns migrações de aplicação do Windows, a Contoso pretende aprender a utilizar uma infraestrutura baseada em Linux no Azure.
- A Contoso pretende minimizar as tarefas de administração de base de dados após a aplicação é movida para a nuvem.

## <a name="proposed-architecture"></a>Arquitetura proposta

Neste cenário:

- A aplicação está em camadas através de duas VMs (OSTICKETWEB e OSTICKETMYSQL).
- As VMs estão localizadas no anfitrião do VMware ESXi **contosohost1.contoso.com** (versão 6.5).
- O ambiente VMware é gerido pelo vCenter Server 6.5 (**vcenter.contoso.com**), em execução numa VM.
- Contoso tem um datacenter no local (contoso-datacenter), com um controlador de domínio no local (**contosodc1**).
- A aplicação de camada web no OSTICKETWEB será migrada para uma VM do IaaS do Azure.
- A base de dados de aplicação será migrada para a base de dados do Azure para o serviço de MySQL PaaS.
- Uma vez que o se estiver a migrar uma carga de trabalho de produção, os recursos irão residir no grupo de recursos de produção **ContosoRG**.
- Os recursos serão replicados para a região primária (EUA Leste 2) e colocados na rede de produção (VNET-PROD-EUS2):
    - O web VM irá residir da sub-rede do front-end (PROD-FE-EUS2).
    - A instância de base de dados irá residir na sub-rede da base de dados (PROD-DB-EUS2).
- A base de dados de aplicação será migrada para o MySQL do Azure com ferramentas de MySQL.
- As VMs no local no Centro de dados Contoso irão ser desativadas após a migração é efetuada.


![Arquitetura do cenário](./media/contoso-migration-rehost-linux-vm-mysql/architecture.png) 


## <a name="migration-process"></a>processo de migração

Contoso irá concluir o processo de migração da seguinte forma:

Para migrar web VM:

1. Como primeiro passo, Contoso define cópias de segurança do Azure e a infraestrutura necessária para implementar a recuperação de Site no local.
2. Depois de preparar os componentes do Azure e no local, estes configurar e ativar a replicação para o web VM.
3. Após a conclusão da replicação de segurança e executar, migre as VM ao efetuar a ativação pós-falha para o Azure.

Para migrar a base de dados:

1. Contoso Aprovisiona uma instância de MySQL no Azure.
2. Configurar o MySQL workbench e cópia de segurança da base de dados localmente.
3. Se, em seguida, restaure a base de dados da cópia de segurança local para o Azure.

![processo de migração](./media/contoso-migration-rehost-linux-vm-mysql/migration-process.png) 


### <a name="azure-services"></a>Serviços do Azure

**Serviço** | **Descrição** | **Custo**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | O serviço orquestra e gere a migração e recuperação após desastre para as VMs do Azure e no local VMs e servidores físicos.  | Durante a replicação para o Azure, são cobradas taxas de armazenamento do Azure.  As VMs do Azure são criadas e implicar custos, quando ocorre a ativação pós-falha. [Saiba mais](https://azure.microsoft.com/pricing/details/site-recovery/) sobre os encargos e preços.
[Base de Dados do Azure para MySQL](https://docs.microsoft.com/azure/mysql/) | A base de dados baseia-se no motor de servidor MySQL de open source. Fornece uma Comunidade completamente gerido, preparada para empresa base de dados MySQL, como um serviço para o desenvolvimento de aplicações e a implementação. 

 
## <a name="prerequisites"></a>Pré-requisitos

Se pretendem que a (e Contoso) executar este cenário, eis o que deve ter.

**Requisitos** | **Detalhes**
--- | ---
**Subscrição do Azure** | Deve já tiver criado uma subscrição durante os artigos nesta série. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Se criar uma conta gratuita, será o administrador da sua subscrição e poderá executar todas as ações.<br/><br/> Se utilizar uma subscrição existente e não tiver o administrador, terá de trabalhar com o administrador para lhe atribuir permissões de proprietário ou contribuinte.<br/><br/> Se precisar de mais granulares permissões, consulte [neste artigo](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Infraestrutura do Azure** | Contoso configurar a sua infraestrutura do Azure conforme descrito em [infraestrutura do Azure para a migração](contoso-migration-infrastructure.md).<br/><br/> Saiba mais sobre específicos [rede](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) e [armazenamento](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) requisitos para a recuperação de Site.
**Servidores no local** | O servidor do vCenter no local deve estar a executar a versão 5.5, 6.0 ou 6.5<br/><br/> Um anfitrião ESXi a executar a versão 5.5, 6.0 ou 6.5<br/><br/> Uma ou mais VMs de VMware no anfitrião ESXi a executar.
**VMs no local** | [Reveja os requisitos de Linux VM](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines) que são suportadas para migração com a recuperação de Site.<br/><br/> Certifique-se suportado [sistemas de ficheiros e armazenamento Linux](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage).<br/><br/> As VMs têm de cumprir [requisitos do Azure](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Passos do cenário

Eis como o Azure irá concluir a migração:

> [!div class="checklist"]
> * **Passo 1: Preparar do Azure Site Recovery**: criar uma conta de armazenamento do Azure para armazenar dados replicados e criar um cofre dos serviços de recuperação.
> * **Passo 2: Preparar VMware no local para a recuperação de Site**: preparar contas para a VM deteção e instalação do agente e preparar a ligação para VMs do Azure após a ativação pós-falha.
 * **Passo 3: aprovisionar a base de dados]**: no Azure, aprovisionar uma instância de base de dados MySQL do Azure.
> * **Passo 4: Replicar VMs**: configurar o ambiente de origem e destino de recuperação de sites, configurar uma política de replicação e iniciar a replicar VMs de armazenamento do Azure.
> * **Passo 5: Migrar a base de dados**: configurar a migração com ferramentas de MySQL.
> * **Passo 6: Migrar as VMs com a recuperação de Site**: executar uma ativação pós-falha de teste para se certificar de que tudo está a funcionar e, em seguida, execute uma ativação pós-falha completa para migrar as VMs no Azure.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Passo 1: Preparar do Azure para o serviço de recuperação de sites

Contoso tem de alguns componentes do Azure para a recuperação de sites:

- A ativação pós-falha uma VNet na qual se encontram recursos (Contoso utilizará a produção VNet já implementados).
- Uma nova conta de armazenamento do Azure para armazenar dados replicados. 
- Um cofre dos serviços de recuperação no Azure.

Contoso já criado a VNet durante [implementação da infraestrutura do Azure](contoso-migration-infrastructure.md), pelo que apenas necessitam para criar uma conta de armazenamento e o cofre.


1. Contoso cria uma conta de armazenamento do Azure (**contosovmsacc20180528**) na região EUA Leste 2.

    - A conta de armazenamento tem de estar na mesma região que o cofre dos Serviços de Recuperação.
    - A Contoso utiliza uma conta de objetivo geral, com o armazenamento padrão e replicação do LRS.

    ![Armazenamento de recuperação de site](./media/contoso-migration-rehost-linux-vm-mysql/asr-storage.png)

3. Com a conta de armazenamento e de rede no local, a Contoso cria um cofre (ContosoMigrationVault) e coloca-lo no **ContosoFailoverRG** grupo de recursos, a região EUA Leste 2 primária.

    ![Cofre dos Serviços de Recuperação](./media/contoso-migration-rehost-linux-vm-mysql/asr-vault.png)

**Precisa de mais ajuda?**

[Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) a configurar do Azure Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Passo 2: Preparar VMware no local para a recuperação de Site

Contoso prepara a infraestrutura do VMware no local da seguinte forma:

- Cria uma conta no vCenter server, para automatizar a deteção VM.
- Cria uma conta que permite a instalação automática do serviço de mobilidade em VMs de VMware que pretende replicar.
- Prepara VMs no local, para que possam ligar para VMs do Azure quando são criados após a migração.


### <a name="prepare-an-account-for-automatic-discovery"></a>Preparar uma conta para a deteção automática

O Site Recovery precisa de ter acesso a servidores VMware para:

- Detete automaticamente as VMs. É necessária, pelo menos, uma conta só de leitura.
- Orquestrar a replicação, a ativação pós-falha e a reativação pós-falha. Precisa de uma conta que pode executar operações, tais como criar e a remoção de discos e a ativar as VMs.

Contoso configura a conta da seguinte forma:

1. Contoso cria uma função ao nível do vCenter.
2. Contoso, em seguida, atribui essa função as permissões necessárias.


### <a name="prepare-an-account-for-mobility-service-installation"></a>Preparar uma conta para a instalação do serviço de Mobilidade

O serviço de mobilidade tem de ser instalado em cada VM que a Contoso pretende migrar.

- Recuperação de sites pode efetuar uma instalação push automática deste componente ao ativar a replicação para as VMs.
- Para a instalação automática. Recuperação de sites precisa de uma conta com permissões para aceder à VM. 
- Detalhes da conta são de entrada durante a configuração de replicação. 
- A conta pode ser domínio ou conta local, enquanto que tem as permissões de instalação.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparar a ligação para VMs do Azure após a ativação pós-falha

Após a ativação pós-falha para o Azure, a Contoso pretende conseguir ligar-se para as VMs do Azure. Para tal, que precisam para duas coisas: 

- Para aceder através da internet, dão SSH no Linux local no VM antes da migração.  Para Ubuntu Isto pode ser concluído com o seguinte comando: **apt Sudo-get ssh instalar -y**.
- Após a ativação pós-falha, deve verificar **diagnóstico de arranque** para ver uma captura de ecrã da VM.
- Se esta ação não funcionar, precisam de verificar que a VM está em execução e reveja estes [sugestões de resolução de problemas](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

**Precisa de mais ajuda?**

- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) criar e atribuir uma função para a deteção automática.
- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) criar uma conta da instalação push do serviço de mobilidade.


## <a name="step-3-provision-azure-database-for-mysql"></a>Passo 3: Aprovisionar a base de dados do Azure para MySQL

Contoso aprovisiona um MySQL da base de dados instância na região primária EUA Leste 2.

1. No portal do Azure, é criar uma base de dados do Azure para o recurso de MySQL. 

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-1.png)

2. Se adicionarem o nome **contosoosticket** para a base de dados do Azure. A base de dados adicionarem ao grupo de recursos de produção **ContosoRG**e especificar credenciais para o mesmo.
3. A base de dados MySQL no local é a versão 5.7, pelo que o se selecionarem esta versão de compatibilidade. Utilizam os tamanhos de predefinido, que corresponde aos respetivos requisitos de base de dados.

     ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-2.png)

4. Para **opções de redundância de cópia de segurança**, Contoso seleciona utilizar **Georredundante**. Esta opção permite-lhes para restaurar a base de dados na sua região EUA Central secundária se ocorrer uma falha. Só pode configurar esta opção quando aprovisionam a base de dados.

     ![Redundância](./media/contoso-migration-rehost-linux-vm-mysql/db-redundancy.png)

4. No **VNET-PROD-EUS2** rede > **pontos finais de serviço**, se adicionarem um ponto final do serviço (uma sub-rede de base de dados) para o serviço SQL Server.

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-3.png)

5. Depois de adicionar a sub-rede, se criarem uma regra de rede virtual que permite o acesso da sub-rede da base de dados na rede de produção.

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-4.png)


## <a name="step-4-replicate-the-on-premises-vms"></a>Passo 4: Replicar as VMs no local

Antes do podem migrar web VM para o Azure, Contoso configura e ativa a replicação.

### <a name="set-a-protection-goal"></a>Definir um objetivo de proteção

1. No cofre, sob o nome do cofre (ContosoVMVault) possam definir um objetivo de replicação (**introdução** > **recuperação de Site** > **preparar a infraestrutura**.
2. Se especificarem que as suas máquinas estão localizados no local, que está VMs de VMware, e que pretende replicar para o Azure.

    ![Objetivo da replicação](./media/contoso-migration-rehost-linux-vm-mysql/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Confirme o planeamento da implementação

Para continuar, confirmarem que concluiu se planeamento da implementação, selecionando **Sim, ter efetuado esta**. Contoso são migrar apenas uma única VM neste cenário e não precisa de planeamento da implementação.

### <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem

Contoso tem de configurar o respetivo ambiente de origem. Para tal, utilizando um modelo OVF que implementam um servidor de configuração da recuperação de sites como uma disponibilidade elevada, no local VM de VMware. Depois do servidor de configuração está a funcionar, se registar no cofre.

O servidor de configuração é executado um número de componentes:

- O componente de servidor de configuração que coordena as comunicações entre no local e o Azure e gere a replicação de dados.
- O servidor de processos atua como um gateway de replicação. Recebe dados de replicação; otimiza-os com a colocação em cache, compressão e encriptação; e envia-os para o armazenamento do Azure.
- O servidor de processos também instala o Serviço de Mobilidade nas VMs que pretende replicar, e efetua a deteção automática de VMs VMware no local.

Contoso execute estes passos da seguinte forma:


1. Transferem o modelo OVF **preparar infraestrutura** > **origem** > **servidor de configuração**.
    
    ![Transferir OVF](./media/contoso-migration-rehost-linux-vm-mysql/add-cs.png)

2. Importar o modelo para VMware para criar a VM e implemente a VM.

    ![Modelo OVF](./media/contoso-migration-rehost-linux-vm-mysql/vcenter-wizard.png)

3. Quando for ativado a VM pela primeira vez, arrancarem para uma experiência de instalação do Windows Server 2016. Aceitar o contrato de licença e introduza uma palavra-passe de administrador.
4. Após a conclusão da instalação, iniciam sessão no VM como administrador. No primeiro início de sessão, a ferramenta de configuração de recuperação de sites do Azure é executado por predefinição.
5. Na ferramenta, se especificarem um nome a utilizar para registar o servidor de configuração no cofre.
6. A ferramenta verifica se a VM pode ligar ao Azure.
7. Depois da ligação for estabelecida, podem iniciar sessão para a subscrição do Azure. As credenciais tem de ter acesso ao Cofre no qual que irá registar o servidor de configuração.

    ![Registar o servidor de configuração](./media/contoso-migration-rehost-linux-vm-mysql/config-server-register2.png)

8. A ferramenta executa algumas tarefas de configuração e, em seguida, é reiniciada.
9. Iniciar sessão máquina novamente e o Assistente de gestão do servidor de configuração é iniciado automaticamente.
10. No assistente, o utilizador selecionar o NIC para receber o tráfego de replicação. Não é possível alterar esta definição depois de estar configurada.
11. O utilizador selecionar a subscrição, o grupo de recursos e o Cofre registar o servidor de configuração.

    ![Cofre](./media/contoso-migration-rehost-linux-vm-mysql/cswiz1.png) 

12. Agora, transferir e instalar o servidor de MySQL e VMWare PowerCLI. 
13. Após a validação, se especificarem o FQDN ou endereço IP do anfitrião do servidor ou vSphere do vCenter. Deixe a porta predefinida e especifique um nome amigável para o servidor vCenter.
14. Introduza a conta que estes criados para a deteção automática e as credenciais que a recuperação de sites irá utilizar para instalar automaticamente o serviço de mobilidade. 

    ![vCenter](./media/contoso-migration-rehost-linux-vm-mysql/cswiz2.png)

14. Após a conclusão de registo, no portal do Azure, a Contoso verifica que o servidor de configuração e o servidor do VMware estão listadas no **origem** página no cofre. Deteção pode demorar 15 minutos ou mais. 
15. Com tudo no local, a recuperação de sites liga-se a servidores VMware e Deteta as VMs.

### <a name="set-up-the-target"></a>Configurar o destino

Agora Contoso entradas destino as definições de replicação.

1. No **preparar a infraestrutura** > **destino**, o utilizador de selecionar as definições de destino.
2. Recuperação de sites verifica que é uma conta de armazenamento do Azure e a rede de destino especificado.


### <a name="create-a-replication-policy"></a>Criar uma política de replicação

Com a origem e destino, configurar, a Contoso está pronta para criar uma política de replicação.

1. No **preparar a infraestrutura** > **as definições de replicação** > **política de replicação** >  **criar e Associar**, se criarem uma política **ContosoMigrationPolicy**.
2. Se utilizarem as predefinições:
    - **Limiar RPO**: predefinição de 60 minutos. Este valor define com que frequência são criados pontos de recuperação. Será gerado um alerta se a replicação contínua exceder este limite.
    - **Retenção do ponto de recuperação**. Predefinição de 24 horas. Este valor Especifica o tempo da janela de retenção para cada ponto de recuperação. As VMs replicadas podem ser recuperadas para qualquer ponto numa janela.
    - **Frequência de instantâneos consistentes com aplicação**. Predefinição de uma hora. Este valor Especifica a frequência com que são criados os instantâneos consistentes com aplicações.
 
        ![Criar política de replicação](./media/contoso-migration-rehost-linux-vm-mysql/replication-policy.png)

5. A política é associada automaticamente ao servidor de configuração. 

    ![Associar a política de replicação](./media/contoso-migration-rehost-linux-vm-mysql/replication-policy2.png)


**Precisa de mais ajuda?**

- Pode ler instruções completa de todos estes passos no [configurar a recuperação após desastre para as VMs de VMware no local](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Instruções detalhadas estão disponíveis para o ajudar a [configurar o ambiente de origem](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementar o servidor de configuração](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), e [configurar as definições de replicação](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Saiba mais](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) sobre o agente convidado do Azure para o Linux.

### <a name="enable-replication-for-the-web-vm"></a>Ativar a replicação para a VM Web

Agora Contoso pode iniciar a replicação a **OSTICKETWEB** VM.

1. No **replicar aplicação** > **origem** > **+ replicar** , selecionarem as definições de origem.
2. Indicam que pretende ativar máquinas virtuais e selecione as definições de origem, incluindo o servidor vCenter e o servidor de configuração.

    ![Ativar a replicação](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication-source.png)

3. Agora que especificarem as definições de destino. Estes incluem o grupo de recursos e rede em que a VM do Azure estarão localizada após a ativação pós-falha e a conta de armazenamento no qual serão armazenados os dados replicados. 

     ![Ativar a replicação](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication2.png)

3. Seleciona contoso **OSTICKETWEB** para replicação. 

    ![Ativar a replicação](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication3.png)

4. Nas propriedades da VM, se selecionarem a conta que deve ser utilizada para instalar automaticamente o serviço de mobilidade na VM.

     ![Serviço de Mobilidade](./media/contoso-migration-rehost-linux-vm-mysql/linux-mobility.png)

5. no **as definições de replicação** > **configurar as definições de replicação**, verifique a que a política de replicação correto é aplicada e selecione **ativar replicação**. O serviço de mobilidade será instalado automaticamente.
6.  Controlar o progresso de replicação no **tarefas**. Depois de a tarefa **Finalizar Proteção** ser executada, a máquina está preparada para ativação pós-falha.


**Precisa de mais ajuda?**

Pode ler instruções completa de todos estes passos no [ativar a replicação](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-5-migrate-the-database"></a>Passo 5: Migrar a base de dados

Contoso irá migrar a base de dados com cópia de segurança e restauro, MySQL ferramentas. Instalar o Workbench do MySQL, cópia de segurança da base de dados do OSTICKETMYSQL e, em seguida, restaurá-lo à base de dados do Azure para o servidor de MySQL.

### <a name="install-mysql-workbench"></a>Instalar MySQL Workbench

1. Verificações de contoso o [pré-requisitos e as transferências MySQL Workbench](https://dev.mysql.com/downloads/workbench/?utm_source=tuicool).
2. Instalarem MySQL Workbench para o Windows em conformidade com a [instruções de instalação](https://dev.mysql.com/doc/workbench/en/wb-installing.html).
3. No MySQL Workbench, se criarem uma ligação de MySQL a OSTICKETMYSQL. 

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench1.png)

4. Se exportar a base de dados como **osticket**, num ficheiro autónomo local.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench2.png)

5. Depois da base de dados foi efetuada cópia de localmente, se criarem uma ligação à base de dados do Azure para a instância de MySQL.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench3.png)

6. Agora, Contoso pode importar (restore) a base de dados na instância do Azure MySQL, o autónomo do ficheiro. É criado um novo esquema (osticket) para a instância.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench4.png)

## <a name="step-6-migrate-the-vms-with-site-recovery"></a>Passo 6: Migrar as VMs com a recuperação de Site

Contoso execute uma rápida ativação pós-falha de teste e, em seguida, migre a VM.

### <a name="run-a-test-failover"></a>Executar uma ativação pós-falha de teste

Executar uma ativação pós-falha de teste ajuda Certifique-se de que tudo está a funcionar conforme esperado, antes da migração. 

1. Contoso executa uma ativação pós-falha de teste para o ponto mais recente no tempo (**mais recentes processados**).
2. O utilizador selecionar **encerrar a máquina antes de iniciar a ativação pós-falha**, para que a recuperação de Site tenta encerre a VM de origem antes de acionar a ativação pós-falha. A ativação pós-falha continua, mesmo que o encerramento falhe. 
3. É executada a ativação pós-falha de teste: 

    - Verificação de um pré-requisitos é executado para se certificar de que todas as condições necessárias para a migração estão em vigor.
    - A ativação pós-falha processa os dados, para que possa ser criada uma VM do Azure. Se for selecionado o último ponto de recuperação, é criado um ponto de recuperação a partir dos dados.
    - É criada uma VM do Azure com base nos dados processados no passo anterior.

3. Após a ativação pós-falha, a réplica é apresentada VM do Azure no portal do Azure. Contoso verifica que a VM é o tamanho adequado, tenha ligado à rede à direita, e que está em execução. 
4. Depois de verificar, se limpar a ativação pós-falha e registam e guardar todas as observações.

### <a name="migrate-the-vm"></a>Migrar a VM

Para migrar a VM, Contoso cria um plano de recuperação que inclui a VM e efetuar o plano de ativação pós-falha para o Azure.

1. Cria um plano de Contoso e adiciona **OSTICKETWEB** ao mesmo.

    ![Plano de recuperação](./media/contoso-migration-rehost-linux-vm-mysql/recovery-plan.png)

2. Se executam uma ativação pós-falha do plano. Selecione o ponto de recuperação mais recente e especifique que a recuperação de sites deve tentar encerre a VM no local antes de acionar a ativação pós-falha. Pode seguir o progresso de ativação pós-falha **tarefas** página.

    ![Ativação pós-falha](./media/contoso-migration-rehost-linux-vm-mysql/failover1.png)

3. Durante a ativação pós-falha, o servidor vCenter emite comandos para parar as duas VMs em execução no anfitrião ESXi.

    ![Ativação pós-falha](./media/contoso-migration-rehost-linux-vm-mysql/vcenter-failover.png)

4. Após a ativação pós-falha, a Contoso verifica que a VM do Azure é apresentado como esperado no portal do Azure.

    ![Ativação pós-falha](./media/contoso-migration-rehost-linux-vm-mysql/failover2.png)  

5. Após verificar a VM, concluírem a migração. Esta ação deixa de replicação para a VM e deixa de faturação de recuperação de Site para a VM.

    ![Ativação pós-falha](./media/contoso-migration-rehost-linux-vm-mysql/failover3.png)

**Precisa de mais ajuda?**

- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) executar uma ativação pós-falha de teste. 
- [Saiba](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) como criar um plano de recuperação.
- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) efetuar a ativação pós-falha para o Azure.


### <a name="connect-the-vm-to-the-database"></a>Ligar a VM para a base de dados

Como o último passo no processo de migração, a Contoso atualizar a cadeia de ligação da aplicação para apontar para a base de dados do Azure para MySQL. 

1. Que efetuam uma ligação SSH para a VM OSTICKETWEB utilizando Putty ou outro cliente SSH. A VM é privada, pelo que se ligam utilizando o endereço IP privado.

    ![Ligar à base de dados](./media/contoso-migration-rehost-linux-vm-mysql/db-connect.png)  

    ![Ligar à base de dados](./media/contoso-migration-rehost-linux-vm-mysql/db-connect2.png)  

2. Estas definições de atualização para que o **OSTICKETWEB** VM pode comunicar com o **OSTICKETMYSQL** base de dados. A configuração está atualmente codificado com o endereço IP local 172.16.0.43.

    **Antes da atualização**
    
    ![Atualizar IP](./media/contoso-migration-rehost-linux-vm-mysql/update-ip1.png)  

    **Após a atualização**
    
    ![Atualizar IP](./media/contoso-migration-rehost-linux-vm-mysql/update-ip2.png) 
    
    ![Atualizar IP](./media/contoso-migration-rehost-linux-vm-mysql/update-ip3.png) 

3. Se reiniciem o serviço com **systemctl reiniciar apache2**.

    ![Reiniciar](./media/contoso-migration-rehost-linux-vm-mysql/restart.png) 

4. Por fim, atualize os registos DNS para **OSTICKETWEB**, um dos controladores de domínio Contoso.

    ![Atualizar DNS](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 


##  <a name="clean-up-after-migration"></a>Limpar após a migração

Migração completa, as camadas de aplicação osTicket estão em execução em VMs do Azure.

Agora, Contoso precisa de fazer o seguinte: 
- Remover as VMs de VMware vCenter inventário
- Remova as VMs no local das tarefas de cópia de segurança locais.
- Atualização de documentação interna Mostrar novas localizações e endereços IP. 
- Reveja quaisquer recursos que interagem com as VMs no local e atualizar as definições relevantes ou documentação para refletir a nova configuração.
- Contoso utilizado o serviço Azure migrar com o mapeamento de dependência para avaliar o **OSTICKETWEB** VM para a migração. Agora que devem remover os agentes (Microsoft Monitoring Agent/dependência Agent) que são instaladas para este fim, da VM.

## <a name="review-the-deployment"></a>Reveja a implementação

Com a aplicação agora em execução, Contoso tem totalmente operacionalizar e proteger a sua infraestrutura de novo.

### <a name="security"></a>Segurança

A equipa de segurança de Contoso reveja a VM e a base de dados para determinar os problemas de segurança.

- Reveja estes grupos de segurança de rede (NSGs) para a VM, para controlar o acesso. Os NSGs são utilizados para garantir que apenas o tráfego permitido para a aplicação pode passar.
- Se considerar a proteger os dados nos discos VM através de encriptação de disco e Azure KeyVault.
- Comunicação entre a instância VM e a base de dados não está configurada para SSL. Precisam de fazê-lo para se certificar de que o tráfego de base de dados não pode ser vítima de acesso ilícito.

[Leia mais](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) sobre práticas de segurança para as VMs.

### <a name="backups"></a>Cópias de segurança

- Contoso irá criar cópias de segurança de dados na VM utilizando o serviço de cópia de segurança do Azure. [Saiba mais](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- Não precisam de configurar a cópia de segurança da base de dados. Base de dados do Azure para MySQL cria automaticamente cópias de segurança do servidor e armazena. Tiver selecionado a utilizar a redundância geográfica para a base de dados, pelo que é resiliente e prontos para produção.

### <a name="licensing-and-cost-optimization"></a>Otimização de licenciamento e custo

- Depois de implementar recursos, Contoso atribui etiquetas do Azure, de acordo com das decisões que tomou durante o [infraestrutura do Azure](contoso-migration-infrastructure.md#set-up-tagging) implementação.
- Não existem não existem problemas de licenciamento para os servidores de Contoso Ubuntu.
- Contoso irá ativar a gestão de custo de Azure licenciados pelos Cloudyn, uma subsidiária Microsoft. É uma solução de gestão de custo de nuvem multi ajuda-o a utilizar e gerir o Azure e outros recursos de nuvem.  [Saiba mais](https://docs.microsoft.com/azure/cost-management/overview) sobre a gestão de custo do Azure.


## <a name="next-steps"></a>Passos Seguintes

Neste cenário é mostrado o cenário de realojamento final que Contoso tentou. Migrar a VM da aplicação no local Linux osTicket de front-end para uma VM do Azure e migrar a base de dados de aplicação para uma instância do Azure MySQL.

O conjunto seguinte de tutoriais na série de migração, vamos mostrar-lhe como Contoso efetuar um conjunto mais complexo de migrações, que envolve refactoring de aplicação, em vez de migrações de comparação de precisão e shift simples.
