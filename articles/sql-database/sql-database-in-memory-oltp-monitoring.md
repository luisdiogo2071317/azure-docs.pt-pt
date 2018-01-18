---
title: "Monitorizar o armazenamento de dentro da memória XTP | Microsoft Docs"
description: "Monitor de armazenamento de dentro da memória XTP e estimativa utilizam, capacidade; Resolva o erro de capacidade 41823"
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: b617308e-692c-4938-8fa2-070034a3ecef
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jodebrui
ms.openlocfilehash: 1e7088e80cc86e3c7cf8ae8ea180d797de613e71
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="monitor-in-memory-oltp-storage"></a>Armazenamento de monitor de memória OLTP
Quando utilizar [OLTP na memória](sql-database-in-memory.md), dados em tabelas com otimização de memória e variáveis de tabela residem no armazenamento do OLTP dentro da memória. Cada escalão de serviço Premium tem um tamanho de armazenamento de OLTP na memória máximo, o que é descrito da [única dos limites de recursos de base de dados](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) e [dos limites de recursos do conjunto elástico](sql-database-resource-limits.md#elastic-pool-change-storage-size). Depois deste limite for excedido, insira e atualizar operações poderão começam a falhar com o erro 41823 para bases de dados autónomo e o erro 41840 para conjuntos elásticos. Nesse momento é necessário para eliminar dados para reclamar memória, ou Atualize o escalão de desempenho da base de dados.

## <a name="determine-whether-data-fits-within-the-in-memory-oltp-storage-cap"></a>Determinar se dados enquadra-se a cobertura de armazenamento OLTP na memória
Determine os caps de armazenamento dos escalões de serviço Premium diferentes. Consulte [única dos limites de recursos de base de dados](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) e [dos limites de recursos do conjunto elástico](sql-database-resource-limits.md#elastic-pool-change-storage-size).

Estimar requisitos de memória para funciona de uma tabela com otimização de memória da mesma forma para o SQL Server que se faz na SQL Database do Azure. Demorar alguns minutos para rever esse artigo de [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Tabela e linhas de variável de tabela, bem como índices, contam para o tamanho de dados de utilizador máximo. Além disso, ALTER TABLE tem espaço suficiente para criar uma nova versão do respetivos índices e a tabela inteira.

## <a name="monitoring-and-alerting"></a>Monitorização e alertas
Pode monitorizar a utilização de memória de armazenamento como uma percentagem da extremidade do armazenamento para o escalão de desempenho no [portal do Azure](https://portal.azure.com/): 

1. No painel da base de dados, localize a caixa de utilização de recursos e clique em Editar.
2. Selecione a métrica `In-Memory OLTP Storage percentage`.
3. Para adicionar um alerta, clique na caixa para abrir o painel de métrico de utilização de recursos, em seguida, clique em Adicionar alerta.

Ou utilize a seguinte consulta para mostrar a utilização de memória de armazenamento:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-in-memory-oltp-storage-situations---errors-41823-and-41840"></a>Corretas-na-memória esgotada OLTP armazenamento situações - 41823 de erros e 41840
Atingir a cobertura de armazenamento OLTP na memória nos resultados de base de dados no inserir, ATUALIZAR, alterar e criar operações falha com a mensagem de erro 41823 (para bases de dados autónomo) ou erro 41840 (para conjuntos elásticos). Ambos os erros fazer com que a transação ativa abortar.

Mensagens de erro 41823 e 41840 indicam que as tabelas com otimização de memória e variáveis de tabela na base de dados ou agrupamento atingiu o tamanho de armazenamento máximo do OLTP dentro da memória.

Para resolver este erro, está:

* Eliminar dados de tabelas com otimização de memória, potencialmente descarregar dados para tabelas tradicionais, com base em disco; ou,
* Atualize o escalão de serviço para um com armazenamento suficiente na memória para os dados que tem de manter em tabelas com otimização de memória.

> [!NOTE] 
> Em casos raros, os erros 41823 e 41840 podem ser transitórios, o que significa que existe armazenamento suficiente disponível OLTP na memória e repetir a operação é concluída com êxito. Por conseguinte, é recomendável ambas monitorizar o armazenamento de OLTP na memória global disponível e tente novamente quando encontrar primeiro erro 41823 ou 41840. Para obter mais informações sobre a lógica de repetição, consulte [deteção de conflitos e repita a lógica com OLTP na memória](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables#conflict-detection-and-retry-logic).

## <a name="next-steps"></a>Passos Seguintes
Para monitorizar orientações, consulte [monitorização SQL Database do Azure utilizar as vistas de gestão dinâmica](sql-database-monitoring-with-dmvs.md).
