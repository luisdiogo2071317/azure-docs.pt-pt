---
title: Uma aplicação da Contoso no local de realojamento ao migrar para uma VM do Azure e a instância gerida do SQL do Azure | Documentos da Microsoft
description: Saiba como Contoso realojar uma aplicação no local em VMs do Azure e a instância gerida do SQL do Azure
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: 49a5fb13a881b206c36dcd08a4c2945880e9a4bd
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39002301"
---
# <a name="contoso-migration-rehost-an-on-premises-app-on-azure-vms-and-azure-sql-managed-instance"></a>Migração de Contoso: realojar a uma aplicação no local em VMs do Azure e a instância gerida do SQL do Azure

Este artigo mostra-lhe como a Contoso migra o frontend de aplicação SmartHotel VM para VMs do Azure com o serviço Azure Site Recovery e a base de dados de aplicação para uma instância gerida do Azure SQL.

> [!NOTE]
> Instância gerida do SQL do Azure está atualmente em pré-visualização.

Este documento é um de uma série de artigos que documente como a empresa fictícia Contoso migra os respetivos recursos no local para a cloud do Microsoft Azure. A série contém informações gerais e uma série de cenários que ilustram como configurar uma infraestrutura de migração e executar diferentes tipos de migrações. Cenários de crescem em complexidade e estamos a adicionar artigos adicionais ao longo do tempo.


