---
title: Uma aplicação Contoso de realojamento ao migrar para o grupo de disponibilidade de VMs do Azure e SQL Server AlwaysOn | Microsoft Docs
description: Saiba como Contoso realojamento uma aplicação no local ao migrar para o grupo de disponibilidade de VMs do Azure e SQL Server AlwaysOn
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: raynew
ms.openlocfilehash: 03e3aaad810f6ccd5fb376765ddbada072dedb06
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301308"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms-and-sql-server-alwayson-availability-group"></a>Migração de Contoso: uma aplicação no local ao grupo de disponibilidade de VMs do Azure e SQL Server AlwaysOn de realojamento

Este artigo demonstra como Contoso realojamento a respetiva aplicação SmartHotel no Azure. O front-end de aplicação VM são migradas para uma VM do Azure e a base de dados de aplicação para uma VM do Azure SQL Server, em execução num cluster de ativação pós-falha do Windows Server com grupos de Disponibilidade AlwaysOn do SQL Server.

Este documento é um de uma série de artigos que mostram como a empresa fictícia com Contoso migra os seus recursos no local para a nuvem do Microsoft Azure. A série inclui cenários que ilustram a configurar uma infraestrutura de migração, avaliar a recursos no local para a migração e com diferentes tipos de migrações e informações de fundo. Cenários de crescimento em complexidade e iremos adicionar artigos adicionais ao longo do tempo.

