---
title: Rearquitetar uma aplicação da Contoso num contentor do Azure e SQL Database do Azure | Documentos da Microsoft
description: Saiba como Contoso rearquitetar uma aplicação em contentores do Windows Azure e base de dados do Azure SQL.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/05/2018
ms.author: raynew
ms.openlocfilehash: 340f7a53198cc8fbbdec9a6721ab3c2ec9ceccb3
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37921424"
---
# <a name="contoso-migration-rearchitect-an-on-premises-app-to-an-azure-container-and-azure-sql-database"></a>Migração de Contoso: Rearquitetar uma aplicação no local para um contentor do Azure e a base de dados do Azure SQL

Este artigo demonstra como Contoso migra e rearquitetar a sua aplicação SmartHotel no Azure. O front-end de aplicação VM estes migrar para um contentor do Windows Azure e a base de dados de aplicação para uma base de dados SQL do Azure.

Este documento é um de uma série de artigos que mostram como a empresa fictícia Contoso migra os seus recursos no local para a cloud do Microsoft Azure. A série contém informações gerais e cenários que ilustram como configurar uma infraestrutura de migração, avaliação dos recursos no local para migração e execução de diferentes tipos de migrações. Cenários de crescem em complexidade e vamos adicionar artigos adicionais ao longo do tempo.

