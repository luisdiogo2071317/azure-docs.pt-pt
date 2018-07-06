---
title: Refatorizar uma aplicação de suporte técnico de serviço do Linux da Contoso para o serviço de aplicações do Azure e o MySQL do Azure | Documentos da Microsoft
description: Saiba como Contoso revisasse, encontrasse aplicação do Linux no local ao migrar para o serviço de aplicações do Azure com o GitHub para a camada Web e a base de dados do Azure SQL.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/04/2018
ms.author: raynew
ms.openlocfilehash: 585076b3fef56fe0e59828137674d7eb14528540
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37872476"
---
# <a name="contoso-migration-refactor-a-contoso-linux-service-desk-app-to-the-azure-app-service-and-azure-mysql"></a>Migração de Contoso: Refatorizar uma aplicação de suporte técnico de serviço do Linux da Contoso para o serviço de aplicações do Azure e o MySQL do Azure

Este artigo mostra como Contoso revisasse, encontrasse seu aplicativo de suporte técnico de serviço na Linux de duas camadas no local (osTicket), ao migrar para o serviço de aplicações do Azure com integração do GitHub e MySQL do Azure.

Este documento é o décimo de uma série de artigos que mostram como a empresa fictícia Contoso migra os respetivos recursos no local para a cloud do Microsoft Azure. A série contém informações gerais e cenários que ilustram como configurar uma infraestrutura de migração e executar diferentes tipos de migrações. Cenários de crescem em complexidade e vamos adicionar artigos adicionais ao longo do tempo.

