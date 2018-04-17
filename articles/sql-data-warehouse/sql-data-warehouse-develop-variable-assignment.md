---
title: Atribuir variáveis no Azure SQL Data Warehouse | Microsoft Docs
description: Sugestões para atribuição de variáveis de T-SQL no Azure SQL Data Warehouse para desenvolver soluções.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/12/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 012bc76950f212d69d26607c666e878b22015e70
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
---
# <a name="assigning-variables-in-azure-sql-data-warehouse"></a>Atribuição de variáveis no Azure SQL Data Warehouse
Sugestões para atribuição de variáveis de T-SQL no Azure SQL Data Warehouse para desenvolver soluções.

## <a name="setting-variables-with-declare"></a>Definir variáveis com DECLARE
As variáveis no armazém de dados do SQL Server são definidas utilizando o `DECLARE` instrução ou a `SET` instrução. A inicializar variáveis com DECLARE é uma das formas mais flexíveis para definir um valor da variável no armazém de dados do SQL Server.

```sql
DECLARE @v  int = 0
;
```

Também pode utilizar DECLARE para definir mais de uma variável de cada vez. Não é possível utilizar SELECIONE ou a ATUALIZAÇÃO para fazer o seguinte:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Não é possível inicializar e utilizar uma variável na mesma instrução DECLARE. Para ilustrar o ponto, o exemplo seguinte é **não** permitido desde @p1 é inicializado tanto utilizado na mesma instrução DECLARE. O exemplo seguinte fornece um erro.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Valores de definição com conjunto
CONJUNTO é um método comum para definir uma única variável.

As seguintes declarações são válidas todas as formas de definir uma variável com o conjunto de:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Só é possível definir uma variável num momento com conjunto. No entanto, os operadores compostos são permitidos.

## <a name="limitations"></a>Limitações
Não é possível utilizar SELECIONE ou a ATUALIZAÇÃO para atribuição de variáveis.

## <a name="next-steps"></a>Passos Seguintes
Para mais sugestões de desenvolvimento, consulte [descrição geral do desenvolvimento](sql-data-warehouse-overview-develop.md).

