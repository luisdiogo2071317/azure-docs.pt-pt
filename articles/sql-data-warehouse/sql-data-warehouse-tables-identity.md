---
title: Utilizar identidade para criar chaves substituto - Azure SQL Data Warehouse | Microsoft Docs
description: As recomendações e os exemplos para criar chaves de substituição em tabelas no Azure SQL Data Warehouse, utilizando a propriedade de identidade.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: ab028705f5af7c37017d2e697240b7d3436f5f71
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="using-identity-to-create-surrogate-keys-in-azure-sql-data-warehouse"></a>Utilizar identidade para criar chaves de substituição no Azure SQL Data Warehouse
As recomendações e os exemplos para criar chaves de substituição em tabelas no Azure SQL Data Warehouse, utilizando a propriedade de identidade.

## <a name="what-is-a-surrogate-key"></a>O que é uma chave de substituto?
Uma chave de substituto numa tabela é uma coluna com um identificador exclusivo para cada linha. A chave não é gerada a partir de dados da tabela. Como modelers de dados criar chaves de substituição nas respetivas tabelas quando estes modelos de armazém de dados de design. Pode utilizar a propriedade de identidade para atingir este objetivo, basta e eficaz, sem afetar o desempenho da carga.  

## <a name="creating-a-table-with-an-identity-column"></a>Criar uma tabela com uma coluna de identidade
A propriedade de identidade foi concebida para aumentar horizontalmente em todas as distribuições no armazém de dados sem afetar o desempenho da carga. Por conseguinte, a implementação de identidade é dedicada a alcançar estes objetivos. 

Pode definir uma tabela como ter a propriedade de identidade ao primeiro criar a tabela utilizando a sintaxe que é semelhante à instrução seguinte:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1) NOT NULL
,   C2 INT NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;
```

Em seguida, pode utilizar `INSERT..SELECT` para preencher a tabela.

Este restantes desta secção realça as nuances da implementação para o ajudar a compreendê-los mais detalhadamente.  

### <a name="allocation-of-values"></a>Alocação de valores
A propriedade de identidade não garante a ordem na qual os valores de substituição são alocados e que reflete o comportamento do SQL Server e SQL Database do Azure. No entanto, no Azure SQL Data Warehouse, mais pronunciada de ausência de uma garantia. 

O exemplo seguinte é uma ilustração:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)    NOT NULL
,   C2 VARCHAR(30)              NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

INSERT INTO dbo.T1
VALUES (NULL);

INSERT INTO dbo.T1
VALUES (NULL);

SELECT *
FROM dbo.T1;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

No exemplo anterior, duas linhas landed distribuição 1. A primeira linha com o valor de substituição de 1 coluna `C1`, e a segunda linha com o valor de substituição de 61. Ambos estes valores foram gerados pela propriedade de identidade. No entanto, a atribuição dos valores não é contígua. Este comportamento é propositado.

### <a name="skewed-data"></a>Dados distorcidos 
O intervalo de valores para o tipo de dados são distribuídos uniformemente pelas distribuições. Se sofrerá de uma tabela distribuída a partir dos dados distorcidos, em seguida, o intervalo de valores disponíveis para o tipo de dados pode ser esgotado prematuramente. Por exemplo, se todos os dados termina uma distribuição único, em seguida, efetivamente a tabela tem acesso a apenas um sixtieth dos valores do tipo de dados. Por este motivo, a propriedade de identidade está limitada a `INT` e `BIGINT` apenas tipos de dados.

### <a name="selectinto"></a>SELECIONAR... PARA
Quando uma coluna de identidade existente é selecionada para uma nova tabela, a nova coluna herda a propriedade de identidade, a menos que uma das seguintes condições for verdadeira:
- A instrução SELECT contém uma associação.
- Múltiplas instruções SELECIONADAS são associadas através da utilização de União.
- A coluna de identidade é listada mais do que uma vez na lista de SELEÇÃO.
- A coluna de identidade faz parte de uma expressão.
    
Se qualquer um dos seguintes condições for VERDADEIRO, a coluna é criada NOT NULL em vez de herdar a propriedade de identidade.

### <a name="create-table-as-select"></a>CRIAR TABLE AS SELECT
Criar tabela AS SELECIONE (CTAS) segue o mesmo comportamento de SQL Server que é descrito selecione... PARA. No entanto, não é possível especificar uma propriedade de identidade na definição de coluna a `CREATE TABLE` fazem parte da instrução. Também não é possível utilizar a função de identidade no `SELECT` fazem parte do CTAS. Para preencher uma tabela, tem de utilizar `CREATE TABLE` para definir a tabela seguida `INSERT..SELECT` para preenchê-lo.

## <a name="explicitly-inserting-values-into-an-identity-column"></a>Inserir explicitamente valores numa coluna de identidade 
Suporta o SQL Data Warehouse `SET IDENTITY_INSERT <your table> ON|OFF` sintaxe. Pode utilizar esta sintaxe de inserir explicitamente valores na coluna de identidade.

Como muitas modelers de dados a utilizar valores negativos predefinidos para algumas linhas no respetivas dimensões. Um exemplo é a -1 ou linha "membro desconhecido". 

O script seguinte mostra como adicionar explicitamente esta linha utilizando IDENTITY_INSERT definir:

```sql
SET IDENTITY_INSERT dbo.T1 ON;