**Artigo** | **Detalhes** | **Estado**
--- | --- | ---
[Artigo 1: Descrição geral](contoso-migration-overview.md) | Fornece uma descrição geral da estratégia de migração da Contoso, a série de artigos e as aplicações de exemplo que usamos. | Disponível
[Artigo 2: Implementar uma infraestrutura do Azure](contoso-migration-infrastructure.md) | Descreve como o Contoso prepara a sua infraestrutura do Azure e no local, para a migração. A mesma infra-estrutura é utilizada para todos os artigos de migração. | Disponível
[Artigo 3: Avaliar a recursos no local](contoso-migration-assessment.md)  | Mostra como Contoso é executado uma avaliação de uma aplicação de SmartHotel de duas camadas no local em execução no VMware. Contoso avalia as VMs de aplicação com o [do Azure Migrate](migrate-overview.md) serviço e a base de dados do SQL Server do aplicativo com o [Assistente de migração de base de dados](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponível
[Artigo 4: Realojar a uma aplicação para as VMs do Azure e uma instância gerida de SQL](contoso-migration-rehost-vm-sql-managed-instance.md) | Demonstra como Contoso é executado uma migração lift-and-shift para o Azure para a aplicação de SmartHotel. Contoso migra o front-end de aplicação VM através de [do Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)e a base de dados de aplicação para a instância gerida de SQL, utilizando o [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Disponível
[Artigo 5: Realojar a uma aplicação nas VMS do Azure](contoso-migration-rehost-vm.md) | Mostra como Contoso migrar a aplicação de SmartHotel VMs com apenas o Site Recovery. | Disponível
[Artigo 6: Realojar a uma aplicação para as VMs do Azure e o SQL Server sempre no grupo de disponibilidade](contoso-migration-rehost-vm-sql-ag.md) | Mostra como a Contoso migra a aplicação de SmartHotel. A Contoso utiliza o Site Recovery para migrar as VMs da aplicação e o serviço de migração de base de dados para migrar a base de dados de aplicação para um cluster do SQL Server protegido por um grupo de Disponibilidade AlwaysOn. | Disponível
[Artigo 7: Realojar uma aplicação do Linux para VMs do Azure](contoso-migration-rehost-linux-vm.md) | Mostra como Contoso faz uma migração lift-and-shift da aplicação de osTicket do Linux para VMs do Azure, com o Site Recovery | Disponível
[Artigo 8: Realojar uma aplicação do Linux para VMs do Azure e o servidor MySQL do Azure](contoso-migration-rehost-linux-vm-mysql.md) | Demonstra como a Contoso migra a aplicação de osTicket do Linux para as VMs do Azure com o Site Recovery e migra a base de dados de aplicação para uma instância do servidor MySQL do Azure com o MySQL Workbench. | Disponível
[Artigo 9: Refatorizar uma aplicação para uma base de dados de aplicação Web do Azure e SQL do Azure](contoso-migration-refactor-web-app-sql.md) | Demonstra como Contoso migra a aplicação de SmartHotel para uma aplicação Web do Azure e migra a base de dados de aplicação para a instância de servidor SQL do Azure | Disponível
[Artigo 10: Refatorizar uma aplicação do Linux para aplicações Web do Azure e MySQL do Azure](contoso-migration-refactor-linux-app-service-mysql.md) | Mostra como a Contoso migra a aplicação de osTicket do Linux para aplicações Web do Azure em vários sites, integrados com o GitHub para a entrega contínua. Estes migrar a base de dados de aplicação para uma instância do MySQL do Azure. | Disponível
Artigo 11: Rearquitetar uma aplicação para contentores do Azure e base de dados SQL | Mostra como Contoso migra e rearchitects a respetiva aplicação SmartHotel para o Azure. Eles rearquitetar a camada de web de aplicação como um contentor do Windows e a base de dados de aplicação numa base de dados SQL do Azure. | Este artigo.

Neste artigo, a Contoso migra o Windows de duas camadas. Aplicação de NET SmartHotel em execução em VMs de VMware para o Azure. Se gostaria de utilizar esta aplicação, são fornecidas como código-fonte aberto e pode baixá-lo partir [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>Fatores comerciais

A equipe de liderança de TI tem trabalhado em estreita colaboração com seus parceiros de negócios para compreender o que quer alcançar com esta migração:

- **Resolver o crescimento do negócio**: Contoso está a crescer e assim há pressão em seus sistemas no local e a infraestrutura.
- **Aumentar a eficiência**: a Contoso precisa de remover procedimentos desnecessários e simplificar processos para desenvolvedores e usuários.  Necessidades de negócio IT velocidade e não desperdício de tempo ou dinheiro, entregando assim mais rapidamente sobre os requisitos dos clientes.
- **Aumente a agilidade**: Contoso TI tem de ser mais reativos para com as necessidades do negócio. Tem de ser capaz de reagir mais rapidamente do que as alterações no marketplace, para ativar o sucesso numa economia global.  Ele não pode obter na forma como ou se tornar um bloqueador de negócios.
- **Dimensionamento**: à medida que a empresa cresce com êxito, TI da Contoso tem de fornecer sistemas que são capazes de aumentar ao ritmo mesmo.
- **Os custos**: Contoso, além de minimizar os custos de licenciamento.

## <a name="migration-goals"></a>Objetivos de migração

A equipe de cloud de Contoso tiver afixado para baixo de objetivos para essa migração. Essas metas foram utilizadas para determinar o melhor método de migração.

**Objetivos** | **Detalhes**
--- | --- 
**Reqs de aplicação** | A aplicação no Azure permanecerá como crítica, pois é hoje mesmo.<br/><br/> Ele deve ter as mesmas capacidades de desempenho, como o faz atualmente no VMWare.<br/><br/> Pretende parar a dar suporte a Windows Server 2008 R2, no qual a aplicação atualmente é executado e está disposto a investir na aplicação.<br/><br/> Pretende mover para fora do SQL Server 2008 R2 para uma plataforma de base de dados PaaS modernos, o que irá minimizar a necessidade de gerenciamento.<br/><br/> Contoso pretende tirar partido dos seus investimentos em licenciamento do SQL Server e o Software Assurance, sempre que possível.<br/><br/> Eles querem ser capazes de aumentar verticalmente a camada de web de aplicação.
**Limitações** | A aplicação é constituída por uma aplicação ASP.NET e um serviço WCF em execução na mesma VM. Pretende dividir isso em duas aplicações web com o serviço de aplicações do Azure. 
**Reqs do Azure** | Pretende mover a aplicação para o Azure e executá-lo num contêiner para estender a vida da aplicação. Não querem iniciar totalmente do zero para implementar a aplicação no Azure. 

## <a name="solution-design"></a>Design da solução

Depois de fixar-se para baixo de seus objetivos e requisitos, a Contoso projeta e rever uma solução de implantação e identifica o processo de migração, incluindo os serviços do Azure que eles usarão para a migração.

### <a name="current-app"></a>Aplicação atual

- A aplicação no local de SmartHotel é em camadas em duas VMs (WEBVM e SQLVM).
- As VMs estão localizadas no anfitrião ESXi do VMware **contosohost1.contoso.com** (versão 6.5)
- O ambiente do VMware é gerido pelo vCenter Server 6.5 (**vcenter.contoso.com**), em execução numa VM.
- A Contoso tiver um datacenter no local (contoso-datacenter), com um controlador de domínio no local (**contosodc1**).
- As VMs no local no Centro de dados Contoso serão encerradas depois de terminar a migração.


### <a name="proposed-architecture"></a>Arquitetura proposta

- Para o escalão de base de dados da aplicação, Contoso em comparação com a base de dados do Azure SQL com o SQL Server a utilizar [este artigo](https://docs.microsoft.com/azure/sql-database/sql-database-features). Eles decidiram a escolher a base de dados do Azure SQL por diversos motivos:
    - Base de dados SQL do Azure é um serviço gerido da base de dados relacional. Ele proporciona um desempenho previsível em vários níveis de serviço, com quase sem administração. As vantagens incluem escalabilidade dinâmica sem tempo de inatividade, a otimização inteligente incorporada e a escalabilidade global e a disponibilidade.
    - Eles podem aproveitar o lightweight dados Assistente de migração (DMA) para avaliar e migrar a base de dados no local para o SQL do Azure.
    - Com o Software Assurance, eles podem trocar suas licenças existentes para as tarifas com desconto num banco de dados do SQL, com o Azure Hybrid Benefit para o SQL Server. Isso poderia fornecer redução de até 30%.
    - Base de dados SQL fornece uma série de recursos de segurança, incluindo a máscara de dados sempre encriptado, dinâmico e deteção de ameaça e segurança ao nível da linha.
- Para a camada web app, decidiu convertê-lo para o contentor do Windows com o Visual Studio.
    - Irá implementar a aplicação com o Azure Service Fabric e extraia a imagem de contentor do Windows do Azure Container Registry (ACR).
    - Um protótipo de extensão da aplicação para incluir a análise de sentimentos irá ser implementado como outro serviço no Service Fabric, ligado ao Cosmos DB.  Isto irá ler as informações de Tweets e apresentar na aplicação.

    ![Arquitetura do cenário](./media/contoso-migration-rearchitect-container-sql/architecture.png) 

  
### <a name="solution-review"></a>Revisão de solução
Contoso avalia o design proposto ao juntar-se de uma lista de prós e contras.

**Consideração** | **Detalhes**
--- | ---
**Profissionais de TI** | O código da aplicação SmartHotel precisará ser alterado para migração para o Azure Service Fabric. No entanto, o esforço é mínimo, com as ferramentas do SDK do Service Fabric para as alterações.<br/><br/> Com a mudança para o Service Fabric, eles podem começar a desenvolver microsserviços para adicionar à aplicação rapidamente ao longo do tempo, sem riscos para a base de código original.<br/><br/> Contentores do Windows oferecem os mesmos benefícios do contentores em geral. Eles aumentam a agilidade, portabilidade e controle.<br/><br/> Podem tirar partido de seus investimentos em Software Assurance com o Azure Hybrid Benefit para o SQL Server e o Windows Server.<br/><br/> Após a migração já não precisam de suporte do Windows Server 2008 R2. [Saiba mais](https://support.microsoft.com/lifecycle).<br/><br/> Pode configurar a camada web da aplicação com várias instâncias, para que ele deixou de ser um ponto único de falha.<br/><br/> Já não vai ser dependentes da SQL Server 2008 R2 de classificação por vencimento.<br/><br/> Base de dados SQL oferece suporte a requisitos técnicos da Contoso. Eles avaliado à base de dados no local com o Assistente de migração de base de dados e que é compatível.<br/><br/> Base de dados SQL tem tolerância a falhas incorporada que Contoso não precisa de configurar. Isto garante que a camada de dados já não é um ponto único de ativação pós-falha.
**Contras** | Os contentores são mais complexos do que outras opções de migração. A curva de aprendizado em contentores pode ser um problema para a Contoso.  Eles introduzem um novo nível de complexidade que fornece muito valor apesar da curva.<br/><br/> A equipe de operações na Contoso terá a incrementação para compreender e suporte Azure, contentores e microsserviços para a aplicação.<br/><br/> Se utilizarem o Assistente de migração de dados em vez de serviço de migração de dados para migrar a base de dados, a Contoso não terá a infraestrutura pronta para migrar bases de dados em escala.



### <a name="migration-process"></a>Processo de migração

1. Contoso Aprovisione o cluster do Azure service fabric para Windows.
2. Aprovisionar uma instância de SQL do Azure e migrar a base de dados SmartHotel a ele.
3. A VM de escalão Web se converterem para um contentor de Docker usando as ferramentas do SDK do Service Fabric.
4. Ligar o cluster do service fabric e o ACR e implementar a aplicação utilizar o Azure service fabric.

    ![Processo de migração](./media/contoso-migration-rearchitect-container-sql/migration-process.png) 

### <a name="azure-services"></a>Serviços do Azure

**Serviço** | **Descrição** | **Custo**
--- | --- | ---
[Assistente de migração de base de dados (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Eles usarão o DMA para avaliar e detetar problemas de compatibilidade que podem afetar sua funcionalidade de base de dados no Azure. O DMA avalia a paridade de funcionalidades entre o SQL origens e destinos e recomenda melhorias de desempenho e confiabilidade. | É uma ferramenta transferível gratuita.
[Base de Dados SQL do Azure](https://azure.microsoft.com/services/sql-database/) | Um serviço de base de dados da cloud relacional completamente gerido inteligente. | Custo com base nas funcionalidades, débito e tamanho. [Saiba mais](https://azure.microsoft.com/pricing/details/sql-database/managed/).
[Registo de contentor do Azure](https://azure.microsoft.com/services/container-registry/) | Store imagens de todos os tipos de implementações de contentor. | Custo com base em recursos, armazenamento e a duração de utilização. [Saiba mais](https://azure.microsoft.com/pricing/details/container-registry/).
[Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Desenvolvemos e utilizamos sempre ativa, aplicações dimensionáveis e distribuídas | Encargos com base no tamanho, a localização e a duração de nós de computação. [Saiba mais](https://azure.microsoft.com/pricing/details/service-fabric/).

## <a name="prerequisites"></a>Pré-requisitos

Eis o que (e Contoso) precisam executar este cenário:

**Requisitos** | **Detalhes**
--- | ---
**Subscrição do Azure** | Deve já tiver criado uma subscrição quando realizada a avaliação no primeiro artigo desta série. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Se criar uma conta gratuita, será o administrador da sua subscrição e poderá executar todas as ações.<br/><br/> Se utilizar uma subscrição existente e não for o administrador, terá de trabalhar com o administrador para lhe atribuir permissões de proprietário ou contribuinte.
**Infraestrutura do Azure** | [Saiba como](contoso-migration-infrastructure.md) Contoso configurar uma infraestrutura do Azure.
**Pré-requisitos do desenvolvedor** | A Contoso precisa de ferramentas a seguir numa estação de trabalho do desenvolvedor:<br/><br/> - [Visual Studio 2017 Community Edition: Versão 15.5](https://www.visualstudio.com/)<br/><br/> -Carga de trabalho .NET ativada.<br/><br/> - [Git](https://git-scm.com/)<br/><br/> - [SDK do Service Fabric v 3.0 ou posterior](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)<br/><br/> - [Docker CE (Windows 10) ou o Docker EE (Windows Server)](https://docs.docker.com/docker-for-windows/install/) definido para utilizar contentores do Windows.



## <a name="scenario-steps"></a>Passos do cenário

Eis como Contoso será executada a migração:

> [!div class="checklist"]
> * **Passo 1: Aprovisionar uma instância de base de dados SQL no Azure**: Contoso Aprovisiona uma instância do SQL no Azure. Depois da VM é migrada para um contentor do Azure web de front-end, a instância de contentor com o front-end da web de aplicação irá apontar para esta base de dados.
> * **Passo 2: Aprovisionar do Azure Service Fabric**: eles aprovisionam um Cluster do Service Fabric.
> * **Passo 4: Migrar a base de dados com o DMA**: estes migrar a base de dados de aplicação com o Assistente de migração de base de dados.
> * **Passo 5: Converta a aplicação para um contentor**: eles convertam a aplicação para um contentor com o Visual Studio e ferramentas do SDK.
> * **Passo 6: Publicar a aplicação**: eles publicam a aplicação para o ACR e o Cluster do Service Fabric.
> * **Passo 7: Expandir a aplicação**: depois da aplicação é pública, eles estendem-lo a tirar partido das capacidades do Azure e publicá-la novamente para o Azure.



## <a name="step-1-provision-an-azure-sql-database"></a>Passo 1: Aprovisionar uma base de dados SQL do Azure

1. Eles selecionam para criar uma base de dados SQL no Azure. 

    ![Aprovisionar SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql1.png)

2. Especifica um nome de base de dados de acordo com a base de dados em execução na VM no local (**SmartHotel.Registration**). A base de dados são colocados no grupo de recursos de ContosoRG. Este é o grupo de recursos que utilizam recursos de produção no Azure.

    ![Aprovisionar SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql2.png)

3. Eles configurar uma nova instância do SQL Server (**smarthotel-sql-eus2**) na região primária.

    ![Aprovisionar SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql3.png)

4. Eles definir o escalão de preço para corresponder ao seu servidor e base de dados precisa. E devem selecionar para poupar dinheiro com o benefício híbrido do Azure, uma vez que já têm uma licença do SQL Server.
5. Para o dimensionamento utilizam v Core-com base em comprar e defina os limites para as suas necessidades esperados.

    ![Aprovisionar SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql4.png)

6. Em seguida, é criar a instância de base de dados.

    ![Aprovisionar SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql5.png)

7. Depois da instância é criada, eles abrir a base de dados e anote os detalhes que necessitam, quando utilizarem a assistência de migração de base de dados para a migração.

    ![Aprovisionar SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql6.png)


**Precisa de mais ajuda?**

- [Obtenha ajuda](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) aprovisionar uma base de dados SQL.
- [Saiba mais sobre](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools) limites de recursos de núcleos.



## <a name="step-2-create-an-acr-and-provision-an-azure-container"></a>Passo 2: Criar um ACR e aprovisionar um contentor do Azure

O contentor do Azure é criado com os ficheiros exportados a partir da VM de Web. O contentor está hospedado no Azure Container Registry (ACR).


1. Contoso cria um registo de contentor no portal do Azure.

     ![Registo de Contentor](./media/contoso-migration-rearchitect-container-sql/container-registry1.png)

2. Eles fornecem um nome para o registo (**contosoacreus2**) e coloque-o na região primária, no grupo de recursos que utilizam para seus recursos de infraestrutura. Ativar o acesso para os utilizadores administradores e defini-lo como um SKU premium, de modo a que podem tirar partido de georreplicação.

    ![Registo de Contentor](./media/contoso-migration-rearchitect-container-sql/container-registry2.png)  


## <a name="step-3-provision-azure-service-fabric"></a>Passo 3: Aprovisionar o Azure Service Fabric

O contentor de SmartHotel será executado no Sluster de recursos de infraestrutura de serviço do Azure. Contoso cria o Cluster do Service Fabric da seguinte forma:

1. Criar um recurso de infraestrutura do serviço do Azure Marketplace

     ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric1.png)

2. Na **básica**, fornecem um nome exclusivo de DS para o cluster e credenciais para aceder a VM no local. Eles colocam o recurso no grupo de recursos de produção (**ContosoRG**) na região E.U.A. Leste 2 primária.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric2.png) 

3. Na **configuração do tipo de nó**, eles um nome de tipo de nó, as definições de durabilidade, tamanho da VM e pontos finais da aplicação de entrada.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric3.png) 


4. Na **criar Cofre de chaves**, ser criar um novo cofre de chaves no seu grupo de recursos de infraestrutura, para abrigar o certificado.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric4.png) 


5. Na **políticas de acesso** eles eanble acesso para máquinas virtuais a implementar o Cofre de chaves.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric5.png) 

6. Especifica um nome para o certificado.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric6.png) 

7. Na página Resumo, eles copie a ligação que é utilizada para transferir o certificado. Eles precisam, esta opção para ligar ao Cluster do Service Fabric.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric7.png) 

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric8.png) 

8. Depois de passar a validação, eles aprovisionam o cluster.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric9.png) 

9. No Assistente para importar certificados, eles importam o certificado transferido para máquinas de desenvolvimento. O certificado é utilizado para autenticar para o cluster.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric10.png) 

10. Depois do cluster for aprovisionado, se ligam para o Cluster do Service Fabric Explorer.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric11.png) 

11. Terá de selecionar o certificado correto.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric12.png) 

