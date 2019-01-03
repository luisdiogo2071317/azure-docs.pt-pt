---
title: Informações de desempenho de consulta na base de dados do Azure para PostgreSQL
description: Este artigo descreve a funcionalidade de informações de desempenho de consulta na base de dados do Azure para PostgreSQL.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/26/2018
ms.openlocfilehash: 1a71fb81acc91036ce12ff15f6b2762b808c7473
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/17/2018
ms.locfileid: "53541625"
---
# <a name="query-performance-insight"></a>Query Performance Insight 

**Aplica-se a:** Base de dados do Azure para PostgreSQL 9.6 e 10

> [!IMPORTANT]
> A funcionalidade de informações de desempenho de consulta está em pré-visualização pública. 

O Query Performance Insight ajuda-o a identificar rapidamente quais são suas consultas de maior duração, como são alterados ao longo do tempo e esperas de que estão a afetá-los.

## <a name="permissions"></a>Permissões
As permissões **Proprietário**ou **Contribuidor** necessárias para ver o texto das consultas no Query Performance Insight. O **Leitor** pode ver gráficos e tabelas, mas não o texto da consulta.

## <a name="prerequisites"></a>Pré-requisitos
Para o Query Performance Insight para a função, dados tem de existir na [Query Store](concepts-query-store.md).

## <a name="viewing-performance-insights"></a>Ver informações de desempenho
A vista [Query Performance Insight](concepts-query-performance-insight.md) no portal do Azure vai apresentar visualizações sobre informações importantes do Arquivo de Consultas. 

Na página do portal da sua base de dados do Azure para o servidor PostgreSQL, selecione **informações de desempenho de consultas** sob a **suporte + resolução de problemas** secção da barra de menus.

![O Query Performance Insight consultas de execução longa](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png)

O separador **Consultas de execução longa** mostra as cinco principais consultas por duração média para cada execução, agregadas em intervalos de 15 minutos. Pode ver mais consultas ao selecionar no menu pendente **Número de Consultas**. Ao fazer isso, as cores do gráfico podem ser alteradas para um ID de Consulta específico.

Pode clicar e arrastar no gráfico para restringir a uma janela de tempo específica. Em alternativa, utilize o zoom os ícones para ver um menor ou maior do período de tempo, respetivamente.

A tabela abaixo o gráfico fornece mais detalhes sobre as consultas de longa execução nessa janela de tempo.

Selecione o separador **Estatísticas de Espera** para ver as visualizações correspondentes sobre esperas no servidor.

![Estatísticas de espera do Query Performance Insight](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>Passos Seguintes
- Saber mais sobre [monitorização e otimização](concepts-monitoring.md) na Base de Dados do Azure para PostgreSQL.


