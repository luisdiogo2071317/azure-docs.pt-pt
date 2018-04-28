---
title: Utilização de procedimentos armazenados no Azure SQL Data Warehouse | Microsoft Docs
description: Sugestões para implementar os procedimentos armazenados no Azure SQL Data Warehouse para desenvolver soluções.
services: sql-data-warehouse
author: ckarst
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: 5659e8f29d87c48c447a5cb81c836b0be9dabd45
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="using-stored-procedures-in-sql-data-warehouse"></a>Utilização de procedimentos armazenados no SQL Data Warehouse
Sugestões para implementar os procedimentos armazenados no Azure SQL Data Warehouse para desenvolver soluções.

## <a name="what-to-expect"></a>O que esperar

Armazém de dados SQL suporta muitas das funcionalidades T-SQL que são utilizadas no SQL Server. Mais importante ainda, existem funcionalidades específicas de escalamento horizontal que pode utilizar para maximizar o desempenho da sua solução.

No entanto, para manter o dimensionamento e desempenho do armazém de dados do SQL Server existe também são algumas funções e funcionalidades que tenham diferenças comportamentais e outras pessoas que não são suportadas.


## <a name="introducing-stored-procedures"></a>Introdução ao procedimentos armazenados
Os procedimentos armazenados são uma excelente forma ao encapsular o código do SQL Server; armazenar próximo os dados no armazém de dados. Os procedimentos armazenados ajudam os programadores modularize as soluções, ao encapsular o código em unidades geríveis; facilitar o início reusability maior de código. Cada procedimento armazenado também pode aceitar parâmetros para torná-las ainda mais flexíveis.

Armazém de dados do SQL Server fornece uma implementação simplificada e simplificada procedimento armazenado. A diferença maior relativamente ao SQL Server é que o procedimento armazenado não está código previamente compilado. Os armazéns de dados, o tempo de compilação é pequeno em comparação com o tempo que demora para executar consultas no volumes de dados de grandes dimensões. É mais importante certificar-se de que o código de procedimento armazenado corretamente está otimizado para consultas grandes. O objetivo é guardar horas, minutos e segundos, não milissegundos. Consequentemente, é mais útil pensar de procedimentos armazenados como contentores para a lógica de SQL.     

Quando o SQL Data Warehouse executa o procedimento armazenado, as instruções SQL são analisadas, traduzidas e otimizadas em tempo de execução. Durante este processo, cada instrução é convertida em consultas distribuídas. O código SQL é y executada contra os dados é diferente da consulta foi submetida.

## <a name="nesting-stored-procedures"></a>Procedimentos de aninhamento armazenado
Quando procedimentos armazenados chamar outros procedimentos armazenados ou executar SQL dinâmico, em seguida, a interna do procedimento armazenado ou invocação de código possui correspondências é denominada ser aninhadas.

Armazém de dados do SQL Server suporta um máximo de oito níveis de aninhamento. Isto é ligeiramente diferente do SQL Server. O nível de aninhamento no SQL Server é 32.

A chamada de procedimento armazenado de nível superior equivale a aninhar nível 1.

```sql
EXEC prc_nesting
```
Se o procedimento armazenado também facilita a EXEC outra chamada, aumenta o nível de aninhamento a dois.

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Se o segundo procedimento, em seguida, executa algumas SQL dinâmico, aumenta o nível de aninhamento a três.

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Tenha em atenção de que o SQL Data Warehouse não suporta atualmente [@@NESTLEVEL](/sql/t-sql/functions/nestlevel-transact-sql). Necessita de controlar o nível de aninhamento. Não é provável que pode exceder o limite de nível de oito aninhamento, mas se o fizer, terá de rework o código para ajustar os níveis de aninhamento dentro este limite.

## <a name="insertexecute"></a>INSERT... EXECUTAR
Armazém de dados do SQL Server não permite a consumir o conjunto de resultados de um procedimento armazenado com uma instrução INSERT. No entanto, é uma abordagem alternativa, que pode utilizar. Por exemplo, consulte o artigo sobre [tabelas temporárias](sql-data-warehouse-tables-temporary.md). 

## <a name="limitations"></a>Limitações
Existem alguns aspetos dos procedimentos armazenados de Transact-SQL que não estão implementados no armazém de dados do SQL Server.

São:

* procedimentos armazenados temporariamente
* procedimentos armazenados numerados
* procedimentos armazenados expandidos
* Procedimentos armazenado de CLR
* 
* opção de encriptação
* opção de replicação
* parâmetros de valor de tabela
* parâmetros só de leitura
* parâmetros predefinidos
* contextos de execução
* instrução de retorno

## <a name="next-steps"></a>Passos Seguintes
Para mais sugestões de desenvolvimento, consulte [descrição geral do desenvolvimento](sql-data-warehouse-overview-develop.md).

