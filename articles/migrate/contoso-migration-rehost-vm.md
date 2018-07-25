---
title: Realojar uma aplicação da Contoso com a migração para VMs do Azure com o Azure Site Recovery | Documentos da Microsoft
description: Saiba como realojar a uma aplicação no local e com uma migração lift-and-shift para o Azure para a migração de máquinas no local através do Azure Site Recovery de serviço.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: e152a95a392fd503e027591a4930fbeef744d6f4
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009421"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms"></a>Migração de Contoso: realojar a uma aplicação no local para VMs do Azure


Este artigo demonstra como Contoso realojar a aplicação de SmartHotel no local no Azure, ao migrar as VMs da aplicação para as VMs do Azure.


Este documento é um de uma série de artigos que mostram como a empresa fictícia Contoso migra recursos n local para a cloud do Microsoft Azure. A série contém informações gerais e cenários que ilustram como configurar uma infraestrutura de migração, avaliação dos recursos no local para migração e execução de diferentes tipos de migrações. Cenários de crescem em complexidade e vamos adicionar artigos adicionais ao longo do tempo.


**Artigo** | **Detalhes** | **Estado**
--- | --- | ---
[Artigo 1: Descrição geral](contoso-migration-overview.md) | Fornece uma descrição geral da estratégia de migração da Contoso, a série de artigos e as aplicações de exemplo que usamos. | Disponível
[Artigo 2: Implementar uma infraestrutura do Azure](contoso-migration-infrastructure.md) | Descreve como o Contoso prepara a sua infraestrutura do Azure e no local, para a migração. A mesma infra-estrutura é utilizada para todos os artigos de migração. | Disponível
[Artigo 3: Avaliar a recursos no local para migração para o Azure](contoso-migration-assessment.md)  | Mostra como Contoso é executado uma avaliação de uma aplicação de SmartHotel de duas camadas no local em execução no VMware. Contoso avalia as VMs de aplicação com o [do Azure Migrate](migrate-overview.md) serviço e a base de dados do SQL Server do aplicativo com o [Assistente de migração de base de dados](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponível
[Artigo 4: Realojar a uma aplicação em VMs do Azure e uma instância gerida de SQL](contoso-migration-rehost-vm-sql-managed-instance.md) | Demonstra como Contoso é executado uma migração lift-and-shift para o Azure para a aplicação de SmartHotel no local. Contoso migra o front-end de aplicação VM através de [do Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)e a base de dados de aplicação para a instância gerida de SQL, utilizando o [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Disponível
Artigo 5: Realojar a uma aplicação em VMs do Azure | Mostra como o Contoso migrar as VMs da aplicação SmartHotel para VMs do Azure, com o serviço Site Recovery. | Este artigo.
[Artigo 6: Realojar a uma aplicação em VMs do Azure e SQL Server sempre no grupo de disponibilidade](contoso-migration-rehost-vm-sql-ag.md) | Mostra como a Contoso migra a aplicação de SmartHotel. A Contoso utiliza o Site Recovery para migrar as VMs da aplicação e o serviço de migração de base de dados para migrar a base de dados de aplicação para um cluster do SQL Server protegido por um grupo de Disponibilidade AlwaysOn. | Disponível
[Artigo 7: Realojar uma aplicação do Linux em VMs do Azure](contoso-migration-rehost-linux-vm.md) | Mostra como Contoso faz uma migração lift-and-shift da aplicação de osTicket do Linux para VMs do Azure, com o Site Recovery | Disponível
[Artigo 8: Realojar uma aplicação do Linux em VMs do Azure e MySQL do Azure](contoso-migration-rehost-linux-vm-mysql.md) | Demonstra como a Contoso migra a aplicação de osTicket do Linux para as VMs do Azure com o Site Recovery e migra a base de dados de aplicação para uma instância do servidor MySQL do Azure com o MySQL Workbench. | Disponível
[Artigo 9: Refatorizar uma aplicação na base de dados de aplicações Web do Azure e SQL do Azure](contoso-migration-refactor-web-app-sql.md) | Demonstra como Contoso migra a aplicação de SmartHotel para uma aplicação Web do Azure e migra a base de dados de aplicação para a instância de servidor SQL do Azure | Disponível
[Artigo 10: Refatorizar uma aplicação do Linux em aplicações Web do Azure e MySQL do Azure](contoso-migration-refactor-linux-app-service-mysql.md) | Mostra como a Contoso migra a aplicação de osTicket do Linux para aplicações Web do Azure em vários sites, integrados com o GitHub para a entrega contínua. Estes migrar a base de dados de aplicação para uma instância do MySQL do Azure. | Disponível
[Artigo 11: Refatorar o TFS no VSTS](contoso-migration-tfs-vsts.md) | Mostra como a Contoso migra a implantação do Team Foundation Server (TFS) no local ao migrá-lo para Visual Studio Team Services (VSTS) no Azure. | Disponível
[Artigo 12: Rearquitetar uma aplicação em contentores do Azure e base de dados do Azure SQL](contoso-migration-rearchitect-container-sql.md) | Mostra como Contoso migra e rearchitects a respetiva aplicação SmartHotel para o Azure. Eles rearquitetar a camada de web de aplicação como um contentor do Windows e a base de dados de aplicação numa base de dados SQL do Azure. | Disponível
[Artigo 13: Recriar uma aplicação no Azure](contoso-migration-rebuild.md) | Mostra como Contoso reconstruir a sua aplicação de SmartHotel com uma gama de capacidades do Azure e serviços, incluindo serviços de aplicações, Kubernetes do Azure, as funções do Azure, serviços cognitivos e Cosmos DB. | Disponível



Neste artigo, a Contoso vai migrar o Windows de duas camadas. Aplicação de NET SmartHotel em execução em VMs de VMware, para o Azure. Se pretender utilizar esta aplicação, são fornecidas como código-fonte aberto e pode baixá-lo partir [github](https://github.com/Microsoft/SmartHotel360).



## <a name="business-drivers"></a>Fatores comerciais

A equipe de liderança de TI tem trabalhado em estreita colaboração com parceiros de negócios para compreender o que quer alcançar com esta migração:

- **Resolver o crescimento do negócio**: Contoso está a crescer e assim há pressão em seus sistemas no local e a infraestrutura.
- **Limitar o risco**: SmartHotel a aplicação é fundamental para a empresa Contoso. Eles querem para movê-lo para o Azure com zero risco.
- **Expandir**: Contoso não quer modificar a aplicação. Simplesmente eles querem garantir que ele seja estável.


## <a name="migration-goals"></a>Objetivos de migração

A equipe de cloud de Contoso tiver afixado para baixo de objetivos para essa migração. Esses objetivos são utilizados para determinar o melhor método de migração:

- Após a migração, a aplicação no Azure deve ter as mesmas capacidades de desempenho, como sucede no VMware.  A aplicação permanecerá como crítica na cloud, pois é no local. 
- Contoso não quer investir nesta aplicação.  É importante para os negócios, mas em sua forma atual eles simplesmente querem movê-la em segurança para a cloud.
- Contoso não quer alterar o modelo de ops para esta aplicação. Eles querem interagir com ele, na cloud, da mesma forma que eles fazem agora.
- Contoso não quer alterar qualquer funcionalidade de aplicação. Apenas a localização da aplicação serão alterados.

## <a name="proposed-architecture"></a>Arquitetura proposta

Aqui está o ambiente atual

- A aplicação está em camadas em duas VMs (**WEBVM** e **SQLVM**).
- As VMs estão localizadas no anfitrião ESXi do VMware **contosohost1.contoso.com** (versão 6.5).
- O ambiente do VMware é gerido pelo vCenter Server 6.5 (**vcenter.contoso.com**), em execução numa VM.
- A Contoso tiver um datacenter no local (contoso-datacenter), com um controlador de domínio no local (**contosodc1**).
- As VMs no local no Centro de dados Contoso serão encerradas depois de terminar a migração.

![Arquitetura do cenário](./media/contoso-migration-rehost-vm/architecture.png) 

## <a name="migration-process"></a>Processo de migração

Contoso migrará o front-end de aplicação e a base de dados VMs para as VMs do Azure com o Site Recovery:

- Como primeiro passo, eles irá preparar e configurar os componentes do Azure para o Site Recovery e preparar a infraestrutura de VMware no local.
- Que já têm seus [infraestrutura do Azure](contoso-migration-infrastructure.md) no lugar, por isso, basta que adicionar alguns componentes do Azure especificamente para o Site Recovery.
- Com tudo o que preparado, eles podem começar a replicar as VMs.
-Após a replicação está ativada e a trabalhar, eles migrar a VM ao failover para o Azure.

![Processo de migração](./media/contoso-migration-rehost-vm/migraton-process.png) 



### <a name="azure-services"></a>Serviços do Azure

**Serviço** | **Descrição** | **Custo**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | O serviço orquestra e gere a migração e recuperação após desastre para VMs do Azure e VMs e servidores físicos no local.  | Durante a replicação para o Azure, são cobradas taxas de armazenamento do Azure.  VMs do Azure são criadas e incorrer em custos, quando ocorre a ativação pós-falha. [Saiba mais](https://azure.microsoft.com/pricing/details/site-recovery/) sobre preços e custos.


## <a name="prerequisites"></a>Pré-requisitos

Aqui está o que (e Contoso) precisam executar este cenário.

**Requisitos** | **Detalhes**
--- | ---
**Subscrição do Azure** | Deve já tiver criado uma subscrição durante os primeiros artigos desta série. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Se criar uma conta gratuita, será o administrador da sua subscrição e poderá executar todas as ações.<br/><br/> Se utilizar uma subscrição existente e não for o administrador, terá de trabalhar com o administrador para lhe atribuir permissões de proprietário ou contribuinte.<br/><br/> Se precisar de permissões mais granulares, reveja [este artigo](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Infraestrutura do Azure** | [Saiba como](contoso-migration-infrastructure.md) Contoso configurar uma infraestrutura do Azure.<br/><br/> Saiba mais sobre específicas [rede](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) e [armazenamento](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) requisitos para a recuperação de Site.
**Servidores no local** | Servidores vCenter devem estar a executar a versão 5.5, 6.0 ou 6.5 no local<br/><br/> Anfitriões ESXi devem executar a versão 5.5, 6.0 ou 6.5<br/><br/> Uma ou mais VMs de VMware deve estar em execução no anfitrião ESXi.
**VMs no local** | As VMs têm de cumprir [requisitos do Azure](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Passos do cenário

Eis como Contoso será executada a migração:

> [!div class="checklist"]
> * **Passo 1: Preparar o Azure para o Site Recovery**: criação de uma conta de armazenamento do Azure para manter os dados replicados e um cofre dos serviços de recuperação.
> * **Passo 2: Preparar o VMware no local para o Site Recovery**: preparar contas para instalação de deteção e o agente da VM e preparar a ligação para as VMs do Azure após a ativação pós-falha.
> * **Passo 3: Replicar VMs**: configurar a replicação e iniciar a replicação de VMs para o armazenamento do Azure.
> * **Passo 4: Migrar as VMs com o Site Recovery**: executar uma ativação pós-falha de teste para se certificar de que está tudo a funcionar e, em seguida, execute uma ativação pós-falha completa para migrar as VMs para o Azure.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Passo 1: Preparar o Azure para o serviço Site Recovery

Aqui estão os componentes do Azure, a Contoso precisa de migrar as VMs para o Azure:

- Uma VNet na qual as VMs do Azure estarão localizadas quando forem criadas durante a ativação pós-falha.
- Uma conta de armazenamento do Azure para armazenar dados replicados. 
- Um cofre dos serviços de recuperação no Azure.

Eles configurar estes da seguinte forma:

1. Contoso já configurado uma rede podem utilizar para o Site Recovery quando eles [implementado a infraestrutura do Azure](contoso-migration-infrastructure.md)

    - A aplicação de SmartHotel é uma aplicação de produção e as VMs serão migradas para a rede de produção do Azure (VNET-PROD-EUS2) na região E.U.A. Leste 2 primária.
    - Ambas as VMs serão colocadas no grupo de recursos ContosoRG, que é utilizado para recursos de produção.
    - O front-end de aplicação VM (WEBVM) serão migrados para a sub-rede de front-end (PROD-FE-EUS2), na rede de produção.
    - A base de dados de aplicação VM (SQLVM) serão migrados para a sub-rede de base de dados (PROD-DB-EUS2), na rede de produção.

2. Contoso cria uma conta de armazenamento do Azure (contosovmsacc20180528) na região primária.
    - A conta de armazenamento tem de estar na mesma região que o cofre dos Serviços de Recuperação.
    - Utilizam uma conta para fins gerais, com o armazenamento standard e replicação LRS. 

    ![Armazenamento de recuperação de site](./media/contoso-migration-rehost-vm/asr-storage.png)

3. Com a conta de armazenamento e de rede no local, Contoso agora cria um cofre de serviços de recuperação (ContosoMigrationVault) e o coloca no grupo de recursos ContosoFailoverRG na região E.U.A. Leste 2 primária.

    ![Cofre dos Serviços de Recuperação](./media/contoso-migration-rehost-vm/asr-vault.png)

**Precisa de mais ajuda?**

[Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) configurar o Azure para o Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Passo 2: Preparar o VMware no local para o Site Recovery

Eis o que Contoso prepara no local:

- Uma conta no vCenter server ou vSphere ESXi no anfitrião, para automatizar a deteção de VMS.
- Uma conta que permite a instalação automática do serviço de mobilidade nas VMs de VMware. 
- No local as definições da VM, para que a Contoso pode ligar às VMs do Azure replicadas após a ativação pós-falha.


### <a name="prepare-an-account-for-automatic-discovery"></a>Preparar uma conta para a deteção automática

O Site Recovery precisa de ter acesso a servidores VMware para:

- Detetar automaticamente VMs. 
- Orquestre a replicação, ativação pós-falha e reativação pós-falha para VMs.
- É necessária, pelo menos, uma conta só de leitura. A conta deve ser capaz de executar operações como criar e remover discos e Ativando a VMs.

Contoso configura a conta da seguinte forma:

1. É possível criar uma função ao nível do vCenter.
2. Essa função que atribuir as permissões necessárias.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Preparar uma conta para a instalação do serviço de Mobilidade

O serviço de mobilidade tem de ser instalado em cada VM.

- Site Recovery pode fazer uma instalação push automática do serviço de mobilidade quando a replicação de VM é ativada.
- É necessária, uma conta para que o Site Recovery pode acessar as VMs para a instalação push. Especificar esta conta quando configurar a replicação.
- A conta pode ser de domínio ou local, com permissões para instalar nas VMs.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparar a ligação para VMs do Azure após a ativação pós-falha

Após a ativação pós-falha, a Contoso quer ligar às VMs do Azure. Para fazer isso, o que fazem o seguinte antes da migração:

1. Para ter acesso através da internet:

 - Ative o RDP na VM no local antes da ativação pós-falha
 - Certifique-se de que as regras TCP e UDP estão adicionadas para o **público** perfil.
 - Verifique se o RDP é permitido na **Firewall do Windows** > **aplicações permitidas** para todos os perfis.
 
2. Para ter acesso através de VPN de site a site, eles:

 - Ative o RDP na máquina no local.
 - Permitir RDP no **Firewall do Windows** -> **aplicações e funcionalidades permitidas**, para **domínio e privadas** redes.
 - Definir política de SAN do sistema operacional na VM no local para **OnlineAll**.

Além disso, quando executam uma ativação pós-falha que precisam verificar o seguinte:

- Não deve haver nenhuma atualização do Windows pendentes na VM quando acionar uma ativação pós-falha. Se existirem, não poderá iniciar sessão na VM, enquanto a atualização for concluída.
- Após a ativação pós-falha, pode verificar **diagnósticos de arranque** para ver uma captura de ecrã da VM. Se não funcionar, deve verificar que a VM está em execução e revisá-los [sugestões de resolução de problemas](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**Precisa de mais ajuda?**

- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) criar e atribuir uma função para a deteção automática.
- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) criar uma conta para a instalação push do serviço de mobilidade.


## <a name="step-3-replicate-the-on-premises-vms"></a>Passo 3: Replicar as VMs no local

Para poder executar uma migração para o Azure, Contoso precisa de configurar e ativar a replicação.

### <a name="set-a-replication-goal"></a>Definir um objetivo de replicação

1. No cofre, sob o nome do cofre (ContosoVMVault) selecionarem um objetivo de replicação (**introdução** > **Site Recovery** > **preparar infraestrutura** .
2. Especifica que suas máquinas estão localizados no local, em execução no VMware e replicar para o Azure.

    ![Objetivo da replicação](./media/contoso-migration-rehost-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Confirmar o planeamento da implementação

Para continuar, confirmarem o que eles concluam planeamento da implementação, selecionando **Sim, já o fiz**. Neste cenário, a Contoso está a migrar apenas duas VMs e não precisa de planeamento da implementação.


### <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem

A Contoso precisa de configurar o seu ambiente de origem. Para fazer isso, eles transferir um modelo OVF e utilizá-lo para implementar o servidor de configuração de recuperação de sites como uma elevada disponibilidade, a VM de VMware no local. Depois do servidor de configuração está a funcionar, eles registá-lo no cofre.

O servidor de configuração é executada um número de componentes:

- O componente de servidor de configuração que coordena as comunicações entre no local e o Azure e gere a replicação de dados.
- O servidor de processos atua como um gateway de replicação. Recebe dados de replicação; otimiza-os com a colocação em cache, compressão e encriptação; e envia-os para o armazenamento do Azure.
- O servidor de processos também instala o Serviço de Mobilidade nas VMs que pretende replicar, e efetua a deteção automática de VMs VMware no local.



Contoso efetua estes passos da seguinte forma:

1. No cofre, transferem o modelo OVF **preparar infraestrutura** > **origem** > **servidor de configuração**.
    
    ![Transferir OVF](./media/contoso-migration-rehost-vm/add-cs.png)

2. Eles importam o modelo para o VMware, para criar e implementar a VM.

    ![Modelo OVF](./media/contoso-migration-rehost-vm/vcenter-wizard.png)

3.  Quando ativam a VM pela primeira vez, ele arranca para uma experiência de instalação do Windows Server 2016. Aceite o contrato de licença e introduza uma palavra-passe de administrador.
4. Após a instalação terminar, iniciam sessão na VM como administrador. No primeiro início de sessão, a ferramenta de configuração do Azure Site Recovery é executado por predefinição.
5. Na ferramenta, eles especificam um nome para registar o servidor de configuração no cofre.
6. A ferramenta verifica se a VM pode ligar ao Azure. Depois da ligação é estabelecida, iniciam sessão para a subscrição do Azure. As credenciais têm de ter acesso ao Cofre no qual eles irá registar o servidor de configuração.

    ![Registar servidor de configuração](./media/contoso-migration-rehost-vm/config-server-register2.png)

7. A ferramenta executa algumas tarefas de configuração e, em seguida, é reiniciada.
8. Iniciam sessão na máquina novamente e o Assistente de gestão de servidor de configuração é iniciado automaticamente.
9. No assistente, eles selecionam o NIC para receber o tráfego de replicação. Não é possível alterar esta definição depois de estar configurada.
10. Selecionar a subscrição, grupo de recursos e o Cofre no qual pretende registar o servidor de configuração.
        ![Cofre](./media/contoso-migration-rehost-vm/cswiz1.png) 

10. Eles transferirem e instalar o servidor MySQL e o VMWare PowerCLI. 
11. Após a validação, eles especificam o FQDN ou endereço IP do vCenter server ou vSphere no anfitrião. Deixe a porta predefinida e especifique um nome amigável para o servidor no Azure.
12. Especifica a conta que o próprio criou para a deteção automática e as credenciais que são utilizadas para instalar automaticamente o serviço de mobilidade. Para máquinas Windows, a conta tem privilégios de administrador local nas VMs.

    ![vCenter](./media/contoso-migration-rehost-vm/cswiz2.png)

7. Após a conclusão do registo, no portal do Azure, a Contoso duplo verifica que o servidor de configuração e o servidor VMware estão listados no **origem** página no cofre. Deteção pode demorar 15 minutos ou mais. 
8. Site Recovery, em seguida, liga-se aos servidores VMware com as definições especificadas e Deteta as VMs.

### <a name="set-up-the-target"></a>Configurar o destino

Agora Contoso Especifica as definições de replicação de destino.

1. Na **preparar infraestrutura** > **destino**, eles selecionam as definições de destino.
2. Recuperação de sites verifica que existe uma conta de armazenamento do Azure e a rede na localização de destino especificado.

### <a name="create-a-replication-policy"></a>Criar uma política de replicação

Agora a Contoso pode criar uma política de replicação.

1. Na **preparar infraestrutura** > **definições de replicação** > **política de replicação** >  **criar e Associar**, criaram uma política **ContosoMigrationPolicy**.
2. Se utilizarem as predefinições:
    - **Limiar RPO**: predefinição de 60 minutos. Este valor define com que frequência são criados pontos de recuperação. Será gerado um alerta se a replicação contínua exceder este limite.
    - **Retenção do ponto de recuperação**. Predefinição de 24 horas. Este valor especifica quanto tempo dura o período de retenção para cada ponto de recuperação. As VMs replicadas podem ser recuperadas para qualquer ponto numa janela.
    - **Frequência de instantâneos consistentes com a aplicação**. Padrão de uma hora. Este valor Especifica a frequência com que os instantâneos consistentes com aplicações são criados.

        ![Criar política de replicação](./media/contoso-migration-rehost-vm/replication-policy.png)

5. A política é associada automaticamente ao servidor de configuração. 

    ![Associar a política de replicação](./media/contoso-migration-rehost-vm/replication-policy2.png)

### <a name="enable-replication-for-webvm"></a>Ativar a replicação para WEBVM

Com tudo instalado, Contoso pode ativar a replicação para as VMs. Eles começam com WebVM.

1. Na **replicar aplicação** > **origem** > **+ replicar** selecionarem as definições de origem.
2. Eles indicam que se deve ativar VMs, selecione o servidor vCenter e o servidor de configuração.

    ![Ativar a replicação](./media/contoso-migration-rehost-vm/enable-replication1.png)

3. Selecionar as definições de destino, incluindo o grupo de recursos e a rede do Azure e a conta de armazenamento.

     ![Ativar a replicação](./media/contoso-migration-rehost-vm/enable-replication2.png)

4. Contoso seleciona **WebVM** para a replicação, verifica a política de replicação e permite a replicação.

    - Nesse estágio o Contoso seleciona apenas WEBVM porque VNet e sub-rede tem de ser selecionadas e, a Contoso é colocar as VMs da aplicação em sub-redes diferentes.
    - Site Recovery instala automaticamente o serviço de mobilidade na VM quando a replicação está ativada.

    ![Ativar a replicação](./media/contoso-migration-rehost-vm/enable-replication3.png)

5. Controlar o progresso de replicação em **tarefas**. Depois de a tarefa **Finalizar Proteção** ser executada, a máquina está preparada para ativação pós-falha.
6. Na **Essentials** no portal do Azure, Contoso pode ver a estrutura para as VMs a replicar para o Azure.


### <a name="enable-replication-for-sqlvm"></a>Ativar a replicação para SQLVM

Agora Contoso pode começar a replicar a máquina SQLVM, usando o mesmo processo, como mostrado acima.

1. Selecionar definições da origem.

    ![Ativar a replicação](./media/contoso-migration-rehost-vm/enable-replication1.png)

2. Eles, em seguida, especificam as definições de destino.

     ![Ativar a replicação](./media/contoso-migration-rehost-vm/enable-replication2-sqlvm.png)

3. Eles selecionam SQLVM para replicação. 

    ![Ativar a replicação](./media/contoso-migration-rehost-vm/enable-replication3-sqlvm.png)

4. Aplicar a mesma política de replicação que foi utilizada pela WEBVM e ativar a replicação.

    ![Vista de infraestrutura](./media/contoso-migration-rehost-vm/essentials.png)

**Precisa de mais ajuda?**

- Pode ler um passo a passo completa de todos estes passos [configurar a recuperação após desastre das VMs de VMware no local](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Instruções detalhadas estão disponíveis para ajudá-lo [configurar o ambiente de origem](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementar o servidor de configuração](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), e [configurar as definições de replicação](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- Pode saber mais sobre [ativar a replicação](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-4-migrate-the-vms"></a>Passo 4: Migrar as VMs 

Contoso é executada uma ativação pós-falha de teste rápida e, em seguida, uma ativação pós-falha completa para migrar as VMs.

### <a name="run-a-test-failover"></a>Executar uma ativação pós-falha de teste

Uma ativação pós-falha de teste ajuda a garantir que está tudo a funcionar conforme esperado. 

1. Contoso executa uma ativação pós-falha de teste para o ponto mais recente no tempo (**processado mais recentemente**).
2. Eles selecionam **encerrar a máquina antes de iniciar a ativação pós-falha**, para que o Site Recovery tenta encerrar a VM de origem antes de acionar a ativação pós-falha. A ativação pós-falha continua, mesmo que o encerramento falhe. 
3. Execuções de ativação pós-falha de teste: 

    - Uma verificação dos pré-requisitos é executado para certificar-se de que todas as condições necessárias para a migração estão em vigor.
    - A ativação pós-falha processa os dados, para que possa ser criada uma VM do Azure. Se for selecionado o último ponto de recuperação, é criado um ponto de recuperação a partir dos dados.
    - É criada uma VM do Azure com base nos dados processados no passo anterior.
    
3. Depois de concluída a ativação pós-falha, a réplica é apresentada VM do Azure no portal do Azure. Contoso verifica que a VM tem um tamanho adequado, ligado à rede certa e está em execução. 
4. Depois de verificar a ativação pós-falha de teste, eles limpar a ativação pós-falha e registram e guarde todas as observações. 

### <a name="create-and-customize-a-recovery-plan"></a>Criar e personalizar um plano de recuperação

 Depois de verificar que a ativação pós-falha de teste funcionou como esperado, a Contoso cria um plano de recuperação para a migração. 

- Um plano de recuperação Especifica a ordem na qual ativação pós-falha ocorre e indica como as VMs do Azure irá ser colocadas online no Azure.
- Uma vez que a aplicação é duas camadas, eles personalizar o plano de recuperação para que os dados a VM (SQLVM) é iniciado antes do front-end (WEBVM).

1. Na **(recuperação de sites) de planos de recuperação** > **+ plano de recuperação**, criam um plano e adicionar as VMs ao mesmo.

    ![Plano de recuperação](./media/contoso-migration-rehost-vm/recovery-plan.png)

2. Depois de criar o plano, eles personalizá-lo (**planos de recuperação** > **SmartHotelMigrationPlan** > **personalizar**).
2.  Remova WEBVM partir **grupo 1: Iniciar**.  Isto garante que a primeira ação de início afeta SQLVM apenas.
3.  Na **+ grupo** > **itens protegidos de adicionar**, adicionem WEBVM ao grupo 2: Iniciar.  As VMs tem de estar em dois grupos diferentes.


### <a name="migrate-the-vms"></a>Migrar as VMs


Agora a Contoso pode executar uma ativação pós-falha completa para concluir a migração.

1. Eles selecionam o plano de recuperação > **ativação pós-falha**.
2. Devem selecionar para efetuar a ativação pós-falha do ponto de recuperação mais recente, e esse Site Recovery tente encerrar a VM no local antes de acionar a ativação pós-falha. Pode seguir o progresso de ativação pós-falha **tarefas** página.

    ![Ativação pós-falha](./media/contoso-migration-rehost-vm/failover1.png)


3. Após a ativação pós-falha, eles Certifique-se de que a VM do Azure é apresentado conforme esperado no portal do Azure.

    ![Ativação pós-falha](./media/contoso-migration-rehost-vm/failover2.png)  

3. Após a verificação, eles concluírem a migração para cada VM. Para a replicação para a VM e interrompe a faturação do Site Recovery para o mesmo.

    ![Ativação pós-falha](./media/contoso-migration-rehost-vm/failover3.png)

**Precisa de mais ajuda?**

- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) executar uma ativação pós-falha de teste. 
- [Saiba](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) como criar um plano de recuperação.
- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) efetuar a ativação pós-falha para o Azure.

## <a name="clean-up-after-migration"></a>Limpar após a migração

Com a migração é concluída, os escalões de aplicação SmartHotel estão agora em execução em VMs do Azure.

Agora, a Contoso precisa de concluir estes passos de limpeza:  

- Remova a máquina WEBVM no inventário do vCenter.
- Remova a máquina SQLVM no inventário do vCenter.
- Remova WEBVM e SQLVM das tarefas de cópia de segurança locais.
- Atualize a documentação interna para mostrar a nova localização e os endereços IP para as VMs.
- Reveja todos os recursos que interagem com as VMs e atualizar as definições relevantes ou documentação para refletir a nova configuração.

## <a name="review-the-deployment"></a>Reveja a implementação

Com a aplicação agora em execução, a Contoso precisa agora totalmente operacionalizar e protegê-lo no Azure.

### <a name="security"></a>Segurança

A equipe de segurança da Contoso analisa as VMs do Azure, para determinar os problemas de segurança.

- Para controlar o acesso, ele Revise os grupos de segurança de rede (NSGs) para as VMs. NSGs são utilizados para garantir que apenas o tráfego permitido para a aplicação pode aceder ao mesmo.
- Eles também consideram a proteção dos dados no disco com o Azure Disk Encryption e o Cofre de chaves.

[Leia mais](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) sobre práticas de segurança para as VMs.

### <a name="backups"></a>Cópias de segurança

Contoso vai fazer backup dos dados nas VMs com o serviço de cópia de segurança do Azure. [Saiba mais](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Otimização de licenciamento e de custo

1. Contoso tem o licenciamento existentes para as suas VMs e irá tirar partido do benefício híbrido do Azure.  Eles converterá as VMs do Azure existente, para tirar partido destes preços.
2. Contoso irá ativar o Azure Cost Management licenciado pela Cloudyn, uma subsidiária da Microsoft. É uma solução de gestão de custos multiclouds que o ajuda a utilizar e gerir o Azure e outros recursos da cloud. [Saiba mais](https://docs.microsoft.com/azure/cost-management/overview) sobre o Azure Cost Management. 

## <a name="conclusion"></a>Conclusão

Neste artigo, a Contoso rehosted a aplicação de SmartHotel no Azure ao migrar as VMs da aplicação para VMs do Azure com o serviço Site Recovery. 


## <a name="next-steps"></a>Passos Seguintes

Na [próximo artigo](contoso-migration-rehost-vm-sql-ag.md) da série, vamos mostrar-lhe como Contoso realojar SmartHotel aplicações front-end VM numa VM do Azure e migra a base de dados para um grupo de Disponibilidade AlwaysOn do SQL Server no Azure.