**Artigo** | **Detalhes** | **Estado**
--- | --- | ---
[Artigo 1: Descrição geral](contoso-migration-overview.md) | Fornece uma descrição geral da estratégia de migração da Contoso, a série de artigo e as aplicações de exemplo, que vamos utilizar. | Disponível
[Artigo 2: Implementar uma infraestrutura do Azure](contoso-migration-infrastructure.md) | Descreve como Contoso prepara a infraestrutura do Azure e no local para a migração. A mesma infraestrutura é utilizada para todos os artigos de migração. | Disponível
[Artigo 3: Avaliar a recursos no local](contoso-migration-assessment.md)  | Mostra como Contoso executa uma avaliação de uma aplicação de SmartHotel de duas camadas no local em execução no VMware. Contoso avalia VMs de aplicação com o [Azure migrar](migrate-overview.md) serviço e a base de dados de SQL Server de aplicação com o [Assistente de migração de base de dados](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponível
[Artigo 4: Uma aplicação para as VMs do Azure e uma instância do SQL gerida de realojamento](contoso-migration-rehost-vm-sql-managed-instance.md) | Demonstra como Contoso executa uma migração de comparação de precisão e shift para o Azure para a aplicação de SmartHotel. Contoso migra o front-end de aplicação VM utilizando [do Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)e a base de dados de aplicação a uma instância gerida do SQL Server, utilizando o [serviço de migração de base de dados do Azure](https://docs.microsoft.com/azure/dms/dms-overview). | Disponível
[Artigo 5: Realojamento uma aplicação para as VMs do Azure](contoso-migration-rehost-vm.md) | Mostra como Contoso migrar a aplicação de SmartHotel VMs utilizando apenas a recuperação de sites.
Artigo 6: Realojamento uma aplicação para as VMs do Azure e SQL Server sempre no grupo de disponibilidade (Este artigo) | Mostra como Contoso migra a aplicação de SmartHotel. A Contoso utiliza a recuperação de sites para migrar a aplicação VMs e o serviço de base de dados de migração para migrar a base de dados de aplicação para um cluster do SQL Server protegido por um grupo de Disponibilidade AlwaysOn. | Disponível
[Artigo 7: Realojamento uma aplicação do Linux para as VMs do Azure](contoso-migration-rehost-linux-vm.md) | Mostra como Contoso funciona uma migração de comparação de precisão e shift da aplicação de osTicket Linux para VMs do Azure, utilizando a recuperação de sites | Planeado
[Artigo 8: Uma aplicação do Linux para as VMs do Azure e o servidor do Azure MySQL de realojamento](contoso-migration-rehost-linux-vm-mysql.md) | Demonstra como Contoso migra a aplicação do Linux osTicket para VMs do Azure com a recuperação de Site e migra a base de dados de aplicação para uma instância de servidor de MySQL do Azure utilizando o MySQL Workbench. | Disponível



Neste artigo, Contoso migrar do Windows de duas camadas. NET SmartHotel aplicação em execução em VMs de VMware para o Azure. Se gostaria de utilizar esta aplicação, é fornecido como código aberto e poderá transferi-lo de [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>Controladores de negócio

A equipa de liderança TI trabalhou coincida com os respetivos parceiros de negócios para compreender o que pretende alcançar com esta migração:

- **Crescimento de negócio de endereços**: Contoso está a crescer e, como resultado existe pressão na infraestrutura e sistemas no local.
- **Aumentar a eficiência**: Contoso tem de remover procedimentos desnecessários e simplificar processos para programadores e os utilizadores.  Necessidades IT para ser rápido e hora não waste ou dinheiro, deste modo, entrega mais rápida nos requisitos de cliente.
- **Aumentar a agilidade**: Contoso TI tem de ser mais responder às necessidades do negócio. Tem de ser capaz de reagir mais rapidamente do que as alterações no marketplace, para ativar o sucesso num economia global.  Não pode obter de forma ou tornar-se um bloqueador de janelas de negócio.
- **Escala**: como o crescimentos de negócio com êxito, Contoso TI tem de fornecer sistemas que são capazes de aumentam com o mesmo ritmo.

## <a name="migration-goals"></a>Objetivos de migração

A equipa de nuvem Contoso tem afixado baixo objetivos para esta migração. Estes objetivos foram utilizados para determinar o melhor método de migração:

- Após a migração, a aplicação no Azure deve ter as mesmas capacidades de desempenho, como sucede no VMWare.  A aplicação irá permanecer como crítica na nuvem porque está no local.
- Contoso não quer investir nesta aplicação.  É importante para o negócio, mas o respetivo formato atual simplesmente pretendem movê-lo em segurança para a nuvem.
- A base de dados no local para a aplicação tenha tido problemas de disponibilidade. Gostaria de ver que esta implementada no Azure como um cluster de elevada disponibilidade, com capacidades de ativação pós-falha.
- A Contoso pretende atualizar a partir da respetiva plataforma atual do SQL Server 2008 R2, para 2017 do SQL Server.
- Contoso não a pretende utilizar uma base de dados do SQL do Azure para esta aplicação e está à procura de alternativas.

## <a name="proposed-architecture"></a>Arquitetura proposta

Neste cenário:

- A aplicação está em camadas através de duas VMs (WEBVM e SQLVM).
- As VMs estão localizadas no anfitrião do VMware ESXi **contosohost1.contoso.com** (versão 6.5)
- O ambiente VMware é gerido pelo vCenter Server 6.5 (**vcenter.contoso.com**), em execução numa VM.
- Contoso tem um datacenter no local (contoso-datacenter), com um controlador de domínio no local (**contosodc1**).
- As VMs no local no Centro de dados Contoso irão ser desativadas após a migração é efetuada.

![Arquitetura do cenário](media/contoso-migration-rehost-vm-sql-ag/architecture.png) 

### <a name="azure-services"></a>Serviços do Azure

**Serviço** | **Descrição** | **Custo**
--- | --- | ---
[Serviço de gestão de base de dados](https://docs.microsoft.com/azure/dms/dms-overview) | Contoso irá migrar a camada de dados de aplicação utilizando o DMS. O DMS ligar à máquina SQLVM no local através de uma VPN de site para site e migrar DMS permite totalmente integradas migrações de várias origens de base de dados para plataformas de dados do Azure, com o período de indisponibilidade mínimo. | Saiba mais sobre [regiões suportadas](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) para DMS e get [detalhes de preços](https://azure.microsoft.com/pricing/details/database-migration/).
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Contoso irá utilizar a recuperação de Site para uma migração de comparação de precisão e shift do front-end de aplicação VM. Recuperação de site orquestra e gere a migração e recuperação após desastre para as VMs do Azure e no local VMs e servidores físicos.  | Durante a replicação para o Azure, são cobradas taxas de armazenamento do Azure.  As VMs do Azure são criadas e implicar custos, quando ocorre a ativação pós-falha. [Saiba mais](https://azure.microsoft.com/pricing/details/site-recovery/) sobre os encargos e preços.

 

## <a name="migration-process"></a>processo de migração

- Contoso irá migrar a aplicação VMs no Azure.
- Estes irá migrar a VM de front-end para a VM do Azure com a recuperação de sites:
    - Como primeiro passo, estes irá preparar e configurar componentes do Azure e preparar a infraestrutura do VMware no local.
    - Com tudo preparado, podem iniciar a VM a replicar.
    - Depois da replicação está ativada e a funcionar, são migradas VM ao efetuar a ativação pós-falha para o Azure.
- Estes irá migrar a base de dados para um cluster do SQL Server no Azure, utilizando o serviço de migração de dados (DMS).
    - Como primeiro passo irá necessitam para aprovisionar as VMs de SQL Server no Azure, configurar o cluster e um balanceador de carga interno e configurar grupos de Disponibilidade AlwaysOn.
    - Com esta opção no local, estes podem migrar a base de dados
- Após a migração, irá ativar a proteção de AlwaysOn para a base de dados.

![processo de migração](media/contoso-migration-rehost-vm-sql-ag/migration-process.png) 
 
## <a name="prerequisites"></a>Pré-requisitos

Eis o (e Contoso) precisam de executar este cenário:

**Requisitos** | **Detalhes**
--- | ---
**Subscrição do Azure** | Deve já tiver criado uma subscrição quando efetuar a avaliação do artigo primeiro esta série. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Se criar uma conta gratuita, será o administrador da sua subscrição e poderá executar todas as ações.<br/><br/> Se utilizar uma subscrição existente e não tiver o administrador, terá de trabalhar com o administrador para lhe atribuir permissões de proprietário ou contribuinte.<br/><br/> Se precisar de mais granulares permissões, consulte [neste artigo](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Infraestrutura do Azure** | [Saiba como](contoso-migration-infrastructure.md) Contoso configurar uma infraestrutura do Azure.<br/><br/> Saiba mais sobre específicos [rede](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) e [armazenamento](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) requisitos para a recuperação de Site.
**Recuperação de sites (no local)** | O servidor do vCenter no local deve estar a executar a versão 5.5, 6.0 ou 6.5<br/><br/> Um anfitrião ESXi a executar a versão 5.5, 6.0 ou 6.5<br/><br/> Uma ou mais VMs de VMware no anfitrião ESXi a executar.<br/><br/> As VMs têm de cumprir [requisitos do Azure](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Suportado [rede](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) e [armazenamento](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) configuração.<br/><br/> As VMs que pretende replicar tem de cumprir [requisitos do Azure](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).
**DMS** | Para que o DMS tem um [compatível no dispositivo VPN local](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> Tem de ser capaz de configurar o dispositivo VPN no local. Tem de ter um endereço IPv4 público com acesso exterior, e o endereço não pode estar localizado atrás de um dispositivo NAT.<br/><br/> Certifique-se de que tem acesso à base de dados do SQL Server no local.<br/><br/> A Firewall do Windows deverá conseguir aceder ao motor de base de dados de origem. [Saiba mais](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Se existir uma firewall à frente do seu computador da base de dados, adicione regras para permitir o acesso à base de dados e aos ficheiros através de porta SMB 445.<br/><br/> As credenciais utilizadas para ligar à origem de SQL Server e instância geridos de destino têm de ser membros da função de servidor sysadmin.<br/><br/> Precisa de uma rede de partilha na base de dados no local que o DMS podem utilizar para criar cópias de segurança da base de dados de origem.<br/><br/> Certifique-se de que a conta de serviço que executa a instância do SQL Server de origem tem privilégios de escrita na partilha de rede.<br/><br/> Tome nota de um utilizador do Windows (e a palavra-passe) que tem o privilégio de controlo total na partilha de rede. O serviço de migração de base de dados do Azure representa tem estas credenciais de utilizador para carregar ficheiros de cópia de segurança para o contentor de armazenamento do Azure.<br/><br/> O processo de instalação do SQL Server Express define o protocolo de TCP/IP para **desativado** por predefinição. Certifique-se de que está ativada.


## <a name="scenario-steps"></a>Passos do cenário

Eis como Contoso irá executar a migração:

> [!div class="checklist"]
> * **Passo 1: Criar as VMs de SQL Server no Azure**: para elevada disponibilidade, Contoso pretende implementar uma base de dados em cluster no Azure. Que implementam duas VMs de SQL Server e um balanceador de carga interno do Azure.
> * **Passo 2: Implementar o cluster**: depois de implementar a VMs de SQL Server, preparar a um cluster de servidor SQL do Azure.  Estes irá migrar a respetiva base de dados para este cluster criada previamente.
> * **Passo 3: Preparar DMS**: preparar DMS que registar o fornecedor de migração de base de dados, criar uma instância do DMS e um projeto. Se configuraram uma assinatura de acesso partilhado (SAS) Uniform Resource Identifier (URI). O DMS utiliza o URI de SA para aceder ao contentor de conta de armazenamento ao qual o serviço de carregamentos de ficheiros de cópia de segurança do SQL Server.
> * **Passo 4: Preparar do Azure Site Recovery**: criarem uma conta de armazenamento do Azure para armazenar dados replicados e um cofre dos serviços de recuperação.
> * **Passo 5: Preparar VMware no local para a recuperação de Site**: preparar contas para a VM deteção e instalação do agente e preparar VMs no local para que possam estabelecer a ligação para VMs do Azure após a ativação pós-falha.
> * **Passo 6: Replicar VMs**: configurar as definições de replicação e ativar a replicação de VM.
> * **Passo 7: Migrar a base de dados com o DMS**: são migradas a base de dados.
> * **Passo 8: Migrar as VMs com a recuperação de Site**: primeiro execute uma ativação pós-falha de teste para se certificar de que tudo está a funcionar, seguido de uma ativação pós-falha completa para migrar a VM.


## <a name="step-1-prepare-a-sql-server-alwayson-availability-group-cluster"></a>Passo 1: Preparar um cluster de grupo de Disponibilidade AlwaysOn do SQL Server

A Contoso pretende planeie com antecedência ao implementar a base de dados de um cluster no Azure. Em seguida, podem utilizar este cluster para outras bases de dados. 

- As VMs de SQL Server será implementada no grupo de recursos de ContosoRG (utilizado para recursos de produção).
- Para além dos nomes exclusivos, ambas as VMs utilizam as mesmas definições.
- O Balanceador de carga interno será implementado no ContosoNetworkingRG (utilizado para recursos de rede).
- VMs irão executar o Windows Server 2016 com SQL Server 2017 Enterprise Edition. Contoso não tem licenças para este sistema operativo, para que irá utilizar uma imagem no Azure Marketplace que fornece a licença como cobrada uma taxa para os seus compromissos de EA do Azure.

Eis como Contoso configurar o cluster:

1. É possível criar duas VMs de SQL Server ao selecionar a imagem do SQL Server 2017 Enterprise do Windows Server 2016 no Azure Marketplace. 

    ![SKU DE VM DO SQL SERVER](media/contoso-migration-rehost-vm-sql-ag/sql-vm-sku.png)

2. No **criar máquina virtual assistente** > **Noções básicas**, configurarem:

    - Os nomes das VMs: **SQLAOG1** e **SQLAOG2**.
    - Uma vez que as máquinas são fundamentais, ative as SSD para o tipo de disco da VM.
    - Se especificarem credenciais de máquina.
    - Que implementam as VMs nos E.U.A. Leste primário 2 região, no grupo de recursos de ContosoRG.

3. No **tamanho**, são iniciados com D2s_V3 SKU para ambas as VMs. Irá dimensionar mais tarde, conforme precisam.
4. No **definições**, que fazem o seguinte:

    - Uma vez que estas VMs são bases de dados críticas para a aplicação, que utilizam discos geridos.
    - Colocar as máquinas na rede de produção dos EUA Leste 2 primário região (**VNET-PROD-EUS2**), na sub-rede da base de dados (**PROD-DB-EUS2**).
    - Se criarem um novo conjunto de disponibilidade: **SQLAOGAVSET**, com dois domínios de falhas e cinco domínios de atualização.

    ![VM DO SQL SERVER](media/contoso-migration-rehost-vm-sql-ag/sql-vm-settings.png)

4. No **definições do SQL Server**, estes limitam conectividade do SQL para a rede virtual (privado), a porta predefinida 1433. Para a autenticação que utilizam as mesmas credenciais que utilizam no local (**contosoadmin**).

    ![VM DO SQL SERVER](media/contoso-migration-rehost-vm-sql-ag/sql-vm-db.png)

**Precisa de mais ajuda?**

- [Obter ajuda](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision#1-configure-basic-settings) aprovisionamento de uma VM do SQL Server.
- [Saiba mais sobre](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-prereq#create-sql-server-vms) configurar VMs para outro SKUs de servidor do SQL Server.

## <a name="step-2-deploy-the-failover-cluster"></a>Passo 2: Implementar o cluster de ativação pós-falha

Eis como Contoso configurar o cluster:

1. Se configuraram uma conta de armazenamento do Azure para agir como o testemunho de nuvem.
2. Se adicionarem as VMs de SQL Server ao domínio do Active Directory no Centro de dados Contoso no local.
3. É possível criar o cluster no Azure.
4. Se configurarem o testemunho de nuvem.
5. Por último, permitem que o SQL Always On nos grupos de disponibilidade.

### <a name="set-up-a-storage-account-as-cloud-witness"></a>Configurar uma conta de armazenamento como testemunho de nuvem

Para configurar um testemunho de nuvem, a Contoso tem uma conta de armazenamento do Azure que irá conter o ficheiro de blob utilizado para arbitragem de cluster. A mesma conta de armazenamento pode ser utilizada para configurar o testemunho de nuvem em vários clusters. 

Contoso cria uma conta de armazenamento da seguinte forma:

1. Se especificarem um nome reconhecível para a conta (**contosocloudwitness**).
2. Que implementam uma conta all-purpose geral, com LRS.
3. A conta se colocar numa região terceira - Sul Central-nos. Estes coloque-o fora da região primária e secundária, para que este irá permanecer disponível em caso de falha regional.
4. Estes colocá-lo no seu grupo de recursos contém recursos de infraestrutura - **ContosoInfraRG**.

    ![Testemunho de nuvem](media/contoso-migration-rehost-vm-sql-ag/witness-storage.png)

5. Quando é criar a conta de armazenamento primária e chaves de acesso secundária são geradas para o mesmo. A chave de acesso primária para criar o testemunho de nuvem que precisam. A chave é apresentado sob o nome de conta de armazenamento > **chaves de acesso**.

    ![Chave de acesso](media/contoso-migration-rehost-vm-sql-ag/access-key.png)

### <a name="add-sql-server-vms-to-contoso-domain"></a>Adicionar VMs de SQL Server ao domínio Contoso

1. Contoso adiciona SQLAOG1 e SQLAOG2 ao domínio contoso.com.
2. Em seguida, em cada VM instalarem as ferramentas e a funcionalidade de Cluster de ativação pós-falha do Windows.

## <a name="set-up-the-cluster"></a>Configurar o cluster

Antes de configurar o cluster, a Contoso tira um instantâneo do disco de SO em cada máquina.

![Instantâneo](media/contoso-migration-rehost-vm-sql-ag/snapshot.png)

2. Em seguida, executam um script que tem a colocar em conjunto para criar o Cluster de ativação pós-falha do Windows.

    ![Criar cluster](media/contoso-migration-rehost-vm-sql-ag/create-cluster1.png)

3. Depois de se tiver criado o cluster, certifique-se de que as VMs são apresentadas como nós de cluster.

     ![Criar cluster](media/contoso-migration-rehost-vm-sql-ag/create-cluster2.png)

## <a name="configure-the-cloud-witness"></a>Configurar o testemunho de nuvem

1. Contoso configurar o testemunho de nuvem utilizando o **Assistente de configuração do quórum** no Gestor de clusters de ativação pós-falha.
2. No assistente, selecionarem para criar um testemunho de nuvem com a conta de armazenamento.
3. Após a configuração do testemunho de nuvem, na aparece no snap-in Gestor de clusters de ativação pós-falha.

    ![Testemunho de nuvem](media/contoso-migration-rehost-vm-sql-ag/cloud-witness.png)
            

## <a name="enable-sql-server-always-on-availability-groups"></a>Ativar o SQL Server Always On nos grupos de disponibilidade

Contoso pode agora ativar Always On:

1. No Gestor de configuração de servidor de SQL Server, ativar **grupos de Disponibilidade AlwaysOn** para o **SQL Server (MSSQLSERVER)** serviço.

    ![Ative o AlwaysOn](media/contoso-migration-rehost-vm-sql-ag/enable-alwayson.png)

2. Se reiniciem o serviço para as alterações surtam efeito.

Com o AlwaysOn ativar, Contoso pode configurar o grupo de Disponibilidade AlwaysOn que irá proteger a base de dados SmartHotel.

**Precisa de mais ajuda?**

- [Leia sobre](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness) nuvem testemunho e configurar uma conta de armazenamento para o mesmo.
- [Obter instruções](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial) para configurar um cluster e criar um grupo de disponibilidade.

## <a name="step-3-deploy-the-azure-load-balancer"></a>Passo 3: Implementar o Balanceador de carga do Azure

Contoso agora pretende implementar um balanceador de carga interno que se encontra à frente de nós de cluster. O Balanceador de carga escuta o tráfego e direciona-o para o nó adequado.

![Balanceamento de carga](media/contoso-migration-rehost-vm-sql-ag/architecture-lb.png)

Se criarem o Balanceador de carga da seguinte forma:

1. No portal do Azure > **redes** > **Load Balancer**, se configuraram um novo Balanceador de carga interno: **ILB-PROD-DB-EUS2-SQLAOG**.
2. Se colocar o Balanceador de carga na rede de produção **VNET-PROD-EUS2**, na sub-rede da base de dados **PROD-DB-EUS2**.
3. Se atribuir um endereço IP estático: 10.245.40.100.
4. Como um elemento de rede, que implementam o Balanceador de carga no grupo de recursos de rede **ContosoNetworkingRG**.

    ![Balanceamento de carga](media/contoso-migration-rehost-vm-sql-ag/lb-create.png)

Depois de implementar o Balanceador de carga interno, Contoso tem de configurá-lo. Criar um conjunto de endereços de back-end, configurar uma sonda do Estado de funcionamento e configurar uma regra de balanceamento de carga.

### <a name="add-a-backend-pool"></a>Adicionar um conjunto de back-end

Para distribuir o tráfego para as VMs do cluster, a Contoso configurar um conjunto de endereços de back-end que contém os endereços IP dos NICs para VMs que irão receber o tráfego de rede do Balanceador de carga.

1. Nas definições do Balanceador de carga no portal, Contoso adicionar um conjunto de back-end: **ILB-PROD-DB-EUS-SQLAOG-BEPOOL**.
2. O conjunto podem associar conjunto de disponibilidade SQLAOGAVSET. As VMs do conjunto (**SQLAOG1** e **SQLAOG2**) são adicionados ao agrupamento.

    ![Conjunto back-end](media/contoso-migration-rehost-vm-sql-ag/backend-pool.png)

### <a name="create-a-health-probe"></a>Criar uma sonda de estado de funcionamento

Contoso cria uma pesquisa de estado de funcionamento para que o Balanceador de carga pode monitorizar o estado de funcionamento da aplicação. A sonda dinamicamente adiciona ou remove as VMs a rotação de Balanceador de carga, com base na forma de responder para verificações de estado de funcionamento.

Se criarem a sonda da seguinte forma: 

1. Nas definições do Balanceador de carga no portal, Contoso cria uma sonda do Estado de funcionamento: **SQLAlwaysOnEndPointProbe**.
2. Configuram a sonda para monitorizar as VMs na porta TCP 59999.
3. Se definir um intervalo de 5 segundos entre as pesquisas e um limiar de 2. Se duas sondas falharem, a VM será considerada mau estado de funcionamento.

    ![Sonda](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

### <a name="configure-the-load-balancer-to-receive-traffic"></a>Configurar o Balanceador de carga para receber tráfego


Agora, Contoso necessidades de que uma regra de Balanceador de carga para defins como o tráfego é distribuído para as VMs.

- O endereço IP Front-end processa tráfego de entrada.
- O conjunto IP back-end recebe o tráfego.

Se criarem a regra da seguinte forma:

1. Nas definições do Balanceador de carga no portal, se adicionarem um nova regra de balanceamento de carga: **SQLAlwaysOnEndPointListener**.
2. Contoso define um serviço de escuta de front-end para receber o tráfego de cliente do SQL Server recebido em TCP 1433.
3. Se especificarem o conjunto de back-end para o qual o tráfego será encaminhado e a porta em que as VMs escutam tráfego.
4. Contoso permite que o IP flutuante (devolução direta do servidor). Isto é sempre necessário para o SQL AlwaysOn.

    ![Sonda](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

**Precisa de mais ajuda?**

- [Obter uma descrição geral](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) do Balanceador de carga do Azure.
- [Saiba mais sobre](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-basic-internal-portal) criação de um balanceador de carga.



## <a name="step-4-prepare-azure-for-the-site-recovery-service"></a>Passo 4: Preparar do Azure para o serviço de recuperação de sites

Seguem-se os componentes do Azure que Contoso tem de implementar a recuperação de Site:

- Uma VNet na qual as VMs estarão localizadas quando estes estiver a criar durante a ativação pós-falha.
- Uma conta de armazenamento do Azure para armazenar dados replicados. 
- Um cofre dos serviços de recuperação no Azure.

Se configuraram estas da seguinte forma:

1.  Contoso já criado uma rede/sub-rede podem utilizar para recuperação de sites quando estes [implementada a infraestrutura do Azure](contoso-migration-rehost-vm-sql-ag.md).

    - A aplicação de SmartHotel é uma aplicação de produção e WEBVM será migrada para a rede de produção do Azure (VNET-PROD-EUS2) na região primária US2 Leste.
    - WEBVM será colocada no grupo de recursos ContosoRG, que é utilizado para recursos de produção, e a sub-rede de produção (PROD-FE-EUS2).

2. Contoso cria uma conta de armazenamento do Azure (contosovmsacc20180528) na região primária.

    - Utilizam uma conta para fins gerais, com o armazenamento padrão e replicação do LRS.
    - A conta tem de ser na mesma região que o cofre.

    ![Armazenamento de recuperação de site](media/contoso-migration-rehost-vm-sql-ag/asr-storage.png)

3. Com a conta de armazenamento e de rede no local, agora criarem um cofre dos serviços de recuperação (**ContosoMigrationVault**) e colocar o **ContosoFailoverRG** grupo de recursos na região primária EUA Leste 2 .

    ![Cofre dos Serviços de Recuperação](media/contoso-migration-rehost-vm-sql-ag/asr-vault.png)

**Precisa de mais ajuda?**

[Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) a configurar do Azure Site Recovery.


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>Passo 4: Preparar VMware no local para a recuperação de Site

Eis o que Contoso prepara no local:

- Uma conta no vCenter server ou vSphere ESXi anfitrião, ao automatizar a deteção VM.
- Uma conta que permite a instalação automática do serviço de mobilidade em VMs de VMware que pretende replicar.
- No local as definições da VM, para que Contoso possam ligar à VM do Azure replicadas após a ativação pós-falha.


### <a name="prepare-an-account-for-automatic-discovery"></a>Preparar uma conta para a deteção automática

O Site Recovery precisa de ter acesso a servidores VMware para:

- Detete automaticamente as VMs. 
- Orquestrar a replicação, a ativação pós-falha e a reativação pós-falha.
- É necessária, pelo menos, uma conta só de leitura. Precisa de uma conta que pode executar operações, tais como criar e a remoção de discos e a ativar as VMs.

Contoso configura a conta da seguinte forma:

1. Contoso cria uma função ao nível do vCenter.
2. Contoso, em seguida, atribui essa função as permissões necessárias.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Preparar uma conta para a instalação do serviço de Mobilidade

O serviço de mobilidade tem de ser instalado em cada VM.

- Recuperação de sites pode efetuar uma instalação push automática deste componente quando a replicação está ativada para a VM.
- Precisa de uma conta que a recuperação de sites podem utilizar para aceder a VM para a instalação push. Especificar esta conta ao configurar a replicação na consola do Azure.
- A conta pode ser o domínio ou local, com permissões para instalar na VM.




### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparar a ligação para VMs do Azure após a ativação pós-falha

Após a ativação pós-falha, a Contoso pretende ligar-se para VMs do Azure. Para tal, podem efetuar o seguinte antes da migração:

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


## <a name="step-5-replicate-the-on-premises-vms-to-azure-with-site-recovery"></a>Passo 5: Replicar as VMs no local para o Azure com a recuperação de Site

Para que podem executar uma migração para o Azure, Contoso tem de configurar e ativar a replicação.

### <a name="set-a-replication-goal"></a>Definir um objetivo de replicação

1. No cofre, sob o nome do cofre (ContosoVMVault) o utilizador selecionar um objetivo de replicação (**introdução** > **recuperação de Site** > **preparar a infraestrutura** .
2. Se especificarem que as suas máquinas estão localizados no local, em execução no VMware e replicar para o Azure.

    ![Objetivo da replicação](./media/contoso-migration-rehost-vm-sql-ag/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Confirme o planeamento da implementação

Para continuar, necessitam de confirmar que que tenham concluído planeamento da implementação, selecionando **Sim, ter efetuado esta**. Neste cenário Contoso são apenas migração de uma VM e não precisa de planeamento da implementação.

### <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem

Contoso tem de configurar o respetivo ambiente de origem. Para tal, podem transferir um modelo OVF e utilizá-lo para implementar o servidor de configuração da recuperação de sites como altamente disponível, VM de VMware no local. Depois do servidor de configuração está a funcionar, se registar no cofre.

O servidor de configuração é executado um número de componentes:

- O componente de servidor de configuração que coordena as comunicações entre no local e o Azure e gere a replicação de dados.
- O servidor de processos atua como um gateway de replicação. Recebe dados de replicação; otimiza-os com a colocação em cache, compressão e encriptação; e envia-os para o armazenamento do Azure.
- O servidor de processos também instala o Serviço de Mobilidade nas VMs que pretende replicar, e efetua a deteção automática de VMs VMware no local.

Contoso execute estes passos da seguinte forma:


1. No cofre, transferem o modelo OVF **preparar infraestrutura** > **origem** > **servidor de configuração**.
    
    ![Transferir OVF](./media/contoso-migration-rehost-vm-sql-ag/add-cs.png)

2. Se importar o modelo para VMware para criar e implementar a VM.

    ![Modelo OVF](./media/contoso-migration-rehost-vm-sql-ag/vcenter-wizard.png)

3. Quando for ativado a VM pela primeira vez, arrancarem para uma experiência de instalação do Windows Server 2016. Aceitar o contrato de licença e introduza uma palavra-passe de administrador.
4. Após a conclusão da instalação, iniciam sessão no VM como administrador. No primeiro início de sessão, a ferramenta de configuração de recuperação de sites do Azure é executado por predefinição.
5. Na ferramenta, se especificarem um nome a utilizar para registar o servidor de configuração no cofre.
6. A ferramenta verifica se a VM pode ligar ao Azure. Depois da ligação for estabelecida, podem iniciar sessão para a subscrição do Azure. As credenciais têm de ter acesso ao cofre no qual pretende registar o servidor de configuração.

    ![Registar o servidor de configuração](./media/contoso-migration-rehost-vm-sql-ag/config-server-register2.png)

7. A ferramenta executa algumas tarefas de configuração e, em seguida, é reiniciada.
8. Iniciar sessão máquina novamente e o Assistente de gestão do servidor de configuração é iniciado automaticamente.
9. No assistente, o utilizador selecionar o NIC para receber o tráfego de replicação. Não é possível alterar esta definição depois de estar configurada.
10. O utilizador selecionar a subscrição, o grupo de recursos e o Cofre registar o servidor de configuração.
        ![Cofre](./media/contoso-migration-rehost-vm-sql-ag/cswiz1.png) 

10. Estes, em seguida, transferiram e instalar o servidor de MySQL e VMWare PowerCLI. 
11. Após a validação, se especificarem o FQDN ou endereço IP do anfitrião do servidor ou vSphere do vCenter. Deixe a porta predefinida e especifique um nome amigável para o servidor vCenter.
12. Se especificarem a conta que estes criados para a deteção automática e as credenciais são utilizadas para instalar automaticamente o serviço de mobilidade. Para máquinas do Windows, a conta tem privilégios de administrador local nas VMs.

    ![vCenter](./media/contoso-migration-rehost-vm-sql-ag/cswiz2.png)

7. Após a conclusão de registo, no portal do Azure, a Contoso duplo verifica que o servidor de configuração e o servidor do VMware estão listadas no **origem** página no cofre. Deteção pode demorar 15 minutos ou mais. 
8. Recuperação de sites, em seguida, liga-se aos servidores do VMware com as definições especificadas e Deteta as VMs.

### <a name="set-up-the-target"></a>Configurar o destino

Agora Contoso Especifica as definições de replicação de destino.

1. No **preparar a infraestrutura** > **destino**, o utilizador de selecionar as definições de destino.
2. Recuperação de sites verifica que é uma conta de armazenamento do Azure e a rede de destino especificado.

### <a name="create-a-replication-policy"></a>Criar uma política de replicação

Não, Contoso pode criar uma política de replicação.

1. No **preparar a infraestrutura** > **as definições de replicação** > **política de replicação** >  **criar e Associar**, se criarem uma política **ContosoMigrationPolicy**.
2. Se utilizarem as predefinições:
    - **Limiar RPO**: predefinição de 60 minutos. Este valor define com que frequência são criados pontos de recuperação. Será gerado um alerta se a replicação contínua exceder este limite.
    - **Retenção do ponto de recuperação**. Predefinição de 24 horas. Este valor Especifica o tempo da janela de retenção para cada ponto de recuperação. As VMs replicadas podem ser recuperadas para qualquer ponto numa janela.
    - **Frequência de instantâneos consistentes com aplicação**. Predefinição de uma hora. Este valor Especifica a frequência com que são criados os instantâneos consistentes com aplicações.
 
        ![Criar política de replicação](./media/contoso-migration-rehost-vm-sql-ag/replication-policy.png)

5. A política é associada automaticamente ao servidor de configuração. 

    ![Associar a política de replicação](./media/contoso-migration-rehost-vm-sql-ag/replication-policy2.png)



### <a name="enable-replication"></a>Ativar a replicação

Agora pode começar Contoso WebVM a replicar.

1. No **replicar aplicação** > **origem** > **+ replicar** , selecionarem as definições de origem.
2. Indicam que pretende ativar a VMs, selecione o servidor vCenter e o servidor de configuração.

    ![Ativar a replicação](./media/contoso-migration-rehost-vm-sql-ag/enable-replication1.png)

3. Agora, se especificarem as definições de destino, incluindo o grupo de recursos e VNet e a conta de armazenamento no qual serão armazenados os dados replicados.

     ![Ativar a replicação](./media/contoso-migration-rehost-vm-sql-ag/enable-replication2.png)

3. Contoso seleciona WebVM para replicação, verifica a política de replicação e ativa a replicação. Recuperação de site é instalado o serviço de mobilidade na VM quando a replicação está ativada.
 
    ![Ativar a replicação](./media/contoso-migration-rehost-vm-sql-ag/enable-replication3.png)

4. Controlar o progresso de replicação no **tarefas**. Depois de a tarefa **Finalizar Proteção** ser executada, a máquina está preparada para ativação pós-falha.
5. No **Essentials** no portal do Azure, Contoso pode ver a estrutura para as VMs replicar para o Azure.

    ![Vista de infraestrutura](./media/contoso-migration-rehost-vm-sql-ag/essentials.png)


**Precisa de mais ajuda?**

- Pode ler instruções completa de todos estes passos no [configurar a recuperação após desastre para as VMs de VMware no local](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Instruções detalhadas estão disponíveis para o ajudar a [configurar o ambiente de origem](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementar o servidor de configuração](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), e [configurar as definições de replicação](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- Pode saber mais sobre [ativar replicação](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-5-install-the-database-migration-assistant-dma"></a>Passo 5: Instalar o Assistente de migração de base de dados (DMA)

Contoso irá migrar a base de dados SmartHotel a VM do Azure **SQLAOG1** utilizando o DMA. Configuraram DMA da seguinte forma:

1. Se transferiram a ferramenta do [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) à VM do servidor de SQL no local (**SQLVM**).
2. Se executam a configuração (DownloadMigrationAssistant.msi) na VM.
3. No **concluir** página, selecione **iniciar dados migração assistente Microsoft** antes de concluir o assistente.

## <a name="step-6-migrate-the-database-with-dma"></a>Passo 6: Migrar a base de dados com DMA

1. No DMA executam uma migração novo - **SmartHotel**.
2. O utilizador selecionar o **tipo de servidor de destino** como **do SQL Server em Azure Virtual Machines**. 

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-1.png)

3. Nos detalhes de migração, adicionar **SQLVM** como servidor de origem, e **SQLAOG1** como destino. Se especificarem credenciais para cada máquina.

     ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-2.png)

4. Se criarem uma partilha local para obter as informações da base de dados e a configuração. Tem de estar acessível com acesso de escrita pela conta de serviço do SQL Server no SQLVM e SQLAOG1.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-3.png)

5. Contoso seleciona os inícios de sessão que devem ser migrados e inicia a migração. Depois de concluído, DMA mostra a migração como concluída com êxito.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-4.png)

6. Estes Certifique-se de que a base de dados está em execução **SQLAOG1**.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-5.png)

O DMS estabelece ligação à VM do servidor de SQL no local através de uma ligação de VPN de site a site entre o Centro de dados da Contoso e o Azure e, em seguida, migra a base de dados.

## <a name="step-7-protect-the-database"></a>Passo 7: Proteger a base de dados

Com a base de dados de aplicação em execução no **SQLAOG1**, Contoso agora pode protegê-lo a utilizar grupos de Disponibilidade AlwaysOn. Configurar AlwaysOn utilizando o SQL Server Management Studio e, em seguida, atribuir um serviço de escuta utilizar clustering do Windows. 

### <a name="create-an-alwayson-availability-group"></a>Criar um grupo de Disponibilidade AlwaysOn

1. No SQL Server Management Studio, clique com botão direito no **sempre de elevada disponibilidade,** para iniciar o **Assistente de novo grupo de disponibilidade**.
2. No **especificar opções de**, nome do grupo de disponibilidade **SHAOG**. No **selecionar bases de dados**, selecionarão SmartHotel base de dados.

    ![Grupo de Disponibilidade AlwaysOn](media/contoso-migration-rehost-vm-sql-ag/aog-1.png)

3. No **especificar réplicas**, adicione os dois nós de SQL Server como as réplicas de disponibilidade e configurá-los para fornecer ativação pós-falha automática com consolidação síncrona.

     ![Grupo de Disponibilidade AlwaysOn](media/contoso-migration-rehost-vm-sql-ag/aog-2.png)

4. Se configurarem um serviço de escuta para o grupo (**SHAOG**) e porta. O endereço IP do Balanceador de carga interno é adicionado como um endereço IP estático (10.245.40.100).

    ![Grupo de Disponibilidade AlwaysOn](media/contoso-migration-rehost-vm-sql-ag/aog-3.png)

5. No **selecionar sincronização de dados**, permitem que o seeding automático. Com esta opção, o SQL Server cria automaticamente réplicas secundárias para cada base de dados no grupo, para Contoso não tem de criar cópias de segurança e restaurar estes manualmente. Após a validação, é criado o grupo de disponibilidade.

    ![Grupo de Disponibilidade AlwaysOn](media/contoso-migration-rehost-vm-sql-ag/aog-4.png)

6. Contoso Ocorreu um problema ao criar o grupo. Não estão a utilizar segurança integrada de Windows de diretório Active Directory e, consequentemente, tem de conceder permissões para o início de sessão do SQL Server para criar as funções de Cluster de ativação pós-falha do Windows.

    ![Grupo de Disponibilidade AlwaysOn](media/contoso-migration-rehost-vm-sql-ag/aog-5.png)

6. Depois do grupo for criado, Contoso pode vê-lo no SQL Server Management Studio.

### <a name="configure-a-listener-on-the-cluster"></a>Configurar um serviço de escuta no cluster

Como último passo na configuração da implementação do SQL Server, Contoso configura o Balanceador de carga interno como o serviço de escuta no cluster e coloca o serviço de escuta online. Se utilizarem um script para efetuar este procedimento.

![O serviço de escuta do cluster](media/contoso-migration-rehost-vm-sql-ag/cluster-listener.png)


### <a name="verify-the-configuration"></a>Verifique a configuração

Com tudo configurado, Contoso tem agora um grupo de disponibilidade funcional no Azure que utiliza a base de dados migrado. Estes verificar isto, ligando-se ao balanceador de carga interno no SQL Server Management Studio.

![Ligar o ILB](media/contoso-migration-rehost-vm-sql-ag/ilb-connect.png)

**Precisa de mais ajuda?**
- Saiba mais sobre como criar um [grupo de disponibilidade](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#create-the-availability-group) e [escuta](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#configure-listener).
- Manualmente [configurar o cluster para utilizar o endereço IP do Balanceador de carga](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener#configure-the-cluster-to-use-the-load-balancer-ip-address).
- [Saiba mais](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-2) sobre como criar e através da SAS.


## <a name="step-8-migrate-the-vm-with-site-recovery"></a>Passo 8: Migrar a VM com a recuperação de Site

Contoso execute uma rápida ativação pós-falha de teste e, em seguida, migre a VM.

### <a name="run-a-test-failover"></a>Executar uma ativação pós-falha de teste

Executar uma ativação pós-falha de teste ajuda a assegurar que tudo está a funcionar conforme esperado antes da migração. 

1. Contoso executa uma ativação pós-falha de teste para o ponto mais recente no tempo (**mais recentes processados**).
2. O utilizador selecionar **encerrar a máquina antes de iniciar a ativação pós-falha**, para que a recuperação de Site tenta encerre a VM de origem antes de acionar a ativação pós-falha. A ativação pós-falha continua, mesmo que o encerramento falhe. 
3. É executada a ativação pós-falha de teste: 

    - Verificação de um pré-requisitos é executado para se certificar de que todas as condições necessárias para a migração estão em vigor.
    - A ativação pós-falha processa os dados, para que possa ser criada uma VM do Azure. Se for selecionado o último ponto de recuperação, é criado um ponto de recuperação a partir dos dados.
    - É criada uma VM do Azure com base nos dados processados no passo anterior.

3. Após a ativação pós-falha, a réplica é apresentada VM do Azure no portal do Azure. Contoso verifica que a VM é o tamanho adequado, tenha ligado à rede à direita, e que está em execução. 
4. Depois de verificar, Contoso e limpar a ativação pós-falha, registar e guardar todas as observações. 

### <a name="run-a-failover"></a>Executar uma ativação pós-falha

1. Depois de verificar que a ativação pós-falha de teste trabalhado conforme esperado, Contoso crie um plano de recuperação para a migração e adicione WEBVM para o plano.

     ![Plano de recuperação](./media/contoso-migration-rehost-vm-sql-ag/recovery-plan.png)

2. Se executam uma ativação pós-falha do plano. Selecione o ponto de recuperação mais recente e especifique que a recuperação de sites deve tentar encerre a VM no local antes de acionar a ativação pós-falha.

    ![Ativação pós-falha](./media/contoso-migration-rehost-vm-sql-ag/failover1.png)

3. Após a ativação pós-falha, certifique-se de que a VM do Azure é apresentado como esperado no portal do Azure.

    ![Plano de recuperação](./media/contoso-migration-rehost-vm-sql-ag/failover2.png)

6. Depois de verificar a VM no Azure, concluírem a migração para concluir o processo de migração, pare a replicação para a VM e parar a faturação da recuperação de Site para a VM.

    ![Ativação pós-falha](./media/contoso-migration-rehost-vm-sql-ag/failover3.png)

### <a name="update-the-connection-string"></a>Atualizar a cadeia de ligação

Como o último passo no processo de migração, a Contoso atualizar a cadeia de ligação da aplicação para apontar para a base de dados migrado com o serviço de escuta SHAOG. Esta configuração irá ser alterada no WEBVM agora em execução no Azure.  Esta configuração está localizada na Web. config da aplicação ASP. 

1. Localize o ficheiro em C:\inetpub\SmartHotelWeb\web.config.  Alterar o nome do servidor para refletir o FQDN do AOG: shaog.contoso.com.

    ![Ativação pós-falha](./media/contoso-migration-rehost-vm-sql-ag/failover4.png)  

2. Depois de atualizar o ficheiro e guardá-lo, de reiniciar o IIS no WEBVM. Tal utilizando o IISRESET /RESTART de uma linha de comandos.
2. Após o reinício do IIS, a aplicação está agora a utilizar a base de dados em execução no MI SQL Server.


**Precisa de mais ajuda?**

- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) executar uma ativação pós-falha de teste. 
- [Saiba](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) como criar um plano de recuperação.
- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) efetuar a ativação pós-falha para o Azure.

## <a name="clean-up-after-migration"></a>Limpar após a migração

Após a migração, a aplicação de SmartHotel está em execução numa VM do Azure e a base de dados SmartHotel está localizada no cluster de SQL do Azure.

Agora, Contoso tem de concluir estes passos de limpeza:  

- Remova as VMs no local a partir do inventário do vCenter.
- Remova as VMs das tarefas de cópia de segurança locais.
- Atualize a documentação interna para mostrar as novas localizações e endereços IP para VMs.
- Reveja quaisquer recursos que interagem com as VMs desativadas e atualizar as definições relevantes ou documentação para refletir a nova configuração.
- Adicione que as duas novas VMs (SQLAOG1 e SQLAOG2) devem ser adicionadas para monitorização de sistemas de produção.

## <a name="review-the-deployment"></a>Reveja a implementação

Com os recursos migrados no Azure, Contoso tem totalmente operacionalizar e proteger a sua infraestrutura de novo.

### <a name="security"></a>Segurança

A equipa de segurança de Contoso revê WEBVM de VMs do Azure, SQLAOG1 e SQLAOG2 para determinar os problemas de segurança. 

- Reveja os grupos de segurança de rede (NSGs) para a VM controlar o acesso. Os NSGs são utilizados para garantir que apenas o tráfego permitido para a aplicação pode passar.
- Se estiver a considerar proteger os dados no disco com o Azure Disk Encryption e KeyVault.
- Deve avaliar a encriptação de dados transparente (TDE) e, em seguida, ativá-la na base de dados SmartHotel a executar o novo AOG do SQL Server. [Saiba mais](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017).

[Leia mais](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) sobre práticas de segurança para as VMs.

### <a name="backups"></a>Cópias de segurança

Contoso vai fazer cópias de segurança de dados em WEBVM, SQLAOG1 e SQLAOG2 utilizando o serviço de cópia de segurança do Azure. [Saiba mais](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Otimização de licenciamento e custo

1. Contoso tem licenciamento existente para as respetivas WEBVM e tirar partido do benefício de híbrida do Azure.  Estes irá converter as VMs do Azure existente para tirar partido deste preços.
2. Contoso irá ativar a gestão de custo de Azure licenciados pelos Cloudyn, uma subsidiária Microsoft. É uma solução de gestão de custo de nuvem multi ajuda-o a utilizar e gerir o Azure e outros recursos de nuvem.  [Saiba mais](https://docs.microsoft.com/azure/cost-management/overview) sobre a gestão de custo do Azure. 

## <a name="conclusion"></a>Conclusão

Neste artigo, Contoso rehosted a aplicação de SmartHotel no Azure ao migrar o front-end de aplicação VM no Azure utilizando o serviço de recuperação de sites. Migrar a base de dados de aplicação para um cluster do SQL Server aprovisionado no Azure e o tiver protegido num grupo de Disponibilidade AlwaysOn do SQL Server.

## <a name="next-steps"></a>Passos Seguintes

No seguinte artigo da série, iremos irá mostrar como Contoso realojamento o respetivo suporte técnico osTicket do app service em execução no Linux e implementados com uma base de dados MySQL.


