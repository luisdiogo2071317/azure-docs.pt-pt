---
title: Otimizar inserções em massa na base de dados do Azure para o servidor PostgreSQL
description: Este artigo descreve como pode otimizar as operações de inserção em massa na base de dados do Azure para o servidor PostgreSQL.
author: dianaputnam
ms.author: dianas
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: 2fe3c3cc71823880d71223334b89816199561ca9
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51629356"
---
# <a name="optimizing-bulk-inserts-and-use-of-transient-data-on-azure-database-for-postgresql-server"></a>Otimizar as inserções em massa e a utilização de dados transitórios na base de dados do Azure para o servidor PostgreSQL 
Este artigo descreve como pode otimizar operações de inserção em massa e a utilização de dados transitórios numa base de dados do Azure para o servidor PostgreSQL.

## <a name="using-unlogged-tables"></a>Utilizar tabelas unlogged
Para os clientes que possuem operações de carga de trabalho que envolvem dados transitórios ou que inserir grandes conjuntos de dados em massa, considere o uso tabelas unlogged.

Tabelas unlogged é uma funcionalidade de PostgreSQL que pode ser utilizada eficazmente para otimizar as inserções em massa. PostgreSQL utiliza Write-Ahead registo (WAL), que fornece atomicidade e durabilidade duas das propriedades do ACID por predefinição. Inserindo numa tabela unlogged média PostgreSQL seria fazer inserções sem gravar no log de transação, que por si só é uma operação de e/s, tornando essas tabelas consideravelmente mais rapidamente do que as tabelas comuns.

Seguem-se as opções para criar uma tabela unlogged:
- Crie uma nova tabela unlogged usando a sintaxe: `CREATE UNLOGGED TABLE <tableName>`
- Converter um existente com sessão iniciada a tabela a uma tabela unlogged usando a sintaxe: `ALTER <tableName> SET UNLOGGED`.  Isso pode ser revertido ao utilizar a sintaxe: `ALTER <tableName> SET LOGGED`

## <a name="unlogged-table-tradeoff"></a>Compensação de tabela unlogged
As tabelas unlogged não são seguras para falhas. Uma tabela unlogged é truncada automaticamente após uma pane ou sujeitos a um encerramento unclean. O conteúdo de uma tabela unlogged também não é replicado para servidores em espera. Índices criados numa tabela unlogged também são automaticamente unlogged.  Depois de concluída a operação de inserção, pode converter a tabela de sessão iniciada para a inserção é durável.

No entanto, em algumas cargas de trabalho do cliente, experienciamos aproximadamente uma melhoria de desempenho de 15 a 20% ao usar tabelas unlogged.

## <a name="next-steps"></a>Passos Seguintes
Reveja a sua carga de trabalho para utilizações de dados transitórios e inserções em grande volume.  

Reveja a seguinte documentação do PostgreSQL - [criar comandos de SQL de tabela](https://www.postgresql.org/docs/current/static/sql-createtable.html)