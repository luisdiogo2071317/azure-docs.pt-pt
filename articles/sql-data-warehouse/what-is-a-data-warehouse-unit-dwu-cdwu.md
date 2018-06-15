---
title: Unidades do Data Warehouse (DWUs, cDWUs) no Azure SQL Data Warehouse | Microsoft Docs
description: Recomendações sobre como escolher o número ideal de unidades do data warehouse (DWUs, cDWUs) para otimizar o preço e desempenho e como alterar o número de unidades.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 94791e4dc3d3c841dde4685d34d4e3fdaf7d9af7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32185965"
---
# <a name="data-warehouse-units-dwus-and-compute-data-warehouse-units-cdwus"></a>Unidades do Data Warehouse (DWUs) e computação unidades do Data Warehouse (cDWUs)
Recomendações sobre como escolher o número ideal de unidades do data warehouse (DWUs, cDWUs) para otimizar o preço e desempenho e como alterar o número de unidades. 

## <a name="what-are-data-warehouse-units"></a>Quais são as unidades do Data Warehouse?
Com o SQL Data Warehouse CPU, memória e e/s estão incluídas em unidades de escala de computação chamado unidades do Data Warehouse (DWUs). Uma DWU representa uma medida abstracta, normalizada de recursos de computação e o desempenho. Ao alterar o nível de serviço alterar o número de DWUs que estão atribuídas ao sistema, que por sua vez ajusta o desempenho e o custo, do seu sistema. 

Pagar para um desempenho superior, pode aumentar o número de unidades do data warehouse. Pagar para um desempenho inferior, reduza unidades do data warehouse. Os custos de armazenamento e computação são faturados separadamente, para que unidades de armazém de dados de alteração não afetam os custos de armazenamento.

Desempenho de unidades do data warehouse baseia-se nestas métricas de carga de trabalho do armazém de dados:

- A rapidez pode uma padrão a dados warehousing consulta analisar um grande número de linhas e de, em seguida, efetuar uma agregação complexa? Esta operação é intensivos em termos de e/s e CPU.
- A rapidez pode no armazém de dados ingestão de dados de Blobs de armazenamento do Azure ou do Azure Data Lake? Esta operação é intensivo da CPU e de rede. 
- A rapidez pode a [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) comando T-SQL copiar uma tabela? Esta operação envolve a leitura de dados do armazenamento, a distribuição dos mesmos por nós do dispositivo de e a escrita para o armazenamento novo. Esta operação é CPU, e/s e que consomem muita rede.

Aumentar as DWUs:
- Alterações de forma linear desempenho do sistema para análises, agregações e CTAS instruções
- Aumenta o número de leitores e escritores para operações de carga do PolyBase
- Aumenta o número máximo de consultas em simultâneo e de ranhuras de concorrência.

## <a name="service-level-objective"></a>Objetivo de nível de serviço
O objetivo de nível de serviço (SLO) é a definição de escalabilidade que determina o nível de desempenho e custo do seu armazém de dados. Os níveis de serviço para Gen2 são medidos em computação unidades do data warehouse (cDWU), por exemplo DW2000c. Níveis de serviço Gen1 são medidas em DWUs, por exemplo, DW2000. 

Em T-SQL a definição de SERVICE_OBJECTIVE determina o nível de serviço e a camada de desempenho para o seu armazém de dados.

```sql
--Gen1
CREATE DATABASE myElasticSQLDW
WITH
(    SERVICE_OBJECTIVE = 'DW1000'
)
;

--Gen2
CREATE DATABASE myComputeSQLDW
WITH
(    SERVICE_OBJECTIVE = 'DW1000c'
)
;
```

## <a name="performance-tiers-and-data-warehouse-units"></a>Os escalões de desempenho e unidades do Data Warehouse

Cada escalão de desempenho utiliza uma unidade de medida ligeiramente diferente para as unidades do data warehouse. Esta diferença é refletida na fatura como a unidade de escala traduz-se diretamente a faturação.

