---
title: Bases de Dados SQL (PaaS) vs SQL Server na nuvem em VMs (IaaS) | Microsoft Docs
description: 'Saiba qual a opção do SQL Server na nuvem que se adequa à sua aplicação: Base de Dados SQL (PaaS) do Azure ou SQL Server na nuvem em Azure Virtual Machines.'
services: sql-database, virtual-machines
keywords: Nuvem SQL Server, SQL Server na nuvem, base de dados PaaS, SQL Server na nuvem, DBaaS
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 2fb5a7cbca4df0faa06864f580814f31cc2b609c
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114406"
---
# <a name="choose-a-cloud-sql-server-option-azure-sql-paas-database-or-sql-server-on-azure-vms-iaas"></a>Escolha uma opção de SQL Server na nuvem: Base de Dados SQL (PaaS) do Azure ou SQL Server em VMs (IaaS) do Azure

No Azure, pode fazer com que as cargas de trabalho do SQL Server em execução numa infraestrutura alojada (IaaS) ou a ser executado como um serviço alojado ([PaaS](https://azure.microsoft.com/overview/what-is-paas/)):

* [Base de dados SQL do Azure](https://azure.microsoft.com/services/sql-database/): motor de base de dados A SQL, com base na Enterprise Edition do SQL Server, que está otimizada para o desenvolvimento de aplicações modernas. Base de dados SQL do Azure oferece duas versões do SQL Server como um serviço alojado: servidores lógicos e [Azure SQL da base de dados geridos instâncias (pré-visualização)](sql-database-managed-instance.md). Com ambas as versões, SQL Database do Azure adiciona funcionalidades adicionais que não estão disponíveis no SQL Server, tal como intelligence incorporado e de gestão. Com a primeira versão, pode ter um servidor lógico que contém [único bases de dados](sql-database-servers-databases.md) e grupo de servidores para um [conjunto elástico](sql-database-elastic-pool.md) para partilhar recursos e reduzir os custos. Um servidor lógico da SQL Database do Azure que contém as bases de dados único e agrupados oferece a maior parte das funcionalidades de âmbito de base de dados do SQL Server. Com o Azure SQL da base de dados geridas por instância, SQL Database do Azure oferece recursos partilhados para bases de dados e funcionalidades adicionais do âmbito de instância. Instância de geridos de base de dados de SQL do Azure suporta a migração de base de dados com o mínimo para nenhuma alteração da base de dados.
* [SQL Server em Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server instalado e alojado na nuvem no Windows Server ou Linux as máquinas virtuais (VMs) em execução no Azure, também conhecido como uma infraestrutura como serviço (IaaS). SQL Server em virtual machines do Azure é uma boa opção para a migração no local bases de dados do SQL Server e as aplicações sem qualquer alteração de base de dados. Todos os recentes versões e edições do SQL Server estão disponíveis para instalação numa máquina virtual IaaS. A diferença mais significativa da base de dados do SQL Server é que as VMs de SQL Server permitir controlo total sobre o motor de base de dados. Pode escolher quando a aplicação de patches/manutenção será iniciada, alterar o modelo de recuperação para simple ou em massa registados para ativar o carregamento mais rápido menos registo, para interromper ou iniciar o motor quando for necessário, e totalmente pode personalizar o motor de base de dados do SQL Server. Com este controlo adicional inclui adicionada responsabilidade para gerir as máquinas virtuais.

Saiba como cada opção se ajusta à plataforma de dados da Microsoft e obtenha ajuda para encontrar a opção adequada aos seus requisitos comerciais. Quer dê prioridade à redução de custos ou a uma administração mínima acima de tudo o resto, este artigo pode ajudá-lo a decidir qual é a abordagem que melhor responde aos requisitos comerciais a que dá mais importância.

## <a name="microsofts-sql-data-platform"></a>Plataforma de dados do Microsoft SQL Server

Um dos primeiros aspetos a compreender em qualquer debate do Azure versus bases de dados do SQL Server no local é que pode utilizar todas. A plataforma de dados da Microsoft tira partido da tecnologia do SQL Server e disponibiliza-a em máquinas físicas no local, ambientes de nuvem privada, ambientes de nuvem privada alojada de terceiros e nuvem pública. O SQL Server nas máquinas virtuais do Azure permite-lhe responder a necessidades comerciais únicas e diversas através de uma combinação de implementações alojadas na cloud e no local, utilizando o mesmo conjunto de produtos de servidor, ferramentas de desenvolvimento e conhecimentos em todos estes ambientes.

   ![Opções do SQL Server na nuvem: SQL server em IaaS ou base de dados SQL SaaS na nuvem.](./media/sql-database-paas-vs-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

Tal como ilustrado no diagrama, cada oferta pode ser caracterizada pelo nível de administração que têm sobre a infraestrutura (no eixo X) e pelo grau de eficiência de custos alcançada através da automatização e consolidação de nível de base de dados (no eixo Y).

Ao conceber uma aplicação, estão disponíveis quatro opções básicas para alojar a parte do SQL Server da aplicação:

* SQL Server em máquinas físicas não virtualizadas
* SQL Server em máquinas virtualizadas no local (nuvem privada)
* SQL Server numa Máquina Virtual do Azure (nuvem pública da Microsoft)
* Base de Dados SQL do Azure (nuvem pública da Microsoft)

Nas secções seguintes, ficará a conhecer o SQL Server na nuvem pública da Microsoft: Base de Dados SQL do Azure e SQL Server em VMs do Azure. Além disso, explorará motivadores de negócio comuns para determinar qual é a opção que funciona melhor para a sua aplicação.

## <a name="a-closer-look-at-azure-sql-database-and-sql-server-on-azure-vms"></a>Um olhar mais atento sobre a Base de Dados SQL do Azure e o SQL Server em VMs do Azure

**Base de dados SQL do Azure** é um relacional da base de dados-como-um-serviço (DBaaS) alojado na nuvem do Azure que insere-se na categoria do setor de *plataforma-como-um-serviço (PaaS)*. A [Base de Dados SQL](sql-database-technical-overview.md) baseia-se em hardware e software normalizado que é detido, alojado e mantido pela Microsoft. Com base de dados do SQL Server, pode utilizar a funcionalidade que necessitam de configuração de um vasto conjunto no SQL Server e as funções incorporadas. Ao utilizar a Base de Dados SQL, paga apenas o que utilizar, dispondo de opções para aumentar verticalmente ou horizontalmente para obter um maior desempenho sem interrupções. SQL Database do Azure, com suporte para ambos [único bases de dados](sql-database-servers-databases.md) e [conjuntos elásticos](sql-database-elastic-pool.md) para a partilha de recursos, é um ambiente ideal para o desenvolvimento de novas aplicações na nuvem. E, com [da base de dados geridas por instância de SQL do Azure](sql-database-managed-instance.md), que pode colocar a sua própria licença. Além disso, esta opção fornece todas as vantagens de PaaS da SQL Database do Azure, mas adiciona funcionalidades que se encontravam anteriormente disponíveis apenas em VMs do SQL Server. Isto inclui uma rede virtual nativa (VNet) e perto da compatibilidade de 100% com SQL Server no local. [Gerido instância](sql-database-managed-instance.md) é ideal para o local da base de dados migrações para o Azure com alterações mínimas necessárias. 

O **SQL Server em Máquinas Virtuais (VMs) do Azure** insere-se na categoria do setor *Infraestrutura como serviço (IaaS)* e permite executar o SQL Server dentro de uma máquina virtual na nuvem. [Máquinas virtuais do SQL Server](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) também executar em hardware normalizado que é detido, alojado e mantido pela Microsoft. Quando utilizar o SQL Server numa VM, pode ou pay-como a viagem uma licença do SQL Server já incluídos numa imagem do SQL Server ou utilizar facilmente uma licença existente. Também pode interromper ou retomar a VM conforme necessário.

Em geral, estas duas opções de SQL estão otimizadas para diferentes fins:

* **Base de dados SQL do Azure** está otimizada para reduzir os custos de gestão globais para o mínimo para o aprovisionamento e gestão de muitas bases de dados. Reduz os custos correntes de administração, uma vez que não é necessário gerir as máquinas virtuais, o sistema operativo ou o software de base de dados. Não é necessário gerir as atualizações, a elevada disponibilidade ou as [cópias de segurança](sql-database-automated-backups.md). Em geral, a Base de Dados SQL do Azure permite aumentar substancialmente o número de bases de dados geridas por um único recurso de TI ou de desenvolvimento. [Conjuntos elásticos](sql-database-elastic-pool.md) também suporta arquiteturas de aplicações de multi-inquilino de SaaS com funcionalidades, incluindo isolamento de inquilino e a capacidade de dimensionar para reduzir os custos ao partilhar recursos em bases de dados. [Instância de geridos de base de dados de SQL do Azure](sql-database-managed-instance.md) fornece suporte para funcionalidades de instância de âmbito ativação fácil migração das aplicações existentes, bem como partilhar recursos entre as bases de dados.
* **O SQL Server em execução em VMs do Azure** está otimizado para migrar as aplicações existentes para o Azure ou para expandir aplicações no local existentes para a nuvem em implementações híbridas. Além disso, pode utilizar o SQL Server numa máquina virtual para desenvolver e testar aplicações do SQL Server tradicionais . Com o SQL Server em VMs do Azure, tem direitos administrativos completos sobre uma instância dedicada do SQL Server e uma VM baseada na nuvem. É uma opção perfeita quando uma organização já tem recursos de TI disponíveis para manter as máquinas virtuais. Estas capacidades permite-lhe criar um sistema altamente personalizado para fazer face aos requisitos de disponibilidade e desempenho específicos da sua aplicação.

A tabela seguinte resume as principais caraterísticas da Base de Dados SQL e do SQL Server em VMs do Azure:

| | Base de Dados SQL do Azure<br>Servidores lógicas, conjuntos elásticos e bases de dados individuais | Base de Dados SQL do Azure<br>Instância Gerida |Máquina Virtual do Azure<br>SQL Server |
| --- | --- | --- |---|
| **Melhor para:** |Novas aplicações concebidas na nuvem que pretendem utilizam as mais recentes estáveis do SQL Server funcionalidades andhave restrições de tempo em desenvolvimento e marketing. | Novas aplicações ou aplicações no local que pretendem utilizar as funcionalidades mais recentes de SQL Server estáveis e de que são migradas para a nuvem com alterações mínimas.  | Aplicações existentes que necessitam de migração rápida para a nuvem com alterações mínimas ou sem alterações. Cenários de desenvolvimento e teste rápidos quando não pretende comprar hardware de SQL Server de não produção no local. |
|  | Equipas que necessitam de atualizações, recuperação após desastre e elevada disponibilidade para a base de dados. | Igual a base de dados SQL. | Equipas que podem configurar, ajustar otimizar, personalizar e gerir a elevada disponibilidade, a recuperação após desastre e a aplicação de patches do SQL Server. Algumas das funcionalidades automatizadas fornecidas simplificam-no significativamente. | |
|  | Equipas que não pretendem gerir o sistema operativo subjacente e definições de configuração. | Igual a base de dados SQL. | Precisa de um ambiente personalizado com direitos administrativos completos. | |
|  | Bases de dados de até 4 TB ou bases de dados maiores que podem ser [particionadas horizontal ou verticalmente](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling) utilizando um padrão de escalamento horizontal. | Igual a base de dados SQL. | Instâncias do SQL Server com até 64 TB de armazenamento. A instância pode suportar tantas bases de dados quanto necessário. |
| **Compatibilidade** | Suporta a maioria das capacidades de nível de base de dados no local. | Suporta quase todas as capacidades de nível de instância e o nível de base de dados no local. | Suporta todas as capacidades no local. |
| **Recursos:** | Não pretende utilizar recursos de TI para configuração e gestão da infraestrutura subjacente, mas pretende focar-se na camada da aplicação. | Igual a base de dados SQL. | Tem alguns recursos de TI para a configuração e gestão. Algumas das funcionalidades automatizadas fornecidas simplificam-no significativamente. |
| **Custo total de propriedade:** | Elimina os custos de hardware e reduz os custos administrativos. | Igual a base de dados SQL. | Elimina os custos com hardware. |
| **Continuidade do negócio:** |Para além [as capacidades de infraestrutura de tolerância a falhas incorporadas](sql-database-high-availability.md), SQL Database do Azure fornece funcionalidades, tais como [cópias de segurança automatizadas](sql-database-automated-backups.md), [no momento restauro](sql-database-recovery-using-backups.md#point-in-time-restore), [georrestauro](sql-database-recovery-using-backups.md#geo-restore), e [ativação pós-falha de grupos e a georreplicação ativa](sql-database-geo-replication-overview.md) para aumentar a continuidade do negócio. Para obter mais informações, consulte [Descrição geral da continuidade de negócio da Base de Dados SQL](sql-database-business-continuity.md). | Mesmo que a base de dados SQL, mais iniciada pelo utilizador só de cópia cópias de segurança estão disponíveis. | O SQL Server em VMs do Azure permite configurar uma solução de elevada disponibilidade e recuperação após desastre para as necessidades específicas da sua base de dados. Assim, poderá ter um sistema que está altamente otimizado para a sua aplicação. Pode testar e executar ativações pós-falha sempre que necessário. Para obter mais informações, consulte [Elevada Disponibilidade e Recuperação Após Desastre do SQL Server em Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md). |
| **Nuvem híbrida:** |A sua aplicação no local pode aceder a dados na Base de Dados SQL do Azure. | [Implementação de rede virtual nativo](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-managed-instance-vnet-configuration) e a conectividade ao seu ambiente no local utilizando o Azure Express Route ou VPN Gateway. | Com o SQL Server em VMs do Azure, pode ter aplicações que são executadas parcialmente na nuvem e parcialmente no local. Por exemplo, pode expandir a sua rede no local e o Domínio do Active Directory para a nuvem através da [Rede Virtual do Azure](../virtual-network/virtual-networks-overview.md). Além disso, pode armazenar ficheiros de dados no local no Armazenamento do Azure, utilizando [Ficheiros de Dados do SQL Server no Azure](http://msdn.microsoft.com/library/dn385720.aspx). Para obter mais informações, consulte [Introdução à Nuvem Híbrida do SQL Server 2014](http://msdn.microsoft.com/library/dn606154.aspx). |
|  | Suporta a [replicação transacional do SQL Server](https://msdn.microsoft.com/library/mt589530.aspx) como um subscritor para replicar os dados. | A replicação não é suportada para a base de dados geridas por instância de SQL do Azure. | Suporta totalmente [replicação transacional do SQL Server](https://msdn.microsoft.com/library/mt589530.aspx), [em grupos de disponibilidade Always](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md), serviços de integração e envio de registo para replicar os dados. Além disso, as cópias de segurança do SQL Server tradicionais são totalmente suportadas | |
|  | | |

## <a name="business-motivations-for-choosing-azure-sql-database-or-sql-server-on-azure-vms"></a>Motivações comerciais para escolher a Base de Dados SQL do Azure ou o SQL Server em VMs do Azure
### <a name="cost"></a>Custo
Quer seja uma start-up com problemas de liquidez ou uma equipa numa empresa estabelecida que opera com grandes restrições orçamentais, o financiamento limitado é, muitas vezes, o principal fator quando tem de decidir como alojar as suas bases de dados. Nesta secção, aprenderá primeiro as noções básicas do licenciamento e da faturação no Azure com respeito a estas duas opções de base de dados relacional: Base de Dados SQL e SQL Server em VMs do Azure. Também podem aprender a calcular o custo total da aplicação.

#### <a name="billing-and-licensing-basics"></a>Noções básicas de faturação e licenciamento

Atualmente, **base de dados SQL** é vendida como um serviço e está disponível em vários escalões de serviço com os preços diferentes para os recursos, sendo todas faturadas à hora um preço fixo com base de nível de desempenho e o escalão de serviço que escolher. Com a instância de geridos de base de dados de SQL, pode também traga a sua própria licença. Para obter mais informações sobre o modelo «traga a sua própria licença», consulte o artigo [Mobilidade de Licenças através do Software Assurance no Azure](https://azure.microsoft.com/pricing/license-mobility/). Além disso, é-lhe faturado o tráfego de Internet de saída a [taxas de transferência de dados](https://azure.microsoft.com/pricing/details/data-transfers/) regulares. Pode ajustar dinamicamente camadas de serviços e níveis de desempenho para corresponder às necessidades variadas de débito da sua aplicação. Para obter as informações mais recentes sobre os atuais suportado escalões de serviço, consulte [DTU com base no modelo de compra](sql-database-service-tiers-dtu.md) e [vCore com base no modelo de compra (pré-visualização)](sql-database-service-tiers-vcore.md). Também pode criar [conjuntos elásticos](sql-database-elastic-pool.md) picos de partilhar recursos entre instâncias de base de dados para reduzir os custos e acomodar utilização.

Com a **Base de Dados SQL**, o software de base de dados é automaticamente configurado, corrigido e atualizado pela Microsoft, o que reduz os custos de administração. Além disso, as capacidades de [cópia de segurança incorporada](sql-database-automated-backups.md) permitem-lhe obter uma redução significativa de custos, sobretudo quando tem um número elevado de bases de dados. 

Com o **SQL Server em VMs do Azure**, pode utilizar qualquer uma das imagens do SQL Server fornecidas pela plataforma (que inclui uma licença) ou utilizar a sua licença do SQL Server. Todas as versões (2008R2, 2012, 2014, 2016) e edições (programador, rápida, Web, Standard, Enterprise) do SQL Server suportadas estão disponíveis. Além disso, as versões Traga a sua própria licença (BYOL) das imagens estão disponíveis. Quando utiliza as imagens fornecidas pelo Azure, o custo operacional depende do tamanho da VM e a edição do SQL Server que escolher. Independentemente do tamanho da VM ou edição do SQL Server, paga por minuto custo de licenciamento do SQL Server e o Windows ou Linux Server, juntamente com o custo de armazenamento do Azure para os discos da VM. A opção de faturação por minuto permite-lhe utilizar o SQL Server durante o tempo que precisar, sem comprar licenças adicionais do SQL Server. Se trouxer a sua própria licença do SQL Server para o Azure, são-lhe cobrados do servidor e apenas os custos de armazenamento. Para obter mais informações sobre o modelo «traga a sua própria licença», consulte o artigo [Mobilidade de Licenças através do Software Assurance no Azure](https://azure.microsoft.com/pricing/license-mobility/). Além disso, é-lhe faturado o tráfego de Internet de saída a [taxas de transferência de dados](https://azure.microsoft.com/pricing/details/data-transfers/) regulares.

#### <a name="calculating-the-total-application-cost"></a>Calcular o custo total da aplicação
Quando começar a utilizar uma plataforma em nuvem, o custo de execução da sua aplicação inclui o custo de programação novo e os custos de administração em curso, mais os custos de serviço de plataforma de nuvem pública.

**Ao utilizar a Base de Dados SQL do Azure:**

- Custos de administração altamente minimizados
- Custos de desenvolvimento limitadas para aplicações migrados
- Custos do serviço de base de dados SQL
- Sem custos de compra de hardware

**Ao utilizar o SQL Server em VMs do Azure:**

- Custos de administração superiores
- Limitado a sem custos de desenvolvimento de aplicações migrados
- Custos do serviço de máquina virtual
- Custos de licenciamento do SQL Server
- Sem custos de compra de hardware

Para obter mais informações sobre preços, consulte os seguintes recursos:

* [Preços de Base de Dados SQL](https://azure.microsoft.com/pricing/details/sql-database/)
* [Preços de Máquina Virtual](https://azure.microsoft.com/pricing/details/virtual-machines/) para [SQL](https://azure.microsoft.com/pricing/details/virtual-machines/#sql) e para [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/#windows)
* [Calculadora de Preços do Azure](https://azure.microsoft.com/pricing/calculator/)

### <a name="administration"></a>Administração
Para muitas empresas, a decisão de fazer a transição para um serviço em nuvem passa tanto por descartar a complexidade de administração como pelo custo. Com o IaaS e PaaS, Microsoft administra infraestrutura subjacente e automaticamente replica todos os dados para fornecer a recuperação após desastre, configura e atualiza o software de base de dados, gere o balanceamento de carga e efetua uma ativação pós-falha transparente se existir um Falha do servidor no Centro de dados. 

- Com **SQL Database do Azure**, pode continuar a administrar a base de dados, mas já não necessita de gerir o motor de base de dados, o sistema operativo do servidor ou o hardware.  Exemplos de itens que pode continuar a administrar incluem bases de dados e inícios de sessão, otimização de índices e consultas, e auditoria e segurança. Além disso, a configuração de elevada disponibilidade para outro Datacenter requer configuração mínima e a administração.
- Com o **SQL Server em VMs do Azure**, tem controlo total sobre o sistema operativo e a configuração da instância do SQL Server. Com uma VM, cabe-lhe a si decidir quando atualizar o sistema operativo e o software de base de dados e quando instalar software adicional, tal como o antivírus. Algumas funcionalidades automatizadas são fornecidas para simplificar significativamente a aplicação de patches, a cópia de segurança e a elevada disponibilidade. Além disso, pode controlar o tamanho da VM, o número de discos e as respetivas configurações de armazenamento. O Azure permite-lhe alterar o tamanho de uma VM conforme necessário. Para obter informações, consulte o artigo [Máquina Virtual e Tamanhos do Serviço em Nuvem do Azure](../virtual-machines/windows/sizes.md). 

### <a name="service-level-agreement-sla"></a>Contrato de Nível de Serviço (SLA)
Para muitos departamentos de TI, cumprir as obrigações de tempo de atividade de um Contrato de Nível de Serviço (SLA) é uma prioridade máxima. Nesta secção, vamos ver que SLA se aplica a cada opção de alojamento de base de dados.

Para **base de dados SQL**, a Microsoft fornece um SLA de 99,99% de disponibilidade. Para obter as informações mais recentes, consulte [Contrato de Nível de Serviço](https://azure.microsoft.com/support/legal/sla/sql-database/). 

Para o **SQL Server em execução em VMs do Azure**, a Microsoft fornece um SLA de 99,95% de disponibilidade, que abrange apenas a Máquina Virtual. Este SLA não abrange os processos (como o SQL Server) em execução na VM e requer que aloje, pelo menos, duas instâncias de VM num conjunto de disponibilidade. Para obter as informações mais recentes, consulte o [SLA de VM](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Para uma base de dados elevada disponibilidade (HA) dentro das VMs, deve configurar uma das opções de elevada disponibilidade suportadas no SQL Server, tais como [em grupos de disponibilidade Always](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server). Utilizar uma opção de elevada disponibilidade suportadas não fornece um SLA adicional, mas permite-lhe alcançar uma disponibilidade de base de dados > 99,99%.

### <a name="market"></a>Tempo para mover para o Azure
**Servidores lógicos de base de dados SQL, conjuntos elásticos e bases de dados individuais** é a solução certa para aplicações concebidas na nuvem quando a produtividade do programador e rápido tempo mercado para soluções de noew são críticas. Com funcionalidade semelhante a DBA programática, é perfeita para programadores e arquitetos de nuvem, uma vez que reduz a necessidade de gerir a base de dados e o sistema operativo subjacentes. 

**A instância de gerido da base de dados do SQL Server** simplifica bastante a migração de aplicações existentes para a SQL Database do Azure, permitindo-lhe trazer uma base de dados migrado aplicações colocação no mercado no Azure rapidamente.

**SQL Server em execução em VMs do Azure** é perfeito se as suas aplicações existentes ou novas necessitam de bases de dados grandes ou aceder a todas as funcionalidades no SQL Server ou o Windows/Linux e pretender evitar para o tempo e despesa dos adquirir novo hardware no local. Também é uma boa opção quando pretender migrar existente no local e bases de dados do Azure como de aplicações-for - em casos onde a base de dados geridas por instância de SQL do Azure não é uma boa opção. Uma vez que não precisa de alterar as camadas de apresentação, de aplicação e de dados, poupa tempo e orçamento na reformulação da sua solução existente. Em vez disso, pode concentrar-se na migração de todas as suas soluções para o Azure e na execução de algumas otimizações de desempenho que possam ser exigidas pela plataforma do Azure. Para obter mais informações, consulte o artigo [Melhores Práticas de Desempenho para o SQL Server em Azure Virtual Machines](../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md).

## <a name="summary"></a>Resumo
Este artigo explorou a Base de Dados SQL e o SQL Server em Máquinas Virtuais (VMs) do Azure e abordou os motivadores de negócio comuns que poderão influenciar a sua decisão. Segue-se um resumo das sugestões que deve considerar:

Opte pela **Base de Dados SQL do Azure** se:

* Está a criar novas aplicações baseadas na nuvem para tirar partido das reduções de custos e da otimização de desempenho que os serviços em nuvem proporcionam. Esta abordagem proporciona as vantagens de um serviço em nuvem completamente gerido, ajuda a diminuir o tempo de colocação no mercado inicial e pode fornecer otimização de custos a longo prazo.
* Pretende que seja a Microsoft a efetuar as operações de gestão comuns nas suas bases de dados e precisa de SLAs de maior disponibilidade para as bases de dados.
* Pretende migrar uma aplicação existente como-está a base de dados geridas por instância de SQL do Azure e tirar partido da paridade adicional com o SQL Server e/ou de rede e de segurança avançada. Instância gerida é uma boa opção para aplicações novas e existentes.

Opte pelo **SQL Server em VMs do Azure** se:

* Tiver aplicações no local que pretende migrar ou expandir para a nuvem, ou se pretender criar aplicações da empresa com mais de 4 TB. Esta abordagem fornece a vantagem de utilizar a versão do SQL Server e a edição da sua escolha, capacidade de base de dados grandes, controlo total sobre o SQL Server e Windows/Linux e túnel num local seguro. Esta abordagem minimiza os custos de programação e de modificação de aplicações existentes.
* tiver os recursos de TI existentes e pode, em última análise, se proprietário da aplicação de patches, de cópias de segurança e de base de dados de elevada disponibilidade. Repare que algumas funcionalidades automatizadas simplificam significativamente estas operações. 

## <a name="next-steps"></a>Passos Seguintes

* Consulte [A sua primeira Base de Dados SQL do Azure](sql-database-get-started-portal.md) para começar a utilizar a Base de Dados SQL.
* Consulte [Preços de Base de Dados SQL](https://azure.microsoft.com/pricing/details/sql-database/).
* Consulte [Aprovisionar uma máquina virtual do SQL Server no Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md) para começar a utilizar o SQL Server em VMs do Azure.