**Artigo** | **Detalhes** | **Estado**
--- | --- | ---
[Artigo 1: Descrição geral](contoso-migration-overview.md) | Fornece uma descrição geral da estratégia de migração da Contoso, a série de artigos e as aplicações de exemplo que usamos. | Disponível
[Artigo 2: Implementar uma infraestrutura do Azure](contoso-migration-infrastructure.md) | Descreve como o Contoso prepara a sua infraestrutura do Azure e no local, para a migração. A mesma infra-estrutura é utilizada para todos os cenários de migração da Contoso. | Disponível
[Artigo 3: Avaliar a recursos no local](contoso-migration-assessment.md)  | Mostra como Contoso é executado uma avaliação da sua aplicação de SmartHotel de duas camadas do local em execução no VMware. Avaliar a VMs de aplicação com o [do Azure Migrate](migrate-overview.md) serviço e a base de dados do SQL Server do aplicativo com o [Assistente de migração de base de dados do Azure](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponível
[Artigo 4: Realojamento para VMs do Azure e uma instância gerida de SQL](contoso-migration-rehost-vm-sql-managed-instance.md) | Demonstra como a Contoso migra a aplicação de SmartHotel para o Azure. Estes migrar a aplicação web VM com [do Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)e a base de dados de aplicação com o [Azure Database Migration](https://docs.microsoft.com/azure/dms/dms-overview) serviço, para migrar para uma instância gerida de SQL. | Disponível
[Artigo 5: Realojar em VMs do Azure](contoso-migration-rehost-vm.md) | Mostra como o Contoso migrar seus SmartHotel para VMs de IaaS do Azure, com o serviço Site Recovery.
[Artigo 6: Realojar em VMs do Azure e grupos de disponibilidade do SQL Server](contoso-migration-rehost-vm-sql-ag.md) | Mostra como a Contoso migra a aplicação de SmartHotel. Utilizam o Site Recovery para migrar as VMs da aplicação e o serviço de migração de base de dados para migrar a base de dados de aplicação para um grupo de disponibilidade do SQL Server. | Disponível
[Artigo 7: Realojar uma aplicação do Linux para VMs do Azure](contoso-migration-rehost-linux-vm.md) | Mostra como a Contoso migra a aplicação de Linux osTicket para VMs de IaaS do Azure com o Azure Site Recovery.
[Artigo 8: Realojar uma aplicação do Linux para VMs do Azure e o servidor MySQL do Azure](contoso-migration-rehost-linux-vm-mysql.md) | Demonstra como a Contoso migra a aplicação de Linux osTicket. Utilizam o Site Recovery para a migração de VM e o MySQL Workbench para migrar para uma instância do servidor MySQL do Azure. | Disponível
Artigo 9: Refatorizar uma aplicação para uma aplicação Web do Azure e a base de dados SQL do Azure | Demonstra como Contoso migra a aplicação de SmartHotel para uma aplicação web do Azure baseadas em contentores e migra a base de dados de aplicação para o servidor SQL do Azure. | Disponível
Artigo 10: Refatorizar uma aplicação do Linux para o serviço de aplicações do Azure e o servidor MySQL do Azure | Mostra como a Contoso migra a aplicação de Linux osTicket para o serviço de aplicações do Azure com o contentor de Docker do PHP 7.0. A base de código para a implementação é migrada para o GitHub. A base de dados de aplicação é migrada para MySQL do Azure. | Este artigo.


Neste artigo, a Contoso migra uma aplicação de suporte técnico de serviço de Linux Apache MySQL PHP (LAMP) de duas camadas (osTicket) para o Azure. Se gostaria de utilizar esta aplicação de código-fonte aberto, pode transferi-lo a partir [GitHub](https://github.com/osTicket/osTicket).



## <a name="business-drivers"></a>Fatores comerciais

A equipe de liderança de TI tem trabalhado em estreita colaboração com seus parceiros de negócios para compreender o que quer atingir:

- **Resolver o crescimento do negócio**: Contoso está crescendo e mover em novos mercados. Eles precisam de agentes de serviço de cliente adicionais. 
- **Dimensionamento**: A solução deve ser criada para que a Contoso pode adicionar mais agentes de serviço de cliente como as escalas de negócios.
- **Aumentar a resiliência**: nos últimos problemas com o sistema afetado apenas a utilizadores internos. Com seu novo modelo de negócios, usuários externos serão afetados e a necessidade de Contoso a aplicação em funcionamento, o tempo todo.

## <a name="migration-goals"></a>Objetivos de migração

A equipe de cloud Contoso tiver afixado para baixo de objetivos para essa migração, para determinar o melhor método de migração:

- O aplicativo deve aumentar além da capacidade de no local atual e o desempenho.  Contoso é mover a aplicação para tirar partido do dimensionamento do Azure a pedido.
- Contoso pretende mover a base de código de aplicação para um pipeline de entrega contínua.  Como as alterações da aplicação são enviados por push para o GitHub, eles querem implementar essas alterações sem tarefas para a equipe de operações.
- O aplicativo deve ser resiliente com capacidades para crescimento e ativação pós-falha. Pretende implementar a aplicação em duas regiões do Azure diferentes e configurá-la para dimensionar automaticamente.
- Contoso quer minimizar as tarefas de administração de base de dados depois da aplicação é movida para a cloud.

## <a name="solution-design"></a>Design da solução
Depois de fixar-se para baixo de seus objetivos e requisitos, a Contoso projeta e rever uma solução de implantação e identifique o processo de migração, incluindo os serviços do Azure que eles usarão para a migração.


## <a name="current-architecture"></a>Arquitetura atual

- A aplicação está em camadas em duas VMs (OSTICKETWEB e OSTICKETMYSQL).
- As VMs estão localizadas no anfitrião ESXi do VMware **contosohost1.contoso.com** (versão 6.5).
- O ambiente do VMware é gerido pelo vCenter Server 6.5 (**vcenter.contoso.com**), em execução numa VM.
- A Contoso tiver um datacenter no local (contoso-datacenter), com um controlador de domínio no local (**contosodc1**).

![Arquitetura atual](./media/contoso-migration-refactor-linux-app-service-mysql/current-architecture.png) 


## <a name="proposed-architecture"></a>Arquitetura proposta

Depois de fixar-se para baixo de sua arquitetura atual, metas e requisitos de migração, Contoso desenvolve uma solução de implantação e identifica o processo de migração, incluindo os serviços do Azure que eles usarão para a migração.

- A aplicação de camada de web OSTICKETWEB será migrada com a criação de um serviço de aplicações do Azure em duas regiões do Azure. Serviço de aplicações do Azure para Linux irá ser implementado com o contentor de Docker do PHP 7.0.
- O código da aplicação será movido para o GitHub e aplicação Web do Azure vai ser configurada para a entrega contínua com o GitHub.
- Servidores de aplicações do Azure será implementada em ambos os primário (E.U.A. Leste 2) e a região secundária do (EUA Central).
- O Gestor de tráfego irá configurar à frente de duas aplicações Web do Azure em ambas as regiões.
- O Gestor de tráfego irá ser configurado no modo de prioridade para forçar o tráfego através do E.U.A. Leste 2.
- Se o servidor de aplicações do Azure na região E.U.A. Leste 2 ficar offline, os utilizadores podem aceder a falha sobre a aplicação na região E.U.A. Central.
- A base de dados de aplicação será migrado para o serviço de PaaS do MySQL do Azure com as ferramentas do MySQL Workbench. A base de dados no local serão uma cópia de segurança localmente e restaurado diretamente para o MySQL do Azure.
- A base de dados irá residir na região E.U.A. Leste 2 primária, na sub-rede da base de dados (PROD-DB-EUS2) na rede de produção (VNET-PROD-EUS2):
- Uma vez que eles estão migrando uma carga de trabalho de produção, os recursos do Azure para a aplicação irão residir no grupo de recursos de produção **ContosoRG**.
- O recurso de Gestor de tráfego será implementado no grupo de recursos de infraestrutura da Contoso **ContosoInfraRG**.
- As VMs no local no Centro de dados Contoso serão encerradas depois de terminar a migração.


![Arquitetura do cenário](./media/contoso-migration-refactor-linux-app-service-mysql/proposed-architecture.png) 


## <a name="migration-process"></a>Processo de migração

Contoso concluirá o processo de migração da seguinte forma:

1. Como primeiro passo, Contoso define a infraestrutura do Azure, incluindo o aprovisionamento dos serviços de aplicações do Azure, a definição de cópia de segurança do Gestor de tráfego e aprovisionar uma instância do MySQL do Azure.
2. Depois de preparar o Azure, estes migrar a base de dados com o MySQL Workbench. 
3. Depois da base de dados está em execução no Azure, que configurar um repositório privado do GitHub para o serviço de aplicações do Azure com a entrega contínua e carregá-lo com a aplicação de osTicket.
4. No portal do Azure, eles carregar a aplicação do GitHub para o contentor do Docker a executar o serviço de aplicações do Azure. 
5. Otimizar as definições de DNS e configurar o dimensionamento automático para a aplicação.

![Processo de migração](./media/contoso-migration-refactor-linux-app-service-mysql/migration-process.png) 


### <a name="azure-services"></a>Serviços do Azure

**Serviço** | **Descrição** | **Custo**
--- | --- | ---
[Serviço de Aplicações do Azure](https://azure.microsoft.com/services/app-service/) | O serviço é executado e dimensiona a aplicações que utilizam o serviço de PaaS do Azure para Web sites.  | Preço baseia-se no tamanho das instâncias e as funcionalidades necessárias. [Saiba mais](https://azure.microsoft.com/pricing/details/app-service/windows/).
[Gestor de Tráfego](https://azure.microsoft.com/services/traffic-manager/) | Um balanceador de carga que utiliza o DNS para direcionar os utilizadores do Azure, ou Web sites externos e serviços. | Preço baseia-se no número de consultas DNS recebidas e o número de pontos finais monitorizados. | [Saiba mais](https://azure.microsoft.com/pricing/details/traffic-manager/).
[Base de Dados do Azure para MySQL](https://docs.microsoft.com/azure/mysql/) | A base de dados baseia-se no motor do servidor MySQL de código-fonte aberto. Ele fornece uma Comunidade totalmente gerida e prontas para empresas base de dados do MySQL, como um serviço para desenvolvimento de aplicações e a implantação. | Preços baseados em computação, armazenamento e requisitos de cópia de segurança. [Saiba mais](https://azure.microsoft.com/pricing/details/mysql/).

 
## <a name="prerequisites"></a>Pré-requisitos

Se (e Contoso) pretendem executar este cenário, eis o que deve ter.

**Requisitos** | **Detalhes**
--- | ---
**Subscrição do Azure** | Deve já tiver criado uma subscrição durante os primeiros artigos desta série. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Se criar uma conta gratuita, será o administrador da sua subscrição e poderá executar todas as ações.<br/><br/> Se utilizar uma subscrição existente e não for o administrador, terá de trabalhar com o administrador para lhe atribuir permissões de proprietário ou contribuinte.<br/><br/> Se precisar de permissões mais granulares, reveja [este artigo](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Infraestrutura do Azure** | Contoso configurar a sua infraestrutura do Azure, conforme descrito em [infraestrutura do Azure para a migração](contoso-migration-infrastructure.md).



## <a name="scenario-steps"></a>Passos do cenário

Eis como o Azure irá concluir a migração:

> [!div class="checklist"]
> * **Passo 1: Aprovisionar serviços de aplicações do Azure**: Contoso irá aprovisionar aplicações Web, nas regiões primárias e secundárias.
> * **Passo 2: Configure o Gestor de tráfego**: configuram o Gestor de tráfego na frente de aplicações Web, para balanceamento de carga e encaminhamento de tráfego.
> * **Passo 3: Aprovisionar MySQL**: no Azure, a Contoso Aprovisiona uma instância da base de dados MySQL do Azure.
> * **Passo 4: Migrar a base de dados**: estes migrar a base de dados com o MySQL Workbench. 
> * **Passo 5: Configurar o GitHub**: Contoso configura um repositório de GitHub local para o web sites/código da aplicação.
> * **Passo 6: Implementar as aplicações web**: Contoso implementa as aplicações web do GitHub.




## <a name="step-1-provision-azure-app-services"></a>Passo 1: Serviços de aplicações do Azure de aprovisionamento

Contoso Aprovisiona duas aplicações Web (um em cada região) usando os serviços de aplicações do Azure.

1. Se criaram um recurso de aplicação Web na região E.U.A. Leste 2 primária (**osticket eus2**) do Azure Marketplace.
2. Elas o colocam o recurso no grupo de recursos de produção **ContosoRG**.

    ![Aplicação do Azure](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app1.png) 

3. Eles criam um novo plano do serviço de aplicações na região primária (**SVP-aplicação-EUS2**), com o tamanho padrão.

     ![Aplicação do Azure](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app2.png) 
    
4. Contoso seleciona um SO Linux com a pilha de runtime de PHP 7.0, o que é um contentor de Docker.

    ![Aplicação do Azure](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app3.png) 

5. Eles criam uma segunda aplicação web (**osticket cus**) e o plano do serviço de aplicações para a região EUA Central.

    ![Aplicação do Azure](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app4.png) 


**Precisa de mais ajuda?**

- Saiba mais sobre [aplicações Web do serviço de aplicações do Azure](https://docs.microsoft.com/azure/app-service/app-service-web-overview).
- Saiba mais sobre [serviço de aplicações do Azure no Linux](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro).


## <a name="step-2-set-up-traffic-manager"></a>Passo 2: Configure o Gestor de tráfego

Contoso define se o Gestor de tráfego para direcionar os pedidos de web de entrada para aplicações Web em execução na camada web osTicket.

1. Contoso cria um recurso de Gestor de tráfego (**osticket.trafficmanager.net**) do Azure Marketplace. Eles usam o encaminhamento de prioridade, para que este dos E.U.A. 2 é o site primário. Eles colocam o recurso no seu grupo de recursos de infraestrutura (**ContosoInfraRG**). Tenha em atenção que o Gestor de tráfego é global e não está vinculado a uma localização específica

    ![Gestor de Tráfego](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager1.png) 

2. Agora, a Contoso configurar Gestor de tráfego com pontos de extremidade. Eles adicionam a aplicação Web de 2 do Leste-nos como o site primário (**osticket eus2**) e a aplicação do centro dos E.U.A. como secundário (**osticket cus**).

    ![Gestor de Tráfego](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager2.png) 

3. Depois de adicionar os pontos finais, eles podem monitorizá-las.

    ![Gestor de Tráfego](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager3.png)

**Precisa de mais ajuda?**

- Saiba mais sobre [Gestor de tráfego](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview).
- Saiba mais sobre [encaminhar o tráfego para um ponto final prioritário](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-priority-routing-method).
 
## <a name="step-3-provision-azure-database-for-mysql"></a>Passo 3: Aprovisionar base de dados do Azure para MySQL

Contoso aprovisiona um MySQL da base de instância na região E.U.A. Leste 2 primária.

1. No portal do Azure, é criar uma base de dados do Azure para MySQL recurso. 

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-1.png)

2. Se adicionam o nome **contosoosticket** para a base de dados do Azure. Eles adicionarem a base de dados para o grupo de recursos de produção **ContosoRG**e especificar credenciais para o mesmo.
3. A base de dados do MySQL no local é a versão 5.7, para que selecionarem esta versão para compatibilidade. Eles usam os tamanhos padrão, o que corresponder aos seus requisitos de base de dados.

     ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-2.png)

4. Para **opções de redundância da cópia de segurança**, Contoso seleciona a utilizar **Georredundante**. Esta opção permite-lhes restaurar a base de dados na sua região secundária do E.U.A. Central, se ocorrer uma falha. Só pode configurar esta opção quando aprovisionam a base de dados.

    ![Redundância](./media/contoso-migration-refactor-linux-app-service-mysql/db-redundancy.png)

4. Configurar a segurança de ligação de contoso. Na base de dados > **segurança de ligação**, eles configurar regras de Firewall para permitir que a base de dados aceder aos serviços do Azure.
5. Se adicionar o endereço IP de cliente de estação de trabalho local para os endereços IP inicial e final. Isso permite que as aplicações Web para acessar o banco de dados do MySQL, juntamente com o cliente de base de dados que está a efetuar a migração.

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-3.png)



## <a name="step-4-migrate-the-database"></a>Passo 4: Migrar a base de dados

Contoso irá migrar a base de dados com cópia de segurança e restauro, com ferramentas do MySQL. Instalar o MySQL Workbench, cópia de segurança da base de dados do OSTICKETMYSQL e, em seguida, restaurá-la à base de dados do Azure para o servidor MySQL.

### <a name="install-mysql-workbench"></a>Instalar MySQL Workbench

1. Verificações de contoso a [pré-requisitos e downloads do MySQL Workbench](https://dev.mysql.com/downloads/workbench/?utm_source=tuicool).
2. Instalar o MySQL Workbench para Windows em conformidade com o [instruções de instalação](https://dev.mysql.com/doc/workbench/en/wb-installing.html). A máquina em que instalar tem de estar acessível para a OSTICKETMYSQL VM e o Azure através da internet.
3. No Workbench do MySQL, eles criam uma ligação ao MySQL para OSTICKETMYSQL. 

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench1.png)

4. Eles exportar a base de dados como **osticket**, num arquivo autônomo local.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench2.png)

5. Depois da base de dados foi feito o backup localmente, eles criaram uma ligação à base de dados do Azure para a instância do MySQL.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench3.png)

6. Agora, Contoso pode importar (restauro) a base de dados na instância do MySQL do Azure, o independente do ficheiro. Um novo esquema (osticket) é criado para a instância.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench4.png)

7. Após o restauro de dados, ele pode ser consultado com a Bancada de trabalho e é apresentada no portal do Azure.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench5.png)

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench6.png)

8. Por fim, a Contoso precisa de atualizar as informações da base de dados em aplicativos web. Na instância do MySQL, abra **cadeias de ligação**. 

     ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench7.png)