12. As cargas do Service Fabric Explorer e o administrador da Contoso, podem gerir o cluster.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric13.png) 


## <a name="step-3-migrate-the-database-with-dma"></a>Passo 3: Migrar a base de dados com o DMA

Contoso irá migrar a base de dados de SmartHotel com o DMA.

### <a name="install-dma"></a>Instalar o DMA

1. Eles transferirem a ferramenta a partir da [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) para a VM do SQL Server no local (**SQLVM**).
2. Eles executam a configuração (Downloadmigrationassistant) na VM.
3. Sobre o **concluir** página, selecione **iniciar Microsoft Data Migration Assistant** antes de concluir o assistente.

### <a name="configure-the-firewall"></a>Configurar a firewall

Para ligar à base de dados SQL do Azure, é necessária uma regra de firewall.

1. Na **firewalls e redes virtuais** propriedades para a base de dados, que permite o acesso aos serviços do Azure e adicionar uma regra para o endereço IP do cliente da VM do SQL Server no local.
2. É criada uma regra de firewall ao nível do servidor.

    ![Firewall](./media/contoso-migration-rearchitect-container-sql/sql-firewall.png)

Precisa de mais ajuda?

[Saiba mais sobre](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure#creating-and-managing-firewall-rules) criar e gerir regras de firewall para a base de dados do Azure SQL.

### <a name="migrate"></a>Migrar

1. O DMA criar um novo projeto (**SmartHotelDB**) e selecione **migração** 
2. Selecionar o tipo de servidor de origem como **SQL Server**e o destino como **base de dados do Azure SQL**. 

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-1.png)

