---
title: Gerir várias bases de dados do SQL Server com o Azure de conjuntos elástico | Microsoft Docs
description: Gerir e dimensionar várias bases de dados SQL - centenas e milhares - utilizam conjuntos elásticos. Um preço para recursos que pode distribuir sempre que necessário.
keywords: várias bases de dados, os recursos de base de dados e de desempenho de base de dados
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.date: 04/10/2018
ms.author: ninarn
ms.topic: article
ms.openlocfilehash: 930b5607f343b87adc253cc99d74ddf28235a50b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="elastic-pools-help-you-manage-and-scale-multiple-azure-sql-databases"></a>Ajudar a gerir e dimensionar várias bases de dados SQL do Azure de conjuntos elásticos

Conjuntos de base de dados SQL elásticas são uma solução simple e rentável para gerir e dimensionamento várias bases de dados que tenham diferentes e imprevisíveis exigências de utilização. As bases de dados num agrupamento elástico estão num único servidor SQL Database do Azure e partilham um número de conjunto de recursos um preço do conjunto. Com os conjuntos elásticos na Base de Dados SQL do Azure, os programadores de SaaS podem otimizar o desempenho do preço num grupo de bases de dados dentro de um orçamento prescrito e, ao mesmo tempo, oferecer elasticidade de desempenho para cada base de dados.

## <a name="what-are-sql-elastic-pools"></a>Quais são os conjuntos elásticos SQL?

Os programadores de SaaS criam as aplicações sobre camadas de dados de grande escala, que consistem em várias bases de dados. Um padrão de aplicação comum é aprovisionar uma base de dados individual para cada cliente. Contudo, muitas vezes, os padrões de cada cliente são distintos e imprevisíveis, sendo difícil prever os requisitos de recursos de cada utilizador de bases de dados individual. Tradicionalmente, tiver duas opções:

- Aprovisionar excessiva recursos com base na utilização máxima e através de pagamento, ou
- Under-aprovisionar custo, em detrimento de desempenho e cliente satisfação durante picos de guardar.

Conjuntos elásticos resolverem este problema, garantindo que as bases de dados obter os recursos de desempenho que precisam e quando precisam-lo. Fornecem um mecanismo de alocação de recursos simples dentro de um orçamento previsível. Para saber mais sobre os padrões de estrutura de aplicações SaaS que utilizam conjuntos elásticos, consulte o artigo [Padrões de Estrutura de Aplicações SaaS Multi-inquilino com a Base de Dados SQL do Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md).

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

