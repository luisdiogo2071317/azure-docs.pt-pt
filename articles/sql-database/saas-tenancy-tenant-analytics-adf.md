---
title: Executar consultas de análises inquilino bases de dados utilizando o Azure SQL Data Warehouse | Microsoft Docs
description: Consultas de análises de inquilino entre utilizando os dados extraídos de várias bases de dados do SQL Database do Azure.
keywords: tutorial de base de dados sql
services: sql-database
documentationcenter: ''
author: anumjs
manager: craigg
editor: MightyPen
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/08/2017
ms.author: anjangsh
ms.openlocfilehash: c7580e5481288695d3b5dea8fd0547f5f2c4c2b0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34644006"
---
# <a name="explore-saas-analytics-with-azure-sql-database-sql-data-warehouse-data-factory-and-power-bi"></a>Explorar a análise de SaaS com a SQL Database do Azure, o SQL Data Warehouse, o fábrica de dados e o Power BI

Neste tutorial, percorrer um cenário de análise de ponto a ponto. O cenário demonstra como análise de dados de inquilino pode capacitar os fornecedores de software para tomar decisões inteligentes. Utilizar dados extraídos a partir de cada base de dados do inquilino, utilize análise para obter informações acerca comportamento de inquilino, incluindo a utilização da aplicação exemplo Wingtip bilhetes SaaS. Este cenário envolve três passos: 

1.  **Extrair dados** cada inquilino da base de dados para um arquivo de análise, neste caso, um SQL Data Warehouse.
2.  **Otimizar os dados extraídos** para processamento de análise.
3.  Utilize **Business Intelligence** ferramentas para desenhar horizontalmente as informações úteis, podem guia faz de decisão. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> - Crie o inquilino análise armazenar para carregar.
> - Utilize o Azure Data Factory (ADF) para extrair dados de cada base de dados do inquilino para o armazém de dados de análise.
> - Otimize os dados extraídos (reorganize para um esquema de estrela).
> - Consulta o armazém de dados de análise.
> - Utilize o Power BI para visualização de dados para realçar as tendências em dados de inquilino e efetuar a recomendação para melhoramentos.

![architectureOverView](media/saas-tenancy-tenant-analytics/adf_overview.png)

## <a name="analytics-over-extracted-tenant-data"></a>Análise de dados extraídos inquilino

Aplicações SaaS manter uma potencialmente grande quantidade de dados de inquilino na nuvem. Estes dados podem fornecer uma origem avançada das informações sobre o funcionamento e a utilização da sua aplicação e o comportamento dos seus inquilinos. Estas informações podem ajudá desenvolvimento da funcionalidade, melhoramentos de Facilidade de utilização e outros investimentos no plataforma e aplicações.

Acesso aos dados para todos os inquilinos é simple quando todos os dados apenas uma base de dados do multi-inquilino. Mas acesso for mais complexo quando distribuídos à escala milhares de bases de dados. É uma forma de tame a complexidade para extrair os dados para uma base de dados de análise ou um armazém de dados para a consulta.

Este tutorial apresenta um cenário de análise de ponto a ponto para a aplicação de Wingtip pedidos de suporte. Primeiro, [fábrica de dados do Azure (ADF)](../data-factory/introduction.md) é utilizado como a ferramenta de orquestração, a extrair dados de vendas e relacionadas com pedidos de suporte de cada base de dados do inquilino. Estes dados são carregados para tabelas de testes num arquivo de análise. O arquivo de análise pode ser uma base de dados do SQL Server ou um SQL Data Warehouse. Este tutorial utiliza [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) como a análise de armazenar.

