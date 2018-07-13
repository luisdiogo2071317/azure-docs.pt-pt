---
title: Refatorizar uma aplicação da Contoso ao migrar para o grupo de disponibilidade de VMs do Azure e SQL Server AlwaysOn | Documentos da Microsoft
description: Saiba como Contoso realojar a uma aplicação no local ao migrar para um aplicação Web de contentor do Azure e uma base de dados do servidor SQL do Azure.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: f4a348815ef058cb795ed12e8f118b494650a555
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39005195"
---
# <a name="contoso-migration-refactor-an-on-premises-app-to-an-azure-web-app-and-azure-sql-database"></a>Migração de Contoso: Refatorizar uma aplicação no local para uma base de dados de aplicação Web do Azure e SQL do Azure

Este artigo demonstra como Contoso revisasse, encontrasse aplicação seus SmartHotel no Azure. O front-end de aplicação VM estes migrar para uma aplicação Web do Azure e a base de dados de aplicação para uma base de dados SQL do Azure.

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
Artigo 9: Refatorizar uma aplicação para uma base de dados de aplicação Web do Azure e SQL do Azure | Demonstra como Contoso migra a aplicação de SmartHotel para uma aplicação Web do Azure e migra a base de dados de aplicação para a instância de servidor SQL do Azure | Este artigo
[Artigo 10: Refatorizar uma aplicação do Linux para aplicações Web do Azure e MySQL do Azure](contoso-migration-refactor-linux-app-service-mysql.md) | Mostra como a Contoso migra a aplicação de osTicket do Linux para aplicações Web do Azure em vários sites, integrados com o GitHub para a entrega contínua. Estes migrar a base de dados de aplicação para uma instância do MySQL do Azure. | Disponível
[Artigo 11: Refatorar o TFS no VSTS](contoso-migration-tfs-vsts.md) | Mostra como a Contoso migra a implantação do Team Foundation Server (TFS) no local ao migrá-lo para Visual Studio Team Services (VSTS) no Azure. | Disponível
[Artigo 12: Rearquitetar uma aplicação em contentores do Azure e base de dados do Azure SQL](contoso-migration-rearchitect-container-sql.md) | Mostra como Contoso migra e rearchitects a respetiva aplicação SmartHotel para o Azure. Eles rearquitetar a camada de web de aplicação como um contentor do Windows e a base de dados de aplicação numa base de dados SQL do Azure. | Disponível
[Artigo 13: Recriar uma aplicação no Azure](contoso-migration-rebuild.md) | Mostra como Contoso reconstruir a sua aplicação de SmartHotel com uma gama de capacidades do Azure e serviços, incluindo serviços de aplicações, Kubernetes do Azure, as funções do Azure, serviços cognitivos e Cosmos DB. | Disponível


