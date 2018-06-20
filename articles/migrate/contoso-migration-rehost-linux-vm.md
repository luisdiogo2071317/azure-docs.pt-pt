---
title: Migrar de realojamento e realojamento uma aplicação do Linux no local para VMs do Azure | Microsoft Docs
description: Saiba como Contoso realojamento uma aplicação do Linux no local ao migrar para as VMs do Azure.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: 8f039884ca0ea46c128078984d6cab6fd29ac9af
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36220555"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms"></a>Migração de Contoso: realojamento uma aplicação do Linux no local para VMs do Azure

Este artigo mostra como Contoso é realojar uma aplicação de secretária de serviço baseado em Linux no local (**osTicket**), para VMs IaaS do Azure.

Este documento é seventh numa série de artigos que documente a forma como a empresa fictícia com Contoso migra respetivos recursos no local para a nuvem do Microsoft Azure. A série inclui informações de fundo e um conjunto de cenários que ilustra como configurar uma infraestrutura de migração e executar diferentes tipos de migrações. Cenários de crescimento em complexidade e será adicionado artigos adicionais ao longo do tempo.

**Artigo** | **Detalhes** | **Estado**
--- | --- | ---
[Artigo 1: Descrição geral](contoso-migration-overview.md) | Fornece uma descrição geral da estratégia de migração da Contoso, a série de artigo e as aplicações de exemplo, que vamos utilizar. | Disponível
[Artigo 2: Implementar uma infraestrutura do Azure](contoso-migration-infrastructure.md) | Descreve como Contoso prepara a infraestrutura do Azure e no local para a migração. A mesma infraestrutura é utilizada para todos os cenários de migração de Contoso. | Disponível
[Artigo 3: Avaliar a recursos no local](contoso-migration-assessment.md)  | Mostra como Contoso executa uma avaliação da respetiva aplicação de SmartHotel de duas camadas no local em execução no VMware. Avaliar a VMs de aplicação com o [Azure migrar](migrate-overview.md) serviço e a base de dados de SQL Server de aplicação com o [Assistente de migração de base de dados do Azure](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponível
[Artigo 4: Realojamento para as VMs do Azure e uma instância do SQL gerida](contoso-migration-rehost-vm-sql-managed-instance.md) | Demonstra como Contoso migra a aplicação de SmartHotel para o Azure. São migradas o front-end de aplicação VM utilizando [do Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)e a base de dados de aplicação utilizando o [migração de base de dados do Azure](https://docs.microsoft.com/azure/dms/dms-overview) serviço, para migrar para uma instância do SQL Server geridos. | Disponível
[Artigo 5: Realojamento para as VMs do Azure](contoso-migration-rehost-vm.md) | Mostra como Contoso migrar a respetiva aplicação SmartHotel VMs utilizando apenas a recuperação de sites.
[Artigo 6: Realojamento para as VMs do Azure e grupos de disponibilidade do SQL Server](contoso-migration-rehost-vm-sql-ag.md) | Mostra como Contoso migra a aplicação de SmartHotel. Podem utilizar a recuperação de sites para migrar a aplicação VMs e o serviço de base de dados de migração para migrar a base de dados de aplicação para um grupo de disponibilidade do SQL Server. | Disponível
Artigo 7: Realojamento uma aplicação do Linux para VMs do Azure (Este artigo) | Mostra como Contoso migra a respetiva aplicação de Linux osService utilizando o Azure Site Recovery.
[Artigo 8: Uma aplicação do Linux para as VMs do Azure e o servidor do Azure MySQL de realojamento](contoso-migration-rehost-linux-vm-mysql.md) | Demonstra como Contoso migra a aplicação do Linux osService, utilizar a recuperação de sites para a migração de VM e o MySQL Workbench para migrar (para uma instância de servidor de MySQL do Azure. | Disponível

Neste artigo, Contoso irá migrar as duas camadas **osTicket** aplicação, em execução no Linux Apache MySQL PHP (LAMP) para o Azure. A aplicação VMs será migrada utilizando o serviço do Azure Site Recovery. Se gostaria de utilizar esta aplicação de open source, poderá transferi-lo de [GitHub](https://github.com/osTicket/osTicket).

## <a name="business-drivers"></a>Controladores de negócio

A equipa de TI liderança trabalhou coincida com os respetivos parceiros de negócios para compreender o que pretende alcançar com esta migração:

- **Crescimento de negócio de endereços**: Contoso está a crescer e, como resultado existe pressão na sistemas no local e infraestrutura.
- **Limitar o risco**: O suporte técnico do serviço de aplicações é fundamental para as empresas de Contoso. Pretende movê-la para o Azure com o risco de zero.
- **Expandir**: não pretende alterar agora a aplicação. Pretendem simplesmente para se certificar de que está estável.


## <a name="migration-goals"></a>Objetivos de migração

A equipa de nuvem Contoso tiver afixado baixo objetivos para migração, para determinar o melhor método de migração:

- Após a migração, a aplicação no Azure deve ter as mesmas capacidades de desempenho, como sucede no respetivo ambiente de VMWare no local.  A aplicação irá permanecer como crítica na nuvem porque está no local. 
- Contoso não quer investir nesta aplicação.  É importante para o negócio, mas o respetivo formato atual simplesmente pretendem movê-lo em segurança para a nuvem.
- Contoso não pretende alterar o modelo do OPS Manager para esta aplicação. Pretende interagir com ele na nuvem da mesma forma que são agora.
- Contoso não pretende alterar as funcionalidades de aplicação. Apenas a localização da aplicação será alterado.
- Ter concluído alguns migrações de aplicação do Windows, a Contoso pretende aprender a utilizar uma infraestrutura baseada em Linux no Azure.

## <a name="proposed-architecture"></a>Arquitetura proposta

Neste cenário:

- A aplicação está em camadas através de duas VMs (OSTICKETWEB e OSTICKETMYSQL).
- As VMs estão localizadas no anfitrião do VMware ESXi **contosohost1.contoso.com** (versão 6.5).
- O ambiente VMware é gerido pelo vCenter Server 6.5 (**vcenter.contoso.com**), em execução numa VM.
- Contoso tem um centro de dados no local (**contoso datacenter**), com um controlador de domínio no local (**contosodc1**).
- Uma vez que a aplicação uma carga de trabalho de produção, as VMs do Azure irão residir no grupo de recursos de produção **ContosoRG**.
- As VMs serão migradas para a região primária (EUA Leste 2) e colocadas na rede de produção (VNET-PROD-EUS2):
    - O web VM irá residir da sub-rede do front-end (PROD-FE-EUS2).
    - A base de dados VM irá residir na sub-rede da base de dados (PROD-DB-EUS2).
- As VMs no local no Centro de dados Contoso irão ser desativadas após a migração é efetuada.

![Arquitetura do cenário](./media/contoso-migration-rehost-linux-vm/architecture.png) 

## <a name="migration-process"></a>processo de migração

Contoso irá migrar da seguinte forma:

1. Como primeiro passo, Contoso define cópias de segurança do Azure e a infraestrutura necessária para implementar a recuperação de Site no local.
2. Depois de preparar os componentes do Azure e no local, estes configurar e ativar a replicação para as VMs.
3. Após a replicação está a funcionar, migre as VMs por consegui-los através do Azure.

![processo de migração](./media/contoso-migration-rehost-linux-vm/migration-process.png)

### <a name="azure-services"></a>Serviços do Azure

**Serviço** | **Descrição** | **Custo**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | O serviço orquestra e gere a migração e recuperação após desastre para as VMs do Azure e no local VMs e servidores físicos.  | Durante a replicação para o Azure, são cobradas taxas de armazenamento do Azure.  As VMs do Azure são criadas e implicar custos, quando ocorre a ativação pós-falha. [Saiba mais](https://azure.microsoft.com/pricing/details/site-recovery/) sobre os encargos e preços.

 
## <a name="prerequisites"></a>Pré-requisitos

Eis o (e Contoso) necessita para este cenário.

**Requisitos** | **Detalhes**
--- | ---
**Subscrição do Azure** | Deve já tiver criado uma subscrição durante os artigos nesta série. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Se criar uma conta gratuita, será o administrador da sua subscrição e poderá executar todas as ações.<br/><br/> Se utilizar uma subscrição existente e não tiver o administrador, terá de trabalhar com o administrador para lhe atribuir permissões de proprietário ou contribuinte.<br/><br/> Se precisar de mais granulares permissões, consulte [neste artigo](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Infraestrutura do Azure** | Contoso configurar a sua infraestrutura do Azure conforme descrito em [infraestrutura do Azure para a migração](contoso-migration-infrastructure.md).<br/><br/> Saiba mais sobre específicos [rede](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) e [armazenamento](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) requisitos para a recuperação de Site.
**Servidores no local** | O servidor do vCenter no local deve estar a executar a versão 5.5, 6.0 ou 6.5<br/><br/> Um anfitrião ESXi a executar a versão 5.5, 6.0 ou 6.5<br/><br/> Uma ou mais VMs de VMware no anfitrião ESXi a executar.
**VMs no local** | [Reveja máquinas Linux](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines) que são suportadas para migração com a recuperação de Site.<br/><br/> Certifique-se suportado [sistemas de ficheiros e armazenamento Linux](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage).<br/><br/> As VMs têm de cumprir [requisitos do Azure](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Passos do cenário

Eis como o Azure irá concluir a migração:

> [!div class="checklist"]
> * **Passo 1: Preparar do Azure Site Recovery**: criar uma conta de armazenamento do Azure para armazenar dados replicados e criar um cofre dos serviços de recuperação.
> * **Passo 2: Preparar VMware no local para a recuperação de Site**: preparar contas a ser utilizada para a VM deteção e instalação do agente e preparar a ligação para VMs do Azure após a ativação pós-falha.
> * **Passo 3: Replicar VMs**: configurar o ambiente de migração de origem e de destino, criar uma política de replicação e iniciar a replicar VMs de armazenamento do Azure.
> * **Passo 4: Migrar as VMs com a recuperação de Site**: executar uma ativação pós-falha de teste para se certificar de que tudo está a funcionar e, em seguida, execute uma ativação pós-falha completa para migrar as VMs no Azure.


## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Passo 1: Preparar do Azure para o serviço de recuperação de sites

Contoso tem de alguns componentes do Azure para a recuperação de sites:

- A ativação pós-falha uma VNet na qual se encontram recursos (Contoso utilizará a produção VNet já implementados)
- Uma nova conta de armazenamento do Azure para armazenar dados replicados. 
- Um cofre dos serviços de recuperação no Azure.

Contoso já criado a VNet durante [implementação da infraestrutura do Azure](contoso-migration-infrastructure.md), pelo que apenas necessitam para criar uma conta de armazenamento e o cofre.

1. Contoso cria uma conta de armazenamento do Azure (contosovmsacc20180528) na região EUA Leste 2.

    - A conta de armazenamento tem de estar na mesma região que o cofre dos Serviços de Recuperação.
    - Se estiver a utilizar uma conta de objetivo geral, com armazenamento padrão e replicação do LRS.

    ![Armazenamento de recuperação de site](./media/contoso-migration-rehost-linux-vm/asr-storage.png)

2. Com a conta de armazenamento e de rede no local, a Contoso criar um cofre (ContosoMigrationVault) e colocar o **ContosoFailoverRG** grupo de recursos, a região EUA Leste 2 primária.

    ![Cofre dos Serviços de Recuperação](./media/contoso-migration-rehost-linux-vm/asr-vault.png)


**Precisa de mais ajuda?**

[Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) a configurar do Azure Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Passo 2: Preparar VMware no local para a recuperação de Site

Contoso prepara a infraestrutura do VMware no local da seguinte forma:

- Crie uma conta no vCenter server ou vSphere ESXi anfitrião, ao automatizar a deteção VM.
- Crie uma conta que permite a instalação automática do serviço de mobilidade em VMs de VMware que pretende replicar.
- Prepare VMs no local, para que possam ligar para VMs do Azure quando são criados após a migração.


### <a name="prepare-an-account-for-automatic-discovery"></a>Preparar uma conta para a deteção automática

O Site Recovery precisa de ter acesso a servidores VMware para:

- Detete automaticamente as VMs. É necessária, pelo menos, uma conta só de leitura.
- Orquestrar a replicação, a ativação pós-falha e a reativação pós-falha. Precisa de uma conta que pode executar operações, tais como criar e a remoção de discos e a ativar as VMs.

Contoso configura a conta da seguinte forma:

1. Contoso cria uma função ao nível do vCenter.
2. Contoso, em seguida, atribui essa função as permissões necessárias.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Preparar uma conta para a instalação do serviço de Mobilidade

O serviço de mobilidade tem de estar instalado nas VMs do Linux que Contoso da migração:

- Recuperação de sites pode efetuar uma instalação push automática deste componente ao ativar a replicação para as VMs.
- Para a instalação push automática, terá de preparar uma conta que a recuperação de sites irá utilizar para aceder as VMs.
- Detalhes de contas são de entrada durante a configuração de replicação. 
- A conta pode ser o domínio ou conta local, com permissões para instalar em VMs.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparar a ligação para VMs do Azure após a ativação pós-falha

Após a ativação pós-falha para o Azure, a Contoso pretende conseguir estabelecer ligação às VMs replicadas no Azure. Para tal, há alguns aspetos que precisam para:

- Para aceder através da internet, dão SSH no Linux local no VM antes da migração.  Para Ubuntu Isto pode ser concluído com o seguinte comando: **apt Sudo-get ssh instalar -y**.
- Após a migração (ativação pós-falha) são executados, deve verificar **diagnóstico de arranque** para ver uma captura de ecrã da VM.
- Se isto não funcionar, deve verificar que a VM está em execução e reveja estes [sugestões de resolução de problemas](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**Precisa de mais ajuda?**

- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) criar e atribuir uma função para a deteção automática.
- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) criar uma conta da instalação push do serviço de mobilidade.


## <a name="step-3-replicate-the-on-premises-vms"></a>Passo 3: Replicar as VMs no local

Antes do podem migrar web VM para o Azure, Contoso configura e ativa a replicação.

### <a name="set-a-protection-goal"></a>Definir um objetivo de proteção

1. No cofre, sob o nome do cofre (ContosoVMVault) possam definir um objetivo de replicação (**introdução** > **recuperação de Site** > **preparar a infraestrutura**.
2. Se especificarem que as suas máquinas estão localizados no local, que está VMs de VMware, e que pretende replicar para o Azure.
    ![Objetivo de replicação](./media/contoso-migration-rehost-linux-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Confirme o planeamento da implementação

Para continuar, confirmarem que concluiu se planeamento da implementação, selecionando **Sim, ter efetuado esta**. Contoso são migrar apenas uma única VM neste cenário e não precisa de planeamento da implementação.

### <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem

Contoso tem de configurar o respetivo ambiente de origem. Para tal, podem transferir um modelo OVF e utilizá-lo para implementar o servidor de configuração da recuperação de sites como altamente disponível, VM de VMware no local. Depois do servidor de configuração está a funcionar, se registar no cofre.

O servidor de configuração é executado um número de componentes:

- O componente de servidor de configuração que coordena as comunicações entre no local e o Azure e gere a replicação de dados.
- O servidor de processos atua como um gateway de replicação. Recebe dados de replicação; otimiza-os com a colocação em cache, compressão e encriptação; e envia-os para o armazenamento do Azure.
- O servidor de processos também instala o Serviço de Mobilidade nas VMs que pretende replicar, e efetua a deteção automática de VMs VMware no local.

Contoso execute estes passos da seguinte forma:

1. Transferem o modelo OVF **preparar infraestrutura** > **origem** > **servidor de configuração**.
    
    ![Transferir OVF](./media/contoso-migration-rehost-linux-vm/add-cs.png)

2. Importar o modelo para VMware para criar a VM e implemente a VM.

    ![Modelo OVF](./media/contoso-migration-rehost-linux-vm/vcenter-wizard.png)

3. Quando for ativado a VM pela primeira vez, arrancarem para uma experiência de instalação do Windows Server 2016. Aceitar o contrato de licença e introduza uma palavra-passe de administrador.
4. Após a conclusão da instalação, iniciam sessão na VM como administrador. No primeiro início de sessão, a ferramenta de configuração de recuperação de sites do Azure é executado por predefinição.
5. Na ferramenta, se especificarem um nome a utilizar para registar o servidor de configuração no cofre.
6. A ferramenta verifica se a VM pode ligar ao Azure. Depois da ligação for estabelecida, podem iniciar sessão para a subscrição do Azure. As credenciais têm de ter acesso ao cofre no qual pretende registar o servidor de configuração.

    ![Registar o servidor de configuração](./media/contoso-migration-rehost-linux-vm/config-server-register2.png)

7. A ferramenta executa algumas tarefas de configuração e, em seguida, é reiniciada.
8. Iniciar sessão máquina novamente e o Assistente de gestão do servidor de configuração é iniciado automaticamente.
9. No assistente, o utilizador selecionar o NIC para receber o tráfego de replicação. Não é possível alterar esta definição depois de estar configurada.
10. O utilizador selecionar a subscrição, o grupo de recursos e o Cofre registar o servidor de configuração.

    ![Cofre](./media/contoso-migration-rehost-linux-vm/cswiz1.png) 

11. Estes, em seguida, transferiram e instalar o servidor de MySQL e VMWare PowerCLI. 
12. Após a validação, se especificarem o FQDN ou endereço IP do anfitrião do servidor ou vSphere do vCenter. Deixe a porta predefinida e especifique um nome amigável para o servidor vCenter.
13. Se especificarem a conta que estes criados para a deteção automática e as credenciais que devem ser utilizadas para instalar automaticamente o serviço de mobilidade.

    ![vCenter](./media/contoso-migration-rehost-linux-vm/cswiz2.png)

14. Após a conclusão de registo, no portal do Azure, a Contoso verifica que o servidor de configuração e o servidor do VMware estão listadas no **origem** página no cofre. Deteção pode demorar 15 minutos ou mais. 
15. Recuperação de sites, em seguida, liga-se a servidores VMware e Deteta as VMs.

### <a name="set-up-the-target"></a>Configurar o destino

Agora Contoso configura as definições de replicação de destino.

1. No **preparar a infraestrutura** > **destino**, o utilizador de selecionar as definições de destino.
2. Recuperação de sites verifica que é uma conta de armazenamento do Azure e a rede de destino especificado.

### <a name="create-a-replication-policy"></a>Criar uma política de replicação

Depois da origem e destino estão configurados, Contoso está pronta para criar uma política de replicação.

1. No **preparar a infraestrutura** > **as definições de replicação** > **política de replicação** >  **criar e Associar**, se criarem uma política **ContosoMigrationPolicy**.
2. Se utilizarem as predefinições:
    - **Limiar RPO**: predefinição de 60 minutos. Este valor define com que frequência são criados pontos de recuperação. Será gerado um alerta se a replicação contínua exceder este limite.
    - **Retenção do ponto de recuperação**. Predefinição de 24 horas. Este valor Especifica o tempo da janela de retenção para cada ponto de recuperação. As VMs replicadas podem ser recuperadas para qualquer ponto numa janela.
    - **Frequência de instantâneos consistentes com aplicação**. Predefinição de uma hora. Este valor Especifica a frequência com que são criados os instantâneos consistentes com aplicações.
 
        ![Criar política de replicação](./media/contoso-migration-rehost-linux-vm/replication-policy.png)

5. A política é associada automaticamente ao servidor de configuração. 

    ![Associar a política de replicação](./media/contoso-migration-rehost-linux-vm/replication-policy2.png)

**Precisa de mais ajuda?**

- Pode ler instruções completa de todos estes passos no [configurar a recuperação após desastre para as VMs de VMware no local](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Instruções detalhadas estão disponíveis para o ajudar a [configurar o ambiente de origem](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementar o servidor de configuração](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), e [configurar as definições de replicação](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Saiba mais](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) sobre o agente convidado do Azure para o Linux.

**Precisa de mais ajuda?**

- Pode ler instruções completa de todos estes passos no [configurar a recuperação após desastre para as VMs de VMware no local](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Instruções detalhadas estão disponíveis para o ajudar a [configurar o ambiente de origem](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementar o servidor de configuração](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), e [configurar as definições de replicação](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Saiba mais](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) sobre o agente convidado do Azure para o Linux.



### <a name="enable-replication-for-osticketweb"></a>Ativar a replicação para OSTICKETWEB

Agora Contoso pode iniciar a replicação a **OSTICKETWEB** VM.

1. No **replicar aplicação** > **origem** > **+ replicar** , selecionarem as definições de origem.
2. O utilizador selecionar se pretende ativar máquinas virtuais, selecione as definições de origem, incluindo o servidor vCenter e o servidor de configuração.

    ![Ativar a replicação](./media/contoso-migration-rehost-linux-vm/enable-replication-source.png)

3. Se especificarem as definições de destino, incluindo o grupo de recursos e VNet em que a VM do Azure estarão localizada após a ativação pós-falha e a conta de armazenamento no qual serão armazenados os dados replicados.

     ![Ativar a replicação](./media/contoso-migration-rehost-linux-vm/enable-replication2.png)

3. Seleciona contoso **OSTICKETWEB** para replicação. 

    - Nesta fase Contoso seleciona apenas **OSTICKETWEB** porque VNet e sub-rede tem de ser selecionados e as VMs que não estão na mesma sub-rede.
    - Recuperação de site instala automaticamente o serviço de mobilidade quando a replicação está ativada para a VM.

    ![Ativar a replicação](./media/contoso-migration-rehost-linux-vm/enable-replication3.png)

4. Nas propriedades da VM, Contoso seleciona a conta que é utilizada pelo servidor de processos para instalar automaticamente o serviço de mobilidade na máquina.

     ![Serviço de Mobilidade](./media/contoso-migration-rehost-linux-vm/linux-mobility.png)

5. no **as definições de replicação** > **configurar as definições de replicação**, verifique a que a política de replicação correto é aplicada e selecione **ativar replicação**.
6.  Controlar o progresso de replicação no **tarefas**. Depois de a tarefa **Finalizar Proteção** ser executada, a máquina está preparada para ativação pós-falha.



### <a name="enable-replication-for-osticketmysql"></a>Ativar a replicação para OSTICKETMYSQL

Agora Contoso pode iniciar a replicação **OSTICKETMYSQL**.

1. No **replicar aplicação** > **origem** > **+ replicar** , selecionarem as definições de origem e de destino.
2. Seleciona contoso **OSTICKETMYSQL** para replicação e seleciona a conta a utilizar para a instalação do serviço de mobilidade.

    ![Ativar a replicação](./media/contoso-migration-rehost-linux-vm/mysql-enable.png)

3. Contoso aplica-se a mesma política de replicação que foi utilizada para OSTICKETWEB e ativa a replicação.  

**Precisa de mais ajuda?**

Pode ler instruções completa de todos estes passos no [ativar a replicação](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-4-migrate-the-vms"></a>Passo 4: Migrar as máquinas virtuais 

Contoso execute uma rápida ativação pós-falha de teste e, em seguida, migre as VMs.

### <a name="run-a-test-failover"></a>Executar uma ativação pós-falha de teste

Executar uma ativação pós-falha de teste ajuda a assegurar que tudo está a funcionar conforme esperado antes da migração. 

1. Contoso executa uma ativação pós-falha de teste para o ponto mais recente no tempo (**mais recentes processados**).
2. O utilizador selecionar **encerrar a máquina antes de iniciar a ativação pós-falha**, para que a recuperação de Site tenta encerre a VM de origem antes de acionar a ativação pós-falha. A ativação pós-falha continua, mesmo que o encerramento falhe. 
3. É executada a ativação pós-falha de teste: 
    - Verificação de um pré-requisitos é executado para se certificar de que todas as condições necessárias para a migração estão em vigor.
    - A ativação pós-falha processa os dados, para que possa ser criada uma VM do Azure. Se o ponto de recuperação mais recente estiver selecionado, é criado um ponto de recuperação dos dados.
    - É criada uma VM do Azure com base nos dados processados no passo anterior.
3. Após a ativação pós-falha, a réplica é apresentada VM do Azure no portal do Azure. Se verificar que a VM é o tamanho adequado, tenha ligado à rede à direita, e que está em execução. 
4. Depois de verificar, se limpar a ativação pós-falha e registam e guardar todas as observações.

### <a name="create-and-customize-a-recovery-plan"></a>Criar e personalizar um plano de recuperação

 Depois de verificar que a ativação pós-falha de teste trabalhado conforme esperado, Contoso criar um plano de recuperação para a migração. 

- Um plano de recuperação Especifica a ordem em que a ativação pós-falha ocorre, como VMs do Azure serão reencaminhadas cópias de segurança no Azure.
- Uma vez que pretende migrar uma aplicação de duas camadas, estes irá personalizar o plano de recuperação para que os dados de VM (SQLVM) é iniciado antes do front-end (WEBVM).


1. No **planos de recuperação (recuperação de sites)** > **+ plano de recuperação**, criam um plano e adicionar VMs ao mesmo.

    ![Plano de recuperação](./media/contoso-migration-rehost-linux-vm/recovery-plan.png)

2. Depois de criar o plano, selecionarem para personalização (**planos de recuperação** > **OsTicketMigrationPlan** > **personalizar**.
3.  Podem remover **OSTICKETWEB** de **grupo 1: Iniciar**.  Isto garante que a primeira ação de iniciação afeta **OSTICKETMYSQL** apenas.

    ![Grupo de recuperação](./media/contoso-migration-rehost-linux-vm/recovery-group1.png)

4.  No **+ grupo** > **adicionar itens protegidos**, adicionarem **OSTICKETWEB** para **grupo 2: Iniciar**.  Contoso tem estes dois grupos diferentes.

    ![Grupo de recuperação](./media/contoso-migration-rehost-linux-vm/recovery-group2.png)


### <a name="migrate-the-vms"></a>Migrar as máquinas virtuais


Contoso está pronto para executar uma ativação pós-falha no plano de recuperação, para migrar as VMs.

1. O utilizador selecionar o plano > **ativação pós-falha**.
2.  O utilizador selecionar para efetuar a ativação pós-falha do ponto de recuperação mais recente e especifique que a recuperação de sites deve tentar encerre a VM no local antes de acionar a ativação pós-falha. Pode seguir o progresso de ativação pós-falha **tarefas** página.

    ![Ativação pós-falha](./media/contoso-migration-rehost-vm/failover1.png)

3. Durante a ativação pós-falha, o servidor vCenter emite comandos para parar as duas VMs em execução no anfitrião ESXi.

    ![Ativação pós-falha](./media/contoso-migration-rehost-linux-vm/vcenter-failover.png)

3. Após a ativação pós-falha, Contoso, certifique-se de que a VM do Azure é apresentado como esperado no portal do Azure.

    ![Ativação pós-falha](./media/contoso-migration-rehost-linux-vm/failover2.png)  

3. Depois de verificar a VM no Azure, concluírem a migração para concluir o processo de migração para cada VM. Esta ação deixa de replicação para a VM e deixa de faturação de recuperação de Site para a VM.

    ![Ativação pós-falha](./media/contoso-migration-rehost-linux-vm/failover3.png)


### <a name="connect-the-vm-to-the-database"></a>Ligar a VM para a base de dados

Como o último passo no processo de migração, Contoso atualizar a cadeia de ligação da aplicação para apontar para a base de dados de aplicação em execução no **OSTICKETMYSQL** VM. 

1. Que efetuam uma ligação SSH para a **OSTICKETWEB** VM ao utilizar o Putty ou outro cliente SSH. A VM é privada, pelo que se ligam utilizando o endereço IP privado.

    ![Ligar à base de dados](./media/contoso-migration-rehost-linux-vm/db-connect.png)  

    ![Ligar à base de dados](./media/contoso-migration-rehost-linux-vm/db-connect2.png)  

2. Tem de se certificar de que o **OSTICKETWEB** VM pode comunicar com o **OSTICKETMYSQL** VM. A configuração está atualmente codificado com o endereço IP local 172.16.0.43.

    **Antes da atualização**
    
    ![Atualizar IP](./media/contoso-migration-rehost-linux-vm/update-ip1.png)  

    **Após a atualização**
    
    ![Atualizar IP](./media/contoso-migration-rehost-linux-vm/update-ip2.png) 
    
3. Se reiniciem o serviço com **systemctl reiniciar apache2**.

    ![Reiniciar](./media/contoso-migration-rehost-linux-vm/restart.png) 

4. Por fim, atualize os registos DNS para **OSTICKETWEB** e **OSTICKETMYSQL**, um dos controladores de domínio Contoso.

    ![Atualizar DNS](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

    ![Atualizar DNS](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

**Precisa de mais ajuda?**

- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) executar uma ativação pós-falha de teste. 
- [Saiba](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) como criar um plano de recuperação.
- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) efetuar a ativação pós-falha para o Azure.

## <a name="clean-up-after-migration"></a>Limpar após a migração

Migração completa, as camadas de aplicação osTicket estão agora em execução em VMs do Azure.

Agora, Contoso precisa de fazer alguns limpeza:  

- As VMs no local podem remover a partir do inventário do vCenter.
- As VMs no local podem remover das tarefas de cópia de segurança locais.
- Atualizar a sua documentação interna para mostrar a nova localização e endereços IP para OSTICKETWEB e OSTICKETMYSQL.
- Reveja quaisquer recursos que interagem com as VMs e atualizar as definições relevantes ou documentação para refletir a nova configuração.
- Contoso utilizado o serviço Azure migrar com o mapeamento de dependência para avaliar as VMs para a migração. Que devem remover o Microsoft Monitoring Agent e o agente de dependência que instaladas para este fim, da VM.

## <a name="review-the-deployment"></a>Reveja a implementação

Com a aplicação agora em execução, Contoso tem totalmente operacionalizar e proteger a sua infraestrutura de novo.

### <a name="security"></a>Segurança

A equipa de segurança de Contoso reveja o OSTICKETWEB e OSTICKETMYSQLVMs para determinar os problemas de segurança.

- Reveja os grupos de segurança de rede (NSGs) para as VMs controlar o acesso. Os NSGs são utilizados para garantir que apenas o tráfego permitido para a aplicação pode passar.
- São também considerar a proteger os dados nos discos VM através de encriptação de disco e Azure KeyVault.

[Leia mais](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) sobre práticas de segurança para as VMs.

### <a name="backups"></a>Cópias de segurança

Contoso cópias de segurança de dados em VMs utilizando o serviço de cópia de segurança do Azure. [Saiba mais](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Otimização de licenciamento e custo

- Depois de implementar recursos, Contoso atribui etiquetas do Azure tal como definido durante a [implementação da infraestrutura do Azure](contoso-migration-infrastructure.md#set-up-tagging).
- Contoso tem sem problemas com os respetivos servidores Ubuntu licenciamento.
- Contoso irá ativar a gestão de custo de Azure licenciados pelos Cloudyn, uma subsidiária Microsoft. É uma solução de gestão de custo de nuvem multi ajuda-o a utilizar e gerir o Azure e outros recursos de nuvem.  [Saiba mais](https://docs.microsoft.com/azure/cost-management/overview) sobre a gestão de custo do Azure. 


## <a name="next-steps"></a>Passos Seguintes

Neste artigo, que vamos mostrou como Contoso migrados um local para o suporte técnico do app service a camadas em duas VMs do Linux para VMs do IaaS do Azure, utilizando o Azure Site Recovery.

No seguinte artigo da série, vamos mostrar-lhe como Contoso migrar a mesma aplicação de serviço de suporte técnico para o Azure. Este tempo, a Contoso utiliza a recuperação de sites para migrar a VM de front-end da aplicação e migrar a base de dados de aplicação utilizando a cópia de segurança e restaurar a base de dados do Azure para MySQL, utilizando a ferramenta de workbench MySQL. [Introdução ao](contoso-migration-rehost-linux-vm-mysql.md).
