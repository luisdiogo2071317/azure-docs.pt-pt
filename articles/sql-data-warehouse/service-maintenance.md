---
title: A manutenção - Azure SQL Data Warehouse planeada | Microsoft Docs
description: Saiba como preparar para eventos de manutenção planeada para o Azure SQL Data Warehouse.
services: sql-data-warehouse
author: antvgski
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/19/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: a273ad4c256267865d3af324f0ef755a6cb75c5c
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="planning-for-maintenance-on-your-azure-sql-data-warehouse"></a>Planeamento de manutenção no seu armazém de dados SQL do Azure

Saiba como preparar para eventos de manutenção planeada no seu armazém de dados SQL do Azure.

## <a name="what-is-a-planned-maintenance-event"></a>O que é um evento de manutenção planeada?
Um evento de manutenção planeada é uma janela de tempo ao seu armazém de dados tem de ser offline para uma operação de manutenção. Estes eventos são oportunidades para aplicar atualizações de serviço, novas funcionalidades ou os patches. 

## <a name="frequency"></a>Frequência
Em média, pelo menos um evento de manutenção planeado ocorre a cada mês. 

## <a name="notifications-and-downtime"></a>Notificações e período de indisponibilidade
Receberá uma notificação antes de cada evento de manutenção planeada. O evento de manutenção cancela todas as consultas em execução e coloca o armazém de dados offline. O período de indisponibilidade esperado para o armazém de dados é aproximadamente 30 minutos. Pode esperar uma notificação quando a manutenção está concluída. 

## <a name="setting-up-alerts"></a>Como configurar alertas

Recomendamos que utilize [Azure Monitor](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md) configurar a ser planeada a alertas de registo de manutenção. Os alertas podem ajudar a planear para a manutenção necessária minimizar o impacto ao seu negócio. 

Para configurar notificações, utilizá-las [instruções de alerta de registo](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md). 

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre a monitorização, consulte [monitorizar a carga de trabalho](sql-data-warehouse-manage-monitor.md).
