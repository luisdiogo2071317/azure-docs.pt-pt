---
title: Uma aplicação Contoso com a migração para as VMs do Azure com o Azure Site Recovery de realojamento | Microsoft Docs
description: Saiba como realojamento uma aplicação no local e com uma migração de comparação de precisão e shift para o Azure para a migração de máquinas no local utilizando o Azure Site Recovery de serviço.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/30/2018
ms.author: raynew
ms.openlocfilehash: 11b5e2a408d3ba514753f3510b36fce02470c6e9
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825264"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms"></a>Migração de Contoso: realojamento uma aplicação no local para VMs do Azure


Este artigo demonstra como Contoso realojamento a respetiva aplicação SmartHotel no Azure, ao migrar a aplicação de VMs para VMs do Azure.


Este documento é um de uma série de artigos que mostram como a empresa fictícia com Contoso migra os seus recursos no local para a nuvem do Microsoft Azure. A série inclui cenários que ilustram a configurar uma infraestrutura de migração, avaliar a recursos no local para a migração e com diferentes tipos de migrações e informações de fundo. Cenários de crescimento em complexidade e iremos adicionar artigos adicionais ao longo do tempo.


**Artigo** | **Detalhes** | **Estado**
--- | --- | ---
Artigo 1: Descrição geral | Fornece uma descrição geral da estratégia de migração da Contoso, a série de artigo e as aplicações de exemplo, que vamos utilizar. | Disponível
Artigo 2: Implementar uma infraestrutura do Azure | Descreve como Contoso prepara a infraestrutura do Azure e no local para a migração. A mesma infraestrutura é utilizada para todos os cenários de migração de Contoso. | Disponível
Artigo 3: Avaliar a recursos no local  | Mostra como Contoso executa uma avaliação da respetiva aplicação de SmartHotel de duas camadas no local em execução no VMware. Avaliar a VMs de aplicação com o [Azure migrar](migrate-overview.md) serviço e a base de dados de SQL Server de aplicação com o [Assistente de migração de base de dados do Azure](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponível
Artigo 4: Refatorar (comparação de precisão e shift) para as VMs do Azure e uma instância do SQL geridos (Este artigo) | Demonstra como Contoso migra a aplicação de SmartHotel para o Azure. São migradas o front-end de aplicação VM utilizando [do Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)e a base de dados de aplicação utilizando o [migração de base de dados do Azure](https://docs.microsoft.com/azure/dms/dms-overview) serviço, para migrar para uma instância do SQL Server geridos. | Disponível
Artigo 5: Refatorar (comparação de precisão e shift) para as VMs do Azure (Este artigo) | Mostra como Contoso migrar a respetiva aplicação SmartHotel VMs utilizando apenas a recuperação de sites.
Artigo 6: Refatorar (comparação de precisão e shift) para as VMs do Azure e grupos de disponibilidade do SQL Server | Mostra como Contoso migra a aplicação de SmartHotel. Podem utilizar a recuperação de sites para migrar a aplicação VMs e o serviço de base de dados de migração para migrar a base de dados de aplicação para um grupo de disponibilidade do SQL Server. | Disponível
Artigo 7: Refatorar (comparação de precisão e shift) para as VMs do Azure e o servidor de MySQL do Azure | Demonstra como Contoso migra SmartHotel aplicação VMs com a recuperação de sites e o MySQL Workbench para migrar (cópia de segurança e restauro) para uma instância de servidor de MySQL do Azure. | Disponível

Neste artigo, Contoso irá migrar do Windows de duas camadas. NET SmartHotel aplicação em execução em VMs de VMware, para o Azure. Se pretender utilizar esta aplicação, é fornecido como código aberto e poderá transferi-lo de [github](https://github.com/Microsoft/SmartHotel360).



## <a name="business-drivers"></a>Controladores de negócio

A equipa de TI liderança trabalhou coincida com os respetivos parceiros de negócios para compreender o que pretende alcançar com esta migração:

- **Crescimento de negócio de endereços**: Contoso está a crescer e, como resultado existe pressão na infraestrutura e sistemas no local.
- **Limitar o risco**: SmartHotel a aplicação é fundamental para as empresas de Contoso. Pretende movê-la para o Azure com o risco de zero.
- **Expandir**: Contoso não pretende modificar a aplicação. Pretendem simplesmente para se certificar de que está estável.


## <a name="migration-goals"></a>Objetivos de migração

A equipa de nuvem Contoso tem afixado baixo objetivos para esta migração. Estes objetivos são utilizados para determinar o melhor método de migração:

- Após a migração, a aplicação no Azure deve ter as mesmas capacidades de desempenho, como sucede no VMware.  A aplicação irá permanecer como crítica na nuvem porque está no local. 
- Contoso não quer investir nesta aplicação.  É importante para o negócio, mas o respetivo formato atual simplesmente pretendem movê-lo em segurança para a nuvem.
- Contoso não pretende alterar o modelo do OPS Manager para esta aplicação. Pretendem interagir com ele na nuvem da mesma forma que são agora.
- Contoso não pretende alterar qualquer funcionalidade de aplicações. Apenas a localização da aplicação será alterado.

## <a name="proposed-architecture"></a>Arquitetura proposta

Eis o ambiente atual

- A aplicação está em camadas através de duas VMs (**WEBVM** e **SQLVM**).
- As VMs estão localizadas no anfitrião do VMware ESXi **contosohost1.contoso.com** (versão 6.5).
- O ambiente VMware é gerido pelo vCenter Server 6.5 (**vcenter.contoso.com**), em execução numa VM.
- Contoso tem um datacenter no local (contoso-datacenter), com um controlador de domínio no local (**contosodc1**).
- As VMs no local no Centro de dados Contoso irão ser desativadas após a migração é efetuada.

![Arquitetura do cenário](./media/contoso-migration-rehost-vm/architecture.png) 

## <a name="migration-process"></a>processo de migração

Contoso irá migrar o front-end de aplicação e a base de dados de VMs para VMs do Azure utilizando a recuperação de sites:

- Como primeiro passo, estes irá preparar e configurar componentes do Azure Site Recovery e preparar a infraestrutura do VMware no local.
- Que já têm as respetivas [infraestrutura do Azure](contoso-migration-infrastructure.md) no local, por isso, estes apenas de adicionar alguns componentes do Azure especificamente para a recuperação de sites.
- Com tudo preparado, podem iniciar replicar as VMs.
-Após a replicação está ativada e a funcionar, são migradas VM ao efetuar a ativação pós-falha para o Azure.

![processo de migração](./media/contoso-migration-rehost-vm/migraton-process.png) 



### <a name="azure-services"></a>Serviços do Azure

**Serviço** | **Descrição** | **Custo**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | O serviço orquestra e gere a migração e recuperação após desastre para as VMs do Azure e no local VMs e servidores físicos.  | Durante a replicação para o Azure, são cobradas taxas de armazenamento do Azure.  As VMs do Azure são criadas e implicar custos, quando ocorre a ativação pós-falha. [Saiba mais](https://azure.microsoft.com/pricing/details/site-recovery/) sobre os encargos e preços.


## <a name="prerequisites"></a>Pré-requisitos

Eis o (e Contoso) precisam de executar este cenário.

**Requisitos** | **Detalhes**
--- | ---
**Subscrição do Azure** | Deve já tiver criado uma subscrição durante os artigos nesta série. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Se criar uma conta gratuita, será o administrador da sua subscrição e poderá executar todas as ações.<br/><br/> Se utilizar uma subscrição existente e não tiver o administrador, terá de trabalhar com o administrador para lhe atribuir permissões de proprietário ou contribuinte.<br/><br/> Se precisar de mais granulares permissões, consulte [neste artigo](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Infraestrutura do Azure** | [Saiba como](contoso-migration-infrastructure.md) Contoso configurar uma infraestrutura do Azure.<br/><br/> Saiba mais sobre específicos [rede](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) e [armazenamento](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) requisitos para a recuperação de Site.
**Servidores no local** | Servidores vCenter versão 5.5, 6.0 ou 6.5 devem estar em execução no local<br/><br/> Anfitriões ESXi devem executar a versão 5.5, 6.0 ou 6.5<br/><br/> Uma ou mais VMs de VMware deve estar em execução no anfitrião ESXi.
**VMs no local** | As VMs têm de cumprir [requisitos do Azure](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Passos do cenário

Eis como Contoso irá executar a migração:

> [!div class="checklist"]
> * **Passo 1: Preparar do Azure Site Recovery**: criarem uma conta de armazenamento do Azure para armazenar dados replicados e um cofre dos serviços de recuperação.
> * **Passo 2: Preparar VMware no local para a recuperação de Site**: preparar contas para a VM deteção e instalação do agente e preparar a ligação para VMs do Azure após a ativação pós-falha.
> * **Passo 3: Replicar VMs**: configurar a replicação e iniciar a replicar VMs de armazenamento do Azure.
> * **Passo 4: Migrar as VMs com a recuperação de Site**: executar uma ativação pós-falha de teste para se certificar de que tudo está a funcionar e, em seguida, execute uma ativação pós-falha completa para migrar as VMs no Azure.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Passo 1: Preparar do Azure para o serviço de recuperação de sites

Seguem-se os componentes do Azure que Contoso tem de migrar as VMs para o Azure:

- Uma VNet na qual as VMs do Azure estarão localizadas quando são criados durante a ativação pós-falha.
- Uma conta de armazenamento do Azure para armazenar dados replicados. 
- Um cofre dos serviços de recuperação no Azure.

Se configuraram estas da seguinte forma:

1. Contoso já configurado uma rede podem utilizar para recuperação de sites quando estes [implementada a infraestrutura do Azure](contoso-migration-infrastructure.md)

    - A aplicação de SmartHotel é uma aplicação de produção e as VMs serão migradas para a rede de produção do Azure (VNET-PROD-EUS2) na região primária EUA Leste 2.
    - Ambas as VMs serão colocadas no grupo de recursos ContosoRG, que é utilizado para recursos de produção.
    - O front-end de aplicação VM (WEBVM) irá migrar para a sub-rede do front-end (PROD-FE-EUS2), na rede de produção.
    - A base de dados de aplicação VM (SQLVM) irá migrar para a sub-rede de base de dados (PROD-DB-EUS2), na rede de produção.

2. Contoso cria uma conta de armazenamento do Azure (contosovmsacc20180528) na região primária.
    - A conta de armazenamento tem de estar na mesma região que o cofre dos Serviços de Recuperação.
    - Utilizam uma conta para fins gerais, com o armazenamento padrão e replicação do LRS. 

    ![Armazenamento de recuperação de site](./media/contoso-migration-rehost-vm/asr-storage.png)

3. Com a conta de armazenamento e de rede no local, Contoso agora cria um cofre dos serviços de recuperação (ContosoMigrationVault) e coloca-o no grupo de recursos ContosoFailoverRG na região primária EUA Leste 2.

    ![Cofre dos Serviços de Recuperação](./media/contoso-migration-rehost-vm/asr-vault.png)

**Precisa de mais ajuda?**

[Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) a configurar do Azure Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Passo 2: Preparar VMware no local para a recuperação de Site

Eis o que Contoso prepara no local:

- Uma conta no vCenter server ou vSphere ESXi anfitrião, ao automatizar a deteção VM.
- Uma conta que permite a instalação automática do serviço de mobilidade nas VMs de VMware. 
- No local as definições da VM, para que Contoso possam ligar para as VMs do Azure replicadas após a ativação pós-falha.


### <a name="prepare-an-account-for-automatic-discovery"></a>Preparar uma conta para a deteção automática

O Site Recovery precisa de ter acesso a servidores VMware para:

- Detete automaticamente as VMs. 
- Orquestrar a replicação, ativação pós-falha e a reativação pós-falha para as VMs.
- É necessária, pelo menos, uma conta só de leitura. A conta deve ser capaz de executar operações, tais como criar e a remoção de discos e a ativar as VMs.

Contoso configura a conta da seguinte forma:

1. Se criarem uma função ao nível do vCenter.
2. Se atribuir essa função as permissões necessárias.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Preparar uma conta para a instalação do serviço de Mobilidade

O serviço de mobilidade tem de ser instalado em cada VM.

- Recuperação de sites pode efetuar uma instalação push automática do serviço de mobilidade quando a replicação de VM está ativada.
- Uma conta é necessária para que a recuperação de sites possam aceder as VMs para a instalação push. Especificar esta conta ao configurar a replicação.
- A conta pode ser o domínio ou local, com permissões para instalar nas VMs.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparar a ligação para VMs do Azure após a ativação pós-falha

Após a ativação pós-falha, a Contoso pretende estabelecer ligação com as VMs do Azure. Para tal, podem efetuar o seguinte antes da migração:

1. Para ter acesso através da internet são:

 - Permitir RDP da VM no local antes da ativação pós-falha
 - Certifique-se de que as regras TCP e UDP estão adicionadas para o **pública** perfil.
 - Verifique se o RDP é permitido no **Firewall do Windows** > **aplicações permitidas** para todos os perfis.
 
2. Para acesso através de VPN de site a site, são:

 - Ative o RDP na máquina no local.
 - Permitir RDP no **Firewall do Windows** -> **aplicações e funcionalidades permitidas**, para **domínio e privada** redes.
 - Definir a política de SAN do sistema operativo na VM no local para **OnlineAll**.

Além disso, quando executam uma ativação pós-falha que precisam de verificar o seguinte:

- Não deverá haver nenhum atualizações pendentes do Windows na VM ao acionar uma ativação pós-falha. Se existirem, não poderá iniciar sessão na VM até a conclusão da atualização.
- Após a ativação pós-falha, pode verificar **diagnóstico de arranque** para ver uma captura de ecrã da VM. Se isto não funcionar, deve verificar que a VM está em execução e reveja estes [sugestões de resolução de problemas](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**Precisa de mais ajuda?**

- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) criar e atribuir uma função para a deteção automática.
- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) criar uma conta da instalação push do serviço de mobilidade.


## <a name="step-3-replicate-the-on-premises-vms"></a>Passo 3: Replicar as VMs no local

Para que podem executar uma migração para o Azure, Contoso tem de configurar e ativar a replicação.

### <a name="set-a-replication-goal"></a>Definir um objetivo de replicação

1. No cofre, sob o nome do cofre (ContosoVMVault) o utilizador selecionar um objetivo de replicação (**introdução** > **recuperação de Site** > **preparar a infraestrutura** .
2. Se especificarem que as suas máquinas estão localizados no local, em execução no VMware e replicar para o Azure.

    ![Objetivo da replicação](./media/contoso-migration-rehost-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Confirme o planeamento da implementação

Para continuar, confirmarem que que tenham concluído planeamento da implementação, selecionando **Sim, ter efetuado esta**. Neste cenário, Contoso está a migrar apenas duas VMs e não precisa de planeamento da implementação.


### <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem

Contoso tem de configurar o respetivo ambiente de origem. Para tal, podem transferir um modelo OVF e utilizá-lo para implementar o servidor de configuração da recuperação de sites como altamente disponível, VM de VMware no local. Depois do servidor de configuração está a funcionar, estes registá-lo no ths cofre.

O servidor de configuração é executado um número de componentes:

- O componente de servidor de configuração que coordena as comunicações entre no local e o Azure e gere a replicação de dados.
- O servidor de processos atua como um gateway de replicação. Recebe dados de replicação; otimiza-os com a colocação em cache, compressão e encriptação; e envia-os para o armazenamento do Azure.
- O servidor de processos também instala o Serviço de Mobilidade nas VMs que pretende replicar, e efetua a deteção automática de VMs VMware no local.



Contoso execute estes passos da seguinte forma:

1. No cofre, transferem o modelo OVF **preparar infraestrutura** > **origem** > **servidor de configuração**.
    
    ![Transferir OVF](./media/contoso-migration-rehost-vm/add-cs.png)

2. Se importar o modelo para VMware para criar e implementar a VM.

    ![Modelo OVF](./media/contoso-migration-rehost-vm/vcenter-wizard.png)

3.  Quando for ativado a VM pela primeira vez, arrancarem para uma experiência de instalação do Windows Server 2016. Aceitar o contrato de licença e introduza uma palavra-passe de administrador.
4. Após a conclusão da instalação, iniciam sessão no VM como administrador. No primeiro início de sessão, a ferramenta de configuração de recuperação de sites do Azure é executado por predefinição.
5. Na ferramenta, se especificarem um nome para registar o servidor de configuração no cofre.
6. A ferramenta verifica se a VM pode ligar ao Azure. Depois da ligação for estabelecida, podem iniciar sessão para a subscrição do Azure. As credenciais tem de ter acesso ao Cofre no qual que irá registar o servidor de configuração.

    [Registar o servidor de configuração](./media/contoso-migration-rehost-vm/config-server-register2.png)

7. A ferramenta executa algumas tarefas de configuração e, em seguida, é reiniciada.
8. Iniciar sessão máquina novamente e o Assistente de gestão do servidor de configuração é iniciado automaticamente.
9. No assistente, o utilizador selecionar o NIC para receber o tráfego de replicação. Não é possível alterar esta definição depois de estar configurada.
10. O utilizador selecionar a subscrição, o grupo de recursos e o Cofre registar o servidor de configuração.
        ![Cofre](./media/contoso-migration-rehost-vm/cswiz1.png) 

10. Estes transferiram e instalar o servidor de MySQL e VMWare PowerCLI. 
11. Após a validação, se especificarem o FQDN ou endereço IP do anfitrião do servidor ou vSphere do vCenter. Deixe a porta predefinida e especifique um nome amigável para o servidor no Azure.
12. Se especificarem a conta que estes criados para a deteção automática e as credenciais são utilizadas para instalar automaticamente o serviço de mobilidade. Para máquinas do Windows, a conta tem privilégios de administrador local nas VMs.

    ![vCenter](./media/contoso-migration-rehost-vm/cswiz2.png)

7. Após a conclusão de registo, no portal do Azure, a Contoso duplo verifica que o servidor de configuração e o servidor do VMware estão listadas no **origem** página no cofre. Deteção pode demorar 15 minutos ou mais. 
8. Recuperação de sites, em seguida, liga-se aos servidores do VMware com as definições especificadas e Deteta as VMs.

### <a name="set-up-the-target"></a>Configurar o destino

Agora Contoso Especifica as definições de replicação de destino.

1. No **preparar a infraestrutura** > **destino**, o utilizador de selecionar as definições de destino.
2. Recuperação de sites verifica que é uma conta de armazenamento do Azure e a rede na localização de destino especificado.

### <a name="create-a-replication-policy"></a>Criar uma política de replicação

Agora Contoso pode criar uma política de replicação.

1. No **preparar a infraestrutura** > **as definições de replicação** > **política de replicação** >  **criar e Associar**, se criarem uma política **ContosoMigrationPolicy**.
2. Se utilizarem as predefinições:
    - **Limiar RPO**: predefinição de 60 minutos. Este valor define com que frequência são criados pontos de recuperação. Será gerado um alerta se a replicação contínua exceder este limite.
    - **Retenção do ponto de recuperação**. Predefinição de 24 horas. Este valor Especifica o tempo da janela de retenção para cada ponto de recuperação. As VMs replicadas podem ser recuperadas para qualquer ponto numa janela.
    - **Frequência de instantâneos consistentes com aplicação**. Predefinição de uma hora. Este valor Especifica a frequência com que são criados os instantâneos consistentes com aplicações.

        ![Criar política de replicação](./media/contoso-migration-rehost-vm/replication-policy.png)

5. A política é associada automaticamente ao servidor de configuração. 

    ![Associar a política de replicação](./media/contoso-migration-rehost-vm/replication-policy2.png)

### <a name="enable-replication-for-webvm"></a>Ativar a replicação para WEBVM

Com tudo no local, Contoso pode ativar a replicação para as VMs. São iniciados com WebVM.

1. No **replicar aplicação** > **origem** > **+ replicar** , selecionarem as definições de origem.
2. Indicam que pretende ativar a VMs, selecione o servidor vCenter e o servidor de configuração.

    ![Ativar a replicação](./media/contoso-migration-rehost-vm/enable-replication1.png)

3. O utilizador selecionar as definições de destino, incluindo o grupo de recursos e rede do Azure e a conta de armazenamento.

     ![Ativar a replicação](./media/contoso-migration-rehost-vm/enable-replication2.png)

4. Seleciona contoso **WebVM** para replicação, verifica a política de replicação e ativa a replicação.

    - Nesta fase, a Contoso apenas seleciona WEBVM porque VNet e sub-rede tem de ser selecionadas e, a Contoso é colocando a aplicação VMs em sub-redes diferentes.
    - Recuperação de site instala automaticamente o serviço de mobilidade na VM quando a replicação está ativada.

    ![Ativar a replicação](./media/contoso-migration-rehost-vm/enable-replication3.png)

5. Controlar o progresso de replicação no **tarefas**. Depois de a tarefa **Finalizar Proteção** ser executada, a máquina está preparada para ativação pós-falha.
6. No **Essentials** no portal do Azure, Contoso pode ver a estrutura para as VMs replicar para o Azure.


### <a name="enable-replication-for-sqlvm"></a>Ativar a replicação para SQLVM

Agora pode começar a Contoso replicar a máquina SQLVM, utilizando o mesmo processo, conforme apresentado acima.

1. O utilizador selecionar as definições de origem.

    ![Ativar a replicação](./media/contoso-migration-rehost-vm/enable-replication1.png)

2. Se, em seguida, especificarem as definições de destino.

     ![Ativar a replicação](./media/contoso-migration-rehost-vm/enable-replication2-sqlvm.png)

3. Se selecionarem SQLVM para replicação. 

    ![Ativar a replicação](./media/contoso-migration-rehost-vm/enable-replication3-sqlvm.png)

4. Aplicar a mesma política de replicação que foi utilizada para WEBVM e ativar a replicação.

    ![Vista de infraestrutura](./media/contoso-migration-rehost-vm/essentials.png)

**Precisa de mais ajuda?**

- Pode ler instruções completa de todos estes passos no [configurar a recuperação após desastre para as VMs de VMware no local](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Instruções detalhadas estão disponíveis para o ajudar a [configurar o ambiente de origem](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementar o servidor de configuração](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), e [configurar as definições de replicação](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- Pode saber mais sobre [ativar replicação](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-4-migrate-the-vms"></a>Passo 4: Migrar as máquinas virtuais 

Contoso executar uma ativação pós-falha de teste rápida e, em seguida, uma ativação pós-falha completa para migrar as VMs.

### <a name="run-a-test-failover"></a>Executar uma ativação pós-falha de teste

Uma ativação pós-falha de teste ajuda a garantir que tudo está a funcionar conforme esperado. 

1. Contoso executa uma ativação pós-falha de teste para o ponto mais recente no tempo (**mais recentes processados**).
2. O utilizador selecionar **encerrar a máquina antes de iniciar a ativação pós-falha**, para que a recuperação de Site tenta encerre a VM de origem antes de acionar a ativação pós-falha. A ativação pós-falha continua, mesmo que o encerramento falhe. 
3. É executada a ativação pós-falha de teste: 

    - Verificação de um pré-requisitos é executado para se certificar de que todas as condições necessárias para a migração estão em vigor.
    - A ativação pós-falha processa os dados, para que possa ser criada uma VM do Azure. Se for selecionado o último ponto de recuperação, é criado um ponto de recuperação a partir dos dados.
    - É criada uma VM do Azure com base nos dados processados no passo anterior.
    
3. Após a ativação pós-falha, a réplica é apresentada VM do Azure no portal do Azure. Contoso verifica se a VM é o tamanho adequado, ligado à rede à direita e está em execução. 
4. Depois de verificar a ativação pós-falha de teste, estes limpar a ativação pós-falha e registam e guardar todas as observações. 

### <a name="create-and-customize-a-recovery-plan"></a>Criar e personalizar um plano de recuperação

 Depois de verificar que a ativação pós-falha de teste trabalhado conforme esperado, Contoso cria um plano de recuperação para a migração. 

- Um plano de recuperação Especifica a ordem na qual ativação pós-falha ocorre e indica como as VMs do Azure irá ser colocadas online no Azure.
- Uma vez que a aplicação de duas camadas, podem personalizar o plano de recuperação para que os dados de VM (SQLVM) é iniciado antes do front-end (WEBVM).

1. No **planos de recuperação (recuperação de sites)** > **+ plano de recuperação**, criam um plano e adicionar VMs ao mesmo.

    ![Plano de recuperação](./media/contoso-migration-rehost-vm/recovery-plan.png)

2. Depois de criar o plano, estes personalizá-lo (**planos de recuperação** > **SmartHotelMigrationPlan** > **personalizar**).
2.  Podem remover WEBVM de **grupo 1: Iniciar**.  Isto garante que a primeira ação de iniciação afeta SQLVM apenas.
3.  No **+ grupo** > **adicionar itens protegidos**, adicionarem WEBVM ao grupo 2: Iniciar.  As VMs têm de ser em dois grupos diferentes.


### <a name="migrate-the-vms"></a>Migrar as máquinas virtuais


Agora Contoso pode executar uma ativação pós-falha completa para concluir a migração.

1. O utilizador selecionar o plano de recuperação > **ativação pós-falha**.
2. Selecionarem para a ativação pós-falha do ponto de recuperação mais recente e que a recuperação de sites deve tentar encerre a VM no local antes de acionar a ativação pós-falha. Pode seguir o progresso de ativação pós-falha **tarefas** página.

    ![Ativação pós-falha](./media/contoso-migration-rehost-vm/failover1.png)


3. Após a ativação pós-falha, certifique-se de que a VM do Azure é apresentado como esperado no portal do Azure.

    ![Ativação pós-falha](./media/contoso-migration-rehost-vm/failover2.png)  

3. Após a verificação, concluírem a migração para cada VM. Esta ação deixa de replicação para a VM e deixa de faturação de recuperação de Site para o mesmo.

    ![Ativação pós-falha](./media/contoso-migration-rehost-vm/failover3.png)

**Precisa de mais ajuda?**

- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) executar uma ativação pós-falha de teste. 
- [Saiba](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) como criar um plano de recuperação.
- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) efetuar a ativação pós-falha para o Azure.

## <a name="clean-up-after-migration"></a>Limpar após a migração

Migração completa, as camadas de aplicação SmartHotel estão agora em execução em VMs do Azure.

Agora, Contoso tem de concluir estes passos de limpeza:  

- Remova a máquina WEBVM do inventário do vCenter.
- Remova a máquina SQLVM do inventário do vCenter.
- Remova WEBVM e SQLVM das tarefas de cópia de segurança locais.
- Atualize a documentação interna para mostrar a nova localização e endereços IP para as VMs.
- Reveja quaisquer recursos que interagem com as VMs e atualizar as definições relevantes ou documentação para refletir a nova configuração.

## <a name="review-the-deployment"></a>Reveja a implementação

Com a aplicação agora em execução, Contoso agora tem totalmente operacionalizar e proteja-a no Azure.

### <a name="security"></a>Segurança

A equipa de segurança de Contoso revê as VMs do Azure, para determinar os problemas de segurança.

- Para controlar o acesso, podem rever os grupos de segurança de rede (NSGs) para as VMs. Os NSGs são utilizados para garantir que apenas o tráfego permitido para a aplicação pode contactá-lo.
- Estes pondere também proteger os dados no disco com o Azure Disk Encryption e KeyVault.

[Leia mais](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) sobre práticas de segurança para as VMs.

### <a name="backups"></a>Cópias de segurança

Contoso vai fazer cópias de segurança de dados em VMs utilizando o serviço de cópia de segurança do Azure. [Saiba mais](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Otimização de licenciamento e custo

1. Contoso tem licenciamento existente para as respetivas VMs e tirar partido do benefício de híbrida do Azure.  Estes irão converter as VMs do Azure existente, para tirar partido deste preços.
2. Contoso irá ativar a gestão de custo de Azure licenciados pelos Cloudyn, uma subsidiária Microsoft. É uma solução de gestão de custo de nuvem multi que ajuda a utilizar e gerir o Azure e outros recursos de nuvem. [Saiba mais](https://docs.microsoft.com/azure/cost-management/overview) sobre a gestão de custo do Azure. 

## <a name="conclusion"></a>Conclusão

Neste artigo, Contoso rehosted a aplicação de SmartHotel no Azure ao migrar a aplicação de VMs para VMs do Azure utilizando o serviço de recuperação de sites. 


## <a name="next-steps"></a>Passos Seguintes

No seguinte artigo da série, vamos mostrar-lhe como Contoso realojamento SmartHotel aplicação front-end VM no Azure utilizando o serviço de recuperação de Site e, migre a base de dados para um grupo de disponibilidade do AlwaysOn de SQL do Azure.

