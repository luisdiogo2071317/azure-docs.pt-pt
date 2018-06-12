---
title: Uma aplicação Contoso no Azure de realojamento ao migrar para uma VM do Azure e geridos instância do SQL do Azure | Microsoft Docs
description: Saiba como Contoso realojar uma aplicação no local na VM do Azure e geridos instância do SQL do Azure
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: raynew
ms.openlocfilehash: c7dc9e8406494739aa5d8f21397a606e0b74a617
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301257"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms-and-azure-sql-managed-instance"></a>Migração de Contoso: realojamento uma aplicação no local para as VMs do Azure e geridos instância do SQL do Azure

Este artigo mostra-lhe como Contoso migra respetivo frontend de aplicação SmartHotel VM para VMs do Azure com o serviço do Azure Site Recovery e a base de dados de aplicação a uma instância de geridos de SQL do Azure.

> [!NOTE]
> A instância de geridos de SQL do Azure está atualmente em pré-visualização.

Este documento é o quarto numa série de artigos que documente a forma como a empresa fictícia com Contoso migra respetivos recursos no local para a nuvem do Microsoft Azure. A série inclui informações de fundo e uma série de cenários que mostram como configurar uma infraestrutura de migração e executar diferentes tipos de migrações. Cenários de crescimento em complexidade e será adicionado artigos adicionais ao longo do tempo.


