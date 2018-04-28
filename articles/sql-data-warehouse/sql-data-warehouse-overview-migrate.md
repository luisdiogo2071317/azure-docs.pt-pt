---
title: Migrar a sua solução ao SQL Data Warehouse | Microsoft Docs
description: Orientações de migração para colocar a sua solução de plataforma do Azure SQL Data Warehouse.
services: sql-data-warehouse
author: jrowlandjones
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: jrj
ms.reviewer: igorstan
ms.openlocfilehash: 5a609fb2da1f9dba1247358f64b284fc3e3ef5bc
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="migrate-your-solution-to-azure-sql-data-warehouse"></a>Migrar a sua solução ao Azure SQL Data Warehouse
Ver o que está envolvido no migrar uma solução de base de dados existente para o Azure SQL Data Warehouse. 

## <a name="profile-your-workload"></a>A carga de trabalho de perfil
Antes de migrar, quer ser que determinadas SQL Data Warehouse é a solução certa para a carga de trabalho. O SQL Data Warehouse é um sistema distribuído concebido para efetuar análise de dados de grandes dimensões.  Migrar para o SQL Data Warehouse requer algumas alterações de estrutura que não são demasiado rígido para compreender mas poderão demorar algum tempo a implementar. Se a sua empresa precisar de um armazém de dados de classe empresarial, são as vantagens, mas o esforço do utilizador. No entanto, se não precisa do poder do SQL Data Warehouse, é mais económico para utilizar o SQL Server ou SQL Database do Azure.

Considere utilizar o SQL Data Warehouse quando tiver:
- Ter um ou mais Terabytes de dados
- Pretende executar a análise na grandes quantidades de dados
- Necessita da capacidade de dimensionar a computação e armazenamento 
- Pretende reduzir os custos ao colocar em pausa recursos de computação quando não precisar deles.

Não utilize o SQL Data Warehouse para cargas de trabalho (OLTP) operacionais que tem:
- Frequência alta lê e escreve
- Grande número de singleton seleciona
- Volumes elevados de inserções de linha única
- Linha por linha tem de processar
- Formatos incompatíveis (JSON, XML)


## <a name="plan-the-migration"></a>Planear a migração

Depois de decidir migrar uma solução existente para o SQL Data Warehouse, é importante planear a migração antes de começar a utilizar. 

É um objetivo de planeamento para garantir que os dados, esquemas de tabela e o código são compatíveis com o SQL Data Warehouse. Existem algumas diferenças de compatibilidade para contornar entre o sistema atual e o SQL Data Warehouse. Plus, a migrar grandes quantidades de dados para o tempo de demora do Azure. Um planeamento cuidado expedites colocar os seus dados para o Azure. 

Outro objetivo do planeamento é efetuar ajustes de design para garantir a que sua solução tira partido do desempenho que do armazém de dados do SQL Server foi concebido para fornecer elevada de consulta. Conceber armazéns de dados para a escala apresenta padrões e abordagens tradicionais, por isso, não são sempre o melhor de design diferentes. Embora pode efetuar alguns ajustes de design após a migração, efetuar as alterações mais cedo no processo irá poupar tempo posteriormente.

Para efetuar uma migração com êxito, terá de migrar as esquemas de tabela, o código e os dados. Para obter orientações sobre estes tópicos de migração, consulte:

-  [Migrar as esquemas](sql-data-warehouse-migrate-schema.md)
-  [Migrar o seu código](sql-data-warehouse-migrate-code.md)
-  [Migrar os dados](sql-data-warehouse-migrate-data.md). 

<!--
## Perform the migration


## Deploy the solution


## Validate the migration

-->

## <a name="next-steps"></a>Passos Seguintes
O CAT (equipa de Consultadora dos clientes) também tem algumas excelente documentação de orientação do armazém de dados do SQL Server, que publicam através de blogues.  Observe respetivo artigo, [migrar dados para o Azure SQL Data Warehouse na prática] [ Migrating data to Azure SQL Data Warehouse in practice] para orientações adicionais sobre a migração.

<!--Image references-->

<!--Article references-->

<!--MSDN references-->

<!--Other Web references-->
[Migrating data to Azure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