3. Nos detalhes de migração, adicione **SQLVM** que o servidor de origem e o **SmartHotel.Registration** base de dados. 

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-2.png)

4. Eles recebem um erro que parece estar associado a autenticação. No entanto depois de investigar, o problema é o ponto final (.) no nome da base de dados. Como solução, eles decidem se pretendem aprovisionar uma base de dados do SQL nova com o nome **SmartHotel-registo**, para resolver o problema. Quando executam DMA novamente, eles podem selecionar **SmartHotel-registo**e continuar com o assistente.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-3.png)

5. Na **selecionar objetos**, selecione as tabelas de base de dados e gerar um script SQL.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-4.png)

6. Depois do DMS cria o script, pode clicar em **implementar esquema**.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-5.png)

7. DMA confirma que a implementação foi concluída com êxito.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-6.png)

8. Agora que iniciar a migração.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-7.png)

9. Depois de concluída a migração, a Contoso pode verificar que a base de dados está em execução na instância do SQL do Azure.

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-8.png)

10. Se eliminar a base de dados do SQL extra **SmartHotel.Registration** no portal do Azure.

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-9.png)



## <a name="step-4-convert-the-app-to-a-container"></a>Passo 4: Converta a aplicação para um contentor

A aplicação no local é uma aplicação de três camadas tradicional:

