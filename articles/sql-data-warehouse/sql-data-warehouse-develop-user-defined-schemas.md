---
title: Utilizar esquemas definido pelo utilizador no SQL Data Warehouse | Microsoft Docs
description: Sugestões para utilizar esquemas definido pelo utilizador de T-SQL no Azure SQL Data Warehouse para desenvolver soluções.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: c18e6d34416390ae7e93b69b28d508a540f7b1ab
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="using-user-defined-schemas-in-sql-data-warehouse"></a>Utilizar esquemas definido pelo utilizador no SQL Data Warehouse
Sugestões para utilizar esquemas definido pelo utilizador de T-SQL no Azure SQL Data Warehouse para desenvolver soluções.

## <a name="schemas-for-application-boundaries"></a>Esquemas de limites de aplicação

Os armazéns de dados tradicionais utilizam frequentemente bases de dados separadas para criar limites de aplicação com base na carga de trabalho, o domínio ou segurança. Por exemplo, um armazém de dados do SQL Server tradicional pode incluir uma base de dados de teste, uma base de dados do armazém de dados e algumas bases de dados do data mart. Nesta topologia, cada base de dados funciona como uma carga de trabalho e o limite de segurança numa arquitetura.

Por outro lado, o SQL Data Warehouse executa a carga de trabalho de armazém de dados completo dentro de uma base de dados. Cruzada da base de dados não são permitidas associações. Por conseguinte, o SQL Data Warehouse espera todas as tabelas utilizadas ao armazém de ser armazenados dentro de uma base de dados.

> [!NOTE]
> Armazém de dados do SQL Server não suporta consultas de base de dados cruzada de qualquer tipo. Por conseguinte, as implementações de armazém de dados que tiram partido deste padrão terão de ser revisto.
> 
> 

## <a name="recommendations"></a>Recomendações
Estes são recomendações para consolidar as cargas de trabalho, segurança, domínio e limites funcionais utilizando esquemas definidas pelo utilizador

1. Utilize uma base de dados do armazém de dados do SQL Server para executar a carga de trabalho do armazém de dados completo
2. Consolidar o seu ambiente do armazém de dados existente para utilizar uma base de dados do armazém de dados do SQL Server
3. Tire partido **definido pelo utilizador esquemas** para fornecer o limite de anteriormente implementado utilizando bases de dados.

Se definido pelo utilizador esquemas não foram utilizadas anteriormente, em seguida, terá uma ardósia limpa. Basta utilize o nome de base de dados antigo como base para as esquemas definido pelo utilizador na base de dados do armazém de dados do SQL Server.

Se já tem sido utilizadas esquemas, em seguida, tem algumas opções:

1. Remova os nomes do esquema de legado e começar do início
2. Manter os nomes de esquema legado por previamente pendente o nome do esquema legado para o nome da tabela
3. Manter os nomes de esquema legado implementando vistas sobre a tabela de um esquema adicional para voltar a criar a estrutura de esquema antigo.

> [!NOTE]
> No primeiro inspeção opção 3 pode parecer como a opção mais apelativos para resolver. No entanto, o devil é o mais detalhadamente. As vistas são só de leitura em SQL Data Warehouse. Qualquer modificação de dados ou tabela seria necessário executar a tabela base. Opção 3 apresenta também uma camada de vistas no seu sistema. Pode querer dar este algumas profundamente adicional se estiver a utilizar vistas na sua arquitetura já.
> 
> 

### <a name="examples"></a>Exemplos:
Implementar esquemas definido pelo utilizador com base nos nomes de base de dados

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Manter os nomes de esquema legado por previamente pendente-los para o nome da tabela. Utilize esquemas para o limite de carga de trabalho.

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

Manter os nomes de esquema legado utilizar as vistas

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [!NOTE]
> Qualquer alteração na estratégia de esquema tem uma revisão do modelo de segurança da base de dados. Em muitos casos poderá simplificar o modelo de segurança ao atribuir permissões ao nível do esquema. Se são necessárias permissões mais granulares, em seguida, pode utilizar funções de base de dados.
> 
> 

## <a name="next-steps"></a>Passos Seguintes
Para mais sugestões de desenvolvimento, consulte [descrição geral do desenvolvimento](sql-data-warehouse-overview-develop.md).

