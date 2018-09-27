---
title: Informações de desempenho de consulta na base de dados do Azure para PostgreSQL
description: Este artigo descreve a funcionalidade de informações de desempenho de consulta na base de dados do Azure para PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/26/2018
ms.openlocfilehash: cc041104169ca8c4344b9d3de597283d122e63db
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394765"
---
# <a name="query-performance-insight"></a>Query Performance Insight 

**Aplica-se a:** base de dados do Azure para PostgreSQL 9.6 e 10

> [!IMPORTANT]
> A funcionalidade de informações de desempenho de consulta está em pré-visualização pública num número limitado de regiões. 

O Query Performance Insight ajuda-o a identificar rapidamente quais são suas consultas de maior duração, como são alterados ao longo do tempo e esperas de que estão a afetá-los.

## <a name="permissions"></a>Permissões
**Proprietário** ou **contribuinte** as permissões necessárias para ver o texto das consultas no Query Performance Insight. **Leitor** pode ver gráficos e tabelas, mas não texto da consulta.

## <a name="prerequisites"></a>Pré-requisitos
Para o Query Performance Insight para a função, dados tem de existir na [Query Store](concepts-query-store.md).

## <a name="viewing-performance-insights"></a>Ver informações de desempenho
O [Query Performance Insight](concepts-query-performance-insight.md) vista no portal do Azure irão descobrir visualizações nas informações de chave de consulta Store. 

Na página do portal da sua base de dados do Azure para o servidor PostgreSQL, selecione **informações de desempenho de consultas** sob a **suporte + resolução de problemas** secção da barra de menus.

![O Query Performance Insight consultas de execução longa](./media/concepts-query-performance-insight/query-performance-insight-landing-page.png)

O **consultas de longa execução** separador mostra as consultas de 5 principais por duração média por execução, agregados em intervalos de 15 minutos. Pode ver mais consultas ao selecionar a partir da **número de consultas** menu pendente. As cores do gráfico podem ser alterados para um ID de consulta específico, ao fazer isso.

Pode clicar e arrastar no gráfico para restringir a uma janela de tempo específico. Em alternativa, utilize o zoom os ícones para ver um menor ou maior do período de tempo, respetivamente.

A tabela abaixo o gráfico fornece mais detalhes sobre as consultas de longa execução nessa janela de tempo.

Selecione o **estatísticas de espera** separador para ver as visualizações correspondentes no esperas no servidor.

![Consultar as estatísticas de espera de Performance Insight](./media/concepts-query-performance-insight/query-performance-insight-wait-statistics.png)

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [monitoramento e ajuste](concepts-monitoring.md) na base de dados do Azure para PostgreSQL.