Em seguida, os dados extraídos são transformados e carregados para um conjunto de [esquemas de estrela](https://www.wikipedia.org/wiki/Star_schema) tabelas. As tabelas consistem de uma tabela de factos central plus as tabelas de dimensões relacionados:

- A tabela de factos central no esquema de estrela contém dados de pedido de suporte.
- As tabelas de dimensão contém dados sobre venues, eventos, os clientes e as datas de compra.

Em conjunto os central e tabelas ativar eficiente analíticos processamento de dimensão. O esquema de estrela utilizado neste tutorial, é apresentado na imagem seguinte:
 
![architectureOverView](media/saas-tenancy-tenant-analytics/starschematables.JPG)

Por fim, as tabelas de esquema de estrela são consultadas. Os resultados da consulta são apresentados visualmente através do Power BI para realçar as informações sobre o comportamento de inquilino e a sua utilização da aplicação. Com este esquema de estrela, execute consultas que expõem:

- Quem é comprar permissões e a partir da qual venue.
- Padrões e tendências de venda de bilhetes.
- A popularidade relativa de cada venue.

Este tutorial fornece exemplos básicos das informações que podem ser obtidas a partir dos dados de pedidos de Wingtip. Compreender a forma como cada venue utiliza o serviço poderá fazer com que o fornecedor de bilhetes Wingtip pensar sobre os planos de serviço diferente direcionados venues maior ou menor do Active Directory, por exemplo. 

## <a name="setup"></a>Configurar

### <a name="prerequisites"></a>Pré-requisitos

> [!NOTE]
> Este tutorial utiliza funcionalidades do Azure Data Factory que estão atualmente a ser uma versão de pré-visualização limitada (serviço ligado parametrização forced). Se pretender fazer neste tutorial, forneça o ID de subscrição [aqui](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxrVywox1_tHk9wgd5P8SVJUNlFINjNEOElTVFdMUEREMjVVUlJCUDdIRyQlQCN0PWcu). Será enviada uma confirmação assim que a sua subscrição tiver sido ativada.

Para concluir este tutorial, devem ser cumpridos os seguintes pré-requisitos:
- A aplicação de Wingtip bilhetes SaaS da base de dados por inquilino é implementada. Para implementar em menos de cinco minutos, consulte [implementar e explorar a aplicação Wingtip SaaS](saas-dbpertenant-get-started-deploy.md).
- Os scripts de Wingtip bilhetes SaaS da base de dados por inquilino e a aplicação [código fonte](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant/) são transferidos a partir do GitHub. Consulte o artigo de instruções de transferência. Certifique-se para *desbloquear o ficheiro zip* antes de a extrair o respetivo conteúdo.
- Ambiente de trabalho do Power BI está instalado. [Transfira o Power BI Desktop](https://powerbi.microsoft.com/downloads/).
- O lote de inquilinos adicionais tiver sido aprovisionado, consulte o [ **aprovisionar inquilinos tutorial**](saas-dbpertenant-provision-and-catalog.md).

### <a name="create-data-for-the-demo"></a>Criar dados de demonstração de

Este tutorial explicar análise através de dados de vendas de permissão. Neste passo, gerar dados de pedido de suporte para todos os inquilinos. Num passo posterior, são extraídos estes dados para análise. _Certifique-se de que aprovisionou o lote de inquilinos_ (conforme descrito anteriormente), de modo a que tem dados suficientes para expor uma gama de diferentes permissão padrões de compras.

1. No ISE do PowerShell, abra *...\Learning Modules\Operational Analytics\Tenant análise DW\Demo-TenantAnalyticsDW.ps1*e defina o valor seguinte:
    - **$DemoScenario** = **1** para eventos em venues todos os pedidos de suporte de compra
2. Prima **F5** para executar o script e criar pedido de compra do histórico para todos os venues. Com 20 inquilinos, o script gera dezenas de milhares de pedidos de suporte e poderá demorar 10 minutos ou mais.

### <a name="deploy-sql-data-warehouse-data-factory-and-blob-storage"></a>Implementar o SQL Data Warehouse, fábrica de dados e armazenamento de BLOBs 
Na aplicação Wingtip bilhetes, dados transacionais dos inquilinos são distribuídos por muitas bases de dados. Fábrica de dados do Azure (ADF) é utilizado para orquestrar a extração, carregamento e transformação (ELT) destes dados para o armazém de dados. Para carregar dados para o SQL Data Warehouse mais eficientemente, ADF extrai dados em ficheiros de blob intermédio e, em seguida, utiliza [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/design-elt-data-loading) para carregar os dados no armazém de dados.   

Neste passo, pode implementa os recursos adicionais que utilizou no tutorial: um SQL Data Warehouse chamado _tenantanalytics_, denominado de um Azure Data Factory _dbtodwload -\<utilizador\>_  , e uma conta de armazenamento do Azure denominada _wingtipstaging\<utilizador\>_. A conta de armazenamento é utilizada para armazenar temporariamente os ficheiros de dados extraídos como blobs antes de serem carregados para o armazém de dados. Este passo também implementa o esquema do armazém de dados e define os pipelines ADF orquestram o processo ELT.
1. No ISE do PowerShell, abra *...\Learning Modules\Operational Analytics\Tenant análise DW\Demo-TenantAnalyticsDW.ps1* e definir:
    - **$DemoScenario** = **2** implementar armazém de dados de análise do inquilino, armazenamento de BLOBs e fábrica de dados 
1. Prima **F5** para executar o script de demonstração e implementar os recursos do Azure. 

Agora, reveja os recursos do Azure que implementou:
#### <a name="tenant-databases-and-analytics-store"></a>Arquivo de bases de dados e análise de inquilino
Utilize [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) para ligar ao **tenants1-dpt -&lt;utilizador&gt;**  e **catálogo-dpt -&lt;utilizador&gt;**  servidores. Substitua &lt;utilizador&gt; com o valor utilizado quando implementou a aplicação. Utilizar o início de sessão = *programador* e palavra-passe = *P@ssword1*. Consulte o [tutorial introdutórias](saas-dbpertenant-wingtip-app-overview.md) para obter mais orientações.

![Ligar ao servidor de base de dados SQL do SSMS](media/saas-tenancy-tenant-analytics/ssmsSignIn.JPG)

No Explorador de objetos:

1. Expanda o *tenants1-dpt -&lt;utilizador&gt;*  servidor.
1. Expanda o nó de bases de dados e ver a lista de bases de dados do inquilino.
1. Expanda o *catálogo-dpt -&lt;utilizador&gt;*  servidor.
1. Certifique-se de que vê a análise de armazenar que contém os seguintes objetos:
    1. As tabelas **raw_Tickets**, **raw_Customers**, **raw_Events** e **raw_Venues** conter dados não processados de extraídos das bases de dados do inquilino.
    1. As tabelas de esquema de estrela são **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events**, e **dim_Dates** .
    1. O procedimento armazenado, **sp_transformExtractedData** é utilizado para transformar os dados e carregue-o nas tabelas de esquema de estrela.

![DWtables](media/saas-tenancy-tenant-analytics/DWtables.JPG)

#### <a name="blob-storage"></a>Armazenamento de blobs
1. No [Portal do Azure](https://ms.portal.azure.com), navegue para o grupo de recursos que utilizou para implementar a aplicação. Certifique-se de que uma conta de armazenamento denominado **wingtipstaging\<utilizador\>**  foi adicionado.

  ![DWtables](media/saas-tenancy-tenant-analytics/adf-staging-storage.PNG)

1. Clique em **wingtipstaging\<utilizador\>**  conta de armazenamento para explorar os objetos presentes.
1. Clique em **Blobs** mosaico
1. Clique em contentor **configfile**
1. Certifique-se de que **configfile** contém um ficheiro JSON chamado **TableConfig.json**. Este ficheiro contém os nomes de tabela de origem e de destino, os nomes das colunas e nome de coluna de controlador.

#### <a name="azure-data-factory-adf"></a>Fábrica de dados do Azure (ADF)
No [Portal do Azure](https://ms.portal.azure.com) no grupo de recursos, certifique-se de que chama um Azure Data Factory _dbtodwload -\<utilizador\>_  foi adicionado. 

 ![adf_portal](media/saas-tenancy-tenant-analytics/adf-data-factory-portal.png)

Esta secção explicar fábrica de dados criada. Siga os passos abaixo para iniciar a fábrica de dados:
1. No portal, clique em fábrica de dados chamada **dbtodwload -\<utilizador\>**.
2. Clique em **autor & Monitor** mosaico para iniciar o estruturador de fábrica de dados num separador separado. 

## <a name="extract-load-and-transform-data"></a>Extrair, a carga e transforme dados
O Azure Data Factory é utilizado para da orquestração extração, carregamento e transformação de dados. Neste tutorial, a extrair dados de quatro vistas SQL diferentes de cada uma das bases de dados do inquilino: **rawTickets**, **rawCustomers**, **rawEvents**, e  **rawVenues**. Estas vistas incluem venue Id, pelo que pode discriminate dados a partir de cada venue no armazém de dados. Os dados são carregados nas tabelas de transição correspondentes no armazém de dados: **raw_Tickets**, **raw_customers**, **raw_Events** e **raw_Venue**. Um procedimento armazenado, em seguida, transforma os dados não processados e preenche as tabelas de esquema de estrela: **fact_Tickets**, **dim_Customers**, **dim_Venues**, **dim_Events** , e **dim_Dates**.

Na secção anterior, implementadas e inicializar os recursos do Azure necessários, incluindo a fábrica de dados. A fábrica de dados implementado inclui os pipelines, conjuntos de dados, serviços ligados, etc., necessário para extrair, carregar e transformar os dados de inquilino. Vamos explorar estes objetos adicional e, em seguida, o pipeline para mover dados de bases de dados do inquilino para o armazém de dados de Acionador.

### <a name="data-factory-pipeline-overview"></a>Descrição geral de pipeline do Data factory
Esta secção explicar os objetos criados na fábrica de dados. A figura seguinte descreve o fluxo de trabalho geral do pipeline ADF utilizado neste tutorial. Se preferir explorar o pipeline mais tarde e ver os resultados pela primeira vez, avance para a secção seguinte **acionar o pipeline executar**.

![adf_overview](media/saas-tenancy-tenant-analytics/adf-data-factory.PNG)

Na página Descrição geral do comutador para **autor** separador no painel esquerdo e reparar que existem três [pipelines](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) e três [conjuntos de dados](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services) criado.
![adf_author](media/saas-tenancy-tenant-analytics/adf_author_tab.JPG)

São três pipelines aninhadas: SQLDBToDW, DBCopy e TableCopy.

**O pipeline 1 - SQLDBToDW** procura os nomes das bases de dados inquilino armazenados na base de dados de catálogo (nome de tabela: [__ShardManagement]. [ ShardsGlobal]) e para cada base de dados do inquilino, executa a **DBCopy** pipeline. Após a conclusão, fornecido **sp_TransformExtractedData** esquema de procedimento armazenado, é executada. Este procedimento armazenado transforma os dados carregados em tabelas de testes e preenche as tabelas de esquema de estrela.

**O pipeline 2 - DBCopy** procura os nomes das tabelas de origem e colunas de um ficheiro de configuração armazenado no blob storage.  O **TableCopy** pipeline, em seguida, é executado para cada um dos quatro tabelas: TicketFacts, CustomerFacts, EventFacts e VenueFacts. O **[Foreach](https://docs.microsoft.com/azure/data-factory/control-flow-for-each-activity)** atividade executa em paralelo para todas as bases de 20 dados. ADF permite um máximo de iterações de ciclo 20 para ser executado em paralelo. Considere criar múltiplas pipelines para bases de dados mais.    

**Pipeline de 3 - TableCopy** utiliza números de versão de linhas na base de dados do SQL Server (_rowversion_) para identificar as linhas que foi alteradas ou atualizadas. Esta atividade procura o início e a versão de linha de fim para extrair as linhas das tabelas de origem. O **CopyTracker** tabela armazenada na base de dados cada inquilino controla a última linha extraída de cada tabela de origem em cada execução. Linhas de novas ou alteradas são copiadas para as tabelas de transição correspondentes no armazém de dados: **raw_Tickets**, **raw_Customers**, **raw_Venues**, e **raw_ Eventos**. Por fim, a última versão de linha é guardada no **CopyTracker** tabela a ser utilizado como a versão de linha inicial para a extração seguinte. 

Existem também três parametrizados serviços ligados dessa ligação fábrica de dados à origem de bases de dados SQL, o armazém de dados do SQL Server de destino e intermédio do Blob storage do. No **autor** separador, clique em **ligações** para explorar os serviços ligados, conforme mostrado na imagem seguinte:

![adf_linkedservices](media/saas-tenancy-tenant-analytics/linkedservices.JPG)

Correspondente para os três serviços ligados, existem três conjuntos de dados que se referem para os dados que utilize as atividades de pipeline como entradas ou saídas. Explore cada um dos conjuntos de dados para observar as ligações e os parâmetros utilizados. _AzureBlob_ aponta para o ficheiro de configuração que contém as tabelas de origem e de destino e colunas, bem como a coluna de controlador em cada origem.
  
### <a name="data-warehouse-pattern-overview"></a>Descrição geral de padrão do armazém de dados
SQL Data Warehouse é utilizado como o arquivo de análise para executar a agregação nos dados de inquilino. Neste exemplo, o PolyBase é utilizado para carregar dados para o armazém de dados do SQL Server. Dados não processados são carregados para tabelas de testes que têm uma coluna de identidade para controlar as linhas que tenham sido transformadas as tabelas de esquema de estrela. A imagem seguinte mostra o padrão de carregamento: ![loadingpattern](media/saas-tenancy-tenant-analytics/loadingpattern.JPG)

Lentamente tabelas de dimensão 1 do tipo de dimensão alterar (SCD) são utilizadas neste exemplo. Cada dimensão tem uma substituto foi definida uma chave a utilizar uma coluna de identidade. Como melhor prática, a tabela de dimensão de data é pré-preenchidos para poupar tempo. Para outras tabelas de dimensões, uma criar tabela como SELECT... Instrução (CTAS) é utilizada para criar uma tabela temporária que contém as linhas de modificada e não modificado existentes, juntamente com as chaves de substituição. Isto é feito com IDENTITY_INSERT = ON. Novas linhas, em seguida, são inseridas na tabela com IDENTITY_INSERT = OFF. Para fácil reversão, a tabela de dimensão existente é mudada e a tabela temporária é mudada para tornar-se a nova tabela de dimensão. Antes de cada execução, a tabela de dimensão antigo é eliminada.

As tabelas de dimensões são carregadas antes da tabela de factos. Esta sequência garante que de facto cada atrasado, todas as dimensões referenciadas já existem. Como os factos são carregados, a chave de negócio para cada dimensão correspondente é correspondida e as correspondentes chaves de substituição são adicionadas ao facto de cada.

Passo final da transformação elimina os dados de testes prontos para a execução seguinte do pipeline.
   
### <a name="trigger-the-pipeline-run"></a>Acionar o pipeline de executar
Siga os passos abaixo para executar a extrair, carregamento e transformação pipeline para todas as bases de dados do inquilino:
1. No **autor** separador da interface de utilizador ADF, selecione **SQLDBToDW** pipeline no painel esquerdo.
1. Clique em **acionador** e do solicitados menu pendente, clique em **acionador agora**. Esta ação executa imediatamente o pipeline. Num cenário de produção, deverá definir uma timetable para executar o pipeline para atualizar os dados com base numa agenda.
  ![adf_trigger](media/saas-tenancy-tenant-analytics/adf_trigger.JPG)
1. No **Pipeline executar** página, clique em **concluir**.
 
### <a name="monitor-the-pipeline-run"></a>Monitorizar a execução do pipeline.
1. Na interface de utilizador do ADF, mude para o **Monitor** separador no menu à esquerda.
1. Clique em **atualizar** até que o estado do pipeline de SQLDBToDW é **com êxito**.
  ![adf_monitoring](media/saas-tenancy-tenant-analytics/adf_monitoring.JPG)
1. Ligar ao armazém de dados com o SSMS e consultar as tabelas de esquema de estrela para verificar que os dados foram carregados nestas tabelas.

Depois de concluída o pipeline, a tabela de factos contém dados de vendas de permissão para todos os venues e as tabelas de dimensão são preenchidas com o venues correspondentes, eventos e os clientes.

## <a name="data-exploration"></a>Exploração de dados

### <a name="visualize-tenant-data"></a>Visualizar dados de inquilino

Os dados do esquema de estrela fornecem todos os permissão vendas dados necessários para a sua análise. Visualizar dados graficamente torna mais fácil ver as tendências de grandes conjuntos de dados. Esta secção, irá utilizar **Power BI** para manipular e visualizar os dados de inquilino no armazém de dados.

Utilize os seguintes passos para ligar ao Power BI e, para importar as vistas que criou anteriormente:

1. Inicie o ambiente de trabalho do Power BI.
2. A partir do Friso inicial, selecione **obter dados**e selecione **mais...** no menu.
3. No **obter dados** janela, selecione **SQL Database do Azure**.
4. Na janela de início de sessão da base de dados, introduza o nome do servidor (**catálogo-dpt -&lt;utilizador&gt;. database.windows.net**). Selecione **importação** para **modo de conectividade de dados**e, em seguida, clique em **OK**. 

    ![início de sessão-na-para-power-bi](./media/saas-tenancy-tenant-analytics/powerBISignIn.PNG)

5. Selecione **base de dados** no painel esquerdo, em seguida, introduza o nome de utilizador = *programador*e introduza a palavra-passe = *P@ssword1*. Clique em **Ligar**.  

    ![base de dados-início de sessão](./media/saas-tenancy-tenant-analytics/databaseSignIn.PNG)

6. No **navegador** painel, sob a base de dados de análise, selecione as tabelas de esquema de estrela: **fact_Tickets**, **dim_Events**, **dim_Venues**, **dim_Customers** e **dim_Dates**. Em seguida, selecione **carga**. 

Parabéns! Carregar os dados no Power BI com êxito. Agora, explore visualizações interessantes para obter informações sobre os seus inquilinos. Vamos guiá-como a análise pode fornecer algumas recomendações condicionada por dados para a equipa de negócio Wingtip pedidos de suporte. As recomendações podem ajudar a otimizar a experiência de cliente e modelo de negócio.

Comece por analisar os dados de vendas permissão para ver a variação na utilização em toda as venues. Selecione as opções apresentadas no Power BI para desenhar um gráfico de barras do número total de pedidos de suporte de artigos vendidos por cada venue. (Devido a variação aleatória no gerador de permissão, os resultados podem ser por diferentes.)
 
![TotalTicketsByVenues](./media/saas-tenancy-tenant-analytics/TotalTicketsByVenues-DW.PNG)

O desenho anterior confirma que o número de pedidos de suporte de artigos vendidos por cada venue varia. Venues propor mais pedidos estão a utilizar o serviço mais fortemente venues propor menos permissões. Pode haver uma oportunidade para personalizar a alocação de recursos de acordo com as necessidades de inquilino diferente.

Ainda pode analisar os dados para ver como vendas permissão variam ao longo do tempo. Selecione as opções apresentadas na imagem seguinte no Power BI para desenhar o número total de pedidos de suporte de artigos vendidos por dia durante um período de 60 dias.
 
![SaleVersusDate](./media/saas-tenancy-tenant-analytics/SaleVersusDate-DW.PNG)

O gráfico anterior mostra esse pico de pedidos vendas do pedido de suporte para alguns venues. Estes picos de impor a ideia que alguns venues poderão estar a consumir recursos de sistema disproportionately. Até ao momento não é não existe nenhuma padrão óbvios no quando ocorrem os picos de.

Seguinte vamos investigar a significância destes dias de venda das horas de ponta. Quando estas picos ocorrer depois de pedidos de suporte, aceda à venda? Para desenhar bilhetes vendidos por dia, selecione as opções apresentadas na imagem seguinte no Power BI.

![SaleDayDistribution](./media/saas-tenancy-tenant-analytics/SaleDistributionPerDay-DW.PNG)

Este desenho mostra que alguns venues propor grande número de pedidos de suporte no primeiro dia de venda. Assim que as permissões aceda à venda nestes venues, parece haver um rush mad. Este rajada de atividade por alguns venues poderá afetar o serviço para os outros inquilinos.

Pode explorar os dados novamente para ver se este rush mad é verdadeiro para todos os eventos hospedados estes venues. No rastreia anterior, vimos que Contoso Concert Hall sells muitos pedidos e que Contoso tem também um pico de pedidos na vendas de permissão em determinados dias. Reproduzir em torno com opções do Power BI para desenhar vendas permissão cumulativa para Contoso Concert Hall, concentrar-se em tendências de venda para cada um dos seus eventos. Todos os eventos siga o mesmo padrão de venda? Tente produzir um desenho como abaixo.

![ContosoSales](media/saas-tenancy-tenant-analytics/EventSaleTrends.PNG)

Este desenho de vendas cumulativa permissão ao longo do tempo para Contoso Concert Hall para cada evento mostra que o rush mad não acontecer para todos os eventos. Reproduzir em torno com as opções de filtro para explorar tendências de venda para outros venues.

As informações para vender padrões de pedido de suporte poderão originar Wingtip permissões para otimizar o seu modelo de negócio. Em vez de charging todos os inquilinos igualmente, talvez Wingtip deve dar origem a escalões de serviço com níveis de desempenho diferentes. Venues maior do que necessita para vender mais pedidos por dia foi oferecidas um escalão superior com um superior contrato de nível serviço (SLA). Esses venues podem ter as bases de dados colocados em conjunto com limites de recursos por base de dados superiores. Cada escalão de serviço pode ter uma alocação de venda por hora, com taxas adicionais cobradas exceder a alocação. Venues maiores que tenham bursts periódicas de vendas seriam beneficiar os escalões superiores e Wingtip permissões pode monetizar seu serviço de forma mais eficiente.

Entretanto, alguns clientes de bilhetes Wingtip queixarem de que estes dificuldade vender suficiente permissões para indicar o custo do serviço. Talvez nestas informações não há uma oportunidade para melhorar vendas de permissão para underperforming venues. Vendas superiores seriam aumentar o valor do serviço percetível. Clique com o botão direito do rato em fact_Tickets e selecione **nova medida**. Introduza a seguinte expressão da medida nova denominada **AverageTicketsSold**:

```
AverageTicketsSold = DIVIDE(DIVIDE(COUNTROWS(fact_Tickets),DISTINCT(dim_Venues[VenueCapacity]))*100, COUNTROWS(dim_Events))
```

Selecione as seguintes opções de visualização para desenhar os pedidos de suporte de percentagem vendidos por cada venue para determinar os respetivos êxito relativo.

![AvgTicketsByVenues](media/saas-tenancy-tenant-analytics/AvgTicketsByVenues-DW.PNG)

O desenho acima mostra que, apesar da maioria das venues propor mais de 80% dos respetivos bilhetes, alguns são struggling preencher mais de metade os seus utilizadores individuais. Reproduzir em torno com os valores bem para selecionar a percentagem de máxima ou mínima de permissões vendida para cada venue.

## <a name="embedding-analytics-in-your-apps"></a>Análise de incorporar nas suas aplicações 
Este tutorial tem centra-se na análise de inquilino entre utilizadas para melhorar a compreensão do fornecedor de software dos respetivos inquilinos. Análise de também pode fornecer informações para o _inquilinos_, para ajudar a gerir os seus negócios com mais eficiência próprios. 

No exemplo Wingtip permissões, poderá anteriormente detetados que vendas permissão tendem a seguir padrões previsíveis. Este informações podem ser utilizadas para ajudar a underperforming venues intensificação permissão vendas. Talvez é uma oportunidade para utilizar as técnicas de aprendizagem máquina para prever vendas de pedido de suporte para os eventos. Os efeitos das alterações de preços foi também ser modelados, para permitir que o impacto da oferta descontos para possível prever. Power BI Embedded pode ser integrada numa aplicação de gestão de eventos para visualizar vendas previstas, incluindo o impacto de descontos no totais licenças vendidos e a receita eventos vender em baixo. Com o Power BI Embedded, pode mesmo integrar a aplicação, na verdade, de desconto aos preços de permissão, com o botão direito na experiência de visualização.


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Implemente um SQL Data Warehouse preenchida com um esquema em estrela para análise de inquilino.
> * Utilize o Azure Data Factory para extrair dados de cada base de dados do inquilino para o armazém de dados de análise.
> * Otimize os dados extraídos (reorganize para um esquema de estrela).
> * Consulta o armazém de dados de análise. 
> * Utilize o Power BI para visualizar tendências em dados em todos os inquilinos.

Parabéns!

## <a name="additional-resources"></a>Recursos adicionais

- Adicionais [tutoriais tirar partido da aplicação Wingtip SaaS](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials).