- Ela contém WebForms e um serviço WCF a ligar ao SQL Server.
- Ele usa o Entity Framework para integrar com os dados na base de dados SQL, expondo-as através de um serviço WCF.
- O aplicativo de WebForms interage com o serviço do WCF.

Contoso irá converter a aplicação para um contentor utilizando o Visual Studio e as ferramentas do SDK, da seguinte forma:

1. Eles clonagem o repositório localmente a uma máquina de programador:

    **clone de Git https://github.com/Microsoft/SmartHotel360-internal-booking-apps.git**

    ![Contentor](./media/contoso-migration-rearchitect-container-sql/container1.png)

2. Com o Visual Studio, abrem o ficheiro de solução (SmartHotel.Registration.sln) no **SmartHotel360-interna-reservas-apps\src\Registration** diretório do repositório local.  São apresentadas duas aplicações. O front-end web SmartHotel.Registration.Web nad a aplicação de serviço do WCF SmartHotel.Registration.WCF.

    ![Contentor](./media/contoso-migration-rearchitect-container-sql/container2.png)


3. Eles com o botão direito na aplicação web > **Add** > **suporte a contentores do Orchestrator**.
4. Na **adicionar suporte para contentores orquestra**, eles selecionam **Service Fabric**.

    ![Contentor](./media/contoso-migration-rearchitect-container-sql/container3.png)