9. Na lista de cadeias de caracteres, eles localizar as definições de aplicação Web e clique para copiá-los.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench8.png)

10. Abra uma janela do bloco de notas e cole a cadeia de caracteres num novo arquivo e atualizá-lo de acordo com a base de dados de osticket, instância MySQL e definições de credenciais.

     ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench9.png)

11. Contoso pode verificar o nome do servidor e o início de sessão do **descrição geral** na instância do MySQL no portal do Azure.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench10.png)


## <a name="step-5-set-up-github"></a>Passo 5: Configurar o GitHub

Contoso cria um novo repositório privado do GitHub e configura uma ligação à base de dados do osTicket MySQL do Azure. Em seguida, eles carregam a aplicação Web do Azure com a aplicação.  

1.  Navegue para o repositório de GitHub público do OsTicket software e bifurcá-la à conta GitHub da Contoso.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github1.png)

2. Após criar o fork, navegue para o **incluem** pasta e localizar o **ost config.php** ficheiro.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github2.png)


3. O ficheiro é aberto no browser e eles editá-lo.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github3.png)

4. No editor, Contoso atualiza os detalhes de base de dados, especificamente **DBHOST** e **DBUSER**. 

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github4.png)

5. Em seguida, eles consolidar as alterações.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github5.png)