Neste artigo, a Contoso migra o Windows de duas camadas. Aplicação de NET SmartHotel em execução em VMs de VMware para o Azure. Se gostaria de utilizar esta aplicação, são fornecidas como código-fonte aberto e pode baixá-lo partir [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>Fatores comerciais

A equipe de liderança de TI tem trabalhado em estreita colaboração com seus parceiros de negócios para compreender o que quer alcançar com esta migração:

- **Resolver o crescimento do negócio**: Contoso está a crescer e há pressão em seus sistemas no local e a infraestrutura.
- **Aumentar a eficiência**: a Contoso precisa de remover procedimentos desnecessários e simplificar processos para desenvolvedores e usuários.  Necessidades de negócio IT velocidade e não desperdício de tempo ou dinheiro, entregando assim mais rapidamente sobre os requisitos dos clientes.
- **Aumente a agilidade**: Contoso TI tem de ser mais reativos para com as necessidades do negócio. Tem de ser capaz de reagir mais rapidamente do que as alterações no marketplace, para ativar o sucesso numa economia global.  Ele não pode obter na forma como ou se tornar um bloqueador de negócios.
- **Dimensionamento**: à medida que a empresa cresce com êxito, TI da Contoso tem de fornecer sistemas que são capazes de aumentar ao ritmo mesmo.
- **Os custos**: Contoso, além de minimizar os custos de licenciamento.

## <a name="migration-goals"></a>Objetivos de migração

A equipe de cloud de Contoso tiver afixado para baixo de objetivos para essa migração. Essas metas foram utilizadas para determinar o melhor método de migração.

**Requisitos** | **Detalhes**
--- | --- 
**Aplicação** | A aplicação no Azure permanecerá como crítica, pois é hoje mesmo.<br/><br/> Ele deve ter as mesmas capacidades de desempenho, como o faz atualmente no VMWare.<br/><br/> Não querem investir na aplicação. Por agora, eles simplesmente querem movê-la em segurança para a cloud.<br/><br/> Pretende parar a dar suporte a Windows Server 2008 R2, no qual a aplicação é atualmente executado.<br/><br/> Pretende mover para fora do SQL Server 2008 R2 para uma plataforma de base de dados PaaS modernos, o que irá minimizar a necessidade de gerenciamento.<br/><br/> Contoso pretende tirar partido dos seus investimentos em licenciamento do SQL Server e o Software Assurance, sempre que possível.<br/><br/> Além disso, Contoso deseja atenuar o ponto único de falha na camada web.
**Limitações** | A aplicação é constituída por uma aplicação ASP.NET e um serviço WCF em execução na mesma VM. Pretende dividir isso em duas aplicações web com o serviço de aplicações do Azure. 
**Azure** | Pretende mover a aplicação para o Azure, mas não quer executá-lo em VMs. Eles querem tirar partido dos serviços de PaaS do Azure para os escalões web e de dados. 

## <a name="solution-design"></a>Design da solução

Depois de fixar-se para baixo de seus objetivos e requisitos, a Contoso projeta e rever uma solução de implantação e identifica o processo de migração, incluindo os serviços do Azure que eles usarão para a migração.

### <a name="current-app"></a>Aplicação atual

- A aplicação no local de SmartHotel é em camadas em duas VMs (WEBVM e SQLVM).
- As VMs estão localizadas no anfitrião ESXi do VMware **contosohost1.contoso.com** (versão 6.5)
- O ambiente do VMware é gerido pelo vCenter Server 6.5 (**vcenter.contoso.com**), em execução numa VM.
- A Contoso tiver um datacenter no local (contoso-datacenter), com um controlador de domínio no local (**contosodc1**).
- As VMs no local no Centro de dados Contoso serão encerradas depois de terminar a migração.


### <a name="proposed-solution"></a>Solução proposta

- Para o escalão de base de dados da aplicação, Contoso em comparação com a base de dados do Azure SQL com o SQL Server a utilizar [este artigo](https://docs.microsoft.com/azure/sql-database/sql-database-features). Eles decidiram a escolher a base de dados do Azure SQL por diversos motivos:
    - Base de dados SQL do Azure é um serviço gerido da base de dados relacional. Ele proporciona um desempenho previsível em vários níveis de serviço, com quase sem administração. As vantagens incluem escalabilidade dinâmica sem tempo de inatividade, a otimização inteligente incorporada e a escalabilidade global e a disponibilidade.
    - Eles podem aproveitar o lightweight dados Assistente de migração (DMA) para avaliar e migrar a base de dados no local para o SQL do Azure.
    - Com o Software Assurance, eles podem trocar suas licenças existentes para as tarifas com desconto num banco de dados do SQL, com o Azure Hybrid Benefit para o SQL Server. Isso poderia fornecer redução de até 30%.
    - Base de dados SQL fornece uma série de recursos de segurança, incluindo a máscara de dados sempre encriptado, dinâmico e deteção de ameaça e segurança ao nível da linha.
- Para a camada de web de aplicação, se tem optado por utilizar o App Service do Azure. Este serviço de PaaS permite que implementar a aplicação com apenas algumas alterações de configuração. Irá utilizar o Visual Studio para fazer a alteração e implementar duas aplicações web. Um para o Web site e outro para o serviço do WCF.
  
### <a name="solution-review"></a>Revisão de solução
Contoso avalia o design proposto ao juntar-se de uma lista de prós e contras.

**Consideração** | **Detalhes**
--- | ---
**Profissionais de TI** | O código da aplicação SmartHotel não terá de ser alterado para a migração para o Azure.<br/><br/> Podem tirar partido de seus investimentos em Software Assurance com o Azure Hybrid Benefit para o SQL Server e o Windows Server.<br/><br/> Após a migração já não precisam de suporte do Windows Server 2008 R2. [Saiba mais](https://support.microsoft.com/lifecycle).<br/><br/> Pode configurar a camada web da aplicação com várias instâncias, para que ele deixou de ser um ponto único de falha.<br/><br/> Já não vai ser dependentes da SQL Server 2008 R2 de classificação por vencimento.<br/><br/> Base de dados SQL oferece suporte a requisitos técnicos da Contoso. Eles avaliado à base de dados no local com o Assistente de migração de base de dados e que é compatível.<br/><br/> Base de dados SQL tem tolerância a falhas incorporada que Contoso não precisa de configurar. Isto garante que a camada de dados já não é um ponto único de ativação pós-falha.
**Contras** | Serviços de aplicações do Azure só suporta uma implementação de aplicações para cada aplicação Web. Isso significa que duas aplicações Web tem de ser aprovisionada (uma para o Web site) e outra para o serviço do WCF.<br/><br/> Se utilizarem o Assistente de migração de dados em vez de serviço de migração de dados para migrar a base de dados, a Contoso não terá a infraestrutura pronta para migrar bases de dados em escala. Precisam de criar outra região para se certificar de ativação pós-falha se a região primária estiver indisponível.

## <a name="proposed-architecture"></a>Arquitetura proposta

![Arquitetura do cenário](media/contoso-migration-refactor-web-app-sql/architecture.png) 


### <a name="migration-process"></a>Processo de migração

1. Aprovisionar uma instância de SQL do Azure Contoso e migrar a base de dados SmartHotel a ele.
2. Aprovisionar e configurar aplicações Web e implementar a aplicação de SmartHotel aos mesmos.

    ![Processo de migração](media/contoso-migration-refactor-web-app-sql/migration-process.png) 

### <a name="azure-services"></a>Serviços do Azure

**Serviço** | **Descrição** | **Custo**
--- | --- | ---
[Assistente de migração de base de dados (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Eles usarão o DMA para avaliar e detetar problemas de compatibilidade que podem afetar sua funcionalidade de base de dados no Azure. O DMA avalia a paridade de funcionalidades entre o SQL origens e destinos e recomenda melhorias de desempenho e confiabilidade. | É uma ferramenta transferível gratuita.
[Base de Dados SQL do Azure](https://azure.microsoft.com/services/sql-database/) | Um serviço de base de dados da cloud relacional completamente gerido inteligente. | Custo com base nas funcionalidades, débito e tamanho. [Saiba mais](https://azure.microsoft.com/pricing/details/sql-database/managed/).
[Serviços de aplicações do Azure - aplicações Web](https://docs.microsoft.com/azure/app-service/app-service-web-overview) | Criar poderosas aplicações na cloud com uma plataforma totalmente gerida | Custo com base na duração de tamanho, a localização e a utilização. [Saiba mais](https://azure.microsoft.com/pricing/details/app-service/windows/).

## <a name="prerequisites"></a>Pré-requisitos

Eis o que (e Contoso) precisam executar este cenário:

**Requisitos** | **Detalhes**
--- | ---
**Subscrição do Azure** | Deve já tiver criado uma subscrição quando realizada a avaliação no primeiro artigo desta série. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Se criar uma conta gratuita, será o administrador da sua subscrição e poderá executar todas as ações.<br/><br/> Se utilizar uma subscrição existente e não for o administrador, terá de trabalhar com o administrador para lhe atribuir permissões de proprietário ou contribuinte.
**Infraestrutura do Azure** | [Saiba como](contoso-migration-infrastructure.md) Contoso configurar uma infraestrutura do Azure.


## <a name="scenario-steps"></a>Passos do cenário

Eis como Contoso será executada a migração:

> [!div class="checklist"]
> * **Passo 1: Aprovisionar uma instância de base de dados SQL no Azure**: Contoso Aprovisiona uma instância do SQL no Azure. Depois do Web site da aplicação está a migrar para o Azure, vai apontar a aplicação de web do serviço WCF para esta instância.
> * **Passo 2: Migrar a base de dados com o DMA**: estes migrar a base de dados de aplicação com o Assistente de migração de base de dados.
> * **Passo 3: Aprovisionar Web Apps**: aprovisionam as duas aplicações web.
> * **Passo 4: Configurar as cadeias de ligação**: eles configurarem cadeias de ligação para que podem comunicar a aplicação de web de camada web, a aplicação de web do serviço WCF e a instância SQL.
> * **Passo 5: Publicar aplicações web**: como passo final, o Contoso publica as aplicações no Azure.


## <a name="step-1-provision-an-azure-sql-database"></a>Passo 1: Aprovisionar uma base de dados SQL do Azure

1. Eles selecionam para criar uma base de dados SQL no Azure. 

    ![Aprovisionar SQL](media/contoso-migration-refactor-web-app-sql/provision-sql1.png)

2. Especifica um nome de base de dados de acordo com a base de dados em execução na VM no local (**SmartHotel.Registration**). A base de dados são colocados no grupo de recursos de ContosoRG. Este é o grupo de recursos que utilizam recursos de produção no Azure.

    ![Aprovisionar SQL](media/contoso-migration-refactor-web-app-sql/provision-sql2.png)

3. Eles configurar uma nova instância do SQL Server (**smarthotel-sql-eus2**) na região primária.

    ![Aprovisionar SQL](media/contoso-migration-refactor-web-app-sql/provision-sql3.png)

4. Eles definir o escalão de preço para corresponder ao seu servidor e base de dados precisa. E devem selecionar para poupar dinheiro com o benefício híbrido do Azure, uma vez que já têm uma licença do SQL Server.
5. Para o dimensionamento utilizam v Core-com base em comprar e defina os limites para as suas necessidades esperados.

    ![Aprovisionar SQL](media/contoso-migration-refactor-web-app-sql/provision-sql4.png)

6. Em seguida, é criar a instância de base de dados.

    ![Aprovisionar SQL](media/contoso-migration-refactor-web-app-sql/provision-sql5.png)

7. Depois da instância é criada, eles abrir a base de dados e anote os detalhes que necessitam, quando utilizarem a assistência de migração de base de dados para a migração.

    ![Aprovisionar SQL](media/contoso-migration-refactor-web-app-sql/provision-sql6.png)


**Precisa de mais ajuda?**

- [Obtenha ajuda](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) aprovisionar uma base de dados SQL.
- [Saiba mais sobre](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools) limites de recursos de núcleos.


## <a name="step-2-migrate-the-database-with-dma"></a>Passo 2: Migrar a base de dados com o DMA

Contoso irá migrar a base de dados de SmartHotel com o DMA.

### <a name="install-dma"></a>Instalar o DMA

1. Eles transferirem a ferramenta a partir da [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) para a VM do SQL Server no local (**SQLVM**).
2. Eles executam a configuração (Downloadmigrationassistant) na VM.
3. Sobre o **concluir** página, selecione **iniciar Microsoft Data Migration Assistant** antes de concluir o assistente.

### <a name="migrate-the-database-with-dma"></a>Migrar a base de dados com o DMA

1. O DMA criar um novo projeto (**SmartHotelDB**) e selecione **migração** 
2. Selecionar o tipo de servidor de origem como **SQL Server**e o destino como **base de dados do Azure SQL**. 

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-1.png)

3. Nos detalhes de migração, adicione **SQLVM** que o servidor de origem e o **SmartHotel.Registration** base de dados. 

     ![DMA](media/contoso-migration-refactor-web-app-sql/dma-2.png)

4. Eles recebem um erro que parece estar associado a autenticação. No entanto depois de investigar, o problema é o ponto final (.) no nome da base de dados. Como solução, eles decidem se pretendem aprovisionar uma base de dados do SQL nova com o nome **SmartHotel-registo**, para resolver o problema. Quando executam DMA novamente, eles podem selecionar **SmartHotel-registo**e continuar com o assistente.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-3.png)

5. Na **selecionar objetos**, selecione as tabelas de base de dados e gerar um script SQL.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-4.png)

6. Depois do DMS cria o script, pode clicar em **implementar esquema**.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-5.png)

7. DMA confirma que a implementação foi concluída com êxito.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-6.png)

8. Agora que iniciar a migração.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-7.png)

9. Depois de concluída a migração, a Contoso pode verificar que a base de dados está em execução na instância do SQL do Azure.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-8.png)

