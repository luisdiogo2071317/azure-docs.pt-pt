---
title: Utilizar vistas de T-SQL no Azure SQL Data Warehouse | Microsoft Docs
description: Sugestões para utilizar as vistas de T-SQL no Azure SQL Data Warehouse para desenvolver soluções.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 28280a067e7008c20361e0a0041c81ba84e7f74c
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="views-in-azure-sql-data-warehouse"></a>Vistas do armazém de dados SQL do Azure
Sugestões para utilizar as vistas de T-SQL no Azure SQL Data Warehouse para desenvolver soluções. 

## <a name="why-use-views"></a>Porquê utilizar vistas?
As vistas podem ser utilizadas num número de diferentes formas para melhorar a qualidade da sua solução.  Este artigo realça alguns exemplos de como enriquecer a sua solução com vistas, bem como as limitações que precisam de ser considerados.

> [!NOTE]
> Sintaxe para criar vista não é abordada neste artigo. Para obter mais informações, consulte o [Criar vista](/sql/t-sql/statements/create-view-transact-sql) documentação.
> 
> 

## <a name="architectural-abstraction"></a>Abstração da arquitetura
É um padrão de aplicação comum voltar a criar tabelas criar tabela AS SELECIONE (CTAS) seguido de um objeto mudar o nome padrão enfrenta carregar dados a utilizar.

O exemplo seguinte adiciona registos data novo a uma dimensão de data. Tenha em atenção a como uma nova tabela, DimDate_New, é criada pela primeira vez e, em seguida, mudar o nome para substituir a versão original da tabela.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

No entanto, esta abordagem pode resultar em tabelas de apresentação e disappearing da vista de um utilizador, bem como as mensagens de erro de "tabela não existe". Vistas podem ser utilizadas para fornecer aos utilizadores com uma camada de apresentação consistente, embora os objetos subjacentes são mudados. Ao fornecer acesso aos dados através de vistas, os utilizadores não têm visibilidade para as tabelas subjacentes. Esta camada oferece uma experiência de utilizador consistente enquanto garantir que o armazém de dados designers pode evoluir o modelo de dados. A capacidade de evoluir as tabelas subjacentes significa designers podem utilizar CTAS para maximizar o desempenho durante o processo de carregamento de dados.   

## <a name="performance-optimization"></a>Otimização do desempenho
Vistas também podem ser utilizadas para impor as associações entre as tabelas com otimização de desempenho. Por exemplo, uma vista pode incorporar uma chave de distribuição redundante como parte dos critérios de joining para minimizar o movimento de dados. Outra vantagem de uma vista pode ser para forçar uma consulta específica ou sugestão de associação. Utilizar vistas desta forma, garante que associações são sempre executadas de forma ideal, evitando a necessidade dos utilizadores memorizem a construção correta para as respetivas associações.

## <a name="limitations"></a>Limitações
Vistas do armazém de dados do SQL Server são armazenadas como apenas metadados. Por conseguinte, não estão disponíveis as seguintes opções:

* Não há nenhuma opção de enlace de esquema
* Tabelas base não podem ser atualizadas através da vista
* Não não possível criar vistas através de tabelas temporárias
* Não são suportadas para o expandir / sugestões NOEXPAND
* Não existem nenhum vistas indexadas no SQL Data Warehouse

## <a name="next-steps"></a>Passos Seguintes
Para mais sugestões de desenvolvimento, consulte [Descrição geral do desenvolvimento no SQL Data Warehouse](sql-data-warehouse-overview-develop.md).