5. Contoso repete o processo para a aplicação de SmartHotel.Registration.WCF.
6. Agora, a Contoso verifica como a solução foi alterada.

    - A nova aplicação é **SmartHotel.RegistrationApplication/**
    - Ele contém dois serviços: **SmartHotel.Registration.WCF** e **SmartHotel.Registration.Web**.

    ![Contentor](./media/contoso-migration-rearchitect-container-sql/container4.png)

7. Visual Studio criou o ficheiro de Docker e obtida as imagens necessárias localmente na máquina do desenvolvedor.

    ![Contentor](./media/contoso-migration-rearchitect-container-sql/container5.png)

8. Um arquivo de manifesto (**servicemanifest. XML**) é criado e aberto pelo Visual Studio. Este ficheiro diz ao Service Fabric como configurar o contentor quando for implementada para o Azure.

    ![Contentor](./media/contoso-migration-rearchitect-container-sql/container6.png)

9. Outro ficheiro de manifesto (* * applicationmanifest. xml) contém os aplicativos de configuração para os contentores.

    ![Contentor](./media/contoso-migration-rearchitect-container-sql/container7.png)

## <a name="step-5-publish-the-app"></a>Passo 5: Publicar a aplicação


Por fim, a Contoso pode publicar a aplicação para o ACR e o Cluster do Service Fabric.

> [!NOTE]
> Algumas alterações relacionadas com o cluster do Service Fabric foram feitas na aplicação SmartHotel. Pode baixar os dois o código de aplicação original e reestruturação da [GitHub](https://github.com/Microsoft/SmartHotel360-internal-booking-apps). É o arquivo alterado **AppliationModern/ApplicationParameters/Cloud.xml**.


1. No Visual Studio, a atualização da cadeia de ligação para ligar a aplicação para a base de dados do SQL do Azure. A cadeia de ligação pode ser encontrada na base de dados no portal do Azure.

    ![Publicar](./media/contoso-migration-rearchitect-container-sql/publish1.png)

2. Contoso publica a aplicação no Service Fabric com o Visual Studio. Eles com o botão direito na aplicação de Service Fabric > **publicar**.

    ![Publicar](./media/contoso-migration-rearchitect-container-sql/publish2.png)

3. Selecionar a subscrição, o ponto final de ligação e o ACR. Em seguida, clique em **publicar**.

    ![Publicar](./media/contoso-migration-rearchitect-container-sql/publish3.png)

4. Após a conclusão da implementação, SmartHotel agora executará o Service Fabric.

    ![Publicar](./media/contoso-migration-rearchitect-container-sql/publish4.png)

5. Para ligar à aplicação, o Contoso direciona o tráfego para o endereço IP público do Balanceador de carga do Azure no fron de nós do Service Fabric.

    ![Publicar](./media/contoso-migration-rearchitect-container-sql/publish5.png)

## <a name="step-6-extend-the-app-and-republish"></a>Passo 6: Expandir a aplicação e voltar a publicar

Após a aplicação de SmartHotel e a base de dados estão em execução no Azure, a Contoso pretende expandir as aplicações.

- Os desenvolvedores da Contoso estão criando um protótipo de um novo aplicativo do .NET Core que será executada no cluster do Service Fabric.
- A aplicação será utilizada para extrair dados de sentimento de cosmos DB.
- Estes dados serão sob a forma de Tweets que são processados utilizando uma função do Azure sem servidor e a API de análise de texto dos serviços cognitivos.

### <a name="provision-azure-cosmos-db"></a>Aprovisionar o Azure Cosmos DB

Como primeiro passo, a Contoso aprovisionar uma base de dados do Cosmos do Azure.

1. Se criaram um recurso do Azure Cosmos DB no Azure Marketplace.

    ![Expansão](./media/contoso-migration-rearchitect-container-sql/extend1.png)

2. Eles fornecem um nome de base de dados (**contososmarthotel**), selecione a API de SQL e colocar o recurso no grupo de recursos de produção, na região E.U.A. Leste 2 primária.

    ![Expansão](./media/contoso-migration-rearchitect-container-sql/extend2.png)

3. Na **introdução**, eles selecionam **Data Explorer**e adicione uma nova coleção.
4. Na **adicionar coleção** fornecem IDs e definir a capacidade de armazenamento e débito.

    ![Expansão](./media/contoso-migration-rearchitect-container-sql/extend3.png)

5. No portal, que abrem a nova base de dados > **recolha** > **documentos** e clique em **novo documento**.
6. Eles cole o seguinte código JSON para a janela de documento. Estes são dados de exemplo na forma de um único tweet.

    ```
    {
            "id": "2ed5e734-8034-bf3a-ac85-705b7713d911",
            "tweetId": 927750234331580911,
            "tweetUrl": "https://twitter.com/status/927750237331580911",
            "userName": "CoreySandersWA",
            "userAlias": "@CoreySandersWA",
            "userPictureUrl": "",
            "text": "This is a tweet about #SmartHotel",
            "language": "en",
            "sentiment": 0.5,
            "retweet_count": 1,
            "followers": 500, 
            "hashtags": [
                ""
            ]
    }
    ```

    ![Expansão](./media/contoso-migration-rearchitect-container-sql/extend4.png)

7. Eles localizam o ponto de final do Cosmos DB e a chave de autenticação. Estes são utilizados na aplicação para ligar à coleção. Na base de dados, clique **chaves**e copiar o URI e a chave primária para o bloco de notas.

    ![Expansão](./media/contoso-migration-rearchitect-container-sql/extend5.png)

### <a name="update-the-sentiment-app"></a>Atualizar a aplicação de sentimentos

Com o Cosmos DB aprovisionado, a Contoso pode configurar a aplicação para ligar à mesma.

1. No Visual Studio, abrir o ficheiro ApplicationModern\ApplicationParameters\cloud.xml no Explorador de soluções.

    ![Aplicação de sentimentos](./media/contoso-migration-rearchitect-container-sql/sentiment1.png)

2. Mostram os dois parâmetros seguintes:

   ```
   <Parameter Name="SentimentIntegration.CosmosDBEndpoint" Value="[URI]" />
   ```
   
   ```
   <Parameter Name="SentimentIntegration.CosmosDBAuthKey" Value="[Key]" />
   ```

    ![Aplicação de sentimentos](./media/contoso-migration-rearchitect-container-sql/sentiment2.png)

### <a name="republish-the-app"></a>Voltar a publicar a aplicação

Após expandir a aplicação, o Contoso republishes-lo para o Azure.

1. No portal, eles com o botão direito na aplicação do Service Fabric > **publicar**.

    ![Voltar a publicar](./media/contoso-migration-rearchitect-container-sql/republish1.png)

2. Selecionar a subscrição, o ponto final de ligação e o ACR. Em seguida, clique em **publicar**.

    ![Voltar a publicar](./media/contoso-migration-rearchitect-container-sql/republish2.png)

4. Após a conclusão da implementação, SmartHotel agora executará o Service Fabric. O console de gerenciamento de recursos de infraestrutura do serviço mostra agora três serviços.

    ![Voltar a publicar](./media/contoso-migration-rearchitect-container-sql/republish3.png)

5. Contoso pode clicar em através dos serviços que a aplicação de SentimentIntegration está em execução

    ![Voltar a publicar](./media/contoso-migration-rearchitect-container-sql/republish4.png)

## <a name="clean-up-after-migration"></a>Limpar após a migração

Após a migração, a Contoso precisa de concluir estes passos de limpeza:  

- Remova as VMs no local no inventário do vCenter.
- Remova as VMs das tarefas de cópia de segurança locais.
- Atualize a documentação interna para mostrar as novas localizações para a aplicação de SmartHotel. Mostre a base de dados como em execução na base de dados SQL do Azure e o front-end como em execução no Service Fabric.
- Reveja todos os recursos que interagem com as VMs desativadas e atualizar as definições relevantes ou documentação para refletir a nova configuração.


## <a name="review-the-deployment"></a>Reveja a implementação

Com os recursos migrados no Azure, a Contoso precisa totalmente operacionalizar e proteger sua infra-estrutura de novo.

### <a name="security"></a>Segurança

- Contoso precisa de garantir que seus novos **SmartHotel-registo** base de dados é segura. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- Em particular, eles devem atualizar o contentor para utilizar SSL com certificados.
- Eles devem considerar a utilização do Cofre de chaves para proteger segredos para as aplicações do Service Fabric. [Saiba mais](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="backups"></a>Cópias de segurança

- A Contoso precisa de analisar os requisitos de cópia de segurança da base de dados do SQL do Azure. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
- Deve considerar implementar grupos de ativação pós-falha para fornecer ativação pós-falha para a base de dados. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).
- Podem tirar partido de georreplicação para a SKU de premium do ACR. [Saiba mais](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication).
- Contoso tem de considerar implementar a aplicação Web na região E.U.A. Central e principal E.U.A. Leste 2 quando a aplicação Web para contentores for disponibilizada. Pode configurar o Gestor de tráfego para garantir que a ativação pós-falha em caso de falhas regionais.

### <a name="licensing-and-cost-optimization"></a>Otimização de licenciamento e de custo

- Depois de todos os recursos são implementados, Contoso deve atribuir etiquetas do Azure com base em seus [planejamento da infraestrutura](contoso-migration-infrastructure.md#set-up-tagging).
- Licenciamento de todos os baseia-se para o custo dos serviços PaaS que Contoso está a consumir. Isso é deduzido do EA.
1. Contoso irá ativar o Azure Cost Management licenciado pela Cloudyn, uma subsidiária da Microsoft. É uma solução de gestão de custos multiclouds que o ajuda a utilizar e gerir o Azure e outros recursos da cloud.  [Saiba mais](https://docs.microsoft.com/azure/cost-management/overview) sobre o Azure Cost Management. 

## <a name="conclusion"></a>Conclusão

Neste artigo, a Contoso refatorado a aplicação de SmartHotel no Azure ao migrar o VM da front-end de aplicação no Service Fabric. A base de dados de aplicação sua migração para uma base de dados SQL do Azure.





