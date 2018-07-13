---
title: Avaliar cargas de trabalho no local para a migração da Contoso para o Azure | Documentos da Microsoft
description: Saiba como Contoso avalia a suas máquinas no local para migração para o Azure com o Azure Migration e migração de base de dados
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: fa6fb4ffe1eea98392b2199f379431b0dffc6774
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006571"
---
# <a name="contoso-migration-assess-on-premises-workloads-for-migration-to-azure"></a>Migração de Contoso: avaliar cargas de trabalho no local para migração para o Azure

Este artigo mostra-lhe como Contoso avalia seu aplicativo de SmartHotel no local, em preparação para a sua migração para o Azure.

Este documento é a terceira série de artigos que documentam como a empresa fictícia Contoso migra os respetivos recursos no local para a cloud do Microsoft Azure. A série contém informações gerais e uma série de cenários de implementação que mostram como configurar uma infraestrutura de migração, avaliar a adequação de recursos no local para migração e executar diferentes tipos de migrações. Cenários de crescem em complexidade e estamos a adicionar artigos adicionais ao longo do tempo.

**Artigo** | **Detalhes** | **Estado**
--- | --- | ---
[Artigo 1: Descrição geral](contoso-migration-overview.md) | Fornece uma descrição geral da estratégia de migração da Contoso, a série de artigos e as aplicações de exemplo que usamos. | Disponível
[Artigo 2: Implementar uma infraestrutura do Azure](contoso-migration-infrastructure.md) | Descreve como o Contoso prepara a sua infraestrutura do Azure e no local, para a migração. A mesma infra-estrutura é utilizada para todos os artigos de migração. | Disponível
Artigo 3: Avaliar a recursos no local para migração para o Azure  | Mostra como Contoso é executado uma avaliação de uma aplicação de SmartHotel de duas camadas no local em execução no VMware. Contoso avalia as VMs de aplicação com o [do Azure Migrate](migrate-overview.md) serviço e a base de dados do SQL Server do aplicativo com o [Assistente de migração de base de dados](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Este artigo.
[Artigo 4: Realojar a uma aplicação em VMs do Azure e uma instância gerida de SQL](contoso-migration-rehost-vm-sql-managed-instance.md) | Demonstra como Contoso é executado uma migração lift-and-shift para o Azure para a aplicação de SmartHotel no local. Contoso migra o front-end de aplicação VM através de [do Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)e a base de dados de aplicação para a instância gerida de SQL, utilizando o [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Disponível
[Artigo 5: Realojar a uma aplicação em VMs do Azure](contoso-migration-rehost-vm.md) | Mostra como o Contoso migrar as VMs da aplicação SmartHotel para VMs do Azure, com o serviço Site Recovery. | Disponível
[Artigo 6: Realojar a uma aplicação em VMs do Azure e SQL Server sempre no grupo de disponibilidade](contoso-migration-rehost-vm-sql-ag.md) | Mostra como a Contoso migra a aplicação de SmartHotel. A Contoso utiliza o Site Recovery para migrar as VMs da aplicação e o serviço de migração de base de dados para migrar a base de dados de aplicação para um cluster do SQL Server protegido por um grupo de Disponibilidade AlwaysOn. | Disponível
[Artigo 7: Realojar uma aplicação do Linux em VMs do Azure](contoso-migration-rehost-linux-vm.md) | Mostra como Contoso faz uma migração lift-and-shift da aplicação de osTicket do Linux para VMs do Azure, com o Site Recovery | Disponível
[Artigo 8: Realojar uma aplicação do Linux em VMs do Azure e MySQL do Azure](contoso-migration-rehost-linux-vm-mysql.md) | Demonstra como a Contoso migra a aplicação de osTicket do Linux para as VMs do Azure com o Site Recovery e migra a base de dados de aplicação para uma instância do servidor MySQL do Azure com o MySQL Workbench. | Disponível
[Artigo 9: Refatorizar uma aplicação na base de dados de aplicações Web do Azure e SQL do Azure](contoso-migration-refactor-web-app-sql.md) | Demonstra como Contoso migra a aplicação de SmartHotel para uma aplicação Web do Azure e migra a base de dados de aplicação para a instância de servidor SQL do Azure | Disponível
[Artigo 10: Refatorizar uma aplicação do Linux em aplicações Web do Azure e MySQL do Azure](contoso-migration-refactor-linux-app-service-mysql.md) | Mostra como a Contoso migra a aplicação de osTicket do Linux para aplicações Web do Azure em vários sites, integrados com o GitHub para a entrega contínua. Estes migrar a base de dados de aplicação para uma instância do MySQL do Azure. | Disponível
[Artigo 11: Refatorar o TFS no VSTS](contoso-migration-tfs-vsts.md) | Mostra como a Contoso migra a implantação do Team Foundation Server (TFS) no local ao migrá-lo para Visual Studio Team Services (VSTS) no Azure. | Disponível
[Artigo 12: Rearquitetar uma aplicação em contentores do Azure e base de dados do Azure SQL](contoso-migration-rearchitect-container-sql.md) | Mostra como Contoso migra e rearchitects a respetiva aplicação SmartHotel para o Azure. Eles rearquitetar a camada de web de aplicação como um contentor do Windows e a base de dados de aplicação numa base de dados SQL do Azure. | Disponível
[Artigo 13: Recriar uma aplicação no Azure](contoso-migration-rebuild.md) | Mostra como Contoso reconstruir a sua aplicação de SmartHotel com uma gama de capacidades do Azure e serviços, incluindo serviços de aplicações, Kubernetes do Azure, as funções do Azure, serviços cognitivos e Cosmos DB. | Disponível


## <a name="overview"></a>Descrição geral

Ao considerar a migração para o Azure, a empresa Contoso quer realizar uma avaliação técnica e financeira para saber se as suas cargas de trabalho no local são adequadas para migração para a cloud. Em particular, a equipe de Contoso pretende avaliar a compatibilidade de máquina e da base de dados para a migração e estimar a capacidade e os custos para executar os respetivos recursos no Azure.

Para se habituar ao e compreende melhor as tecnologias envolvidas, vai avaliar duas das suas aplicações no local, resumidos na tabela seguinte. Tenha em atenção que se estiver a avaliar para cenários de migração que aplicações de realojamento e o refactor para migração. Saiba mais sobre o realojamento e refatoração no [descrição geral da migração da Contoso](contoso-migration-overview.md).

**Nome da aplicação**  | **Plataforma** | **Escalões de aplicação** | **Detalhes**
--- | --- | --- | ---
SmartHotel<br/><br/> Gere os requisitos de deslocação de Contoso | Em execução no Windows com uma base de dados do SQL Server | Aplicação em duas camadas com o Web site ASP.NET de front-end em execução numa VM (WEBVM) e o SQL Server em execução em outra VM (SQLVM) | As VMs são VMware, em execução num anfitrião ESXi gerido pelo vCenter server.<br/><br/> A aplicação de exemplo pode ser transferida a partir [GitHub](https://github.com/Microsoft/SmartHotel360).
OSTicket<br/><br/> Aplicativo de suporte técnico do serviço de contoso | Em execução no Linux/Apache, com um PHP MySQL (LAMP). | Aplicação em duas camadas com um Web site PHP de front-end numa VM (OSTICKETWEB) e a base de dados MySQL em execução em outra VM (OSTICKETMYSQL) | A aplicação é utilizada pelo cliente do serviço de aplicações para controlar problemas para os funcionários internos e clientes externos.<br/><br/> A aplicação de exemplo pode ser transferida a partir [GitHub](https://github.com/osTicket/osTicket).

## <a name="current-architecture"></a>Arquitetura atual


Aqui está um diagrama que mostra a infra-estrutura de locais de Contoso atual.

![Arquitetura de contoso](./media/contoso-migration-assessment/contoso-architecture.png)  

- A Contoso tiver um Data Center principal localizado na cidade de Nova Iorque no Leste dos Estados Unidos.
- Eles têm três ramos locais adicionais nos Estados Unidos.
- O datacenter principal está ligado à internet com uma ligação de ethernet metro fiber (500 mbps).
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

## <a name="assessment-goals"></a>Objetivos de avaliação

A equipe de cloud Contoso tiver afixado para baixo de objetivos para suas avaliações de migração:

- Após a migração, e aplicações no Azure devem ter as mesmas capacidades de desempenho, como sucede no respetivo ambiente de VMWare no local.  Mover para a nuvem não significa que o desempenho de aplicações é menos crítico.
- A Contoso precisa de compreender a compatibilidade de seus aplicativos e bancos de dados com os requisitos do Azure, bem como as suas opções de alojamento no Azure.
- Administração de banco de dados da Contoso deve ser minimizada depois de aplicações foram movidos para a cloud.  
- Contoso quer compreender não apenas as opções de migração, mas também os custos associados com a infraestrutura depois que for movido para a cloud.

## <a name="assessment-tools"></a>Ferramentas de avaliação
Contoso está a utilizar as ferramentas da Microsoft para a avaliação. Essas ferramentas aos seus objetivos e devem fornecê-los com todas as informações que precisam.

**Tecnologia** | **Descrição** | **Custo**
--- | --- | ---
[Assistente de migração de base de dados (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Eles usarão o DMA para avaliar e detetar problemas de compatibilidade que podem afetar sua funcionalidade de base de dados no Azure. O DMA avalia a paridade de funcionalidades entre o SQL origens e destinos e recomenda melhorias de desempenho e confiabilidade. | É uma ferramenta transferível gratuita.
[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-overview) | Contoso irá utilizar este serviço para avaliar as respetivas VMs de VMware. Avalia a adequabilidade da migração das máquinas e disponibiliza estimativas de dimensionamento e custos para a execução no Azure.  | Não existe atualmente (talvez de 2018) sem custos para utilizar este serviço.
[Mapa do Serviço](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) | O Azure Migrate utiliza o Mapa de Serviços para mostrar as dependências entre as máquinas que quer migrar. |  O Mapa de Serviços faz parte do Azure Log Analytics. Atualmente, pode ser utilizado durante 180 dias sem incorrer em custos.

Neste cenário, o Contoso transfere e executa o DMA para avaliar a base de dados do SQL Server no local para a respetiva aplicação de viagens. Que utilizar o Azure migrate com o mapeamento da dependência para avaliar as VMs da aplicação antes da migração para o Azure.



## <a name="assessment-architecture"></a>Arquitetura da avaliação


![A arquitetura da avaliação da migração](./media/contoso-migration-assessment/migration-assessment-architecture.png)

- Contoso é um nome fictício que representa uma organização empresarial típica.
- A Contoso tiver um datacenter no local (**contoso-datacenter**), com controladores de domínio no local (CONTOSODC1, CONTOSODC2).
- VMware VMs estão localizadas num VMware ESXI anfitriões que executam a versão 6.5. Anfitriões: **contosohost1**, **contosohost2**
- O ambiente do VMware é gerido pelo vCenter server 6.5 (**venter**, em execução numa VM.
- Aplicação de viagens SmartHotel:
    - A aplicação está em camadas em duas VMs de VMware, **WEBVM** e **SQLVM**.
    - As VMs estão localizadas no anfitrião ESXi do VMware **contosohost1.contoso.com**.
    - As VMs estão em execução o Centro de dados do Windows Server 2008 R2 com SP1.
- O ambiente do VMware é gerido pelo vCenter Server (**vcenter.contoso.com**) em execução numa VM.
- O OSTicket aplicativo de suporte técnico do serviço:
    - A aplicação está em camadas em duas VMs, **OSTICKETWEB** e **OSTICKETMYSQL**.
    - As VMs em execução no servidor do Linux Ubuntu 16.04-LTS.
    - A VM OSTICKETWEB está em execução Apache 2 e o PHP 7.0.
    - A VM OSTICKETMYSQL está em execução MySQL 5.7.22.

![Arquitetura](./media/contoso-migration-assessment/architecture.png)


## <a name="prerequisites"></a>Pré-requisitos

Eis o que Contoso (e) precisa para a avaliação:

- Acesso de proprietário ou contribuinte para a subscrição do Azure ou para um grupo de recursos na subscrição do Azure.
- Um vCenter Server no local a executar a versão 5.5, 6.0 ou 6.5.
- Uma conta só de leitura no vCenter Server ou permissões para criar uma.
- Permissões para criar uma VM no vCenter Server com um modelo .OVA.
- Pelo menos, um anfitrião ESXi a executar a versão 5.0 ou posterior.
- Pelo menos, duas VMs do VMware no local, uma a executar uma base de dados do SQL Server.
- Permissões para instalar agentes do Azure Migrate em cada VM.
- As VMs devem ter conectividade à Internet direta.
        - Pode restringir o acesso à Internet aos [URLs necessários](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-pre-requisites).
        -If máquinas sem conectividade de internet, o [gateway OMS](../log-analytics/log-analytics-oms-gateway.md) tem de ser instalado nos mesmos.
- Do FQDN da VM a executar a instância do SQL Server, para avaliação da base de dados.
- A Firewall do Windows em execução na VM do SQL Server deve permitir ligações externas na porta TCP 1433 (predefinição), para que seja possível ligar o DMA.


## <a name="assessment-overview"></a>Descrição geral da avaliação

Aqui está como Contoso vai fazer a avaliação:


> [!div class="checklist"]
> * **Passo 1: Transferir e instalar o DMA**: Prepare o DMA para avaliação da base de dados do SQL Server no local.
> * **Passo 2: Avaliar a base de dados com o DMA**: executar e analisar a avaliação da base de dados.
> * **Passo 3: Preparar a avaliação da VM com o Azure Migrate**: Configurar contas no local e o ajuste de VMware definições.
> * **Passo 4: Detetar VMs no local com o Azure Migrate**: criar uma VM do recoletor do Azure Migrate. Em seguida, eles são executados o recoletor para detetar VMs para avaliação.
> * **Passo 5: Preparar para a análise de dependência com o Azure Migrate**: instalar o Azure Migrate agentes nas VMs, para que eles podem ver o mapeamento das dependências entre as VMs.
> * **Passo 6: Avaliar as VMs com o Azure Migrate**: Verifique as dependências, agrupe as VMs e execute a avaliação. Depois da avaliação estiver pronta, eles analisá-los em preparação para a migração.


## <a name="step-1-download-and-install-the-dma"></a>Passo 1: Transferir e instalar o DMA

1. Contoso transfere o DMA no [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595).
    - O assistente pode ser instalado em qualquer máquina que pode ligar à instância do SQL. Não tem de executá-lo na máquina do SQL Server.
    - Ele não deve ser executado na máquina de anfitrião do SQL Server.
2. Eles são executados o ficheiro de configuração transferido (Downloadmigrationassistant), para iniciar a instalação.
3. Sobre o **concluir** página, selecione **iniciar Microsoft Data Migration Assistant** antes de concluir o assistente.

## <a name="step-2-run-and-analyze-the-database-assessment-for-smarthotel"></a>Passo 2: Executar e analisar a avaliação da base de dados para SmartHotel

Agora a Contoso pode executar uma avaliação para analisar o respetivo servidor de SQL no local para a aplicação de SmartHotel.

1. No Assistente de migração de base de dados, clique **New**, selecione **avaliação**e dê a avaliação de um nome de projeto - **SmartHotel**.
2. Eles selecionam a **tipo de servidor de origem** como **SQL Server em Azure Virtual Machines**.

    ![Selecionar origem](./media/contoso-migration-assessment/dma-assessment-1.png)

    > [!NOTE]
      Neste momento, o DMA não suporta a avaliação para a migração para uma Instância Gerida do SQL. Como solução, Contoso utiliza o SQL Server numa VM do Azure como o suposto destino para a avaliação.

3. Na **versão de destino selecione**, eles selecionam o SQL Server 2017 como a versão de destino. Terá de selecionar esta opção, porque é a versão utilizada pela instância gerida de SQL.
4. Eles selecionam para detetar informações sobre a compatibilidade e as novas funcionalidades:
    - **Problemas de compatibilidade** tenha em atenção as alterações que podem interromper a migração ou que exigem um pequeno Ajuste antes da migração. Ele mantém-lo informado sobre quaisquer funcionalidades atualmente em utilização que foram preteridas. Os problemas estão organizados por nível de compatibilidade.
    - **Recomendação de novas funcionalidades** notas de novos recursos na plataforma do SQL Server de destino que podem ser utilizados para a base de dados após a migração. As funcionalidades estão organizadas por Desempenho, Segurança e Armazenamento.

    ![Selecionar o destino](./media/contoso-migration-assessment/dma-assessment-2.png)

2. Na **ligar a um servidor**, introduza o nome da VM a executar a base de dados e as credenciais para aceder ao mesmo. Tem de ativar **certificado de servidor fidedigno** para se certificar de que eles podem chegar a SQL Server. Em seguida, clique em **Connect**.

    ![Selecionar o destino](./media/contoso-migration-assessment/dma-assessment-3.png)

3. Na **Adicionar origem**, adicionam a base de dados que pretendem avaliar e clique em **próxima** para iniciar a avaliação.
4. A avaliação é criada.

    ![Criar avaliação](./media/contoso-migration-assessment/dma-assessment-4.png)

5. Na **resultados de revisão**, eles podem ver os resultados da avaliação.


### <a name="analyze-the-database-assessment"></a>Analisar a avaliação da base de dados

Os resultados são apresentados quando eles estão disponíveis. Se eles corrijam problemas que necessitam de clicar em **reiniciar avaliação** voltar a executar a avaliação.

1. Na **problemas de compatibilidade** comunicar, eles verificam para quaisquer problemas em cada nível de compatibilidade. Os níveis de compatibilidade são mapeados para as versões do SQL Server da seguinte forma:

    - 100: SQL Server 2008/Base de Dados Azure SQL
    - 110: SQL Server 2012/Base de Dados Azure SQL
    - 120: SQL Server 2014/Base de Dados Azure SQL
    - 130: SQL Server 2016/Base de Dados Azure SQL
    - 140: SQL Server 2017/Base de Dados Azure SQL

    ![Problemas de compatibilidade](./media/contoso-migration-assessment/dma-assessment-5.png)

2. Na **recomendações de funcionalidades** comunicar, a Contoso pode ver recursos de desempenho, segurança e armazenamento que a avaliação lhe recomenda após a migração. São recomendadas uma variedade de funcionalidades, incluindo OLTP dentro da memória e Columnstore, Stretch Database, Always Encrypted, máscara de dados dinâmicos e encriptação de dados transparente (TDE).

    ![Recomendações de funcionalidades](./media/contoso-migration-assessment/dma-assessment-6.png)

    > [!NOTE]
    > Recomendamos que Contoso [permite a TDE](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) para todos os SQL Server bases de dados e isso é ainda mais crítico quando as bases de dados estão na cloud. TDE só deve ser ativada após a migração. Se a TDE já estiver ativada, terá de mover o certificado ou chave assimétrica para a base de dados mestra do servidor de destino. [Saiba mais](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).

2. Eles podem exportar a avaliação no formato JSON ou CSV.

Tenha em atenção que se estiver a executar uma avaliação numa escala maior, pode:

- Executar várias avaliações em simultâneo e ver o estado das mesmas na abrindo o **todas as avaliações** página.
- [Consolidar avaliações numa base de dados do SQL Server](https://docs.microsoft.com/sql/dma/dma-consolidatereports?view=ssdt-18vs2017#import-assessment-results-into-a-sql-server-database).
- [Consolidar avaliações num relatório do PowerBI](https://docs.microsoft.com/sql/dma/dma-powerbiassesreport?view=ssdt-18vs2017).


## <a name="step-3-prepare-for-vm-assessment-with-azure-migrate"></a>Passo 3: Preparar a avaliação da VM com o Azure Migrate

Contoso precisa para criar uma conta do Mvware que o Azure Migrate vai utilizar automaticamente detetar VMs para avaliação, verifique as permissões para criar uma VM, tenha em atenção as portas que têm de ser aberta e definir as estatísticas de nível de definições.

### <a name="set-up-a-vmware-account"></a>Configurar uma conta do VMware

 Deteção de VMS exige uma conta só de leitura no vCenter, com as seguintes propriedades:

- Tipo de utilizador: pelo menos um utilizador só de leitura.
- Permissões: objeto Data Center –> Propagar ao Objeto Subordinado, função=Só de Leitura.
- Detalhes: utilizador atribuído ao nível do datacenter, com acesso a todos os objetos no datacenter.
- Para restringir o acesso, atribua a função **Sem acesso** com **Propagar ao objeto subordinado** aos objetos subordinados (anfitriões vSphere, arquivos de dados, VMs e redes).

### <a name="verify-permissions-to-create-a-vm"></a>Verificar as permissões para criar VMs

Verifica se a Contoso, tem permissões para criar uma VM ao importar um ficheiro no. Formato de OVA. [Saiba mais](https://kb.vmware.com/s/article/1023189?other.KM_Utility.getArticleLanguage=1&r=2&other.KM_Utility.getArticleData=1&other.KM_Utility.getArticle=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1&other.KM_Utility.getGUser=1).

### <a name="verify-ports"></a>Verificar as portas

A avaliação de Contoso utiliza o mapeamento da dependência. Esta funcionalidade requer um agente instalado nas VMs que pretende avaliar. O agente tem de conseguir ligar ao Azure a partir da porta TCP 443 em cada VM. [Saiba mais](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid) sobre os requisitos da ligação.


### <a name="set-statistics-settings"></a>Configurar as definições das estatísticas

Antes de começar a implementação, Contoso tem de definir as definições de estatísticas para o vCenter Server para o nível 3. Tenha em atenção que:

- Depois de definir o nível, terá de aguardar pelo menos um dia antes de executar a avaliação. Caso contrário, aquela poderá não funcionar conforme esperado.
- Se o nível for superior a 3, a avaliação funcionará, mas:
    - Não serão recolhidos dados de desempenho dos discos e da rede.
    - Quanto ao armazenamento, o Azure Migrate recomenda um disco padrão no Azure, com o mesmo tamanho que o disco no local.
    - Quanto à rede, para cada placa de rede no local, será recomendada uma placa de rede no Azure.
    - Quanto à computação, o Azure Migrate observará os núcleos e o tamanho da memória das VM e recomendará uma VM do Azure com a mesma configuração. Se existirem vários tamanhos de VMs do Azure elegíveis, é recomendado aquele que tiver o custo mais baixo.
- [Saiba mais](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing) sobre o dimensionamento com o nível 3.

Se definir o nível da seguinte forma:

1. No vSphere Web Client, que abrem a instância de servidor do vCenter.
2. Na **Manage** > **definições** > **geral**, clicarem **editar**.
3. Na **estatísticas**, eles definido a estatística definições de nível de como **nível 3**.

    ![Nível de estatísticas do vCenter](./media/contoso-migration-assessment/vcenter-statistics-level.png)



## <a name="step-4-discover-vms"></a>Passo 4: Detetar VMs

Para detetar VMs, a Contoso cria um projeto do Azure Migrate. Transferir e configurar a VM do recoletor e execute o recoletor para detetar as suas VMs no local.

### <a name="create-a-project"></a>Criar um projeto

1. Na [portal do Azure](https://portal.azure.com), estes procurem **Azure Migrate**e crie um projeto (ContosoMigration).
2. Se especificar um nome de projeto, a subscrição do Azure e criar um novo grupo de recursos do Azure, **ContosoFailoverRG**. Tenha em atenção que:

    - Só pode criar um projeto do Azure Migrate na região E.U.A. Centro-Oeste ou E.U.A. Leste.
    - Pode planear uma migração para qualquer localização de destino.
    - A localização do projeto só é utilizada para armazenar os metadados recolhidos das VMs no local.

    ![Azure Migrate](./media/contoso-migration-assessment/project-1.png)


### <a name="download-the-collector-appliance"></a>Transferir a aplicação recoletora

O Azure Migrate cria uma VM no local, conhecida como aplicação recoletora. Esta VM deteta as VMs VMware no local e envia os metadados sobre as mesmas para o serviço Azure Migrate. Para configurar a aplicação recoletora, Contoso transfere um. Modelo de OVA e o importa para o vCenter server no local para criar a VM.

1. No projeto do Azure Migrate > **introdução** > **detetar e avaliar** > **detetar máquinas**, transferem o. Ficheiro de modelo OVA.
2. Eles copie o ID do projeto e a chave. Estes são necessárias para configurar o recoletor.

    ![Transferir o ficheiro .ova](./media/contoso-migration-assessment/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Verificar a aplicação recoletora

Antes de implementar a VM, a Contoso verifica se o. Ficheiro OVA é seguro.

1. No computador no qual o ficheiro transferido, abrem uma janela de comando de administrador.
2. Eles executam o seguinte comando para gerar o hash para o OVA:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Utilização de exemplo: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. O hash gerado deve corresponder a estas definições (versão 1.0.9.12)

**Algoritmo** | **Valor de hash**
--- | ---
MD5 | d0363e5d1b377a8eb08843cf034ac28a
SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

### <a name="create-the-collector-appliance"></a>Criar a aplicação recoletora

Agora a Contoso pode importar o ficheiro transferido para o vCenter Server e aprovisionar a VM do servidor de configuração.

1. Na consola do vSphere Client, clique **arquivo** > **implementar o modelo OVF**.

    ![Implementar OVF](./media/contoso-migration-assessment/vcenter-wizard.png)

2. No Assistente de implementação de modelo OVF > **origem**, eles especificam a localização da. Ficheiro OVA.
3. Na **nome e localização**, eles especificam um nome amigável para a VM do recoletor e a localização do inventário em que a VM será alojada. Eles também especificam o anfitrião ou cluster em que a aplicação recoletora será executado.
5. Na **armazenamento**, eles especificam a localização de armazenamento e, na **formato de disco**, como pretende aprovisionar o armazenamento.
7. Na **mapeamento da rede**, eles especificam a rede à qual se ligará a VM do recoletor. A rede necessita de conectividade à Internet para enviar metadados para o Azure.
8. Reveja as definições e selecione **ligar após a implementação**> **concluir**. Após a criação da aplicação, é enviada uma mensagem a confirmar a conclusão bem-sucedida.

### <a name="run-the-collector-to-discover-vms"></a>Executar o recoletor para detetar VMs

Agora eles são executados o recoletor para detetar VMs. Tenha em atenção que o recoletor atualmente apenas suporta "Inglês (Estados Unidos)" como idioma do sistema operativo e o idioma da interface do recoletor.

1. No vSphere cliente da consola > **abra a consola**, eles especificam o idioma, fuso horário e preferências de palavra-passe para a VM do recoletor.
2. Na área de trabalho, clique em o **executar recoletor** atalho.

    ![Atalho do recoletor](./media/contoso-migration-assessment/collector-shortcut.png)

4. No Recoletor do Azure Migrate > **configurar pré-requisitos**, aceitar os termos de licenciamento e leia as informações de terceiros.
5. O recoletor verifica que a VM tem acesso à internet, que a hora está sincronizada e que o serviço do recoletor está em execução (está instalado por predefinição na VM). Ele também instala o VMWare PowerCLI.

    > [!NOTE]
    > Estamos a pressupor que a VM tem acesso direto à Internet, sem um proxy.

    ![Verificar os pré-requisitos](./media/contoso-migration-assessment/collector-verify-prereqs.png)


5. Na **especificar detalhes do vCenter Server**, especifica o nome (FQDN) ou o endereço IP do servidor vCenter e as credenciais só de leitura utilizado para a deteção.
7. Eles selecionam um âmbito de deteção de VMS. O recoletor só pode detetar VMs dentro do âmbito especificado. O âmbito pode ser definido para uma pasta, datacenter ou cluster específicos. Não deve conter mais de 1500 VMs.

    ![Ligar ao vCenter](./media/contoso-migration-assessment/collector-connect-vcenter.png)

6. Na **especificar projeto de migração**, especifica o ID do projeto do Azure Migrate e a chave que copiou do portal. Pode obtê-las novamente no projeto **descrição geral** página > **detetar máquinas**.  

    ![Ligar ao Azure](./media/contoso-migration-assessment/collector-connect-azure.png)

7. Na **ver o progresso da coleção** Contoso pode monitorizar a deteção e verifique se os metadados recolhidos das VMs estão no âmbito. O recoletor fornece um período de deteção aproximado.

    ![Recolha em curso](./media/contoso-migration-assessment/collector-collection-process.png)



### <a name="verify-vms-in-the-portal"></a>Verificar as VMs no portal

Após a conclusão da coleção, a Contoso verifica se as VMs aparecem no portal.

1. No projeto do Azure Migrate > **Manage** > **máquinas**, eles verificam se as VMs que pretende detetar aparecem.

    ![Máquinas detetadas](./media/contoso-migration-assessment/discovery-complete.png)

3. Tenha em atenção que as máquinas não têm, atualmente, os agentes do Azure Migrate instalados. A Contoso precisa de instalá-los em para se poder ver dependências.

    ![Máquinas detetadas](./media/contoso-migration-assessment/machines-no-agent.png)



## <a name="step-5-prepare-for-dependency-analysis"></a>Passo 5: Preparar para a análise de dependência

Para ver as dependências entre as VMs que pretendem aceder a Contoso, eles transferirem em instalar agentes as VMs da aplicação. Contoso fá-lo em todas as VMs para as aplicações, Windows e Linux.

### <a name="take-a-snapshot"></a>Criar um Instantâneo

Eles mantenham uma cópia da VM antes de modificá-los, tirando um instantâneo antes dos agentes estiverem instalados.

![Instantâneo da máquina](./media/contoso-migration-assessment/snapshot-vm.png)


### <a name="download-and-install-the-vm-agents"></a>Transferir e instalar os agentes da VM

1. Na **máquinas** página, selecionam a máquina e, em seguida **requer a instalação** no **dependências** coluna.
2. Sobre o **detetar máquinas** página que fazem o seguinte:
    - Transferir o agente MMA e de dependência para cada VM do Windows
    - Transferir o agente MMA e de dependência para cada VM do Linux
3. Agora copie o ID de área de trabalho e a chave. Eles precisam estes ao instalar o MMA.

    ![Transferência do agente](./media/contoso-migration-assessment/download-agents.png)

### <a name="install-the-agents-on-windows-vms"></a>Instalar os agentes em VMs do Windows

Eles são executados a instalação em cada VM.

#### <a name="install-the-mma-on-windows-vms"></a>Instalar o MMA em VMs do Windows

1. Eles faça duplo clique no agente transferido.
2. Na **pasta de destino**, que mantenham a pasta de instalação predefinida > **próxima**.
2. Na **opções de configuração do agente**, eles selecionam **ligar o agente ao Azure Log Analytics** > **seguinte**.

    ![Instalação do MMA](./media/contoso-migration-assessment/mma-install.png)

5. Na **do Azure Log Analytics**, eles colar o ID de área de trabalho e a chave que copiou do portal.

    ![Instalação do MMA](./media/contoso-migration-assessment/mma-install2.png)

6. Na **pronto para instalar**, agora podem instalar o MMA.

#### <a name="install-the-dependency-agent-on-windows-vms"></a>Instalar o agente de dependência em VMs do Windows

1. Eles faça duplo clique no agente de dependência transferido.
2. Eles aceitam os termos de licenciamento e aguarde a conclusão da instalação.

    ![Agente de Dependência](./media/contoso-migration-assessment/dependency-agent.png)


### <a name="install-the-agents-on-linux-vms"></a>Instalar os agentes em VMs do Linux

Eles são executados a instalação em cada VM.

#### <a name="install-the-mma-on-linux-vms"></a>Instalar o MMA em VMs do Linux

1. Eles instalarem a biblioteca de ctypes python em cada VM com: **sudo apt-get instalar python ctypeslib**.
2. Eles devem executar o comando para instalar o agente MMA como raiz.  Para se tornar a execução de raiz, o seguinte comando e introduza a palavra-passe de raiz: **sudo -i**.
3. Agora que instalarem o agente MMA.
    - Insira o ID de área de trabalho correto e a chave para o comando.
    - Os comandos estão para 64 bits.
    - O **ID da área de trabalho** e **chave primária** podem ser encontrados no Portal do OMS > **definições**, no **origens ligadas** separador.
    - Execute os seguintes comandos para transferir o agente do OMS, validar a soma de verificação e instalação/carregar o agente.

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w 6b7fcaff-7efb-4356-ae06-516cacf5e25d -s k7gAMAw5Bk8pFVUTZKmk2lG4eUciswzWfYLDTxGcD8pcyc4oT8c6ZRgsMy3MmsQSHuSOcmBUsCjoRiG2x9A8Mg==
    ```



#### <a name="install-the-dependency-agent-on-linux-vms"></a>Instalar o agente de dependência em VMs do Linux

Após a instalação do MMA, a Contoso pode instalar o agente de dependência nas VMs do Linux.

1. O agente de dependência é instalado em computadores com Linux utilizar InstallDependencyAgent-Linux64.bin, um script de shell com um binário auto-extraível. Podem executar o ficheiro ao utilizar sh ou adicionar permissões para o próprio ficheiro de execução.

2. Se instalarem o agente de dependência de Linux como raiz:

    ```
    wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin && sudo sh InstallDependencyAgent-Linux64.bin -s
    ```


## <a name="step-6-run-and-analyze-the-vm-assessment"></a>Passo 6: Executar e analisar a avaliação da VM

Contoso agora pode verificar as dependências das máquinas e crie um grupo. Em seguida, eles são executados a avaliação para o grupo.

### <a name="verify-dependencies-and-create-a-group"></a>Verifique as dependências e criar um grupo


1. Para as máquinas analisar, clicarem **ver dependências**.

    ![Ver as dependências das máquinas](./media/contoso-migration-assessment/view-machine-dependencies.png)

2. Para SQLVM, o mapa de dependências mostra os seguintes detalhes:

    - Grupos de processos/processos com ligações de rede ativas em execução em SQLVM, durante o período de tempo especificado (uma hora, por predefinição)
    - Ligações TCP de entrada (cliente) e saída (servidor) de e para todas as máquinas dependentes
    - As máquinas dependentes com os agentes do Azure Migrate instalados são apresentadas como caixas separadas
    - As máquinas que não têm os agentes instalados mostram as informações de porta e de endereço IP.

3. Para computadores com o agente instalado (WEBVM), eles clicam na caixa da máquina para ver mais informações, incluindo o FQDN, o sistema operativo e o endereço MAC.

    ![Ver dependências de grupo](./media/contoso-migration-assessment/sqlvm-dependencies.png)

4. Agora, selecionam as VMs para adicionar ao grupo (SQLVM e WEBVM).  Eles podem usar CTRL + clique para selecionar várias VMs.
5. Clicarem **criar grupo**e especifique um nome (smarthotelapp).

> [!NOTE]
    > Para ver dependências mais granulares, pode expandir o intervalo de tempo. Pode selecionar uma duração específica ou datas de início e de fim.


### <a name="run-an-assessment"></a>Executar uma avaliação


1. Sobre o **grupos** página, abra o grupo (smarthotelapp) e clique em **Criar avaliação**.

    ![Criar uma avaliação](./media/contoso-migration-assessment/run-vm-assessment.png)

2. A avaliação aparece na página **Gerir** > **Avaliações**.

Contoso utilizado as predefinições de avaliação, mas pode personalizar as definições. [Saiba mais](how-to-modify-assessment.md).



### <a name="analyze-the-vm-assessment"></a>Analisar a avaliação da VM

Uma avaliação do Azure Migrate incluem informações sobre a compatibilidade de VMs no local para o Azure, dimensionamento certo sugerido para VM do Azure e custos do Azure mensais estimados.

![Relatório de avaliação](./media/contoso-migration-assessment/assessment-overview.png)

#### <a name="review-confidence-rating"></a>Rever a classificação de confiança

![Apresentação da avaliação](./media/contoso-migration-assessment/assessment-display.png)

Uma avaliação obtém uma classificação de confiança de 1 a 5 estrelas (1 estrela corresponde a ser a mais baixa e 5 a mais alta).
- A classificação de confiança é alocada a uma avaliação com base na disponibilidade dos pontos de dados necessários para calcular a avaliação.
- A classificação ajuda a calcular a fiabilidade das recomendações de tamanho disponibilizadas pelo Azure Migrate.
- Classificação de confiança é útil quando estiver fazendo *dimensionamento com base no desempenho* como o Azure Migrate poderá não ter pontos de dados suficientes para fazer o dimensionamento baseado em utilização. Para o *dimensionamento no local*, a classificação de confiança é sempre de 5 estrelas, dado que o Azure Migrate tem todos os pontos de dados de que precisa para dimensionar a VM.
- Consoante a percentagem de pontos de dados disponíveis, é fornecida a classificação de confiança para a avaliação:

   **Disponibilidade de pontos de dados** | **Classificação de confiança**
   --- | ---
   0%-20% | 1 Estrela
   21%-40% | 2 Estrelas
   41%-60% | 3 Estrelas
   61%-80% | 4 Estrelas
   81%-100% | 5 Estrelas

#### <a name="verify-readiness"></a>Verificar a preparação

![Preparação para a avaliação](./media/contoso-migration-assessment/azure-readiness.png)  

O relatório da avaliação mostra as informações resumidas na tabela. Tenha em conta que, para mostrar o dimensionamento baseado no desempenho, o Azure Migrate precisa das seguintes informações. Se não for possível recolhê-las, a avaliação do tamanho poderá não ser precisa.

- Dados de utilização da CPU e da memória.
- IOPS de leitura/escrita e débito de cada disco ligado à VM.
- Informações de entrada e saída da rede de cada placa de rede ligada à VM.


**Definição** | **Indicação** | **Detalhes**
--- | --- | ---
**Preparado para a VM do Azure** | Indica se a VM está preparada para migração | Estados possíveis:</br><br/>- Preparada para o Azure<br/><br/>- Preparada com condições <br/><br/>- Não está preparada para o Azure<br/><br/>- Preparação desconhecida<br/><br/> Se uma VM não estiver preparada, mostraremos alguns passos para a resolução.
**Tamanho da VM do Azure** | Para VMs prontas, recomendamos um tamanho de VM do Azure. | A recomendação do tamanho depende das propriedades da avaliação:<br/><br/>- Se tiver utilizado o dimensionamento baseado no desempenho, é considerado o histórico do desempenho das VMs.<br/><br/>- Se tiver utilizado “como no local”, o tamanho tem por base o tamanho da VM no local e os dados de utilização não são usados.
**Ferramenta sugerida** | Uma vez que as nossas máquinas estão a executar os agentes, o Azure Migrate analisa os processos em execução dentro das mesmas e identifica se são máquinas de base de dados ou não.
**Informações da VM** | O relatório mostra definições da VM no local, incluindo informações do sistema operativo, do tipo de arranque, do disco e do armazenamento.


#### <a name="review-monthly-cost-estimates"></a>Rever as estimativas de custo mensal

Esta vista mostra o custo total de computação e armazenamento da execução das VMs no Azure, bem como os detalhes de cada máquina.

![Preparação para a avaliação](./media/contoso-migration-assessment/azure-costs.png)

- As recomendações de tamanho das máquinas são utilizadas para calcular as estimativas de custos.
- Os custos mensais estimados para computação e armazenamento são agregados para todas as VMs no grupo.


## <a name="clean-up-after-assessment"></a>Limpar após a avaliação

- Depois de concluída a avaliação, a Contoso retém a aplicação de migração do Azure para avaliações futuras.
- Eles desativar a VM de VMware. Eles começarão-lo novamente quando eles avaliar VMs adicionais.
- Eles irá manter o projeto de migração da Contoso no Azure.  Está atualmente implementada no grupo de recursos de ContosoFailoverRG, na região E.u.a. leste do Azure.
-  A VM do recoletor tem uma licença de avaliação de 180 dias. Se este limite expirar, precisam de transferir e configurar o recoletor novamente.


## <a name="conclusion"></a>Conclusão

Neste cenário Contoso avaliado a respetiva base de dados de aplicação SmartHotel usando a ferramenta DMA e as VMs no local com o serviço Azure Migrate. Eles, em seguida, revimos as avaliações para se certificar de que os recursos no local estão prontos para a migração para o Azure.

## <a name="next-steps"></a>Passos Seguintes

No próximo artigo desta série, a Contoso realojar sua SmartHotel de aplicação no Azure com uma migração lift-and-shift. Contoso migra do WEBVM front-end para a aplicação com o Azure Site Recovery e a base de dados de aplicação para uma instância SQL do Azure geridos, com o serviço de migração de base de dados. [Introdução ao](contoso-migration-rehost-vm-sql-managed-instance.md) com esta implementação.
