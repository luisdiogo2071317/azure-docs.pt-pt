---
title: Utilizar o bcp para carregar os dados para o SQL Data Warehouse | Microsoft Docs
description: "Saiba o que é o bcp e como utilizá-lo para cenários de armazenamento de dados."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: f9467d11-fcd6-4131-a65a-2022d2c32d24
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 01/22/2018
ms.author: cakarst;barbkess
ms.openlocfilehash: 146c6fdada651551c05b2cbcadc3e1248a40b613
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
# <a name="load-data-with-bcp"></a>Carregar dados com o bcp

**[bcp](/sql/tools/bcp-utility.md)** é um utilitário da linha de comandos para carregamento em massa, que permite copiar dados entre o SQL Server, ficheiros de dados e o SQL Data Warehouse. Utilize o bcp para importar um grande número de linhas para tabelas do SQL Data Warehouse ou exportar dados de tabelas do SQL Server para ficheiros de dados. Exceto quando utilizado com a opção de queryout, o bcp não necessita de nenhum conhecimento de Transact-SQL.

O bcp é uma forma rápida e fácil de mover conjuntos de dados mais pequenos para dentro e fora de uma base de dados SQL Data Warehouse. A quantidade exata de dados recomendada para carregamento/extração através do bcp depende da ligação de rede ao Azure.  Podem ser carregadas e extraídas tabelas de pequena dimensão com o bcp. No entanto, o Polybase, não o bcp, é a ferramenta recomendada para carregar e extrair grandes volumes de dados. O PolyBase foi concebido para a arquitetura de processamento paralelo em massa do SQL Data Warehouse.

O bcp permite:

* Utilizar um utilitário de linha de comandos para carregar dados para o SQL Data Warehouse.
* Utilizar um utilitário de linha de comandos para extrair dados do SQL Data Warehouse.

Este tutorial permite:

* Importar dados para uma tabela com o comando de entrada do bcp
* Exportar dados de uma tabela com o comando de saída do bcp

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="prerequisites"></a>Pré-requisitos
Para seguir este tutorial, é necessário:

* Uma base de dados SQL Data Warehouse
* utilitários de linha de comandos do bcp e sqlcmd. Pode transferi-los a partir do [Centro de Transferências da Microsoft](https://www.microsoft.com/download/details.aspx?id=36433). 

### <a name="data-in-ascii-or-utf-16-format"></a>Dados no formato ASCII ou UTF-16
Se estiver a experimentar este tutorial com os seus dados, estes têm de utilizar a codificação ASCII ou UTF-16, uma vez que o bcp não suporta UTF-8. 

O PolyBase suporta UTF-8, mas ainda não suporta UTF-16. Para utilizar o bcp para exportar dados e o PolyBase para carregamento de dados, tem de transformar os dados para UTF-8 depois de serem exportados do SQL Server. 

## <a name="import-data-into-sql-data-warehouse"></a>Importar dados para o SQL Data Warehouse
Neste tutorial, irá criar uma tabela no Azure SQL Data Warehouse e importar dados para a tabela.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Passo 1: criar uma tabela no Azure SQL Data Warehouse
A partir de uma linha de comandos, utilize o sqlcmd para executar a consulta seguinte, para criar uma tabela na sua instância:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```

Para obter mais informações sobre como criar uma tabela, veja [Descrição Geral da Tabela](sql-data-warehouse-tables-overview.md) ou a sintaxe [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse.md).
 

### <a name="step-2-create-a-source-data-file"></a>Passo 2: criar um ficheiro de dados de origem
Abra o Bloco de Notas e copie as seguintes linhas de dados para um novo ficheiro de texto e, em seguida, guarde este ficheiro no diretório temporário local, C:\Temp\DimDate2.txt.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

> [!NOTE]
> É importante não esquecer que bcp.exe não suporta a codificação UTF-8 de ficheiros. Utilize ficheiros ASCII ou ficheiros codificados com UTF-16 ao utilizar o bcp.exe.
> 
> 

### <a name="step-3-connect-and-import-the-data"></a>Passo 3: ligar e importar os dados
Ao utilizar o bcp, pode ligar e importar os dados com o seguinte comando, substituindo os valores conforme adequado:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Pode verificar os dados que foram carregados, executando a seguinte consulta com o sqlcmd:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

A consulta deve devolver os seguintes resultados:

| DateId | CalendarQuarter | FiscalQuarter |
| --- | --- | --- |
| 20150101 |1 |3 |
| 20150201 |1 |3 |
| 20150301 |1 |3 |
| 20150401 |2 |4 |
| 20150501 |2 |4 |
| 20150601 |2 |4 |
| 20150701 |3 |1 |
| 20150801 |3 |1 |
| 20150801 |3 |1 |
| 20151001 |4 |2 |
| 20151101 |4 |2 |
| 20151201 |4 |2 |

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Passo 4: criar Estatísticas nos dados recentemente carregados
Depois de carregar os dados, o passo final é criar ou atualizar as estatísticas. Isto ajuda a melhorar o desempenho das consultas. Para obter mais informações, veja [Estatísticas](sql-data-warehouse-tables-statistics.md). O seguinte exemplo de sqlcmd cria estatísticas na tabela que contém os dados recentemente carregados.


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Exportar dados do SQL Data Warehouse
Este tutorial cria um ficheiro de dados a partir de uma tabela no SQL Data Warehouse. Exporta os dados que importou na secção anterior. Os resultados aparecem num ficheiro denominado DimDate2_export.txt.

### <a name="step-1-export-the-data"></a>Passo 1: exportar os dados
Ao utilizar o utilitário bcp, pode ligar e exportar dados com o seguinte comando, substituindo os valores conforme adequado:

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Pode verificar se os dados foram exportados corretamente ao abrir o novo ficheiro. Os dados no ficheiro devem corresponder ao texto seguinte:

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

> [!NOTE]
> Devido à natureza dos sistemas distribuídos, a ordem dos dados poderá não ser a mesma nas Bases de Dados SQL Data Warehouse. Outra opção consiste em utilizar a função **queryout** do bcp para escrever um extrato de consulta, em vez de exportar a tabela inteira.
> 
> 

## <a name="next-steps"></a>Passos seguintes
Para estruturar o seu processo de carregamento, veja [Descrição geral do processo de carregamento](sql-data-warehouse-design-elt-data-loading).  



<!--MSDN references-->



<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