6. Para cada aplicação web (**osticket eus2** e **osticket cus**), Contoso modificar o **definições de aplicação** no portal do Azure.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github6.png)

7. Eles introduza a cadeia de ligação com o nome **osticket**e copie a cadeia de bloco de notas para o **valor área**. Eles selecionam **MySQL** na lista pendente junto a cadeia de caracteres e guarde as definições.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github7.png)

## <a name="step-6-configure-the-web-apps"></a>Passo 6: Configurar as aplicações Web

Como a etapa final do processo de migração, o Contoso configurar as aplicações web com os web sites do osTicket.



1. Na aplicação web primário (**osticket eus2**), em seguida, abra **opção de implementação** e defina a origem para **GitHub**.

    ![Configurar aplicação](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app1.png)

2. Selecionar as opções de implementação.

    ![Configurar aplicação](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app2.png)

3. Depois de definir as opções, o mostra de configuração como pendentes no portal do Azure.

    ![Configurar aplicação](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app3.png)

4. Depois da configuração é atualizada e a aplicação de web de osTicket é carregada a partir do GitHub para o contentor de Docket que executa o serviço de aplicações do Azure, o site é apresentado como ativo.

    ![Configurar aplicação](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app4.png)

5. Contoso, em seguida, se repetir os passos acima para a aplicação web secundário (**osticket cus**).
6. Depois do site está configurado, pode ser acessado pelo perfil do Gestor de tráfego. O nome DNS é a nova localização da aplicação osTicket. [Saiba mais](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain#map-a-cname-record).

    ![Configurar aplicação](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app5.png)
    
7. Contoso, além de um nome DNS que é fácil de memorizar. Eles criam um registo de alias (CNAME) **osticket.contoso.com** que aponta para o nome do Gestor de tráfego, no DNS em seus controladores de domínio.

    ![Configurar aplicação](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app6.png)

8. Configurar ambos os **osticket eus2** e **osticket cus** aplicações para permitir que os nomes de anfitrião personalizado da web.

    ![Configurar aplicação](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app7.png)

### <a name="set-up-autoscaling"></a>Configurar dimensionamento automático

Por fim, configuraram o dimensionamento automático para a aplicação. Isto garante que, como agentes de utilizam a aplicação, as instâncias da aplicação aumentarem e diminuir de acordo com as necessidades de negócio. 

1. No serviço de aplicações **aplicação-SRV-EUS2**, abra Contoso **unidade de escala**.
2. Configurar uma nova definição de dimensionamento automático com uma única regra que aumenta a contagem de instâncias por um quando a percentagem de CPU para a instância atual é superior a 70% durante 10 minutos.

    ![Dimensionamento Automático](./media/contoso-migration-refactor-linux-app-service-mysql/autoscale1.png)

3. Configurar a mesma definição no **aplicação-SRV-CUS** para se certificar de que o mesmo comportamento se aplica se a aplicação faz a ativação pós-falha para a região secundária. A única diferença é que eles definir o limite de instância como 1, uma vez que isso é para apenas as ativações pós-falha.

   ![Dimensionamento Automático](./media/contoso-migration-refactor-linux-app-service-mysql/autoscale2.png)

##  <a name="clean-up-after-migration"></a>Limpar após a migração

Com a migração é concluída, a aplicação de osTicket é refatorada para em execução numa aplicação Web do Azure com a entrega contínua através de um repositório privado do GitHub. A aplicação em execução em duas regiões para uma maior resiliência. A base de dados osTicket está em execução na base de dados do Azure para MySQL após a migração para a plataforma de PaaS.

Agora, a Contoso precisa de fazer o seguinte: 
- Remova as VMs do VMware no inventário do vCenter.
- Remova as VMs no local das tarefas de cópia de segurança locais.
- Documentação de atualização interna Mostrar novas localizações e endereços IP. 
- Reveja todos os recursos que interagem com as VMs no local e atualizar as definições relevantes ou documentação para refletir a nova configuração.
- Reconfigure a monitorização para apontar para o URL de osticket trafficmanager.net, para controlar a que a aplicação está ativo e em execução.

## <a name="review-the-deployment"></a>Reveja a implementação

Com a aplicação agora em execução, Contoso tem totalmente operacionalizar e proteger sua infra-estrutura de novo.

### <a name="security"></a>Segurança

A equipe de segurança da Contoso revistas a aplicação para determinar os problemas de segurança. Identificaram que a comunicação entre a aplicação de osTicket e a instância de base de dados MySQL não está configurada para SSL. Eles, terá de fazê-lo para se certificar de que o tráfego de base de dados não pode ser invadido por hackers. [Saiba mais](https://docs.microsoft.com/azure/mysql/howto-configure-ssl).

### <a name="backups"></a>Cópias de segurança

- As aplicações de web osTicket não contêm dados de estado e, portanto, não precisam de uma cópia de segurança.
- Eles não precisam de configurar a cópia de segurança da base de dados. Base de dados do Azure para MySQL cria automaticamente os cópias de segurança do servidor e armazena. Optaram por fazer a utilizar a redundância geográfica para a base de dados, pelo que é resiliente e prontos para produção. As cópias de segurança podem ser utilizadas para restaurar o servidor para um ponto anterior no tempo. [Saiba mais](https://docs.microsoft.com/azure/mysql/concepts-backup).


### <a name="licensing-and-cost-optimization"></a>Otimização de licenciamento e de custo

- Não existem não existem problemas de licenciamento para a implementação de PaaS.
- Contoso irá ativar o Azure Cost Management licenciado pela Cloudyn, uma subsidiária da Microsoft. É uma solução de gestão de custos multiclouds que o ajuda a utilizar e gerir o Azure e outros recursos da cloud.  [Saiba mais](https://docs.microsoft.com/azure/cost-management/overview) sobre o Azure Cost Management.