10. Se eliminar a base de dados do SQL extra **SmartHotel.Registration** no portal do Azure.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-9.png)


## <a name="step-3-provision-web-apps"></a>Passo 3: Aprovisionar Web Apps

Com a base de dados migradas, aprovisionar agora podem de Contoso as duas aplicações web.

1. Eles selecionam **aplicação Web** no portal.

    ![Aplicação Web](media/contoso-migration-refactor-web-app-sql/web-app1.png)

2. Eles fornecem um nome de aplicação (**SHWEB EUS2**), executá-lo no Windows e colocá-lo a anular o grupo de recursos de produção **ContosoRG**. Eles criam um novo serviço de aplicações e o plano.

    ![Aplicação Web](media/contoso-migration-refactor-web-app-sql/web-app2.png)

3. Depois da aplicação web é aprovisionada, eles repetir o processo para criar uma aplicação web para o serviço do WCF (**SHWCF EUS2**)

    ![Aplicação Web](media/contoso-migration-refactor-web-app-sql/web-app3.png)

4. Depois de terminar, eles navegam para o endereço de aplicações para verificar que tenha sido criado com êxito.

## <a name="step-4-configure-connection-strings"></a>Passo 4: Configurar as cadeias de ligação

A Contoso precisa de certificar-se de que as aplicações web e base de dados pode comunicar. Para fazer isso, eles configurarem cadeias de ligação no código e nas aplicações web.