**Artigo** | **Detalhes** | **Estado**
--- | --- | ---
[Artigo 1: Descrição geral](contoso-migration-overview.md) | Fornece uma descrição geral da estratégia de migração da Contoso, a série de artigo e as aplicações de exemplo, que vamos utilizar. | Disponível
[Artigo 2: Implementar uma infraestrutura do Azure](contoso-migration-infrastructure.md) | Descreve como Contoso prepara a infraestrutura do Azure e no local para a migração. A mesma infraestrutura é utilizada para todos os cenários de migração de Contoso. | Disponível
[Artigo 3: Avaliar a recursos no local](contoso-migration-assessment.md)  | Mostra como Contoso executa uma avaliação da respetiva aplicação de SmartHotel de duas camadas no local em execução no VMware. Avaliar a VMs de aplicação com o [Azure migrar](migrate-overview.md) serviço e a base de dados de SQL Server de aplicação com o [Assistente de migração de base de dados do Azure](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponível
Artigo 4: Realojamento para as VMs do Azure e uma instância do SQL geridos (Este artigo) | Demonstra como Contoso migra a aplicação de SmartHotel para o Azure. São migradas o front-end de aplicação VM utilizando [do Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)e a base de dados de aplicação utilizando o [migração de base de dados do Azure](https://docs.microsoft.com/azure/dms/dms-overview) serviço, para migrar para uma instância do SQL Server geridos. | Disponível
[Artigo 5: Realojamento para as VMs do Azure](contoso-migration-rehost-vm.md) | Mostra como Contoso migra a aplicação de SmartHotel VMs utilizando apenas a recuperação de sites.
[Artigo 6: Realojamento para as VMs do Azure e grupos de disponibilidade do SQL Server](contoso-migration-rehost-vm-sql-ag.md) | Mostra como Contoso migra a aplicação de SmartHotel. Podem utilizar a recuperação de sites para migrar a aplicação VMs e o serviço de base de dados de migração para migrar a base de dados de aplicação para um grupo de disponibilidade do SQL Server. | Disponível
[Artigo 7: Realojamento uma aplicação do Linux para as VMs do Azure](contoso-migration-rehost-linux-vm.md) | Demonstra como Contoso migra a aplicação do Linux osTicket para VMs do Azure com a recuperação de Site. | Disponível
[Artigo 8: Uma aplicação do Linux para as VMs do Azure e o servidor do Azure MySQL de realojamento](contoso-migration-rehost-linux-vm-mysql.md) | Demonstra como Contoso migra a aplicação do Linux osTicket para as VMs do Azure com a recuperação de Site e para uma instância de servidor de MySQL do Azure utilizando o MySQL Workbench. | Disponível

Se gostaria de utilizar a aplicação de SmartHotel exemplo utilizada neste artigo, poderá transferi-lo de [github](https://github.com/Microsoft/SmartHotel360).

## <a name="on-premises-architecture"></a>Arquitetura de no local

Eis um diagrama que mostra a infraestrutura no local Contoso atual.

![Arquitetura de contoso](./media/contoso-migration-rehost-vm-sql-managed-instance/contoso-architecture.png)  

- Contoso tem um centro de dados principal, localizado na cidade de Nova Iorque no Leste dos Estados Unidos.
- Têm três ramos locais adicionais nos Estados Unidos.
- O Centro de dados principal está ligado à internet com uma ligação de ethernet de metro de fibra (500 mbps).
- Cada ramo está ligado localmente à internet através de ligações de classe de negócio, com túneis IPSec VPN voltar ao centro de dados principal. Isto permite que os respetivos toda a rede estar permanentemente ligados e otimiza a conectividade à internet.
- O Centro de dados principal é totalmente virtualizado com o VMware. Têm dois anfitriões de Virtualização de ESXi 6.5, geridos pelo vCenter Server 6.5.
- A Contoso utiliza o Active Directory para gestão de identidades e servidores DNS na rede interna.
- Executam os controladores de domínio no Centro de dados em VMs de VMware. Os controladores de domínio locais ramos executados em servidores físicos.



## <a name="business-drivers"></a>Controladores de negócio

A equipa de TI liderança trabalhou coincida com os respetivos parceiros de negócios para compreender o que a empresa pretende alcançar com esta migração:

- **Crescimento de negócio de endereços**: Contoso está a crescer e assim existe pressão na infraestrutura e sistemas no local.
- **Aumentar a eficiência**: Contoso tem de remover procedimentos desnecessários e simplificar processos para os programadores e os utilizadores.  Necessidades IT para ser rápido e hora não waste ou dinheiro, deste modo, entrega mais rápida nos requisitos de cliente.
- **Aumentar a agilidade**: Contoso TI tem de ser mais responder às necessidades do negócio. Tem de ser capaz de reagir mais rapidamente do que as alterações no marketplace, para ativar o sucesso num economia global.  Não pode obter de forma ou tornar-se um bloqueador de janelas de negócio.
- **Escala**: como o crescimentos de negócio com êxito, Contoso TI tem de fornecer sistemas que são capazes de aumentam com o mesmo ritmo.

## <a name="migration-goals"></a>Objetivos de migração

A equipa de nuvem Contoso tem afixado baixo objetivos para esta migração. Estes objetivos são utilizados para determinar o melhor método de migração:

- Após a migração, a aplicação no Azure deve ter as mesmas capacidades de desempenho, como sucede no respetivo ambiente de VMWare no local.  Mover para a nuvem não significa que o desempenho da aplicação seja menos crítico.
- Contoso não quer investir nesta aplicação.  É críticos e importantes para o negócio, mas o respetivo formato atual simplesmente pretendem movê-lo tal como está para a nuvem.
- Devem ser minimizadas tarefas administrativas da base de dados após a aplicação é migrada.
- Contoso não a pretende utilizar uma base de dados do SQL do Azure para esta aplicação e está à procura de alternativas.

## <a name="proposed-architecture"></a>Arquitetura proposta

Neste cenário:

- A Contoso pretende migrar as respetivas aplicações de levar de duas camadas no local.
- A aplicação está em camadas através de duas VMs (WEBVM e SQLVM) e localizada num anfitrião do VMware ESXi **contosohost1.contoso.com** (versão 6.5)
- O ambiente VMware é gerido pelo vCenter Server 6.5 (**vcenter.contoso.com**), em execução numa VM.
- A base de dados de aplicação (SmartHotelDB) são migradas para uma instância do Azure SQL geridos.
- As VMs de VMware no local são migradas para uma VM do Azure.
- Contoso tem um datacenter no local (contoso-datacenter), com um controlador de domínio no local (**contosodc1**).
- As VMs no local no Centro de dados Contoso irão ser desativadas após a migração é efetuada.

![Arquitetura do cenário](media/contoso-migration-rehost-vm-sql-managed-instance/architecture.png) 

### <a name="azure-services"></a>Serviços do Azure

**Serviço** | **Descrição** | **Custo**
--- | --- | ---
[Serviço de gestão de base de dados](https://docs.microsoft.com/azure/dms/dms-overview) | O DMS permite migrações totalmente integradas de várias origens de base de dados para plataformas de dados do Azure, com o período de indisponibilidade mínimo. | Saiba mais sobre [regiões suportadas](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) para DMS e get [detalhes de preços](https://azure.microsoft.com/pricing/details/database-migration/).
[Instância de SQL gerida do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | Instância gerida é um serviço de base de dados gerida que representa uma instância de servidor de SQL completamente gerido na nuvem do Azure. -Partilha o mesmo código com a versão mais recente do motor de base de dados do SQL Server e tem as funcionalidades mais recentes, melhoramentos de desempenho e patches de segurança. | Utilizar base de dados geridos instâncias do SQL do Azure em execução no Azure implicar custos com base na capacidade. [Saiba mais](https://azure.microsoft.com/pricing/details/sql-database/managed/). 
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | O serviço orquestra e gere a migração e recuperação após desastre para as VMs do Azure e no local VMs e servidores físicos.  | Durante a replicação para o Azure, são cobradas taxas de armazenamento do Azure.  As VMs do Azure são criadas e implicar custos, quando ocorre a ativação pós-falha. [Saiba mais](https://azure.microsoft.com/pricing/details/site-recovery/) sobre os encargos e preços.

 

## <a name="migration-process"></a>processo de migração

Contoso irá migrar escalões web e de dados da aplicação SmartHotel para o Azure.

1. Contoso já tem a sua infraestrutura do Azure no local, pelo que apenas terá de adicionar alguns componentes do Azure específicos para este cenário.
2. A camada de dados será migrada utilizando o serviço de migração de dados (DMS).  O DMS estabelece ligação à VM do servidor de SQL no local através de uma ligação de VPN de site a site entre o Centro de dados da Contoso e o Azure e, em seguida, migra a base de dados.
3. A camada web será migrada utilizando uma migração de comparação de precisão e shift com o Azure Site Recovery. Isto irá entail preparar o ambiente de VMware no local, configurar e ativar a replicação e migrar as VMs por consegui-los através do Azure.

     ![Arquitetura de migração](media/contoso-migration-rehost-vm-sql-managed-instance/migration-architecture.png) 


## <a name="prerequisites"></a>Pré-requisitos

Eis o que Contoso (e) tem de para este cenário.

**Requisitos** | **Detalhes**
--- | ---
**Inscrever-se na pré-visualização** | Tem de estar inscritos no SQL Server geridos instância limitado pré-visualização pública. Necessita de uma subscrição do Azure para [inscrever](https://portal.azure.com#create/Microsoft.SQLManagedInstance). Inscrição pode demorar de alguns dias para concluir, por isso, certifique-se de que fazê-lo antes de começar a implementar este cenário.
**Subscrição do Azure** | Deve já tiver criado uma subscrição quando efetuar a avaliação do artigo primeiro esta série. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Se criar uma conta gratuita, será o administrador da sua subscrição e poderá executar todas as ações.<br/><br/> Se utilizar uma subscrição existente e não tiver o administrador, terá de trabalhar com o administrador para lhe atribuir permissões de proprietário ou contribuinte.<br/><br/> Se precisar de mais granulares permissões, consulte [neste artigo](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Recuperação de sites (no local)** | O servidor do vCenter no local deve estar a executar a versão 5.5, 6.0 ou 6.5<br/><br/> Um anfitrião ESXi a executar a versão 5.5, 6.0 ou 6.5<br/><br/> Uma ou mais VMs de VMware no anfitrião ESXi a executar.<br/><br/> As VMs têm de cumprir [requisitos do Azure](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Suportado [rede](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) e [armazenamento](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) configuração.
**DMS** | Para que o DMS tem um [compatível no dispositivo VPN local](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> Tem de ser capaz de configurar o dispositivo VPN no local. Tem de ter um endereço IPv4 público com acesso exterior, e o endereço não pode estar localizado atrás de um dispositivo NAT.<br/><br/> Certifique-se de que tem acesso à base de dados do SQL Server no local.<br/><br/> A Firewall do Windows deverá conseguir aceder ao motor de base de dados de origem. [Saiba mais](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Se existir uma firewall à frente do seu computador da base de dados, adicione regras para permitir o acesso à base de dados e aos ficheiros através de porta SMB 445.<br/><br/> As credenciais utilizadas para ligar à origem de SQL Server e instância geridos de destino têm de ser membros da função de servidor sysadmin.<br/><br/> Precisa de uma rede de partilha na base de dados no local que o DMS podem utilizar para criar cópias de segurança da base de dados de origem.<br/><br/> Certifique-se de que a conta de serviço que executa a instância do SQL Server de origem tem privilégios de escrita na partilha de rede.<br/><br/> Tome nota de um utilizador do Windows (e a palavra-passe) que tem o privilégio de controlo total na partilha de rede. O serviço de migração de base de dados do Azure representa tem estas credenciais de utilizador para carregar ficheiros de cópia de segurança para o contentor de armazenamento do Azure.<br/><br/> O processo de instalação do SQL Server Express define o protocolo de TCP/IP para **desativado** por predefinição. Certifique-se de que está ativada.


## <a name="scenario-steps"></a>Passos do cenário

Eis como Contoso vai configurar a implementação:

> [!div class="checklist"]
> * **Passo 1: Configurar uma instância do SQL do Azure gerida**: necessitam de uma instância de gerido previamente criada para o qual irá migrar a base de dados do SQL Server no local.
> * **Passo 2: Preparar DMS**: que precisam para registar o fornecedor de migração de base de dados, criar uma instância e, em seguida, criar um projeto DMS. Também têm de configurar o URI de SA para DMS. Uma assinatura de acesso partilhado (SAS) Uniform Resource Identifier (URI) concede acesso delegado a recursos na sua conta de armazenamento, para que pode conceder permissões limitadas para objetos de armazenamento. Se configuraram um URI de SAS para que o DMS pode aceder ao contentor de conta de armazenamento ao qual o serviço de carregamentos de ficheiros de cópia de segurança do SQL Server.
> * **Passo 3: Preparar do Azure Site Recovery**: para recuperação de sites, tem de criar uma conta de armazenamento do Azure para armazenar dados replicados e criar um cofre dos serviços de recuperação.
> * **Passo 4: Preparar VMware no local para a recuperação de Site**: Contoso irá preparar contas VM deteção e instalação do agente e preparar a ligação para VMs do Azure após a ativação pós-falha.
> * **Passo 5: Replicar VMs**: para configurar a replicação, configurar o ambiente de origem e destino de recuperação de sites, configurar uma política de replicação e iniciar a replicar VMs de armazenamento do Azure.
> * **Passo 6: Migrar a base de dados com o DMS**: agora, podem migrar a base de dados.
> * **Passo 7: Migrar as VMs com a recuperação de Site**: executar uma ativação pós-falha de teste para se certificar de que tudo está a funcionar e, em seguida, execute uma ativação pós-falha completa para migrar as VMs no Azure.


## <a name="step-1-prepare-an-azure-sql-managed-instance"></a>Passo 1: Preparar uma instância de gerido de SQL do Azure

Para configurar uma instância de geridos de SQL do Azure, a Contoso tem uma sub-rede:

- A sub-rede tem de estar dedicada. Tem de estar vazio e não contém qualquer outro serviço de nuvem e não pode ser uma sub-rede de gateway.
- Depois da instância gerida é criada, não pode adicionar recursos à mesma.
- A sub-rede não pode ter um NSG associado.
- A sub-rede tem de ter uma tabela de rota de utilizador (UDR) com Internet de salto seguinte 0.0.0.0/0 como a rota apenas atribuída. 
- DNS personalizado opcional: Se não for especificado DNS personalizado na VNet, endereço IP do Azure recursiva resoluções (por exemplo, 168.63.129.16) tem de ser adicionado à lista. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).
- A sub-rede não pode ter um ponto final de serviço (armazenamento ou SQL) associado ao mesmo. Pontos finais de serviço devem ser desativados na rede virtual.
- A sub-rede tem de ter pelo menos 16 endereços IP. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#determine-the-size-of-subnet-for-managed-instances) sobre o dimensionamento a sub-rede de instância gerido.
- No respetivo ambiente híbrido, são necessárias definições de DNS personalizadas. Contoso irá configurar as definições de DNS para utilizar um ou mais dos seus servidores de DNS do Azure. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns) sobre a personalização de DNS.

### <a name="set-up-a-virtual-network-for-the-managed-instance"></a>Configurar uma rede virtual para a instância gerida

Contoso configura a VNet da seguinte forma: 

1. Contoso cria uma nova VNet (VNET-SQLMI-EU2) na região primária EUA Leste 2, no grupo de recursos de ContosoNetworkingRG.
2. Contoso atribui um espaço de endereços de 10.235.0.0/24, garantindo que o intervalo não se sobrepõe a outras redes da empresa Contoso.
2. Se adicionarem duas sub-redes para a rede:
    - SQLMI-DS-EUS2 (10.235.0.0.25)
    - SQLMI-VIMOS-EUS2 (10.235.0.128/29). Esta sub-rede será utilizada para ligar o diretório para a instância gerida (SQLMI).

    ![Rede de instância geridos](media/contoso-migration-rehost-vm-sql-managed-instance/mi-vnet.png)

6. Depois da VNet e sub-redes são implementadas, Contoso elementos redes da seguinte forma:

    - Elementos de rede VNET-SQLMI-EUS2 com VNET-HUB-EUS2 (VNet hub para EUA Leste 2).
    - Elementos de rede VNET-SQLMI-EUS2 com VNET-PROD-EUS2 (a rede de produção).

    ![Peering de rede](media/contoso-migration-rehost-vm-sql-managed-instance/mi-peering.png)

7. Contoso define as definições de DNS personalizadas. DNS irá apontar primeiro para os seus controladores de domínio do Azure. O DNS do Azure será secundário. Os controladores de domínio do Azure de Contoso estão localizados da seguinte forma:

    - Localizado na sub-rede PROD-DC-EUS2, numa rede de produção 2 EUA Leste (VNET-PROD-EUS2).
    - Endereço de CONTOSODC3: 10.245.42.4
    - Endereço de CONTOSODC4: 10.245.42.5
    - Resolução DNS do Azure: 168.63.129.16

     ![Rede DNS](media/contoso-migration-rehost-vm-sql-managed-instance/mi-dns.png)

**Precisa de mais ajuda?**

- [Obter uma descrição geral](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) de geridos instâncias do SQL do Azure.
- [Saiba mais sobre](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#create-a-new-virtual-network-for-managed-instances) sobre como criar uma VNet para a instância do SQL geridos.
- [Saiba mais sobre](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering) configurar o peering.
- [Saiba mais sobre](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-dns) a atualizar as definições de DNS do Azure AD.



### <a name="set-up-routing"></a>Configurar o encaminhamento

A instância geridos é colocada numa VNET privada, pelo que a Contoso tem uma tabela de rota para o mesmo comunicar com o serviço de gestão do Azure. Se este não é possível comunicar com o serviço que o gere, torna-se inacessível.

- A tabela de rotas contém um conjunto de regras (rotas) que especifica como devem ser encaminhados os pacotes enviados a partir de instância geridos na VNet.
- A tabela de rotas é associada às sub-redes nas quais as instâncias geridas são implementadas. Cada pacote que sai de uma sub-rede é processado com base na tabela de rotas associada.
- Uma sub-rede só pode ser associada a uma tabela de rota única.
- Não existem sem encargos adicionais para criar as tabelas de rotas no Microsoft Azure.

1. Contoso cria uma tabela de rota definida pelo utilizador. A tabela de rotas é criada no grupo de recursos de ContosoNetworkingRG.

    ![Tabela de rotas](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table.png)

2. Para cumprir os requisitos de instância geridos, depois da tabela de rotas (MIRouteTable) é implementada, a Contoso adicione uma rota com um prefixo de endereço de 0.0.0.0/0, e **próximo salto tipo** definido como **Internet**.

    ![Prefixo da tabela de rota](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-prefix.png)
    
3. Contoso associar a tabela de rotas com a sub-rede SQLMI-DB-EUS2 (numa rede VNET-SQLMI-EUS2). 

    ![Sub-rede de tabela de rota](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-subnet.png)
    
**Precisa de mais ajuda?**

[Saiba mais sobre](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal#create-new-route-table-and-a-route) configurar rotas para uma instância gerida.

### <a name="create-a-managed-instance"></a>Criar uma instância gerida

Agora, Contoso pode aprovisionar uma instância do SQL da base de dados geridos.

1. Uma vez que a instância geridos serve uma aplicação de negócio, Contoso implementá-la na respetiva região EUA Leste 2 primário, no grupo de recursos ContosoRG, 
2. O utilizador selecionar um preço camada e tamanho armazenamento e computação para a instância. [Saiba mais](https://azure.microsoft.com/pricing/details/sql-database/managed/) sobre preços.

    ![Instância gerida](media/contoso-migration-rehost-vm-sql-managed-instance/mi-create.png)

3. Depois da instância de gerido for implementada, são apresentados dois novos recursos no grupo de recursos de ContosoRG:

    - Virtual no caso de ter múltiplas instâncias geridas, de cluster
    - O SQL Server gerido instância. 

    ![Instância gerida](media/contoso-migration-rehost-vm-sql-managed-instance/mi-resources.png)

**Precisa de mais ajuda?**

[Saiba mais sobre](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal) aprovisionamento de uma instância geridos.

## <a name="step-2-prepare-dms"></a>Passo 2: Preparar DMS

Para preparar o DMS Contoso tem de fazer duas coisas:

- Registar o fornecedor DMS no Azure
- Forneça o DMS com acesso ao armazenamento do Azure para carregar os ficheiros de cópia de segurança utilizados para migrar uma base de dados. Fazê-lo através da criação de um contentor de blob storage e gerar um acesso assinatura partilhado (SAS) URI para o mesmo. 
- Crie um projeto DMS.


Conclua os passos da seguinte forma:

1. Contoso registe o fornecedor de migração de base de dados na sua subscrição.
    ![O DMS registar](media/contoso-migration-rehost-vm-sql-managed-instance/dms-subscription.png)

2. Criar um contentor de blob de armazenamento e gerar um URI de SAS para que o DMS consegue aceder-lhe.

    ![URI DE SAS](media/contoso-migration-rehost-vm-sql-managed-instance/dms-sas.png)

3. Por fim, se criarem uma instância DMS. 

    ![Instância DMS](media/contoso-migration-rehost-vm-sql-managed-instance/dms-instance.png)

4. Contoso coloca a instância DMS na sub-rede da VNet a VNET-PROD DC EUS2 PROD-DC-EUS2.
    - Estes colocá-lo existe porque tem de ter uma VNet pode aceder a VM do servidor de SQL no local através de um gateway de VPN.
    - VNET-PROD-EUS2 está em modo de peering para VNET-HUB-EUS2 e é permitido utilizar gateways remotos.  Isto garante que o DMS pode comunicar conforme necessário.

        ![Rede DMS](media/contoso-migration-rehost-vm-sql-managed-instance/dms-network.png)

**Precisa de mais ajuda?**
- [Saiba mais sobre](https://docs.microsoft.com/azure/dms/quickstart-create-data-migration-service-portal) configurar DMS.
- [Saiba mais](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-2) sobre como criar e através da SAS.


## <a name="step-3-prepare-azure-for-the-site-recovery-service"></a>Passo 3: Preparar do Azure para o serviço de recuperação de sites

Existem um número de elementos do Azure, que tem de Contoso para configurar a recuperação de Site para a migração da respetiva camada web VM (WEBMV)

- A ativação pós-falha uma VNet na qual recursos estão localizados.
- Uma conta de armazenamento do Azure para armazenar dados replicados. 
- Um cofre dos serviços de recuperação no Azure.

Se configuraram a recuperação de sites da seguinte forma:

1. Uma vez que a VM web front-end para a aplicação de SmartHotel, estas irão falhar ao longo de VM para a respetiva rede de produção existente (VNET-PROD-EUS2) e a sub-rede (PROD-FE-EUS2), na região primária EUA Leste 2. Definir a esta rede cópias de segurança quando estes [implementada a infraestrutura do Azure](contoso-migration-infrastructure.md)
2. Se criarem uma conta de armazenamento do Azure (contosovmsacc20180528). Se estiver a utilizar uma conta de objetivo geral, com armazenamento padrão e replicação do LRS.

    ![Armazenamento de recuperação de site](media/contoso-migration-rehost-vm-sql-managed-instance/asr-storage.png)

3. Com a conta de armazenamento e de rede no local, Contoso pode agora criar um cofre (ContosoMigrationVault) e colocá-lo num grupo de recursos de ContosoFailoverRG, a região EUA Leste 2 primário.

    ![Cofre dos Serviços de Recuperação](media/contoso-migration-rehost-vm-sql-managed-instance/asr-vault.png)

**Precisa de mais ajuda?**

[Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) a configurar do Azure Site Recovery.


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>Passo 4: Preparar VMware no local para a recuperação de Site

Para preparar o VMware para a recuperação de sites, é aqui que Contoso precisa de fazer:

- Prepare uma conta no vCenter server ou vSphere ESXi anfitrião, ao automatizar a deteção VM.
- Prepare uma conta que permite a instalação automática do serviço de mobilidade em VMs de VMware que pretende replicar.
- Prepare VMs no local, para que possam ligar para VMs do Azure quando são criados após a ativação pós-falha.


### <a name="prepare-an-account-for-automatic-discovery"></a>Preparar uma conta para a deteção automática

O Site Recovery precisa de ter acesso a servidores VMware para:

- Detete automaticamente as VMs. É necessária, pelo menos, uma conta só de leitura.
- Orquestrar a replicação, a ativação pós-falha e a reativação pós-falha. Precisa de uma conta que pode executar operações, tais como criar e a remoção de discos e a ativar as VMs.

Contoso configura a conta da seguinte forma:

1. Cria uma função ao nível do vCenter.
2. Atribui as permissões necessárias para essa função.

**Precisa de mais ajuda?**

[Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) criar e atribuir uma função para a deteção automática.

### <a name="prepare-an-account-for-mobility-service-installation"></a>Preparar uma conta para a instalação do serviço de Mobilidade

O serviço de Mobilidade tem de ser instalado na VM que quer replicar.

- Recuperação de sites pode efetuar uma instalação push automática deste componente ao ativar a replicação para a VM.
- Para a instalação push automática, terá de preparar uma conta que a recuperação de sites irá utilizar para aceder a VM.
- Especificar esta conta ao configurar a replicação na consola do Azure.
- Precisa de um domínio ou conta local com permissões para instalar na VM.

**Precisa de mais ajuda**

[Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) criar uma conta da instalação push do serviço de mobilidade.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparar a ligação para VMs do Azure após a ativação pós-falha

Após a ativação pós-falha para o Azure, a Contoso pretende conseguir estabelecer ligação às VMs replicadas no Azure. Para tal, há alguns aspetos que precisam para na VM no local, antes de poderem executam a migração: 

1. Para acesso através da internet, ative o RDP na VM no local antes da ativação pós-falha e certifique-se de que as regras TCP e UDP estão adicionadas para o **pública** perfil e que o RDP é permitido na **Firewall do Windows**  >  **Aplicações permitidas** para todos os perfis.
2. Para ter acesso através da respetiva VPN site a site, ativar RDP na máquina no local e permitir que o RDP no **Firewall do Windows** -> **aplicações e funcionalidades permitidas** para **domínio e Privada** redes.
3. Configuram a política de SAN do sistema operativo na VM no local para **OnlineAll**.

Além disso, quando executam uma ativação pós-falha que precisam de verificar o seguinte:

- Não deverá haver nenhum atualizações pendentes do Windows na VM ao acionar uma ativação pós-falha. Se existirem, não poderá iniciar sessão para a máquina virtual enquanto a atualização for concluída.
- Após a ativação pós-falha, deve verificar **diagnóstico de arranque** para ver uma captura de ecrã da VM. Se isto não funcionar, deve verificar que a VM está em execução e reveja estes [sugestões de resolução de problemas](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


## <a name="step-5-replicate-the-on-premises-vms-to-azure-with-site-recovery"></a>Passo 5: Replicar as VMs no local para o Azure com a recuperação de Site

Antes de executar uma migração para o Azure, Contoso tem de configurar a replicação e ativar a replicação para a sua VM no local.

### <a name="set-a-replication-goal"></a>Definir um objetivo de replicação

1. No cofre, sob o nome do cofre (ContosoVMVault) possam definir um objetivo de replicação (**introdução** > **recuperação de Site** > **preparar a infraestrutura**.
2. Se especificarem que as suas máquinas estão localizados no local, que está VMs de VMware, e que pretende replicar para o Azure.

    ![Objetivo da replicação](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Confirme o planeamento da implementação

Para continuar, necessitam de confirmar que que tenham concluído planeamento da implementação, selecionando **Sim, ter efetuado esta**. Nesta implementação Contoso são migrar apenas uma única VM e não precisa de planeamento da implementação.

### <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem

Agora que precisam de configurar o respetivo ambiente de origem. Para efetuar este procedimento se transferir um modelo OVF e utilizá-lo para implementar o servidor de configuração e os respetivos componentes associados como altamente disponível, VM do VMware no local. Os componentes no servidor incluem:

- O servidor de configuração que coordena as comunicações entre no local e o Azure e gere a replicação de dados.
- O servidor de processos atua como um gateway de replicação. Recebe dados de replicação; otimiza-os com a colocação em cache, compressão e encriptação; e envia-os para o armazenamento do Azure.
- O servidor de processos também instala o Serviço de Mobilidade nas VMs que pretende replicar, e efetua a deteção automática de VMs VMware no local.
- Depois do servidor de configuração VM é criada e iniciada, Contoso pode registar no cofre.


Contoso execute estes passos da seguinte forma:

1. Transferem o modelo OVF do portal do Azure (**preparar infraestrutura** > **origem** > **servidor de configuração**).
    
    ![Transferir OVF](./media/contoso-migration-rehost-vm-sql-managed-instance/add-cs.png)

2. Se importar o modelo para VMware para criar e implementar a VM.

    ![Modelo OVF](./media/contoso-migration-rehost-vm-sql-managed-instance/vcenter-wizard.png)

3.  Quando for ativado a VM pela primeira vez, arrancarem para uma experiência de instalação do Windows Server 2016. Aceitar o contrato de licença e introduza uma palavra-passe de administrador.
4. Após a conclusão da instalação, iniciam sessão no VM como administrador. Na primeira vez início de sessão, a ferramenta de configuração de recuperação de sites do Azure é executado por predefinição.
5. Na ferramenta, se especificarem um nome a utilizar quando registar o servidor de configuração no cofre.
6. A ferramenta verifica se a VM pode ligar ao Azure. Depois da ligação for estabelecida, selecione **sessão**, para iniciar sessão para a subscrição do Azure. As credenciais tem de ter acesso ao Cofre no qual o servidor de configuração será registado. 

    [Registar o servidor de configuração](./media/contoso-migration-rehost-vm-sql-managed-instance/config-server-register2.png)

7. A ferramenta executa algumas tarefas de configuração e, em seguida, é reiniciada. Iniciar sessão máquina novamente e o Assistente de gestão do servidor de configuração é iniciado automaticamente.
8. No assistente, o utilizador selecionar o NIC para receber o tráfego de replicação. Não é possível alterar esta definição depois de estar configurada.
9. O utilizador selecionar a subscrição, o grupo de recursos e o Cofre registar o servidor de configuração.
        ![Cofre](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz1.png) 

10. Estes, em seguida, transferiram e instalar o servidor de MySQL e VMWare PowerCLI. Em seguida, estes validar as definições do servidor.
11. Após a validação, se especificarem o FQDN ou endereço IP do anfitrião do servidor ou vSphere do vCenter. Deixe a porta predefinida e especifique um nome amigável para o servidor vCenter no Azure.
12. Têm de especificar a conta criaram anteriormente, para que a recuperação de sites pode detetar automaticamente as VMs de VMware que estão disponíveis para replicação. 
13. Se especificarem as credenciais para instalar automaticamente o serviço de mobilidade quando a replicação está ativada. Para máquinas do Windows, a conta tem privilégios de administrador local nas VMs. 

    ![vCenter](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz2.png)

7. Após a conclusão de registo, no portal do Azure, a Contoso duplo verifica que o servidor de configuração e o servidor do VMware estão listadas no **origem** página no cofre. Deteção pode demorar 15 minutos ou mais. 
8. Recuperação de sites, em seguida, liga-se aos servidores do VMware com as definições especificadas e Deteta as VMs.

### <a name="set-up-the-target"></a>Configurar o destino

Agora Contoso tem de configurar o ambiente de replicação de destino.

1. No **preparar a infraestrutura** > **destino**, o utilizador de selecionar as definições de destino.
2. Recuperação de sites verifica que é uma conta de armazenamento do Azure e a rede de destino especificado.

### <a name="create-a-replication-policy"></a>Criar uma política de replicação

Depois da origem e destino estão configurados, Contoso está pronta para criar uma política de replicação e associe-o servidor de configuração.

1. No **preparar a infraestrutura** > **as definições de replicação** > **política de replicação** >  **criar e Associar**, se criarem uma política **ContosoMigrationPolicy**.
2. Se utilizarem as predefinições:
    - **Limiar RPO**: predefinição de 60 minutos. Este valor define com que frequência são criados pontos de recuperação. Será gerado um alerta se a replicação contínua exceder este limite.
    - **Retenção do ponto de recuperação**. Predefinição de 24 horas. Este valor Especifica o tempo da janela de retenção para cada ponto de recuperação. As VMs replicadas podem ser recuperadas para qualquer ponto numa janela.
    - **Frequência de instantâneos consistentes com aplicação**. Predefinição de uma hora. Este valor Especifica a frequência com que são criados os instantâneos consistentes com aplicações.
 
        ![Criar política de replicação](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy.png)

5. A política é associada automaticamente ao servidor de configuração. 

    ![Associar a política de replicação](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy2.png)


**Precisa de mais ajuda?**

- Pode ler instruções completa de todos estes passos no [configurar a recuperação após desastre para as VMs de VMware no local](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Instruções detalhadas estão disponíveis para o ajudar a [configurar o ambiente de origem](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementar o servidor de configuração](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server), e [configurar as definições de replicação](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).

### <a name="enable-replication"></a>Ativar a replicação

Agora Contoso pode iniciar o WebVM a replicar.

1. No **replicar aplicação** > **origem** > **+ replicar** , selecionarem as definições de origem.
2. Indicam que pretende ativar a máquinas virtuais, selecione o servidor vCenter e o servidor de configuração.

 ![Ativar a replicação](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication1.png)
 
3. Agora, se especificarem as definições de destino, incluindo o grupo de recursos e rede em que a VM do Azure estarão localizada após a ativação pós-falha e a conta de armazenamento no qual serão armazenados os dados replicados.

     ![Ativar a replicação](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication2.png)

4. Contoso seleciona WebVM para replicação. Recuperação de site instala o serviço de mobilidade em cada VM ao ativar a replicação para o mesmo. 

    ![Ativar a replicação](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication3.png)

5. Contoso verifica que a política de replicação correto está selecionada e ativa a replicação para WEBVM. Controlar o progresso de replicação no **tarefas**. Depois de a tarefa **Finalizar Proteção** ser executada, a máquina está preparada para ativação pós-falha.
6. No **Essentials** no portal do Azure, Contoso pode ver a estrutura para as VMs replicar para o Azure.

    ![Vista de infraestrutura](./media/contoso-migration-rehost-vm-sql-managed-instance/essentials.png)


**Precisa de mais ajuda?**

Pode ler instruções completa de todos estes passos no [ativar a replicação](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).

## <a name="step-6-migrate-the-database-with-dms"></a>Passo 6: Migrar a base de dados com o DMS

Contoso tem de criar um projeto DMS e migrar a base de dados.

### <a name="create-a-dms-project"></a>Criar um projeto DMS
1. Se criarem um projeto DMS. O tipo de servidor de origem, especificarem como o SQL Server e o destino como base de dados geridas por instância de SQL do Azure.

     ![Projeto DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-project.png)

2. Depois de criar o projeto, é aberto o Assistente de migração.

### <a name="migrate-the-database"></a>Migrar a base de dados 

1. No Assistente de migração, a Contoso Especifica a VM de origem em que a base de dados no local encontra-se e as credenciais para aceder ao mesmo.

    ![Origem DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-source.png)

2. O utilizador selecionar a base de dados para migrar (SmartHotel.Registration).

    ![Base de dados de origem DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-sourcedb.png)

3. Como um destino, estes especificarem o nome da instância geridas no Azure e as credenciais de acesso.

    ![Destino DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-target-details.png)

4. Em seguida, no **+ nova atividade** > **executar migração**, especificarem definições para executar a migração:
    - Credenciais de origem e de destino.
    - Base de dados para migrar.
    - A partilha de rede que criaram na VM no local. O DMS leva cópias de segurança de origem para esta partilha.
        - A conta de serviço com a origem de SQL Server instância tem de ter privilégios de escrita nesta partilha.
        - Especifique o caminho do FQDN para a partilha.
    - O URI de SAS que fornece o DMS com acesso ao contentor de conta de armazenamento para o qual o serviço carrega os ficheiros de cópia de segurança para a migração.

        ![Definições de DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-migration-settings.png)

5. Guardar a migração e executá-la.
6. No **descrição geral**, eles monitorizam o estado de migração.

    ![Monitor DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor1.png)

7. Após a conclusão da migração, certifique-se de que as bases de dados de destino existem na instância de gerido.

    ![Monitor DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor2.png)

## <a name="step-7-migrate-the-vm-with-site-recovery"></a>Passo 7: Migrar a VM com a recuperação de Site

Contoso é executada uma ativação pós-falha de teste rápida e, em seguida, migre a VM.

### <a name="run-a-test-failover"></a>Executar uma ativação pós-falha de teste

Antes de migrar ajuda de uma ativação pós-falha de teste WEBVM, certifique-se de que tudo está a funcionar conforme esperado. 

1. Se executam uma ativação pós-falha de teste, para o último ponto no tempo (**mais recentes processados**).
2. O utilizador selecionar **encerrar a máquina antes de iniciar a ativação pós-falha**, para que a recuperação de Site tenta encerre a VM de origem antes de acionar a ativação pós-falha. A ativação pós-falha continua, mesmo que o encerramento falhe. 
3. É executada a ativação pós-falha de teste: 

    - Verificação de um pré-requisitos é executado para se certificar de que todas as condições necessárias para a migração estão em vigor.
    - A ativação pós-falha processa os dados, para que possa ser criada uma VM do Azure. Se for selecionado o último ponto de recuperação, é criado um ponto de recuperação a partir dos dados.
    - É criada uma VM do Azure com base nos dados processados no passo anterior.
3. Após a ativação pós-falha, a réplica é apresentada VM do Azure no portal do Azure. Se verificar que tudo está a funcionar corretamente. A VM é o tamanho adequado, está ligado à rede à direita e estiver em execução. 
4. Depois de verificar a ativação pós-falha de teste, Contoso limpa a ativação pós-falha e os registos e guarda todas as observações. 

### <a name="migrate-the-vm"></a>Migrar a VM

1. Depois de verificar que a ativação pós-falha de teste trabalhado conforme esperado, Contoso cria um plano de recuperação para a migração. Se adicionarem WEBVM para o plano.

     ![Plano de recuperação](./media/contoso-migration-rehost-vm-sql-managed-instance/recovery-plan.png)

2. Em seguida, executam uma ativação pós-falha do plano de. Selecione o ponto de recuperação mais recente e especifique que a recuperação de sites deve tentar encerre a VM no local antes de acionar a ativação pós-falha.

    ![Ativação pós-falha](./media/contoso-migration-rehost-vm-sql-managed-instance/failover1.png)

3. Após a ativação pós-falha, Contoso, certifique-se de que a VM do Azure é apresentado como esperado no portal do Azure.

   ![Plano de recuperação](./media/contoso-migration-rehost-vm-sql-managed-instance/failover2.png)

4. Depois de verificar a VM no Azure, concluírem a migração para concluir o processo de migração, pare a replicação para a VM e parar a faturação da recuperação de Site para a VM.

    ![Ativação pós-falha](./media/contoso-migration-rehost-vm-sql-managed-instance/failover3.png)

### <a name="update-the-connection-string"></a>Atualizar a cadeia de ligação

Como o último passo no processo de migração, a Contoso atualiza a cadeia de ligação da aplicação para apontar para a base de dados migrado em execução no respetivo MI do SQL Server.

1. No portal do Azure, é possível localizar a cadeia de ligação ao clicar em **definições** > **cadeias de ligação**.

    ![Ativação pós-falha](./media/contoso-migration-rehost-vm-sql-managed-instance/failover4.png)  

2. Se atualizar a cadeia com o nome de utilizador e palavra-passe da instância do SQL Server geridos.
3. Depois da cadeia é configurada, estas atualizações vem substituir a cadeia de ligação atual no ficheiro Web. config da respetiva aplicação.
4. Depois de atualizar o ficheiro e guardá-lo, de reiniciar o IIS no WEBVM. Tal com **IISRESET /RESTART** de uma linha de comandos.
5. Após o reinício do IIS, a aplicação utilizará a base de dados em execução na instância do SQL Server geridos.
6. Neste momento Contoso pode encerrar o SQLVM máquina no local e a migração estar concluída.

**Precisa de mais ajuda?**

- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) executar uma ativação pós-falha de teste. 
- [Saiba](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) como criar um plano de recuperação.
- [Saiba mais sobre](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) efetuar a ativação pós-falha para o Azure.

## <a name="clean-up-after-migration"></a>Limpar após a migração

Com a migração é concluída, a aplicação de SmartHotel está em execução numa VM do Azure e a base de dados SmartHotel está disponível no Azure geridos a instância do SQL.  

Agora, Contoso precisa de fazer alguns limpeza, da seguinte forma:  

- Remova a máquina WEBVM do inventário do vCenter.
- Remova a máquina SQLVM do inventário do vCenter.
- Remova WEBVM e SQLVM das tarefas de cópia de segurança locais.
- Atualização de documentação interna mostram a nova localização, endereço IP para WEBVM.
- Remova SQLVM na documentação. Em alternativa, se foi marcá-la para mostrar como eliminada e já não na VM de inventário.
- Reveja quaisquer recursos que interagem com as VMs desativadas e atualizar as definições relevantes ou documentação para refletir a nova configuração.

## <a name="review-the-deployment"></a>Reveja a implementação

Com os recursos migrados no Azure, Contoso tem totalmente operacionalizar e proteger a sua infraestrutura de novo.

### <a name="security"></a>Segurança

A equipa de segurança de Contoso revê as VMs do Azure e a instância do SQL geridos, para determinar a quaisquer problemas de segurança com a sua implementação.

- Reveja estes grupos de segurança de rede (NSGs) para a VM, para controlar o acesso. Os NSGs são utilizados para garantir que apenas o tráfego permitido para a aplicação pode passar.
- São também considerar a proteger os dados no disco com o Azure Disk Encryption e KeyVault do Azure.
- Ativar a deteção de ameaças no SQL Server geridos instância (SQLMI). Serão enviados alertas para o sistema de suporte técnico de equipa/serviço de segurança para abrir um pedido, desde que seja detetada uma ameaça. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-threat-detection).

     ![Segurança de instância gerido](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-security.png)  

[Leia mais](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) sobre práticas de segurança para as VMs.

### <a name="backups"></a>Cópias de segurança
Contoso vai fazer cópias de segurança de dados no WEBVM utilizando o serviço de cópia de segurança do Azure. [Saiba mais](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Otimização de licenciamento e custo

1. Contoso tem licenciamento existente para WEBVM e tirar partido do benefício de híbrida do Azure.  Estes irão converter VM do Azure existente para tirar partido deste preços.
2. Contoso irá ativar a gestão de custo de Azure licenciados pelos Cloudyn, uma subsidiária Microsoft. É uma solução de gestão de custo de nuvem multi que ajuda a utilizar e gerir o Azure e outros recursos de nuvem.  [Saiba mais](https://docs.microsoft.com/azure/cost-management/overview) sobre a gestão de custo do Azure. 


## <a name="conclusion"></a>Conclusão

Neste artigo, Contoso rehosted a aplicação de SmartHotel no Azure ao migrar o front-end de aplicação VM no Azure utilizando o serviço de recuperação de sites. A base de dados no local são migrados a um Azure SQL geridos instância utilizando DMS.

## <a name="next-steps"></a>Passos Seguintes

No seguinte artigo da série, vamos mostrar como Contoso realojamento a aplicação de SmartHotel para VMs do Azure utilizando apenas o serviço do Azure Site Recovery.

