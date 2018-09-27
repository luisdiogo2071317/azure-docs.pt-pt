---
title: Agendamentos de manutenção do Azure (pré-visualização) | Documentos da Microsoft
description: Agendamento de manutenção permite aos clientes planejar os eventos de manutenção agendada necessário que o serviço do Azure SQL Data warehouse utiliza para implementar novas funcionalidades, atualizações e patches.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 10/06/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: 6fbf914c8035344d33e8d2739fb9d92d5757c3d1
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228430"
---
# <a name="viewing-a-maintenance-schedule"></a>Visualizar um agendamento de manutenção 

## <a name="portal"></a>Portal

Por predefinição, todas as recém-criadas Azure SQL Data Warehouse instâncias terá uma 8 horas definido pelo sistema primária e secundária janela de manutenção aplicada durante a implementação, isso pode ser editado que em breve a implementação estiver concluída. Sem manutenção terá lugar fora das janelas de manutenção especificado sem notificação anterior.
Conclua os passos seguintes para ver a agenda de manutenção que foi aplicada ao seu armazém de dados no portal.

Conclua os passos seguintes para ver a agenda de manutenção que foi aplicada ao seu armazém de dados no portal.
1.  Inicie sessão no [portal do Azure](https://portal.azure.com/).
2.  Selecione o armazém de dados que gostaria de ver. 
3.  O Azure SQL Data Warehouse selecionado será aberta no painel de descrição geral. A agenda de manutenção aplicada ao armazém de dados selecionada será mostrada a seguir a agenda de manutenção (pré-visualização).

![Painel de descrição geral](media/sql-data-warehouse-maintenance-scheduling/clear-overview-blade.PNG)

## <a name="next-steps"></a>Passos Seguintes
[Saiba mais](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) sobre como criar, visualizar e gerir alertas através do Azure Monitor.
[Saiba mais](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) sobre as ações de Webhook para regras de alerta de registo.
[Saiba mais](https://docs.microsoft.com/azure/service-health/service-health-overview) sobre o Azure Service Health