Conjuntos elásticos ativar o programador comprar recursos para um agrupamento partilhado por várias bases de dados para acomodar imprevisíveis períodos de utilização por bases de dados individuais. Pode configurar recursos para o conjunto baseada em qualquer no [DTU com base no modelo de compra (pré-visualização)](sql-database-service-tiers.md#dtu-based-purchasing-model) ou [vCore com base no modelo de compra (pré-visualização)](sql-database-service-tiers.md#vcore-based-purchasing-model-preview). O requisito de recursos para um conjunto é determinado pela agregação de utilização das suas bases de dados. A quantidade de recursos disponíveis para o conjunto é controlada pela atribuição de programador. O programador simplesmente adiciona as bases de dados para o conjunto, define os recursos mínimos e máximo para as bases de dados (DTUs minumumn e máximo ou mínimo ou máximo vCores consoante à sua escolha do modelo de resourceing) e, em seguida, define os recursos do conjunto com base no seu orçamento. Os programadores podem utilizar os conjuntos para fazer crescer de forma contínua o serviço, de uma “lean startup” para uma empresa madura e em constante crescimento.

Dentro do conjunto, é dada às bases de dados individuais a flexibilidade para se dimensionarem automaticamente dentro de parâmetros definidos. Com muita carga, uma base de dados pode consumir mais recursos para satisfazer o pedido. Bases de dados carrega leve consumam inferior e bases de dados não carga consumam não existem recursos. O aprovisionamento de recursos para o conjunto completo e não para bases de dados individuais simplifica as tarefas de gestão. Além disso, tem um orçamento previsível para o conjunto. Recursos adicionais podem ser adicionados a um conjunto existente sem período de indisponibilidade de base de dados, exceto que as bases de dados poderão ter de ser movidos para fornecer os recursos de computação adicionais para a nova reserva de eDTU. Da mesma forma, se recursos adicionais já não são necessárias estes podem ser removidos de um conjunto existente em qualquer ponto no tempo. Além disso, pode adicionar ou subtrair bases de dados ao conjunto. Se uma base de dados estiver a subutilizar recursos de forma previsível, remova-a.

## <a name="when-should-you-consider-a-sql-database-elastic-pool"></a>Quando considerar um conjunto elástico da base de dados SQL?

Os conjuntos são bastante adequados para um grande número de bases de dados com padrões de utilização específicos. Para uma determinada base de dados, este padrão caracteriza-se por uma utilização média baixa com picos de utilização relativamente raros.

Quantas mais bases de dados adicionar a um conjunto, maior será a poupança. Dependendo do padrão de utilização de aplicações, é possível ver poupanças com um mínimo de duas bases de dados S3.

As secções seguintes ajudam-no a avaliar se a sua coleção de bases de dados específica pode beneficiar de estar num conjunto. Os exemplos utilizam conjuntos Standard, mas os mesmos princípios também se aplicam aos conjuntos Básicos e Premium.

### <a name="assessing-database-utilization-patterns"></a>Avaliar os padrões de utilização da base de dados

A figura seguinte mostra um exemplo de uma base de dados que está inativa muito tempo, mas que também, periodicamente, apresenta picos de atividade. Este é um padrão de utilização adequado para um conjunto:

   ![uma base de dados individual adequada para um conjunto](./media/sql-database-elastic-pool/one-database.png)

Para o período de cinco minutos ilustrado, DB1 tem um pico até 90 DTUs, mas a utilização média global é inferior a cinco DTUs. Para executar esta carga de trabalho numa base de dados individual, é necessário o nível de desempenho S3, mas tal deixa a maior dos recursos inutilizados durante os períodos de pouca atividade.

Um conjunto permite que estas DTUs não utilizadas sejam partilhadas entre várias bases de dados, pelo que reduz as DTUs necessárias e os custos gerais.

Com base no exemplo anterior, imagine que existem bases de dados adicionais com padrões de utilização semelhantes a DB1. No seguinte duas figuras abaixo, a utilização de quatro e bases de dados 20 são em camadas para o mesmo gráfico para ilustrar a natureza sem sobreposição da respetiva utilização ao longo do tempo utilizando o modelo de compra de DTU com base em:

   ![quatro bases de dados com um padrão de utilização adequado para um conjunto](./media/sql-database-elastic-pool/four-databases.png)

   ![vinte bases de dados com um padrão de utilização adequado para um conjunto](./media/sql-database-elastic-pool/twenty-databases.png)

A utilização agregada de DTUs nas 20 bases de dados é ilustrada pela linha preta na figura anterior. Mostra que a utilização agregada de DTUs nunca excede as cem e indica que as 20 bases de dados podem partilhar 100 eDTUs ao longo deste período de tempo. Isto resulta numa redução em 20 vezes das DTUs e numa redução de 13 vezes no preço em comparação com colocar cada base de dados nos níveis de desempenho S3 para bases de dados individuais.

Este exemplo é ideal pelos motivos seguintes:

* Existem grandes diferenças entre a utilização de pico e utilização média por base de dados.
* A utilização de pico de cada base de dados ocorre em diferentes alturas no tempo.
* Os eDTUs são partilhados entre muitas bases de dados.

O preço de um conjunto é uma função das eDTUs do conjunto. Embora o preço unitário de eDTU de um conjunto seja 1,5 vezes superior ao preço unitário de DTU para bases de dados individuais, **as eDTUs de conjuntos podem ser partilhadas por muitas bases de dados e são necessárias menos eDTUs totais**. Estas distinções nos preços e na partilha das eDTUs são as bases do potencial de poupança nos custos que os conjuntos podem proporcionar.

As regras básicas seguintes relacionadas com a contagem e a utilização de bases de dados ajudam a garantir que os conjuntos oferecem custos reduzidos quando comparados com a utilização de níveis de desempenho para bases de dados individuais.

### <a name="minimum-number-of-databases"></a>Número mínimo de bases de dados

Se a quantidade de agregação de recursos das bases de dados é mais de 1,5 x os recursos necessários para o agrupamento, um conjunto elástico é mais económico.

***Com base em DTU compra exemplo de modelo***<br>
Para que a relação de custo-eficácia de um conjunto de 100 eDTUs seja superior à utilização de níveis de desempenho para bases de dados individuais, são necessárias, pelo menos, duas bases de dados S3 ou 15 bases de dados S0.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Número máximo de bases de dados com picos simultâneos

Ao partilhar resourcess, nem todas as bases de dados num agrupamento podem utilizar em simultâneo resourcess até ao limite disponível das bases de dados. As bases de dados menos pico ou em simultâneo, o menor podem ser definidos os recursos de agrupamento e fica do conjunto mais económica. Em geral, não mais do que 2/3 (ou 67%) das bases de dados no conjunto devem peak em simultâneo para o respetivo limite de recursos.

***Com base em DTU compra exemplo de modelo***<br>
Para reduzir os custos de três bases de dados S3 num conjunto de 200 eDTU, duas bases de dados, no máximo, podem ter o pico de utilização ao mesmo tempo. Caso contrário, se mais de duas destas quatro bases de dados S3 tiverem o pico em simultâneo, o conjunto terá de ser dimensionado para mais de 200 eDTUs. Se o conjunto for redimensionado para mais de 200 eDTUs, será necessário adicionar mais bases de dados S3 ao conjunto, de modo a manter os custos inferiores aos dos níveis de desempenho para bases de dados individuais.

Tenha em conta que este exemplo não considera a utilização de outras bases de dados do conjunto. Se todas as bases de dados tiverem alguma utilização num determinado momento, menos de 2/3 (67%) das bases de dados podem ter o pico em simultâneo.

### <a name="resource-utilization-per-database"></a>Utilização de recursos por base de dados
Uma grande diferença entre o pico e a utilização média das bases de dados indica períodos longos de pouca utilização e curtos períodos de elevada utilização. Este padrão de utilização é ideal para partilhar recursos entre bases de dados. A inclusão de bases de dados num conjunto deve ser considerada quando o pico de utilização for cerca de 1,5 vezes superior à utilização média.

***Com base em DTU compra exemplo de modelo***<br>
Uma base de dados S3 que tenha como pico 100 DTUs e utilize, em média, 67 DTUs ou menos, é uma boa candidata para partilhar eDTUs num conjunto. Em alternativa, uma base de dados S1 que tenha como pico 20 DTUs e utilize, em média, 13 DTUs ou menos, é uma boa candidata para um conjunto.

## <a name="how-do-i-choose-the-correct-pool-size"></a>Como devo escolher o tamanho do conjunto correto?

O tamanho de um agrupamento de melhor depende os agregado dos recursos necessários para todas as bases de dados no agrupamento de. Isto envolve a determinar o seguinte:

* Máximos recursos utilizados por todas as bases de dados no conjunto (máximas DTUs ou vCores máximo consoante à sua escolha do modelo de resourceing).
* Bytes de armazenamento máximos utilizados por todas as bases de dados do conjunto.

Para os escalões de serviço disponíveis para cada modelo de recursos, consulte o [DTU com base no modelo de compra](sql-database-service-tiers.md#dtu-based-purchasing-model) ou [vCore com base no modelo de compra (pré-visualização)](sql-database-service-tiers.md#vcore-based-purchasing-model-preview).

A Base de Dados SQL avalia automaticamente o histórico de utilização de recursos de bases de dados num servidor de Base de Dados SQL existente e recomenda a configuração de conjunto adequada no portal do Azure. Para além das recomendações, uma experiência incorporada prevê a utilização de eDTUs para um grupo personalizado de bases de dados no servidor. Desta forma, pode fazer uma análise de hipóteses ao adicionar, interativamente, bases de dados ao conjunto e removê-las para ver uma análise da utilização de recursos e obter conselhos de dimensionamento antes de consolidar as alterações. Para obter as instruções, veja [Monitor, manage, and size an elastic pool (Monitorizar, gerir e dimensionar conjuntos elásticos)](sql-database-elastic-pool-manage-portal.md).

Nos casos em que não pode utilizar as ferramentas, as instruções passo a passo seguintes podem ajudá-lo a prever se um conjunto tem uma relação de custo-eficácia melhor do que as bases de dados individuais.

1. Estimar o eDTUs ou vCores necessário para o conjunto da seguinte forma:

   Para o modelo de compra baseado em DTU: máx. (<*número Total de bds* X *médio de utilização da DTU por BD*>,<br>
   <*Número de DBs com pico em simultâneo* X *utilização de pico de DTUs por DB*)

   Para o modelo de compra baseado em vCore: máx. (<*número Total de bds* X *médio vCore utilização por BD*>,<br>
   <*Número de em simultâneo peaking DBs* X *picos de utilização vCore por BD*)

2. Calcule o espaço de armazenamento necessário para o conjunto ao adicionar o número de bytes de que todas as bases de dados do conjunto precisam. Em seguida, determine o tamanho do conjunto de eDTUs que disponibiliza esta quantidade de armazenamento.
3. O DTU com base no modelo de compra, têm maior as estimativas de eDTU do passo 1 e o passo 2. Modelo de compra baseado em vCore, têm a estimativa vCore do passo 1.
4. Consulte o [base de dados SQL página de preços](https://azure.microsoft.com/pricing/details/sql-database/) e localizar o conjunto de menor tamanho que é maior do que a estimativa do passo 3.
5. Compare o preço do conjunto do Passo 5 com o preço de utilizar os níveis de desempenho adequado para bases de dados individuais.

## <a name="using-other-sql-database-features-with-elastic-pools"></a>Utilizar outras funcionalidades de base de dados do SQL Server com conjuntos elásticos

### <a name="elastic-jobs-and-elastic-pools"></a>As tarefas elásticas e conjuntos elásticos

Com um conjunto, as tarefas de gestão são simplificadas através da execução de scripts em **[tarefas elásticas](sql-database-elastic-jobs-overview.md)**. Uma tarefa elástica elimina a maior parte da monotonia associada a grandes números de bases de dados. Para começar, veja [Introdução às Tarefas elásticas](sql-database-elastic-jobs-getting-started.md).

Para obter mais informações sobre outras ferramentas de base de dados para trabalhar com várias bases de dados, veja [Aumentar horizontalmente com a Base de Dados SQL do Azure](sql-database-elastic-scale-introduction.md).

### <a name="business-continuity-options-for-databases-in-an-elastic-pool"></a>Opções de continuidade do negócio para bases de dados num agrupamento elástico
As bases de dados agrupadas suportam geralmente as mesmas [funcionalidades de continuidade de negócio](sql-database-business-continuity.md) que estão disponíveis para as bases de dados individuais.

- **Restauro de ponto no tempo**: restauro de ponto no tempo utiliza cópias de segurança da base de dados automática para recuperar uma base de dados num agrupamento para um ponto específico no tempo. Veja [Restauro para um ponto anterior no tempo](sql-database-recovery-using-backups.md#point-in-time-restore)

- **Georrestauro**: georrestauro fornece a opção de recuperação predefinida quando uma base de dados não está disponível devido a um incidente na região onde está alojada a base de dados. Veja [Restaurar uma Base de Dados SQL do Azure ou fazer a ativação pós-falha para uma secundária](sql-database-disaster-recovery.md)

- **Replicação geográfica activa**: para aplicações que têm requisitos de recuperação mais agressiva do que pode oferecer georrestauro, configurar [georreplicação ativa](sql-database-geo-replication-overview.md).

## <a name="manage-elastic-pools-and-databases-using-the-azure-portal"></a>Gerir conjuntos elásticos e bases de dados no portal do Azure

### <a name="creating-a-new-sql-database-elastic-pool-using-the-azure-portal"></a>Criar um novo conjunto elástico de SQL Database no portal do Azure

Existem duas formas de criar um conjunto elástico no portal do Azure.
1. Pode criar um conjunto elástico pesquisando **agrupamento elástico de SQL** no **Marketplace** ou clicar **+ adicionar** nos conjuntos elásticos SQL procurar painel. É possível especificar um servidor novo ou existente através deste agrupamento de aprovisionamento de fluxo de trabalho.
2. Ou pode criar um conjunto elástico, navegando para um servidor existente do SQL Server e clicando em **criar conjunto** para criar um conjunto diretamente no servidor. A única diferença aqui é que ignorar o passo em que especifique o servidor durante o aprovisionamento de fluxo de trabalho de agrupamento.

> [!NOTE]
> Pode criar vários conjuntos num servidor, mas não é possível adicionar bases de dados de diferentes servidores ao mesmo conjunto.

Camada de serviço do conjunto determina as funcionalidades disponíveis para os elastics no agrupamento e a quantidade máxima de recursos disponíveis para cada base de dados. Para obter detalhes, consulte os limites de recursos para conjuntos elásticos no [modelo DTU](sql-database-dtu-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels) e [vCore modelo](sql-database-vcore-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels).

Para configurar os recursos e de preço do conjunto, clique em **configurar conjunto**. Em seguida, selecione uma camada de serviço, adicione bases de dados para o conjunto e configurar os limites de recursos para o conjunto e respetivas bases de dados.

Quando tiver terminado de configurar o conjunto, pode clique em "Apply", nome do conjunto e clique em 'OK' para criar o conjunto.

### <a name="monitor-an-elastic-pool-and-its-databases"></a>Monitorizar um conjunto elástico e respetivas bases de dados

No portal do Azure, pode monitorizar a utilização de um conjunto elástico e as bases de dados nesse agrupamento. Também pode efetuar um conjunto de alterações para o conjunto elástico e submeter todas as alterações ao mesmo tempo. Estas alterações incluem a adição ou remoção de bases de dados, alterar as definições do conjunto elástico ou alterar as definições de base de dados.

Para iniciar a monitorização do conjunto elástico, localize e abra um conjunto elástico no portal. Primeiro, verá um ecrã que dá-lhe uma descrição geral do Estado do seu agrupamento elástico. Isto inclui:

* Monitorização de gráficos que mostra a utilização de recursos do conjunto elástico
* Os alertas recentes e as recomendações, se estiver disponível, para o conjunto elástico

O gráfico seguinte mostra um agrupamento elástico de exemplo:

![Vista de conjunto](./media/sql-database-elastic-pool-manage-portal/basic.png)

Se pretender obter mais informações sobre o agrupamento, pode clicar em qualquer uma das informações disponíveis nesta descrição geral. Clicar no **utilização de recursos** gráfico leva-o para a vista de monitorização do Azure onde pode personalizar a janela de métricas e a hora apresentada no gráfico. Clicar em qualquer notificações disponíveis leva-o para um painel que mostra todos os detalhes desse alerta ou uma recomendação.

Se gostaria de monitorizar as bases de dados dentro do conjunto, pode clicar em **base de dados de utilização de recursos** no **monitorização** secção do menu do recurso à esquerda.

![Página de utilização de recursos de base de dados](./media/sql-database-elastic-pool-manage-portal/db-utilization.png)

#### <a name="to-customize-the-chart-display"></a>Para personalizar a apresentação de gráfico

Pode editar o gráfico e a página de métrica para apresentar outras métricas, tais como a percentagem de CPU, percentagem de es de dados e a percentagem de es de registo utilizados.

No **editar gráfico** formulário, pode selecionar uma hora fixa intervalo ou clique em **personalizado** para selecionar qualquer período de 24 horas nas últimas duas semanas e, em seguida, selecione os recursos para monitorizar.

#### <a name="to-select-databases-to-monitor"></a>Para selecionar as bases de dados para monitorizar

Por predefinição, o gráfico no **utilização de recursos de base de dados** painel irá mostrar as bases de dados primeiros 5 pela DTU ou CPU (consoante o escalão de serviço). Pode mudar as bases de dados neste gráfico selecionando e unselecting bases de dados da lista abaixo do gráfico através das caixas de verificação à esquerda.

Também pode selecionar métricas mais para a vista de lado a lado nesta tabela de base de dados para obter uma vista mais completa do desempenho das bases de dados.

Para obter mais informações, consulte [criam alertas de base de dados SQL no portal do Azure](sql-database-insights-alerts-portal.md).

### <a name="manage-an-elastic-pool-and-its-databases"></a>Gerir um conjunto elástico e respetivas bases de dados

Todas as definições de agrupamento podem ser encontradas no mesmo local: o **configurar conjunto** painel. Para obter aqui, localizar um conjunto elástico no portal e clique **configurar conjunto** da parte superior do painel ou no menu de recursos, à esquerda.

Aqui pode efetuar qualquer combinação das seguintes alterações e guarde-los todos num batch:
1. Alterar o escalão de serviço do conjunto
2. Aumentar o desempenho (DTU ou vCores) e armazenamento ou para baixo
3. Adicionar ou remover bases de dados do agrupamento
4. Definir um mínimo (garantido) e máximo de limite de desempenho para os conjuntos de bases de dados
5. Reveja o resumo de custos para ver as alterações a fatura como resultado as suas seleções novo

![Painel de configuração do conjunto elástico](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

## <a name="manage-elastic-pools-and-databases-using-powershell"></a>Gerir conjuntos elásticos e bases de dados com o PowerShell

Para criar e gerir conjuntos elásticos da base de dados SQL com o Azure PowerShell, utilize os seguintes cmdlets PowerShell. Se precisar de instalar ou atualizar o PowerShell, consulte [módulo Azure PowerShell instalar](/powershell/azure/install-azurerm-ps). Para criar e gerir bases de dados, servidores e as regras de firewall, consulte [criar e gerir servidores SQL Database do Azure e as bases de dados com o PowerShell](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-powershell).

> [!TIP]
> Para scripts de exemplo do PowerShell, consulte [criar conjuntos elásticos e mover bases de dados entre conjuntos e fora de um conjunto com o PowerShell](scripts/sql-database-move-database-between-pools-powershell.md) e [utilize o PowerShell para monitorizar e dimensionar um agrupamento elástico de SQL na base de dados do Azure SQL](scripts/sql-database-monitor-and-scale-pool-powershell.md).
>

| Cmdlet | Descrição |
| --- | --- |
|[New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool)|Cria um conjunto de bases de dados elásticas num servidor lógico SQL.|
|[Get-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/get-azurermsqlelasticpool)|Obtém os conjuntos elásticos e os respetivos valores de propriedade num servidor SQL lógico.|
|[Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool)|Modifica as propriedades de um conjunto de bases de dados elásticas num servidor lógico SQL. Por exemplo, utilizar o **StorageMB** propriedade para modificar o armazenamento máximo do conjunto elástico.|
|[Remove-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/remove-azurermsqlelasticpool)|Elimina um conjunto de bases de dados elásticas num servidor lógico SQL.|
|[Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity)|Obtém o estado das operações num conjunto elástico num servidor SQL lógico.|
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Cria uma nova base de dados num conjunto existente ou como uma base de dados. |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Obtém uma ou mais bases de dados.|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Define as propriedades para uma base de dados ou move de uma base de dados para fora do ou entre conjuntos elásticos.|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Remove uma base de dados.|


> [!TIP]
> Criação de muitas bases de dados num agrupamento elástico pode demorar concluída ao utilizar o portal ou cmdlets do PowerShell que criam apenas uma única base de dados de cada vez. Para automatizar a criação de para um conjunto elástico, consulte o artigo [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).
>

## <a name="manage-elastic-pools-and-databases-using-the-azure-cli"></a>Gerir conjuntos elásticos e bases de dados utilizando a CLI do Azure

Para criar e gerir conjuntos elásticos da base de dados SQL com o [CLI do Azure](/cli/azure), utilize o seguinte [SQL Database do Azure CLI](/cli/azure/sql/db) comandos. Utilize o [Cloud Shell](/azure/cloud-shell/overview) para executar a CLI no seu browser ou [instale-a](/cli/azure/install-azure-cli) no macOS, Linux ou Windows.

> [!TIP]
> Para scripts de exemplo da CLI do Azure, consulte [CLI de utilização para mover uma base de dados SQL do Azure num agrupamento elástico de SQL](scripts/sql-database-move-database-between-pools-cli.md) e [CLI do Azure de utilização para dimensionar um agrupamento elástico de SQL na base de dados do Azure SQL](scripts/sql-database-scale-pool-cli.md).
>

| Cmdlet | Descrição |
| --- | --- |
|[az sql elastic-pool create](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_create)|Cria um conjunto elástico.|
|[az sql elastic-pool list](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list)|Devolve uma lista de conjuntos elásticos num servidor.|
|[az sql elastic-pool list-dbs](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_dbs)|Devolve uma lista de bases de dados num agrupamento elástico.|
|[az sql elastic-pool list-editions](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_editions)|Também inclui definições de DTU do agrupamento disponíveis, os limites de armazenamento e por definições de base de dados. Para reduzir verbosidade, limites de armazenamento adicional e por base de dados, as definições estão ocultadas por predefinição.|
|[az sql elastic-pool update](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update)|As atualizações de um conjunto elástico.|
|[az sql elastic-pool delete](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_delete)|Elimina o conjunto elástico.|

## <a name="manage-databases-within-elastic-pools-using-transact-sql"></a>Gerir bases de dados dentro de conjuntos elásticos, utilizando Transact-SQL

Para criar e mover bases de dados dentro de conjuntos elásticos existentes ou para devolver informações sobre um conjunto elástico da base de dados SQL com o Transact-SQL, utilize os seguintes comandos de T-SQL. Pode emitir estes comandos no portal do Azure, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), ou qualquer outro programa que pode ligar a um servidor da SQL Database do Azure e passar comandos Transact-SQL. Para criar e gerir bases de dados, servidores e as regras de firewall, consulte [criar e gerir servidores SQL Database do Azure e as bases de dados com o Transact-SQL](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-transact-sql).

> [!IMPORTANT]
> Não é possível criar, atualizar ou eliminar um agrupamento elástico de SQL Database do Azure com o Transact-SQL. Pode adicionar ou remover bases de dados a partir de um conjunto elástico e pode utilizar DMVs para devolver informações sobre conjuntos elásticos existentes.
>

| Comando | Descrição |
| --- | --- |
|[Criar base de dados (SQL Database do Azure)](/sql/t-sql/statements/create-database-azure-sql-database)|Cria uma nova base de dados num conjunto existente ou como uma base de dados. Tem de estar ligado à base de dados principal para criar uma nova base de dados.|
| [Falha de ALTER DATABASE (base de dados SQL do Azure)](/sql/t-sql/statements/alter-database-azure-sql-database) |Mova uma base de dados para, de ou entre conjuntos elásticos.|
|[REMOVER a base de dados (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Elimina uma base de dados.|
|[sys.elastic_pool_resource_stats (SQL Database do Azure)](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)|Devolve estatísticas de utilização de recursos para todos os conjuntos de bases de dados elásticas um servidor lógico. Para cada conjunto de bases de dados elásticas, não há uma linha para cada segundo 15 reporting janela (quatro linhas por minuto). Isto inclui a CPU, e/s, registo, o consumo de armazenamento e utilização de pedido/sessão em simultâneo por todas as bases de dados no conjunto.|
|[sys.database_service_objectives (SQL Database do Azure)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Devolve o edition (camada de serviço), o objetivo de serviço (escalão de preço) e o nome do conjunto elástico, se existir, para uma base de dados SQL do Azure ou um Azure SQL Data Warehouse. Se tem sessão iniciada base de dados mestra num servidor de base de dados do Azure SQL, devolve informações sobre todas as bases de dados. Para o Azure SQL Data Warehouse, tem de estar ligado à base de dados mestra.|

## <a name="manage-elastic-pools-and-databases-using-the-rest-api"></a>Gerir conjuntos elásticos e bases de dados utilizando a API REST

Para criar e gerir elástico da base de dados SQL agrupamentos utilizam estes pedidos de REST API.

| Comando | Descrição |
| --- | --- |
|[Conjuntos elásticos - criar ou atualizar](/rest/api/sql/elasticpools/createorupdate)|Cria um novo conjunto elástico ou atualiza um conjunto elástico existente.|
|[Eliminar conjuntos elásticos-](/rest/api/sql/elasticpools/delete)|Elimina o conjunto elástico.|
|[Obter conjuntos elásticos-](/rest/api/sql/elasticpools/get)|Obtém um conjunto elástico.|
|[Conjuntos elásticos - lista pelo servidor](/rest/api/sql/elasticpools/listbyserver)|Devolve uma lista de conjuntos elásticos num servidor.|
|[Conjuntos elásticos - atualização](/rest/api/sql/elasticpools/update)|As atualizações de um conjunto elástico existente.|
|[Recomenda conjuntos elásticos - Get](/rest/api/sql/recommendedelasticpools/get)|Obtém um conjunto elástico recommented.|
|[Recomenda conjuntos elásticos - lista pelo servidor](/rest/api/sql/recommendedelasticpools/listbyserver)|Devolve os conjuntos elásticos recomendados.|
|[Recomenda conjuntos elásticos - as métricas de lista](/rest/api/sql/recommendedelasticpools/listmetrics)|Devolve recommented métricas do conjunto elástico.|
|[Atividades do conjunto elástico](/rest/api/sql/elasticpoolactivities)|Devolve as atividades do conjunto elástico.|
|[Atividades de base de dados do conjunto elástico](/rest/api/sql/elasticpooldatabaseactivities)|Devolve a atividade em bases de dados dentro de um conjunto elástico.|
|[Bases de dados - criar ou atualizar](/rest/api/sql/databases/createorupdate)|Cria uma nova base de dados ou atualiza uma base de dados existente.|
|[Get - bases de dados](/rest/api/sql/databases/get)|Obtém uma base de dados.|
|[Para obter agrupamento elástico de bases de dados-](/rest/api/sql/databases/getbyelasticpool)|Obtém uma base de dados dentro de um conjunto elástico.|
|[Para obter as bases de dados - recomendado conjunto elástico](/rest/api/sql/databases/getbyrecommendedelasticpool)|Obtém uma base de dados dentro de um conjunto elástico recommented.|
|[Bases de dados - lista por agrupamento elástico](/rest/api/sql/databases/listbyelasticpool)|Devolve uma lista de bases de dados num agrupamento elástico.|
|[Bases de dados - lista por conjunto elástico recomendado](/rest/api/sql/databases/listbyrecommendedelasticpool)|Devolve uma lista de bases de dados dentro de um conjunto elástico recommented.|
|[Bases de dados - lista pelo servidor](/rest/api/sql/databases/listbyserver)|Devolve uma lista de bases de dados num servidor.|
|[Bases de dados - atualização](/rest/api/sql/databases/update)|Atualiza a base de dados existente.|

## <a name="next-steps"></a>Passos Seguintes

* Para obter um vídeo, consulte [decorrer vídeo do Microsoft Virtual Academy nas capacidades de elásticas de SQL Database do Azure](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
* Para saber mais sobre os padrões de estrutura de aplicações SaaS que utilizam conjuntos elásticos, consulte o artigo [Padrões de Estrutura de Aplicações SaaS Multi-inquilino com a Base de Dados SQL do Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* Para um tutorial de SaaS que utilizam conjuntos elásticos, consulte [introdução à aplicação Wingtip SaaS](sql-database-wtp-overview.md).
