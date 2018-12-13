---
title: Manutenção planeada - Azure SQL Data Warehouse | Documentos da Microsoft
description: Aprenda a preparar para eventos de manutenção planeada para o Azure SQL Data Warehouse.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/19/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: c1e8f94a0131ace6354d070e932e414a1897260e
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/10/2018
ms.locfileid: "53166307"
---
# <a name="planning-for-maintenance-on-your-azure-sql-data-warehouse"></a>Planear a manutenção no seu armazém de dados SQL do Azure

Aprenda a preparar para eventos de manutenção planeada no seu armazém de dados SQL do Azure.

## <a name="what-is-a-planned-maintenance-event"></a>O que é um evento de manutenção planeada?
Um evento de manutenção planeada é uma janela de tempo quando o armazém de dados tem de estar offline para uma operação de manutenção. Esses eventos são oportunidades para a aplicação de patches, novos recursos ou atualizações de serviço. 

## <a name="frequency"></a>Frequência
Em média, pelo menos um evento de manutenção planeada ocorre a cada mês. 

## <a name="notifications-and-downtime"></a>Notificações e tempo de inatividade
Receberá uma notificação antes de cada evento de manutenção planeada. O evento de manutenção cancela todas as consultas em execução e coloca o seu armazém de dados offline. O tempo de inatividade esperado para cada armazém de dados é de aproximadamente 30 minutos. Pode esperar uma notificação quando a manutenção for concluída. 

## <a name="setting-up-alerts"></a>Configuração de alertas

Recomendamos que utilize [do Azure Monitor](../azure-monitor/platform/alerts-activity-log-service-notifications.md) configurar de alertas de registo de manutenção planeada. Os alertas podem ajudar a planear a manutenção necessária minimizar o impacto para o seu negócio. 

Para configurar notificações, utilizá-las [instruções de alerta de registo](../azure-monitor/platform/alerts-activity-log-service-notifications.md). 

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre a monitorização, consulte [monitorizar a carga de trabalho](sql-data-warehouse-manage-monitor.md).