1. Na aplicação web para o serviço do WCF (**SHWCF EUS2**) > **definições** > **definições da aplicação**, adicionam uma nova cadeia de ligação com o nome  **DefaultConnection**.
2. A cadeia de ligação é obtida a partir da **SmartHotel-registo** de base de dados e deve ser atualizado com as credenciais corretas.

    ![Cadeia de ligação](media/contoso-migration-refactor-web-app-sql/string1.png)

3. Com o Visual Studio, a Contoso abre o arquivo da solução do **SmartHotel360--reservas-aplicações internas** pasta. O **connectionStrings** secção do ficheiro Web. config para o serviço WCF SmartHotel.Registration.Wcf devem ser atualizados com a cadeia de ligação.

     ![Cadeia de ligação](media/contoso-migration-refactor-web-app-sql/string2.png)

4. O **cliente** secção do ficheiro Web. config para o SmartHotel.Registration.Web deve ser alterada para apontar para a nova localização do serviço WCF. Este é o URL da aplicação web WCF que aloja o ponto final de serviço.

    ![Cadeia de ligação](media/contoso-migration-refactor-web-app-sql/strings3.png)


## <a name="step-5-publish-web-apps"></a>Passo 5: Publicar aplicações web

Como passo final, a Contoso publica as aplicações web no Azure.