**Artigo** | **Detalhes** | **Estado**
--- | --- | ---
[Artigo 1: Descrição geral](contoso-migration-overview.md) | Fornece uma descrição geral da estratégia de migração da Contoso, a série de artigos e as aplicações de exemplo que usamos. | Disponível
[Artigo 2: Implementar uma infraestrutura do Azure](contoso-migration-infrastructure.md) | Descreve como o Contoso prepara a sua infraestrutura do Azure e no local, para a migração. A mesma infra-estrutura é utilizada para todos os artigos de migração. | Disponível
[Artigo 3: Avaliar a recursos no local para migração para o Azure](contoso-migration-assessment.md)  | Mostra como Contoso é executado uma avaliação de uma aplicação de SmartHotel de duas camadas no local em execução no VMware. Contoso avalia as VMs de aplicação com o [do Azure Migrate](migrate-overview.md) serviço e a base de dados do SQL Server do aplicativo com o [Assistente de migração de base de dados](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponível
Artigo 4: Realojar a uma aplicação em VMs do Azure e uma instância gerida de SQL | Demonstra como Contoso é executado uma migração lift-and-shift para o Azure para a aplicação de SmartHotel no local. Contoso migra o front-end de aplicação VM através de [do Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)e a base de dados de aplicação para a instância gerida de SQL, utilizando o [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Este artigo.
[Artigo 5: Realojar a uma aplicação em VMs do Azure](contoso-migration-rehost-vm.md) | Mostra como o Contoso migrar as VMs da aplicação SmartHotel para VMs do Azure, com o serviço Site Recovery. | Disponível
[Artigo 6: Realojar a uma aplicação em VMs do Azure e SQL Server sempre no grupo de disponibilidade](contoso-migration-rehost-vm-sql-ag.md) | Mostra como a Contoso migra a aplicação de SmartHotel. A Contoso utiliza o Site Recovery para migrar as VMs da aplicação e o serviço de migração de base de dados para migrar a base de dados de aplicação para um cluster do SQL Server protegido por um grupo de Disponibilidade AlwaysOn. | Disponível
[Artigo 7: Realojar uma aplicação do Linux em VMs do Azure](contoso-migration-rehost-linux-vm.md) | Mostra como Contoso faz uma migração lift-and-shift da aplicação de osTicket do Linux para VMs do Azure, com o Site Recovery | Disponível
[Artigo 8: Realojar uma aplicação do Linux em VMs do Azure e MySQL do Azure](contoso-migration-rehost-linux-vm-mysql.md) | Demonstra como a Contoso migra a aplicação de osTicket do Linux para as VMs do Azure com o Site Recovery e migra a base de dados de aplicação para uma instância do servidor MySQL do Azure com o MySQL Workbench. | Disponível
[Artigo 9: Refatorizar uma aplicação na base de dados de aplicações Web do Azure e SQL do Azure](contoso-migration-refactor-web-app-sql.md) | Demonstra como Contoso migra a aplicação de SmartHotel para uma aplicação Web do Azure e migra a base de dados de aplicação para a instância de servidor SQL do Azure | Disponível
[Artigo 10: Refatorizar uma aplicação do Linux em aplicações Web do Azure e MySQL do Azure](contoso-migration-refactor-linux-app-service-mysql.md) | Mostra como a Contoso migra a aplicação de osTicket do Linux para aplicações Web do Azure em vários sites, integrados com o GitHub para a entrega contínua. Estes migrar a base de dados de aplicação para uma instância do MySQL do Azure. | Disponível
[Artigo 11: Refatorar o TFS no VSTS](contoso-migration-tfs-vsts.md) | Mostra como a Contoso migra a implantação do Team Foundation Server (TFS) no local ao migrá-lo para Visual Studio Team Services (VSTS) no Azure. | Disponível
[Artigo 12: Rearquitetar uma aplicação em contentores do Azure e base de dados do Azure SQL](contoso-migration-rearchitect-container-sql.md) | Mostra como Contoso migra e rearchitects a respetiva aplicação SmartHotel para o Azure. Eles rearquitetar a camada de web de aplicação como um contentor do Windows e a base de dados de aplicação numa base de dados SQL do Azure. | Disponível
[Artigo 13: Recriar uma aplicação no Azure](contoso-migration-rebuild.md) | Mostra como Contoso reconstruir a sua aplicação de SmartHotel com uma gama de capacidades do Azure e serviços, incluindo serviços de aplicações, Kubernetes do Azure, as funções do Azure, serviços cognitivos e Cosmos DB. | Disponível


Se gostaria de utilizar a aplicação de SmartHotel de exemplo utilizada neste artigo, pode transferi-lo a partir [github](https://github.com/Microsoft/SmartHotel360).

## <a name="on-premises-architecture"></a>Arquitetura de no local

Aqui está um diagrama que mostra a infra-estrutura de locais de Contoso atual.

![Arquitetura de contoso](./media/contoso-migration-rehost-vm-sql-managed-instance/contoso-architecture.png)  

- A Contoso tiver um Data Center principal localizado na cidade de Nova Iorque no Leste dos Estados Unidos.
- Eles têm três ramos locais adicionais nos Estados Unidos.
- O datacenter principal está ligado à internet com uma ligação de ethernet metro Fiber (500 mbps).
- Cada ramo está ligado localmente para a internet através de ligações de classe de negócio, com túneis de IPSec VPN para o datacenter principal. Isso permite que seus toda a rede estar conectada permanentemente e otimiza a conectividade à internet.
- O Centro de dados principal é totalmente virtualizado com o VMware. Eles têm dois anfitriões de Virtualização do ESXi 6.5, geridas pelo vCenter Server 6.5.
- A Contoso utiliza o Active Directory para gestão de identidades e servidores DNS na rede interna.
- Executam os controladores de domínio no Centro de dados em VMs de VMware. Executam os controladores de domínio em ramos locais em servidores físicos.



## <a name="business-drivers"></a>Fatores comerciais

A equipe de liderança de TI tem trabalhado em estreita colaboração com seus parceiros de negócios para compreender o que a empresa quer alcançar com esta migração:

- **Resolver o crescimento do negócio**: Contoso está a crescer e assim há pressão em seus sistemas no local e a infraestrutura.
- **Aumentar a eficiência**: a Contoso precisa de remover procedimentos desnecessários e simplificar processos para seus desenvolvedores e usuários.  Necessidades de negócio IT velocidade e não desperdício de tempo ou dinheiro, entregando assim mais rapidamente sobre os requisitos dos clientes.
- **Aumente a agilidade**: Contoso TI tem de ser mais reativos para com as necessidades do negócio. Tem de ser capaz de reagir mais rapidamente do que as alterações no marketplace, para ativar o sucesso numa economia global.  Ele não pode obter na forma como ou se tornar um bloqueador de negócios.
- **Dimensionamento**: à medida que a empresa cresce com êxito, TI da Contoso tem de fornecer sistemas que são capazes de aumentar ao ritmo mesmo.

## <a name="migration-goals"></a>Objetivos de migração

A equipe de cloud de Contoso tiver afixado para baixo de objetivos para essa migração. Esses objetivos são utilizados para determinar o melhor método de migração:

- Após a migração, a aplicação no Azure deve ter as mesmas capacidades de desempenho, como sucede no respetivo ambiente de VMWare no local.  Mover para a nuvem não significa que o desempenho de aplicações é menos crítico.
- Contoso não quer investir nesta aplicação.  É críticos e importantes para os negócios, mas em sua forma atual pretendem simplesmente mover para a cloud.
- Tarefas administrativas de base de dados devem ser minimizadas após a aplicação é migrada.
- Contoso não quer utilizar uma base de dados do SQL do Azure para esta aplicação e está à procura de alternativas.

## <a name="proposed-architecture"></a>Arquitetura proposta

Neste cenário:

- Contoso quer migrar a sua aplicação de viagens de duas camadas no local.
- A aplicação está em camadas em duas VMs (WEBVM e SQLVM) e localizada no anfitrião ESXi do VMware **contosohost1.contoso.com** (versão 6.5)
- O ambiente do VMware é gerido pelo vCenter Server 6.5 (**vcenter.contoso.com**), em execução numa VM.
- Estes migrar a base de dados de aplicação (SmartHotelDB) para uma instância gerida de SQL do Azure.
- Estes migrar as VMs do VMware no local para uma VM do Azure.
- A Contoso tiver um datacenter no local (contoso-datacenter), com um controlador de domínio no local (**contosodc1**).
- As VMs no local no Centro de dados Contoso serão encerradas depois de terminar a migração.

![Arquitetura do cenário](media/contoso-migration-rehost-vm-sql-managed-instance/architecture.png) 

### <a name="azure-services"></a>Serviços do Azure

**Serviço** | **Descrição** | **Custo**
--- | --- | ---
[Serviço de gestão de base de dados](https://docs.microsoft.com/azure/dms/dms-overview) | O DMS permite migrações totalmente integradas de várias origens de base de dados para plataformas de dados do Azure, com o período de indisponibilidade mínimo. | Saiba mais sobre [regiões suportadas](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) DMS e get [os detalhes dos preços](https://azure.microsoft.com/pricing/details/database-migration/).
[Instância gerida de SQL do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | A instância gerida é um serviço de base de dados gerida que representa uma instância de servidor de SQL totalmente gerida na cloud do Azure. Ele compartilha o mesmo código com a versão mais recente do motor de base de dados do SQL Server e tem as funcionalidades mais recentes, melhoramentos de desempenho e patches de segurança. | Utilizar instâncias geridas da base de dados SQL do Azure em execução no Azure incorrer em encargos com base na capacidade. [Saiba mais](https://azure.microsoft.com/pricing/details/sql-database/managed/). 
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | O serviço orquestra e gere a migração e recuperação após desastre para VMs do Azure e VMs e servidores físicos no local.  | Durante a replicação para o Azure, são cobradas taxas de armazenamento do Azure.  VMs do Azure são criadas e incorrer em custos, quando ocorre a ativação pós-falha. [Saiba mais](https://azure.microsoft.com/pricing/details/site-recovery/) sobre preços e custos.

 

## <a name="migration-process"></a>Processo de migração

Contoso migrará escalões web e de dados da aplicação SmartHotel para o Azure.

1. Contoso já tem a sua infraestrutura do Azure no local, então, eles só precisam de adicionar alguns componentes do Azure específicos para este cenário.
2. A camada de dados será migrada utilizando o serviço de migração de dados (DMS).  O DMS liga-se para a VM do SQL Server no local através de uma ligação de VPN de site a site entre o datacenter da Contoso e o Azure e, em seguida, migra a base de dados.
3. A camada web será migrada utilizando uma migração lift-and-shift com o Azure Site Recovery. Isto irá envolve a preparação do ambiente de VMware no local, configurar e ativar a replicação e para migrar as VMs por pós-falha das mesmas para o Azure.

     ![Arquitetura de migração](media/contoso-migration-rehost-vm-sql-managed-instance/migration-architecture.png) 


## <a name="prerequisites"></a>Pré-requisitos

Eis o que Contoso (e) precisa para este cenário.

**Requisitos** | **Detalhes**
--- | ---
**Inscreva-se na pré-visualização** | Tem de estar inscritos no SQL gerida instância pré-visualização pública limitada. Precisa de uma subscrição do Azure para [Inscreva-se](https://portal.azure.com#create/Microsoft.SQLManagedInstance). Inscreva-se pode demorar alguns dias para concluir, por isso, certifique-se de que o fizer antes de começar a implementar este cenário.
**Subscrição do Azure** | Deve já tiver criado uma subscrição quando realizada a avaliação no primeiro artigo desta série. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Se criar uma conta gratuita, será o administrador da sua subscrição e poderá executar todas as ações.<br/><br/> Se utilizar uma subscrição existente e não for o administrador, terá de trabalhar com o administrador para lhe atribuir permissões de proprietário ou contribuinte.<br/><br/> Se precisar de permissões mais granulares, reveja [este artigo](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Recuperação de sites (no local)** | O vCenter server no local deve estar a executar a versão 5.5, 6.0 ou 6.5<br/><br/> Um anfitrião ESXi a executar a versão 5.5, 6.0 ou 6.5<br/><br/> Um ou mais VMs de VMware em execução no anfitrião ESXi.<br/><br/> As VMs têm de cumprir [requisitos do Azure](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Suportado [rede](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) e [armazenamento](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) configuração.
**DMS** | Para o DMS é necessário um [compatível no dispositivo VPN local](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> Tem de ser capaz de configurar o dispositivo VPN no local. Tem de ter um endereço IPv4 público com acesso exterior e o endereço não pode estar localizado atrás de um dispositivo NAT.<br/><br/> Certifique-se de que ter acesso à base de dados do SQL Server no local.<br/><br/> O Firewall do Windows deve ser capaz de acessar o motor de base de dados de origem. [Saiba mais](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Se existir uma firewall na frente do seu computador de base de dados, adicione regras para permitir o acesso à base de dados e aos ficheiros através da porta SMB 445.<br/><br/> As credenciais utilizadas para ligar à origem de SQL Server e instância gerida de destino têm de ser membros da função de servidor sysadmin.<br/><br/> Precisa de uma rede partilhar na base de dados no local que o DMS pode usar para criar cópias de segurança da base de dados de origem.<br/><br/> Certifique-se de que a conta de serviço a executar a instância do SQL Server de origem tem privilégios de escrita na partilha de rede.<br/><br/> Tome nota de um utilizador do Windows (e a palavra-passe) que tem o privilégio de controlo total na partilha de rede. O Azure Database Migration Service representa estas credenciais de utilizador para carregar ficheiros de cópia de segurança para o contentor de armazenamento do Azure.<br/><br/> O processo de instalação do SQL Server Express define o protocolo de TCP/IP para **desativado** por predefinição. Certifique-se de que está ativada.


## <a name="scenario-steps"></a>Passos do cenário

Aqui está como Contoso vai configurar a implementação:

> [!div class="checklist"]
> * **Passo 1: Configurar uma instância gerida do SQL Azure**: que precisam de uma instância gerida previamente criada para o qual irá migrar a base de dados do SQL Server no local.
> * **Passo 2: Preparar o DMS**: que precisam para registar o fornecedor de migração de base de dados, criar uma instância e, em seguida, crie um projeto do DMS. Eles também precisam de configurar o URI de SA para DMS. Uma assinatura de acesso partilhado (SAS) Uniform Resource Identifier (URI) disponibilizam acesso delegado a recursos na sua conta de armazenamento, para que pode conceder permissões limitadas a objetos de armazenamento. Eles configurar um URI de SAS para que o DMS pode aceder ao contentor de conta de armazenamento para o qual o serviço carrega os ficheiros de cópia de segurança do SQL Server.
> * **Passo 3: Preparar o Azure para o Site Recovery**: para o Site Recovery, tem de criar uma conta de armazenamento do Azure para armazenar dados replicados e criar um cofre dos serviços de recuperação.
> * **Passo 4: Preparar o VMware no local para o Site Recovery**: Contoso vai preparar contas para instalação de deteção e o agente da VM e preparar a ligação para as VMs do Azure após a ativação pós-falha.
> * **Passo 5: Replicar VMs**: para configurar a replicação, eles possível configurar o ambiente de origem e destino de recuperação de sites, configurar uma política de replicação em iniciar a replicação de VMs para o armazenamento do Azure.
> * **Passo 6: Migrar a base de dados com o DMS**: agora, eles podem migrar a base de dados.
> * **Passo 7: Migrar as VMs com o Site Recovery**: executar uma ativação pós-falha de teste para se certificar de que está tudo a funcionar e, em seguida, execute uma ativação pós-falha completa para migrar as VMs para o Azure.


## <a name="step-1-prepare-an-azure-sql-managed-instance"></a>Passo 1: Preparar uma instância gerida de SQL do Azure

Para configurar uma instância gerida do Azure SQL, a Contoso precisa de uma sub-rede:

- A sub-rede tem de estar dedicada. Tem de estar vazio e não conter qualquer outro serviço em nuvem, e não pode ser uma sub-rede de gateway.
- Depois da instância gerida é criada, não pode adicionar recursos a ele.
- A sub-rede não pode ter um NSG associado ao mesmo.
- A sub-rede tem de ter uma tabela de rota de utilizador (UDR) com Internet de salto seguinte 0.0.0.0/0 como o único caminho atribuído ao mesmo. 
- DNS personalizado opcional: se o DNS personalizado é especificado na VNet, endereço IP de resoluções de recursiva do Azure (por exemplo, 168.63.129.16) tem de ser adicionado à lista. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).
- A sub-rede não pode ter um ponto final de serviço (armazenamento ou SQL) associado a ele. Pontos finais de serviço devem ser desativados na rede virtual.
- A sub-rede tem de ter o mínimo de 16 endereços IP. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#determine-the-size-of-subnet-for-managed-instances) sobre o dimensionamento a sub-rede de instância gerida.
- Nos respetivos ambientes híbridos, são necessárias definições de DNS personalizadas. Contoso irá configurar as definições de DNS para utilizar um ou mais dos seus servidores de DNS do Azure. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns) sobre a personalização de DNS.

### <a name="set-up-a-virtual-network-for-the-managed-instance"></a>Configurar uma rede virtual para a instância gerida

Contoso configura a VNet da seguinte forma: 

1. Contoso cria uma nova VNet (VNET-SQLMI-EU2) na região E.U.A. Leste 2 primária, no grupo de recursos de ContosoNetworkingRG.
2. Contoso atribui um espaço de endereços de 10.235.0.0/24, garantindo que o intervalo não se sobrepõe quaisquer outras redes da empresa Contoso.
2. Eles adicionar duas sub-redes para a rede:
    - SQLMI-DS-EUS2 (10.235.0.0.25)
    - SQLMI-SAW-EUS2 (10.235.0.128/29). Esta sub-rede vai ser utilizada para anexar o diretório para a instância gerida (SQLMI).

    ![Rede de instância gerida](media/contoso-migration-rehost-vm-sql-managed-instance/mi-vnet.png)

6. Depois da VNet e sub-redes são implementadas, o Contoso elementos redes da seguinte forma:

    - Para preparar o VMware para o Site Recovery, eis o que a Contoso precisa de fazer:
    - Prepare uma conta no vCenter server ou vSphere ESXi no anfitrião, para automatizar a deteção de VMS.

    ![Prepare uma conta que permite a instalação automática do serviço de mobilidade em VMs de VMware que pretende replicar.](media/contoso-migration-rehost-vm-sql-managed-instance/mi-peering.png)

7. Prepare VMs no local, para que se possam ligar a VMs do Azure quando forem criadas após a ativação pós-falha. Precisa de uma conta que pode executar operações como criar e remover discos e Ativando a VMs. Contoso configura a conta da seguinte forma: Cria uma função ao nível do vCenter.

    - Atribui as permissões necessárias para essa função.
    - Saiba mais sobre criar e atribuir uma função para a deteção automática.
    - Site Recovery pode fazer uma instalação push automática deste componente quando ativa a replicação para a VM.
    - Para instalação push automática do, terá de preparar uma conta que o Site Recovery irá utilizar para aceder à VM.

     ![Esta conta é especificada quando configurou a replicação na consola do Azure.](media/contoso-migration-rehost-vm-sql-managed-instance/mi-dns.png)

**Precisa de mais ajuda?**

- [Precisa de um domínio ou conta local com permissões para instalar na VM.
- [Precisa de mais ajuda
- [Saiba mais sobre](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering) criar uma conta para a instalação push do serviço de mobilidade.
- [Após a ativação pós-falha para o Azure, a Contoso quer ser capaz de ligar as VMs replicadas no Azure.



### <a name="set-up-routing"></a>Para fazer isso, há algumas coisas que precisam de fazer na VM no local, antes de executarem a migração:

Para obter acesso através da internet, que ative o RDP na VM no local antes da ativação pós-falha e certifique-se de que as regras TCP e UDP estão adicionadas para o pública perfil e que o RDP é permitido na Firewall do Windows  Aplicações permitidas para todos os perfis. Para ter acesso através da respetiva VPN de site a site, que ative o RDP na máquina no local e permitir que o RDP na Firewall do Windowsaplicações e funcionalidades permitidas para domínio e Privada redes.

- Eles definir a política de SAN do sistema operacional na VM no local para OnlineAll.
- Além disso, quando executam uma ativação pós-falha que precisam verificar o seguinte: Não deve haver nenhuma atualização do Windows pendentes na VM quando acionar uma ativação pós-falha.
- Se existirem, não poderá iniciar sessão para a máquina virtual enquanto a atualização for concluída.
- Após a ativação pós-falha, deverá verificar diagnósticos de arranque para ver uma captura de ecrã da VM.

1. Se não funcionar, deve verificar que a VM está em execução e revisá-los sugestões de resolução de problemas. Passo 5: Replicar as VMs no local para o Azure com o Site Recovery

    ![Tabela de rotas](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table.png)

2. Antes de executar uma migração para o Azure, a Contoso precisa de configurar a replicação e ativar a replicação para as suas VM no local.

    ![Definir um objetivo de replicação](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-prefix.png)
    
3. No cofre, sob o nome do cofre (ContosoVMVault) definir um objetivo de replicação (introduçãoSite Recoverypreparar a infraestrutura. 

    ![Especifica que suas máquinas estão localizados no local, que são VMs do VMware, e que pretende replicar para o Azure.](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-subnet.png)
    
**Precisa de mais ajuda?**

[Para continuar, o que precisam para confirmar que eles concluam planeamento da implementação, selecionando [Sim, já o fiz](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal#create-new-route-table-and-a-route).

### <a name="create-a-managed-instance"></a>Nesta implementação Contoso são apenas a migrar uma VM única e não precisa de planeamento da implementação.

Agora que precisam de configurar o seu ambiente de origem.

1. Para fazer isso eles transferir um modelo OVF e utilizá-lo para implementar o servidor de configuração e seus componentes associados como de elevada disponibilidade, a VM de VMware no local. 
2. Depois do servidor de configuração VM é criada e iniciada, Contoso pode registá-lo no cofre. [Contoso execute estes passos da seguinte forma:

    ![Instância gerida](media/contoso-migration-rehost-vm-sql-managed-instance/mi-create.png)

3. Eles transferem o modelo OVF a partir do portal do Azure (preparar infraestruturaorigemservidor de configuração).

    - Transferir OVF
    - Eles importam o modelo para o VMware, para criar e implementar a VM. 

    ![Instância gerida](media/contoso-migration-rehost-vm-sql-managed-instance/mi-resources.png)

**Precisa de mais ajuda?**

[Quando ativam a VM pela primeira vez, ele arranca para uma experiência de instalação do Windows Server 2016.

## <a name="step-2-prepare-dms"></a>Aceite o contrato de licença e introduza uma palavra-passe de administrador.

Para preparar o DMS, a Contoso precisa de fazer duas coisas:

- No tempo de primeiro início de sessão, a ferramenta de configuração do Azure Site Recovery é executado por predefinição.
- Na ferramenta, eles especificam um nome a utilizar ao registar o servidor de configuração no cofre. Depois da ligação é estabelecida, selecione iniciar sessão, para iniciar sessão para a subscrição do Azure. 
- As credenciais têm de ter acesso ao Cofre no qual o servidor de configuração será registado.


Registar servidor de configuração

1. Contoso regista o fornecedor de migração de base de dados na sua subscrição.
    ![No assistente, eles selecionam o NIC para receber o tráfego de replicação.](media/contoso-migration-rehost-vm-sql-managed-instance/dms-subscription.png)

2. Não é possível alterar esta definição depois de estar configurada.

    ![SELECIONAR A SUBSCRIÇÃO, GRUPO DE RECURSOS E COFRE NO QUAL PRETENDE REGISTAR O SERVIDOR DE CONFIGURAÇÃO.](media/contoso-migration-rehost-vm-sql-managed-instance/dms-sas.png)

3. Cofre 

    ![Eles, em seguida, transferirem e instalar o servidor MySQL e o VMWare PowerCLI.](media/contoso-migration-rehost-vm-sql-managed-instance/dms-instance.png)

4. Em seguida, eles validar as definições do servidor.
    - Após a validação, eles especificam o FQDN ou endereço IP do vCenter server ou vSphere no anfitrião.
    - Deixe a porta predefinida e especifique um nome amigável para o servidor vCenter no Azure.  Eles precisam especificar a conta que o próprio criou anteriormente, para que o Site Recovery pode detetar automaticamente as VMs de VMware que estão disponíveis para replicação.

        ![Especifica as credenciais para instalar automaticamente o serviço de mobilidade quando a replicação está ativada.](media/contoso-migration-rehost-vm-sql-managed-instance/dms-network.png)

**Precisa de mais ajuda?**
- [Para máquinas Windows, a conta tem privilégios de administrador local nas VMs.
- [Após a conclusão do registo, no portal do Azure, a Contoso duplo verifica que o servidor de configuração e o servidor VMware estão listados no [origem](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2) página no cofre.


## <a name="step-3-prepare-azure-for-the-site-recovery-service"></a>Deteção pode demorar 15 minutos ou mais.

Em seguida, o site Recovery liga-se aos servidores VMware com as definições especificadas e Deteta as VMs.

- Configurar o destino
- Agora a Contoso precisa de configurar o ambiente de replicação de destino. 
- Na preparar infraestruturadestino, eles selecionam as definições de destino.

Recuperação de sites verifica que existe uma conta de armazenamento do Azure e a rede no destino especificado.

1. Depois de origem e de destino estão configurados, a Contoso está pronta para criar uma política de replicação e associá-la com o servidor de configuração. Na [preparar infraestrutura](contoso-migration-infrastructure.md)definições de replicaçãopolítica de replicaçãocriar e Associar, criaram uma política ContosoMigrationPolicy.](contoso-migration-infrastructure.md)
2. Se utilizarem as predefinições: Limiar RPO: predefinição de 60 minutos.

    ![Retenção do ponto de recuperação.](media/contoso-migration-rehost-vm-sql-managed-instance/asr-storage.png)

3. Predefinição de 24 horas.

    ![Cofre dos Serviços de Recuperação](media/contoso-migration-rehost-vm-sql-managed-instance/asr-vault.png)

**Precisa de mais ajuda?**

[Este valor especifica quanto tempo dura o período de retenção para cada ponto de recuperação.


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>Frequência de instantâneos consistentes com a aplicação.

Padrão de uma hora.

- Este valor Especifica a frequência com que os instantâneos consistentes com aplicações são criados.
- Associar a política de replicação
- Pode ler um passo a passo completa de todos estes passos configurar a recuperação após desastre das VMs de VMware no local.


### <a name="prepare-an-account-for-automatic-discovery"></a>Preparar uma conta para a deteção automática

O Site Recovery precisa de ter acesso a servidores VMware para:

- Detetar automaticamente VMs. É necessária, pelo menos, uma conta só de leitura.
- Orquestrar a replicação, a ativação pós-falha e a reativação pós-falha. Instruções detalhadas estão disponíveis para ajudá-lo configurar o ambiente de origem, implementar o servidor de configuração, e configurar as definições de replicação.

Agora Contoso pode começar a replicar o WebVM.

1. Na replicar aplicaçãoorigem+ replicar selecionarem as definições de origem.
2. Eles indicam que se deve ativar as máquinas virtuais, selecione o servidor vCenter e o servidor de configuração.

**Precisa de mais ajuda?**

[Agora, eles especificam as definições de destino, incluindo o grupo de recursos e a rede em que a VM do Azure estarão localizada após a ativação pós-falha e a conta de armazenamento no qual serão armazenados os dados replicados.

### <a name="prepare-an-account-for-mobility-service-installation"></a>Preparar uma conta para a instalação do serviço de Mobilidade

O serviço de Mobilidade tem de ser instalado na VM que quer replicar.

- Contoso seleciona WebVM para replicação.
- Site Recovery instala o serviço de mobilidade em cada VM quando ativa a replicação para o mesmo.
- Com a migração é concluída, a aplicação de SmartHotel está em execução numa VM do Azure e a base de dados SmartHotel está disponível na instância gerida do Azure SQL.
- Agora, a Contoso precisa de fazer uma limpeza, da seguinte forma:

**Remova a máquina WEBVM no inventário do vCenter.**

[Remova a máquina SQLVM no inventário do vCenter.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparar a ligação para VMs do Azure após a ativação pós-falha

Remova WEBVM e SQLVM das tarefas de cópia de segurança locais. Documentação de atualização interna mostram a nova localização, o endereço IP para WEBVM. 

1. Remova SQLVM da documentação.
2. Em alternativa, eles poderiam marcá-la para mostrar como eliminada e já não na VM de inventário.
3. A equipe de segurança da Contoso analisa as VMs do Azure e a instância gerida de SQL, para determinar quaisquer problemas de segurança com a sua implementação.

Eles analisam os grupos de segurança de rede (NSGs) para a VM, para controlar o acesso.

- NSGs são utilizados para garantir que apenas o tráfego permitido para a aplicação pode passar. Eles também Considerando proteger os dados no disco com o Azure Disk Encryption e o Cofre de chaves do Azure.
- Ativar a deteção de ameaças sobre o SQL gerida instância (SQLMI). Eles enviará alertas para seu sistema de suporte técnico de equipe/serviço de segurança para abrir um pedido, desde que seja detetada uma ameaça.


## <a name="step-5-replicate-the-on-premises-vms-to-azure-with-site-recovery"></a>Segurança de instância gerida

Leia mais sobre práticas de segurança para as VMs.

### <a name="set-a-replication-goal"></a>Contoso vai fazer backup dos dados no WEBVM com o serviço de cópia de segurança do Azure.

1. Contoso tem licenças existentes do WEBVM e irá tirar partido do benefício híbrido do Azure.
2. Eles converterá a VM do Azure existentes para tirar partido destes preços.

    ![Objetivo da replicação](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Confirmar o planeamento da implementação

É uma solução de gestão de custos multiclouds que o ajuda a utilizar e gerir o Azure e outros recursos da cloud. Neste artigo, a Contoso rehosted a aplicação de SmartHotel no Azure ao migrar o VM da front-end de aplicação para o Azure com o serviço Site Recovery.

### <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem

Sua migração da base de dados no local para uma instância SQL do Azure geridos com o DMS. No próximo artigo da série, vamos mostrar como Contoso realojar a aplicação de SmartHotel para VMs do Azure com apenas o serviço do Azure Site Recovery. Os componentes no servidor incluem:

- O servidor de configuração que coordena as comunicações entre no local e o Azure e gere a replicação de dados.
- O servidor de processos atua como um gateway de replicação. Recebe dados de replicação; otimiza-os com a colocação em cache, compressão e encriptação; e envia-os para o armazenamento do Azure.
- O servidor de processos também instala o Serviço de Mobilidade nas VMs que pretende replicar, e efetua a deteção automática de VMs VMware no local.
- Depois do servidor de configuração VM é criada e iniciada, Contoso pode registá-lo no cofre.


Contoso execute estes passos da seguinte forma:

1. Eles transferem o modelo OVF a partir do portal do Azure (**preparar infraestrutura** > **origem** > **servidor de configuração**).
    
    ![Transferir OVF](./media/contoso-migration-rehost-vm-sql-managed-instance/add-cs.png)

2. Eles importam o modelo para o VMware, para criar e implementar a VM.

    ![Modelo OVF](./media/contoso-migration-rehost-vm-sql-managed-instance/vcenter-wizard.png)

3.  Quando ativam a VM pela primeira vez, ele arranca para uma experiência de instalação do Windows Server 2016. Aceite o contrato de licença e introduza uma palavra-passe de administrador.
4. Após a instalação terminar, iniciam sessão na VM como administrador. No tempo de primeiro início de sessão, a ferramenta de configuração do Azure Site Recovery é executado por predefinição.
5. Na ferramenta, eles especificam um nome a utilizar ao registar o servidor de configuração no cofre.
6. A ferramenta verifica se a VM pode ligar ao Azure. Depois da ligação é estabelecida, selecione **iniciar sessão**, para iniciar sessão para a subscrição do Azure. As credenciais têm de ter acesso ao Cofre no qual o servidor de configuração será registado. 

    [Registar servidor de configuração](./media/contoso-migration-rehost-vm-sql-managed-instance/config-server-register2.png)

7. A ferramenta executa algumas tarefas de configuração e, em seguida, é reiniciada. Iniciam sessão na máquina novamente e o Assistente de gestão de servidor de configuração é iniciado automaticamente.
8. No assistente, eles selecionam o NIC para receber o tráfego de replicação. Não é possível alterar esta definição depois de estar configurada.
9. Selecionar a subscrição, grupo de recursos e cofre no qual pretende registar o servidor de configuração.
        ![Cofre](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz1.png) 

10. Eles, em seguida, transferirem e instalar o servidor MySQL e o VMWare PowerCLI. Em seguida, eles validar as definições do servidor.
11. Após a validação, eles especificam o FQDN ou endereço IP do vCenter server ou vSphere no anfitrião. Deixe a porta predefinida e especifique um nome amigável para o servidor vCenter no Azure.
12. Eles precisam especificar a conta que o próprio criou anteriormente, para que o Site Recovery pode detetar automaticamente as VMs de VMware que estão disponíveis para replicação. 
13. Especifica as credenciais para instalar automaticamente o serviço de mobilidade quando a replicação está ativada. Para máquinas Windows, a conta tem privilégios de administrador local nas VMs. 

    ![vCenter](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz2.png)

7. Após a conclusão do registo, no portal do Azure, a Contoso duplo verifica que o servidor de configuração e o servidor VMware estão listados no **origem** página no cofre. Deteção pode demorar 15 minutos ou mais. 
8. Em seguida, o site Recovery liga-se aos servidores VMware com as definições especificadas e Deteta as VMs.

### <a name="set-up-the-target"></a>Configurar o destino

Agora a Contoso precisa de configurar o ambiente de replicação de destino.

1. Na **preparar infraestrutura** > **destino**, eles selecionam as definições de destino.
2. Recuperação de sites verifica que existe uma conta de armazenamento do Azure e a rede no destino especificado.

### <a name="create-a-replication-policy"></a>Criar uma política de replicação

Depois de origem e de destino estão configurados, a Contoso está pronta para criar uma política de replicação e associá-la com o servidor de configuração.

1. Na **preparar infraestrutura** > **definições de replicação** > **política de replicação** >  **criar e Associar**, criaram uma política **ContosoMigrationPolicy**.
2. Se utilizarem as predefinições:
    - **Limiar RPO**: predefinição de 60 minutos. Este valor define com que frequência são criados pontos de recuperação. Será gerado um alerta se a replicação contínua exceder este limite.
    - **Retenção do ponto de recuperação**. Predefinição de 24 horas. Este valor especifica quanto tempo dura o período de retenção para cada ponto de recuperação. As VMs replicadas podem ser recuperadas para qualquer ponto numa janela.
    - **Frequência de instantâneos consistentes com a aplicação**. Padrão de uma hora. Este valor Especifica a frequência com que os instantâneos consistentes com aplicações são criados.
 
        ![Criar política de replicação](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy.png)

5. A política é associada automaticamente ao servidor de configuração. 

    ![Associar a política de replicação](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy2.png)


**Precisa de mais ajuda?**

- Pode ler um passo a passo completa de todos estes passos [configurar a recuperação após desastre das VMs de VMware no local](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Instruções detalhadas estão disponíveis para ajudá-lo [configurar o ambiente de origem](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementar o servidor de configuração](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), e [configurar as definições de replicação](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).

### <a name="enable-replication"></a>Ativar a replicação

Agora Contoso pode começar a replicar o WebVM.

1. Na **replicar aplicação** > **origem** > **+ replicar** selecionarem as definições de origem.
2. Eles indicam que se deve ativar as máquinas virtuais, selecione o servidor vCenter e o servidor de configuração.

 ![Ativar a replicação](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication1.png)
 
3. Agora, eles especificam as definições de destino, incluindo o grupo de recursos e a rede em que a VM do Azure estarão localizada após a ativação pós-falha e a conta de armazenamento no qual serão armazenados os dados replicados.

     ![Ativar a replicação](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication2.png)

4. Contoso seleciona WebVM para replicação. Site Recovery instala o serviço de mobilidade em cada VM quando ativa a replicação para o mesmo. 

    ![Ativar a replicação](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication3.png)

5. Contoso verifica que a política de replicação correta está selecionada e permite a replicação para WEBVM. Controlar o progresso de replicação em **tarefas**. Depois de a tarefa **Finalizar Proteção** ser executada, a máquina está preparada para ativação pós-falha.
6. Na **Essentials** no portal do Azure, Contoso pode ver a estrutura para as VMs a replicar para o Azure.

    ![Vista de infraestrutura](./media/contoso-migration-rehost-vm-sql-managed-instance/essentials.png)


**Precisa de mais ajuda?**

Pode ler um passo a passo completa de todos estes passos [ativar a replicação](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).

## <a name="step-6-migrate-the-database-with-dms"></a>Passo 6: Migrar a base de dados com o DMS

A Contoso precisa de criar um projeto do DMS e migrar a base de dados.

### <a name="create-a-dms-project"></a>Criar um projeto do DMS
1. É criar um projeto do DMS. Especifica o tipo de servidor de origem do SQL Server e o destino como a instância gerida da base de dados SQL do Azure.

     ![Projeto do DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-project.png)

2. Depois de criar o projeto, é aberto o Assistente de migração.

### <a name="migrate-the-database"></a>Migrar a base de dados 

1. No Assistente de migração, a Contoso Especifica a VM de origem em que está localizada a base de dados no local e as credenciais para aceder ao mesmo.

    ![Origem DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-source.png)

2. Eles selecionam a base de dados para migrar (SmartHotel.Registration).

    ![Base de dados de origem DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-sourcedb.png)

3. Como um destino, eles especificam o nome da instância gerida no Azure e as credenciais de acesso.

    ![Destino DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-target-details.png)

4. Em seguida, na **+ nova atividade** > **executar migração**, eles especificam definições para executar a migração:
    - Credenciais de origem e de destino.
    - Base de dados para migrar.
    - A partilha de rede que criaram na VM no local. DMS leva as cópias de segurança de origem para esta partilha.
        - A conta de serviço da SQL Server de origem a executar instância tem de ter privilégios de escrita nesta partilha.
        - Especifique o caminho do FQDN para a partilha.
    - O URI de SAS que fornece o DMS com acesso ao contentor da conta de armazenamento para o qual o serviço carrega os ficheiros de cópia de segurança para a migração.

        ![Definições de DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-migration-settings.png)

5. Guardar a migração e executá-lo.
6. Na **descrição geral**, eles monitoram o estado de migração.

    ![Monitor DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor1.png)

7. Após a conclusão da migração, eles Certifique-se de que as bases de dados de destino existem na instância gerida.

    ![Monitor DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor2.png)

## <a name="step-7-migrate-the-vm-with-site-recovery"></a>Passo 7: Migrar a VM com o Site Recovery

Contoso é executada uma ativação pós-falha de teste rápida e, em seguida, migre a VM.

### <a name="run-a-test-failover"></a>Executar uma ativação pós-falha de teste

Antes de migrar uma ativação pós-falha de teste WEBVM, ajuda a tornar-se de que está tudo a funcionar conforme esperado. 

1. Eles executarem uma ativação pós-falha de teste, para o último ponto no tempo (**processado mais recentemente**).
2. Eles selecionam **encerrar a máquina antes de iniciar a ativação pós-falha**, para que o Site Recovery tenta encerrar a VM de origem antes de acionar a ativação pós-falha. A ativação pós-falha continua, mesmo que o encerramento falhe. 
3. Execuções de ativação pós-falha de teste: 

    - Uma verificação dos pré-requisitos é executado para certificar-se de que todas as condições necessárias para a migração estão em vigor.
    - A ativação pós-falha processa os dados, para que possa ser criada uma VM do Azure. Se for selecionado o último ponto de recuperação, é criado um ponto de recuperação a partir dos dados.
    - É criada uma VM do Azure com base nos dados processados no passo anterior.
3. Depois de concluída a ativação pós-falha, a réplica é apresentada VM do Azure no portal do Azure. Eles verificam se tudo o que está a funcionar corretamente. A VM tem um tamanho adequado, está ligado à rede certa e está em execução. 
4. Depois de verificar a ativação pós-falha de teste, a Contoso limpa a ativação pós-falha e os registos e guarda todas as observações. 

### <a name="migrate-the-vm"></a>Migrar a VM

1. Depois de verificar que a ativação pós-falha de teste funcionou como esperado, a Contoso cria um plano de recuperação para a migração. Eles adicionam WEBVM ao plano.

     ![Plano de recuperação](./media/contoso-migration-rehost-vm-sql-managed-instance/recovery-plan.png)

2. Em seguida, eles executarem uma ativação pós-falha no plano. Selecione o ponto de recuperação mais recente e especificar que o Site Recovery deve tentar encerrar a VM no local antes de acionar a ativação pós-falha.

    ![Ativação pós-falha](./media/contoso-migration-rehost-vm-sql-managed-instance/failover1.png)

3. Após a ativação pós-falha, Contoso, certifique-se de que a VM do Azure é apresentado conforme esperado no portal do Azure.

   ![Plano de recuperação](./media/contoso-migration-rehost-vm-sql-managed-instance/failover2.png)

4. Depois de verificar a VM no Azure, que concluir a migração para concluir o processo de migração, parar a replicação para a VM e parar a faturação do Site Recovery para a VM.

    ![Ativação pós-falha](./media/contoso-migration-rehost-vm-sql-managed-instance/failover3.png)

### <a name="update-the-connection-string"></a>Atualizar a cadeia de ligação

Como a etapa final do processo de migração, a Contoso atualiza a cadeia de ligação da aplicação para apontar para a base de dados migrada em execução no seu MI de SQL.

1. No portal do Azure, é possível localizar a cadeia de ligação ao clicar em **configurações** > **cadeias de ligação**.

    ![Ativação pós-falha](./media/contoso-migration-rehost-vm-sql-managed-instance/failover4.png)  

2. Eles atualizar a cadeia de caracteres com o nome de utilizador e palavra-passe da instância gerida SQL.
3. Após a configuração a cadeia de caracteres, estas atualizações vem substituir a cadeia de ligação atual no arquivo Web. config de seu aplicativo.
4. Depois de atualizar o arquivo e salvá-lo, de reiniciar o IIS no WEBVM. Eles fazem-lo **IISRESET /RESTART** num prompt de comando.
5. Após o reinício do IIS, a aplicação utilizará a base de dados em execução na instância gerida do SQL.
6. Neste momento Contoso pode encerrar a SQLVM máquina no local e a migração estiver concluída.

**Precisa de mais ajuda?**

- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) executar uma ativação pós-falha de teste. 
- [Saiba](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) como criar um plano de recuperação.
- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) efetuar a ativação pós-falha para o Azure.

## <a name="clean-up-after-migration"></a>Limpar após a migração

Com a migração é concluída, a aplicação de SmartHotel está em execução numa VM do Azure e a base de dados SmartHotel está disponível na instância gerida do Azure SQL.  

Agora, a Contoso precisa de fazer uma limpeza, da seguinte forma:  

- Remova a máquina WEBVM no inventário do vCenter.
- Remova a máquina SQLVM no inventário do vCenter.
- Remova WEBVM e SQLVM das tarefas de cópia de segurança locais.
- Documentação de atualização interna mostram a nova localização, o endereço IP para WEBVM.
- Remova SQLVM da documentação. Em alternativa, eles poderiam marcá-la para mostrar como eliminada e já não na VM de inventário.
- Reveja todos os recursos que interagem com as VMs desativadas e atualizar as definições relevantes ou documentação para refletir a nova configuração.

## <a name="review-the-deployment"></a>Reveja a implementação

Com os recursos migrados no Azure, a Contoso precisa totalmente operacionalizar e proteger sua infra-estrutura de novo.

### <a name="security"></a>Segurança

A equipe de segurança da Contoso analisa as VMs do Azure e a instância gerida de SQL, para determinar quaisquer problemas de segurança com a sua implementação.

- Eles analisam os grupos de segurança de rede (NSGs) para a VM, para controlar o acesso. NSGs são utilizados para garantir que apenas o tráfego permitido para a aplicação pode passar.
- Eles também Considerando proteger os dados no disco com o Azure Disk Encryption e o Cofre de chaves do Azure.
- Ativar a deteção de ameaças sobre o SQL gerida instância (SQLMI). Eles enviará alertas para seu sistema de suporte técnico de equipe/serviço de segurança para abrir um pedido, desde que seja detetada uma ameaça. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-threat-detection).

     ![Segurança de instância gerida](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-security.png)  

[Leia mais](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) sobre práticas de segurança para as VMs.

### <a name="backups"></a>Cópias de segurança
Contoso vai fazer backup dos dados no WEBVM com o serviço de cópia de segurança do Azure. [Saiba mais](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Otimização de licenciamento e de custo

1. Contoso tem licenças existentes do WEBVM e irá tirar partido do benefício híbrido do Azure.  Eles converterá a VM do Azure existentes para tirar partido destes preços.
2. Contoso irá ativar o Azure Cost Management licenciado pela Cloudyn, uma subsidiária da Microsoft. É uma solução de gestão de custos multiclouds que o ajuda a utilizar e gerir o Azure e outros recursos da cloud.  [Saiba mais](https://docs.microsoft.com/azure/cost-management/overview) sobre o Azure Cost Management. 


## <a name="conclusion"></a>Conclusão

Neste artigo, a Contoso rehosted a aplicação de SmartHotel no Azure ao migrar o VM da front-end de aplicação para o Azure com o serviço Site Recovery. Sua migração da base de dados no local para uma instância SQL do Azure geridos com o DMS.

## <a name="next-steps"></a>Passos Seguintes

Na [próximo artigo](contoso-migration-rehost-vm.md) da série, vamos mostrar como Contoso realojar a aplicação de SmartHotel em VMs do Azure com o serviço Azure Site Recovery.

