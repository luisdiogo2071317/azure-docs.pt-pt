---
title: Monitorização e otimização de desempenho - base de dados SQL do Azure | Documentos da Microsoft
description: Sugestões para ajuste na base de dados do Azure SQL através de avaliação e melhoria de desempenho.
services: sql-database
author: v-shysun
manager: craigg
editor: ''
keywords: Ajuste, dicas de ajuste de desempenho de sql, de ajuste de desempenho da base de dados de desempenho de SQL ajuste de desempenho de base de dados do sql
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: v-shysun
ms.openlocfilehash: 79f41ab133cba539e5f855b3ab8fa21723694acb
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39092567"
---
# <a name="monitoring-and-performance-tuning"></a>Monitorização e otimização de desempenho

Base de dados SQL do Azure é gerida automaticamente e serviço de dados flexível, onde pode facilmente monitorizar a utilização, adicionar ou remover recursos (CPU, memória, e/s), localizar recomendações que podem melhorar o desempenho da base de dados ou permitir que a base de dados se adaptar a sua carga de trabalho e automaticamente a otimizar o desempenho.

Este artigo fornece uma descrição geral da monitorização e as opções disponíveis na base de dados do Azure SQL de ajuste de desempenho.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="monitoring-and-troubleshooting-database-performance"></a>Monitorização e resolução de problemas de desempenho de base de dados

Base de dados SQL do Azure permite-lhe facilmente monitorizar a utilização da base de dados e identificar consultas que podem causar problemas de desempenho. Pode monitorizar o desempenho de base de dados através de vistas de sistema ou do portal do Azure. Tem as seguintes opções de monitorização e resolução de problemas de desempenho de base de dados:

1. Na [portal do Azure](https://portal.azure.com), clique em **bases de dados SQL**, selecione a base de dados e, em seguida, utilize o gráfico de monitorização para procurar recursos máximas a aproximar-se. Consumo de DTU, é apresentado por predefinição. Clique em **editar** para alterar o intervalo de tempo e os valores mostrados.
2. Uso [Query Performance Insight](sql-database-query-performance.md) para identificar as consultas que gastar o máximo de recursos.
3. Pode utilizar vistas de gestão dinâmica (DMVs), Extended Events (`XEvents`) e o Store de consulta no SSMS para obter parâmetros de desempenho em tempo real.

Consulte a [tópico de documentação de orientação de desempenho](sql-database-performance-guidance.md) para descobrir técnicas que pode utilizar para melhorar o desempenho da base de dados do Azure SQL se identificar algum problema com esses relatórios ou vistas.

> [!IMPORTANT] 
> É recomendado utilizar sempre a versão mais recente do Management Studio, para permanecer sincronizado com as atualizações do Microsoft Azure e da Base de Dados SQL. [Atualize o SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
>

## <a name="optimize-database-to-improve-performance"></a>Otimizar para melhorar o desempenho da base de dados

Base de dados SQL do Azure permite-lhe identificar oportunidades para melhorar e otimizar o desempenho de consulta sem alterar recursos ao rever [recomendações de otimização de desempenho](sql-database-advisor.md). Os índices em falta e as consultas mal otimizadas são razões comuns para um desempenho fraco da base de dados. Pode aplicar estas recomendações de otimização para melhorar o desempenho da carga de trabalho.
Também pode permitir que banco de dados SQL do Azure para [automaticamente otimizar o desempenho das suas consultas](sql-database-automatic-tuning.md) aplicando todos identificados, recomendações e verificar o que melhorar o desempenho da base de dados. Pode utilizar as seguintes opções para melhorar o desempenho da base de dados:

1. Uso [Assistente de base de dados SQL](sql-database-advisor-portal.md) para ver as recomendações para criar e remover índices, parametrização consultas e corrigir problemas de esquema.
2. [Ativar o ajuste automático](sql-database-automatic-tuning-enable.md) e permita que o SQL do Azure, base de dados automaticamente problemas de desempenho de correção identificada.

## <a name="improving-database-performance-with-more-resources"></a>Melhorando o desempenho de base de dados com mais recursos

Por fim, se não existirem não existem itens passíveis de ação que podem melhorar o desempenho da base de dados, pode alterar a quantidade de recursos disponíveis no banco de dados do Azure SQL. Pode atribuir mais recursos ao alterar o [escalão de serviço DTU](sql-database-service-tiers-dtu.md) de uma base de dados autónoma ou aumentar as eDTUs de um conjunto elástico em qualquer altura. Em alternativa, se estiver a utilizar o [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md), pode alterar a camada de serviço ou aumentar os recursos alocados à base de dados. 
1. Bases de dados autónomo, pode [alterar escalões de serviço](sql-database-service-tiers-dtu.md) ou [recursos de computação](sql-database-service-tiers-vcore.md)sob demanda para melhorar o desempenho da base de dados.
2. Para várias bases de dados, considere a utilização [conjuntos elásticos](sql-database-elastic-pool-guidance.md) Dimensionar recursos automaticamente.

## <a name="tune-and-refactor-application-or-database-code"></a>Otimizar e um aplicativo do refactor ou código de base de dados

Pode alterar o código do aplicativo de maneira ideal, utilizar a base de dados, alterar os índices, forçar planos ou utilizar sugestões para adaptar manualmente a base de dados para a sua carga de trabalho. Encontrar algumas orientações e sugestões para ajuste manual e reescrever o código na [tópico de documentação de orientação de desempenho](sql-database-performance-guidance.md) artigo.


## <a name="next-steps"></a>Passos Seguintes

- Para ativar a otimização automática na base de dados do Azure SQL e permitir que a funcionalidade de otimização automática gerenciar totalmente a sua carga de trabalho, consulte [ativar o ajuste automático](sql-database-automatic-tuning-enable.md).
- Para utilizar a ajuste manual, pode rever [recomendações no portal do Azure de otimização](sql-database-advisor-portal.md) e aplicar manualmente os que melhorar o desempenho das suas consultas.
- Alterar os recursos que estão disponíveis na sua base de dados ao alterar [escalões de serviço de base de dados do Azure SQL](sql-database-performance-guidance.md)