1. No Visual Studio, eles com o botão direito no projeto SmartHotel.REgistration.Wcf > **publicar**.

    ![Publicar](media/contoso-migration-refactor-web-app-sql/publish-web1.png)

2. Começar a publicação.

    ![Publicar](media/contoso-migration-refactor-web-app-sql/publish-web2.png)

3. Eles selecionam um serviço de aplicações existentes porque eles já criou a aplicação web.

    ![Publicar](media/contoso-migration-refactor-web-app-sql/publish-web3.png)

4. Depois de selecionar a aplicação WCF, o Visual Studio implementa-o.

    ![Publicar](media/contoso-migration-refactor-web-app-sql/publish-web4.png)

5. Eles, em seguida, repita o processo para publicar a aplicação web - SmartHotel.Registration.Web.

    ![Publicar](media/contoso-migration-refactor-web-app-sql/publish-web5.png)


Neste momento, o aplicativo for migrado com êxito para o Azure.

## <a name="clean-up-after-migration"></a>Limpar após a migração

Após a migração, a Contoso precisa de concluir estes passos de limpeza:  

- Remova as VMs no local no inventário do vCenter.
- Remova as VMs das tarefas de cópia de segurança locais.
- Atualize a documentação interna para mostrar as novas localizações para a aplicação de SmartHotel. Mostre a base de dados como em execução na base de dados SQL do Azure e o front-end como em execução em duas aplicações web.
- Reveja todos os recursos que interagem com as VMs desativadas e atualizar as definições relevantes ou documentação para refletir a nova configuração.