INSERT INTO dbo.T1
(   C1
,   C2
)
VALUES (-1,'UNKNOWN')
;

SET IDENTITY_INSERT dbo.T1 OFF;

SELECT  *
FROM    dbo.T1
;
```    

## <a name="loading-data"></a>Carregar dados

A presença da propriedade de identidade tem algumas implicações ao código de carregamento de dados. Esta secção realça alguns padrões básicos para carregar dados para tabelas através da utilização de identidade. 

Para carregar dados para uma tabela e gerar uma chave de substituição, utilizando a identidade, criar a tabela e, em seguida, utilizar INSERT... SELECIONE ou INSIRA... VALORES para executar a carga.

O exemplo seguinte realça o padrão básico:
 
```sql
--CREATE TABLE with IDENTITY
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)
,   C2 VARCHAR(30)
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

--Use INSERT..SELECT to populate the table from an external table
INSERT INTO dbo.T1
(C2)
SELECT  C2
FROM    ext.T1
;

SELECT  *
FROM    dbo.T1
;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

> [!NOTE] 
> Não é possível criar tabela AS que tiver de utilizar ' atualmente ao carregar dados para uma tabela com uma coluna de identidade.
> 

Para mais informações sobre como carregar dados, consulte [conceber extrair, carregamento e transformação (ELT) para o Azure SQL Data Warehouse](design-elt-data-loading.md) e [carregar as melhores práticas](guidance-for-loading-data.md).


## <a name="system-views"></a>Vistas de sistema
Pode utilizar o [sys.identity_columns](/sql/relational-databases/system-catalog-views/sys-identity-columns-transact-sql) vista para identificar uma coluna que tem a propriedade de identidade do catálogo.

Para ajudar a compreender melhor o esquema de base de dados, este exemplo mostra como integrar sys.identity_column' com outras vistas de catálogo de sistema:

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       CASE WHEN ic.column_id IS NOT NULL
             THEN 1
        ELSE 0
        END AS is_identity 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
LEFT JOIN   sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="limitations"></a>Limitações
Não é possível utilizar a propriedade de identidade:
- Quando o tipo de dados da coluna não é INT ou BIGINT
- Quando a coluna também é a chave de distribuição
- Quando a tabela é uma tabela externa 

As seguintes funções relacionadas não são suportadas no armazém de dados do SQL Server:

- [IDENTITY()](/sql/t-sql/functions/identity-function-transact-sql)
- [@@IDENTITY](/sql/t-sql/functions/identity-transact-sql)
- [SCOPE_IDENTITY](/sql/t-sql/functions/scope-identity-transact-sql)
- [IDENT_CURRENT](/sql/t-sql/functions/ident-current-transact-sql)
- [IDENT_INCR](/sql/t-sql/functions/ident-incr-transact-sql)
- [IDENT_SEED](/sql/t-sql/functions/ident-seed-transact-sql)
- [DBCC CHECK_IDENT()](/sql/t-sql/database-console-commands/dbcc-checkident-transact-sql)

## <a name="common-tasks"></a>Tarefas comuns

Esta secção fornece algum código de exemplo, que pode utilizar para executar tarefas comuns quando trabalha com colunas de identidade. 

C1 de coluna é a identidade em todas as tarefas seguintes.
 
 
### <a name="find-the-highest-allocated-value-for-a-table"></a>Localizar o valor mais alto alocado para uma tabela
Utilize o `MAX()` função para determinar o valor mais alto atribuído a uma tabela distribuída:

```sql
SELECT  MAX(C1)
FROM    dbo.T1
``` 

### <a name="find-the-seed-and-increment-for-the-identity-property"></a>Localizar o seed e incrementação maiores para a propriedade de identidade
Pode utilizar as vistas de catálogo para detetar os valores de configuração de identidade incremento e seed, para uma tabela utilizando a seguinte consulta: 

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       ic.seed_value
,       ic.increment_value 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
JOIN        sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="next-steps"></a>Passos Seguintes

* Para obter mais informações sobre como desenvolver tabelas, consulte a [descrição geral da tabela] [descrição geral].  

