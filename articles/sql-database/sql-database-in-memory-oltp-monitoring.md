---
title: Monitorizar o armazenamento de dentro da memória XTP | Microsoft Docs
description: Monitor de armazenamento de dentro da memória XTP e estimativa utilizam, capacidade; Resolva o erro de capacidade 41823
services: sql-database
author: jodebrui
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: article
ms.date: 04/04/2018
ms.author: jodebrui
ms.openlocfilehash: 0802a3b51847236efb64e628ed259dc7776bac4e
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="monitor-in-memory-oltp-storage"></a>Armazenamento de monitor de memória OLTP
Quando utilizar [OLTP na memória](sql-database-in-memory.md), dados em tabelas com otimização de memória e variáveis de tabela residem no armazenamento do OLTP dentro da memória. Cada escalão de serviço Premium e críticos de negócio tem um tamanho de armazenamento de OLTP na memória máximo, o que é descrito da [limites de recursos baseados em DTU](sql-database-dtu-resource-limits.md) e [limites de recursos baseados em vCore](sql-database-vcore-resource-limits.md). Depois deste limite for excedido, insira e atualizar operações poderão começam a falhar com o erro 41823 para bases de dados autónomo e o erro 41840 para conjuntos elásticos. Nesse momento é necessário para eliminar dados para reclamar memória, ou Atualize o escalão de desempenho da base de dados.

## <a name="determine-whether-data-fits-within-the-in-memory-oltp-storage-cap"></a>Determinar se dados enquadra-se a cobertura de armazenamento OLTP na memória
Determine os caps de armazenamento das camadas de serviços diferentes. Consulte [limites de recursos baseados em DTU](sql-database-dtu-resource-limits.md) e [limites de recursos baseados em vCore](sql-database-vcore-resource-limits.md).

Estimar requisitos de memória para funciona de uma tabela com otimização de memória da mesma forma para o SQL Server que se faz na SQL Database do Azure. Demorar alguns minutos para rever esse artigo de [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Tabela e linhas de variável de tabela, bem como índices, contam para o tamanho de dados de utilizador máximo. Além disso, ALTER TABLE tem espaço suficiente para criar uma nova versão do respetivos índices e a tabela inteira.

## <a name="monitoring-and-alerting"></a>Monitorização e alertas
Pode monitorizar a utilização do armazenamento em memória como uma percentagem da extremidade do armazenamento para o escalão de desempenho no [portal do Azure](https://portal.azure.com/): 

1. No painel da base de dados, localize a caixa de utilização de recursos e clique em Editar.
2. Selecione a métrica `In-Memory OLTP Storage percentage`.
3. Para adicionar um alerta, clique na caixa para abrir o painel de métrico de utilização de recursos, em seguida, clique em Adicionar alerta.

Ou utilize a seguinte consulta para mostrar a utilização de armazenamento na memória:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-in-memory-oltp-storage-situations---errors-41823-and-41840"></a>Corretas-na-memória esgotada OLTP armazenamento situações - 41823 de erros e 41840
Atingir a cobertura de armazenamento OLTP na memória nos resultados de base de dados no inserir, ATUALIZAR, alterar e criar operações falha com a mensagem de erro 41823 (para bases de dados autónomo) ou erro 41840 (para conjuntos elásticos). Ambos os erros fazer com que a transação ativa abortar.

Mensagens de erro 41823 e 41840 indicam que as tabelas com otimização de memória e variáveis de tabela na base de dados ou agrupamento atingiu o tamanho de armazenamento máximo do OLTP dentro da memória.

Para resolver este erro, está:

* Eliminar dados de tabelas com otimização de memória, potencialmente descarregar dados para tabelas tradicionais, com base em disco; ou,
* Atualize o escalão de serviço para um com armazenamento suficiente na memória para os dados que tem de manter em tabelas com otimização de memória.

> [!NOTE] 
> Em casos raros, os erros 41823 e 41840 podem ser transitórios, o que significa que existe armazenamento suficiente disponível OLTP na memória e repetir a operação é concluída com êxito. Por conseguinte, é recomendável ambas monitorizar o armazenamento de OLTP na memória global disponível e tente novamente quando encontrar primeiro erro 41823 ou 41840. Para obter mais informações sobre a lógica de repetição, consulte [deteção de conflitos e repita a lógica com OLTP na memória](https://docs.microsoft.com/sql/relational-databases/In-memory-oltp/transactions-with-memory-optimized-tables#conflict-detection-and-retry-logic).

## <a name="next-steps"></a>Passos Seguintes
Para monitorizar orientações, consulte [monitorização SQL Database do Azure utilizar as vistas de gestão dinâmica](sql-database-monitoring-with-dmvs.md).