## <a name="review-the-deployment"></a>Reveja a implementação

Com os recursos migrados no Azure, a Contoso precisa totalmente operacionalizar e proteger sua infra-estrutura de novo.

### <a name="security"></a>Segurança

- Contoso precisa de garantir que seus novos **SmartHotel-registo** base de dados é segura. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- Em particular, eles devem atualizar as aplicações web para utilizar SSL com certificados.

### <a name="backups"></a>Cópias de segurança

- A Contoso precisa de analisar os requisitos de cópia de segurança da base de dados do SQL do Azure. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
- Deve considerar implementar grupos de ativação pós-falha para fornecer ativação pós-falha para a base de dados. [Saiba mais](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).
- Contoso precisa de considerar a implementar a aplicação Web nos E.U.A. Leste 2 principal e a região E.U.A. Central para resiliência. Pode configurar o Gestor de tráfego para garantir que a ativação pós-falha em caso de falhas regionais.

### <a name="licensing-and-cost-optimization"></a>Otimização de licenciamento e de custo

- Depois de todos os recursos são implementados, Contoso deve atribuir etiquetas do Azure com base em seus [planejamento da infraestrutura](contoso-migration-infrastructure.md#set-up-tagging).
- Licenciamento de todos os baseia-se para o custo dos serviços PaaS que Contoso está a consumir. Isso é deduzido do EA.
1. Contoso irá ativar o Azure Cost Management licenciado pela Cloudyn, uma subsidiária da Microsoft. É uma solução de gestão de custos multiclouds que o ajuda a utilizar e gerir o Azure e outros recursos da cloud.  [Saiba mais](https://docs.microsoft.com/azure/cost-management/overview) sobre o Azure Cost Management. 

## <a name="conclusion"></a>Conclusão

Neste artigo, a Contoso refatorado a aplicação de SmartHotel no Azure ao migrar o VM da front-end de aplicação para duas aplicações Web do Azure. A base de dados de aplicação sua migração para uma base de dados SQL do Azure.