- Os armazéns de dados de Gen1 são medidos em unidades do Data Warehouse (DWUs).
- Gen2 dados warehousesr é medido em unidades do Data Warehouse (cDWUs) de computação. 

As DWUs e cDWUs suporta computação aumentar ou reduzir verticalmente e colocar em pausa computação quando não precisa de utilizar o armazém de dados. Estas operações são todas as-a pedido. Gen2 utiliza uma cache com base em disco local em nós de computação para melhorar o desempenho. Quando dimensionar ou colocar em pausa o sistema, a cache é invalidada e, por isso, durante um período de cache warming é necessário antes de um desempenho ideal é alcançado.  

Como aumentar unidades do data warehouse, forma linear estão aumentar recursos informáticos. Gen2 fornece o melhor desempenho das consultas e a escala mais elevada, mas tem um preço de entrada superior. Foi concebido para as empresas que têm um pedido de constante de desempenho. Estes sistemas de efetuar a maioria dos utilização da cache. 

### <a name="capacity-limits"></a>Limites de capacidade
Cada SQL server (por exemplo, myserver.database.windows.net) tem um [unidade de transação de base de dados (DTU)](../sql-database/sql-database-what-is-a-dtu.md) quota que permite que um número específico de unidades do data warehouse. Para obter mais informações, consulte o [os limites de capacidade de gestão de carga de trabalho](sql-data-warehouse-service-capacity-limits.md#workload-management).


## <a name="how-many-data-warehouse-units-do-i-need"></a>Como muitas unidades do data warehouse que preciso?
O número ideal de unidades do data warehouse depende muito a carga de trabalho e a quantidade de dados que carregou no sistema.

Passos para localizar o DWU melhor para a carga de trabalho:

1. Comece por selecionar uma DWU mais pequeno. 
2. Monitorize o desempenho de aplicações como testar as cargas de dados no sistema, observar o número de DWUs selecionado em comparação comparado o desempenho que observar.
3. Identifique quaisquer requisitos adicionais para periódicos períodos de atividade de pico. Se a carga de trabalho mostra picos significativos e troughs na atividade e existir um bom motivo para dimensionar com frequência.

O SQL Data Warehouse é um sistema de escalamento horizontal que pode aprovisionar grandes quantidades de quantidades que a computação e de consulta de dados. Para ver as suas verdadeiras capacidades de dimensionamento, especialmente em DWUs maiores, recomendamos que o conjunto de dados de dimensionamento, à medida que para se certificar de que tem dados suficientes para as CPUs do feed. Para testar a escala, recomendamos que utilize, pelo menos, 1 TB.

> [!NOTE]
>
> Desempenho de consulta apenas aumenta com mais parallelization se o trabalho pode ser dividido entre nós de computação. Se achar que dimensionamento é não alterar o desempenho, poderá ter de otimizar a estrutura da tabela e/ou as suas consultas. Para obter orientações sobre a otimização de consulta, consulte [gerir consultas do utilizador](sql-data-warehouse-overview-manage-user-queries.md). 

## <a name="permissions"></a>Permissões

Alterar as unidades de armazém de dados requer que as permissões descritas [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql). 

## <a name="view-current-dwu-settings"></a>Ver definições de DWU atuais

Para ver a definição de DWU atual:

1. Abra o SQL Server Object Explorer no Visual Studio.
2. Ligar à base de dados mestra associado o servidor lógico de base de dados SQL.
3. Selecione a vista de gestão dinâmica sys.database_service_objectives. Segue-se um exemplo: 

```sql
SELECT  db.name [Database]
,       ds.edition [Edition]
,       ds.service_objective [Service Objective]
FROM    sys.database_service_objectives   AS ds
JOIN    sys.databases                     AS db ON ds.database_id = db.database_id
;
```

## <a name="change-data-warehouse-units"></a>Unidades de armazém de dados de alteração

### <a name="azure-portal"></a>Portal do Azure
Para alterar as DWUs ou cDWUs:

1. Abra o [portal do Azure](https://portal.azure.com), abra a sua base de dados e clique em **escala**.

2. Em **escala**, mova o controlo de deslize à esquerda ou o botão direito do rato para alterar a definição de DWU.

3. Clique em **Guardar**. É apresentada uma mensagem de confirmação. Clique em **sim** para confirmar ou **não** para cancelar.

### <a name="powershell"></a>PowerShell
Para alterar as DWUs ou cDWUs, utilize o [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) cmdlet do PowerShell. O exemplo a seguir define o objetivo de nível de serviço para DW1000 MySQLDW que está alojado no servidor MyServer da base de dados.

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

Para obter mais informações, consulte [cmdlets do PowerShell para o SQL Data Warehouse](sql-data-warehouse-reference-powershell-cmdlets.md)

### <a name="t-sql"></a>T-SQL
Com o T-SQL pode ver as definições atuais de DWU ou cDWU, altere as definições e verificar o progresso. 

Para alterar as DWUs ou cDWUs:

1. Ligar à base de dados mestra associado o servidor lógico de base de dados SQL.
2. Utilize o [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql) instrução TSQL. O exemplo a seguir define o objetivo de nível de serviço para DW1000 MySQLDW da base de dados. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

### <a name="rest-apis"></a>APIs REST

Para alterar as DWUs, utilize o [criação ou atualização de base de dados](/rest/api/sql/databases/createorupdate) REST API. O exemplo a seguir define o objetivo de nível de serviço para DW1000 MySQLDW que está alojado no servidor MyServer da base de dados. O servidor está num grupo de recursos do Azure com o nome ResourceGroup1.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

Para obter mais exemplos de REST API, consulte [APIs REST para o SQL Data Warehouse](sql-data-warehouse-manage-compute-rest-api.md).

## <a name="check-status-of-dwu-changes"></a>Verifique o estado das alterações DWU

Alterações DWU podem demorar vários minutos a concluir. Se estiver a dimensionar automaticamente, pondere implementar lógica para garantir que determinadas operações forma concluídas antes de continuar com a ação de outra. 

A verificar o estado da base de dados através de vários pontos finais permite-lhe implementar corretamente automatização. O portal fornece notificações após a conclusão de uma operação e o estado atual de bases de dados, mas não permite a programático a verificação do Estado. 

Não é possível verificar o estado da base de dados de operações de escalamento horizontal com o portal do Azure.

Para verificar o estado de DWU alterações:

1. Ligar à base de dados mestra associado o servidor lógico de base de dados SQL.
2. Submeta a consulta seguinte para verificar o estado da base de dados.


```sql
SELECT    *
FROM      sys.databases
;
```

3. Submeter a consulta seguinte para verificar o estado da operação

```sql
SELECT    *
FROM      sys.dm_operation_status
WHERE     resource_type_desc = 'Database'
AND       major_resource_id = 'MySQLDW'
;
```

Este DMV devolve informações sobre a gestão de várias operações no SQL Data Warehouse, tais como a operação e o estado da operação, o que está ser IN_PROGRESS ou foi concluída.

## <a name="the-scaling-workflow"></a>O fluxo de trabalho de dimensionamento

Quando iniciar uma operação de dimensionamento, o sistema inutilizam primeiro abrir todas as sessões, a reverter as transações abertas para Certifique-se num estado consistente. As operações de dimensionamento, dimensionamento só ocorre uma vez concluída esta reversão transacional.  

- Para uma operação de dimensionamento, Aprovisiona o sistema adicionais de computação e, em seguida, reattaches para a camada de armazenamento. 
- Para uma operação de dimensionamento inferior, o desnecessária nós desligue do armazenamento e reattach para os nós restantes.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre a gestão de desempenho, consulte [classes de recursos para a gestão de carga de trabalho](resource-classes-for-workload-management.md) e [limites de memória e simultaneidade](memory-and-concurrency-limits.md).



