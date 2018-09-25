---
title: Recomendações de desempenho na base de dados do Azure para PostgreSQL
description: Este artigo descreve as recomendações de desempenho, um pode obter na base de dados do Azure para PostgreSQL.
services: postgresql
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 25ab2f735cfd4b0870729cb86992665fa8984580
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46976381"
---
# <a name="performance-recommendations-in-azure-database-for-postgresql"></a>Recomendações de desempenho na base de dados do Azure para PostgreSQL

**Aplica-se a:** base de dados do Azure para PostgreSQL 9.6 e 10

> [!IMPORTANT]
> Recomendações de desempenho está em pré-visualização pública.

A funcionalidade de recomendações de desempenho identifica os índices principais que podem ser criados na base de dados do Azure para o servidor PostgreSQL para melhorar o desempenho. Para produzir recomendações de índice, a funcionalidade leva em consideração várias características de base de dados, incluindo o respetivo esquema e a carga de trabalho, conforme comunicado pelo Store de consulta. Depois de implementar quaisquer recomendações de desempenho, os clientes devem testar o desempenho para avaliar o impacto dessas alterações. 

## <a name="permissions"></a>Permissões
**Proprietário** ou **contribuinte** as permissões necessárias para executar a análise de utilização da funcionalidade de recomendações de desempenho.

## <a name="performance-recommendations"></a>Recomendações de desempenho
O [recomendações de desempenho](concepts-performance-recommendations.md) funcionalidade analisa as cargas de trabalho em seu servidor para identificar índices com o potencial para melhorar o desempenho.

Open **recomendações de desempenho** partir a **suporte + resolução de problemas** secção da barra de menus na página do portal do Azure para o servidor PostgreSQL.

![Recomendações de desempenho a página de destino](./media/concepts-performance-recommendations/performance-recommendations-landing-page.png)

Selecione **Analyze** e escolha uma base de dados. Isto irá iniciar a análise. Dependendo da sua carga de trabalho, isto pode demorar vários minutos a concluir. Depois de fazer a análise, haverá uma notificação no portal.

O **recomendações de desempenho** janela mostrará uma lista de recomendações se for detetada qualquer. Uma recomendação mostrará informações sobre o relevante **base de dados**, **tabela**, **coluna**, e **tamanho de índice**.

![Nova página recomendações de desempenho](./media/concepts-performance-recommendations/performance-recommendations-result.png)

Para implementar a recomendação, copie o texto da consulta e executá-lo a partir do seu cliente da preferência.

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [monitoramento e ajuste](concepts-monitoring.md) na base de dados do Azure para PostgreSQL.

