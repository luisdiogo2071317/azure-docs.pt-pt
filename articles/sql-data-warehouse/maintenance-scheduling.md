---
title: Agendamentos de manutenção do Azure (pré-visualização) | Documentos da Microsoft
description: Agendamento de manutenção permite aos clientes planejar os eventos de manutenção agendada necessário que o serviço do Azure SQL Data warehouse utiliza para implementar novas funcionalidades, atualizações e patches.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 09/20/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: 85e8327d1cac17059b2e91b1ea21becc23002c8e
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228467"
---
# <a name="using-maintenance-schedules-to-manage-service-updates-and-maintenance"></a>Utilizar agendamentos de manutenção para gerir atualizações de serviço e manutenção

O Azure SQL Data Warehouse manutenção de agendamento está agora em pré-visualização. Esta nova funcionalidade de forma totalmente integrada integra-se as notificações de manutenção planeada do serviço de estado de funcionamento do Monitor de verificação de estado de funcionamento de recursos e o serviço de agendamento de manutenção do Azure SQL Data Warehouse.

Agendamento de manutenção permite agendar uma janela de tempo quando é conveniente receber as novas funcionalidades, atualizações e patches. Os clientes selecionará um site primário e uma janela de manutenção secundário dentro de um período de 7 dias, ou seja, Sábado 22:00 – Domingo 01:00 (primário) e a quarta-feira 19:00 – 22:00 (secundário). Se não for possível executar a manutenção durante a janela de manutenção primário, podemos irá tentar novamente durante a janela de manutenção secundário.

Todos os recentemente criado Azure SQL Data Warehouse instâncias terá uma agenda de manutenção definidas pelo sistema aplicada durante a implementação. O agendamento pode ser editado, assim como a implementação estiver concluída.

Cada janela de manutenção pode ter entre 3 e 8 horas cada, com 3hrs atualmente a ser a opção disponível mais curta. Manutenção pode ocorrer em qualquer altura dentro da janela, e deve esperar uma breve perda de conectividade, como o service implementa o novo código para o armazém de dados. 

Durante a pré-visualização de funcionalidade, pedimos aos clientes para identificar suas janelas primária e secundária nos intervalos de dias separados.   
Todas as operações de manutenção devem ser executadas nas janelas de manutenção agendada e sem manutenção terá lugar fora das janelas de manutenção especificado sem notificação anterior. Se o seu armazém de dados está em pausa durante a manutenção agendada, ele será atualizado durante a operação de retoma.  


## <a name="alerts-and-monitoring"></a>Alertas e monitorização

Integração total com as notificações de estado de funcionamento do serviço e o estado de funcionamento do recurso da verificação monitor permite aos clientes para se manter informado da atividade de manutenção iminente. A nova automação tira partido do Monitor do Azure e permite aos clientes determinar como quiser ser notificado de uma iminente eventos de manutenção e deverá ser acionada automatizada que fluxos para gerir o tempo de inatividade e minimizar o impacto de suas operações.

Todos os eventos de manutenção são precedidos por uma notificação antecipada de 24 horas. Para minimizar o período de indisponibilidade de instância, deve certificar-se de que não há nenhuma transação de longa execução no seu armazém de dados antes do início durante seu período de manutenção escolhido. Quando inicia de manutenção, serão canceladas todas as sessões ativas, as transações não consolidadas serão revertidas e o armazém de dados terão uma pequena perda de conectividade. Será notificado imediatamente após a manutenção foi concluída no seu armazém de dados. 

Se tiver recebido uma notificação antecipada que manutenção ocorrerá, mas não é possível realizar a manutenção durante esse período, receberá uma notificação de cancelamento. Em seguida, retoma-se manutenção durante o período de manutenção agendada seguinte.
 
Todos os eventos de manutenção do Active Directory serão apresentados no "serviço de estado de funcionamento - manutenção planeada" secção. Um registo completo de eventos anteriores será mantido como parte do histórico do Estado de funcionamento do serviço. Manutenção pode ser monitorizada através do dashboard do portal do Azure Service Health verificação durante um evento de Active Directory.

### <a name="maintenance-schedule-availability"></a>Disponibilidade de agenda de manutenção

Mesmo que o agendamento de manutenção ainda não está disponível na sua região selecionada, pode ainda ver e editar a sua agenda de manutenção em qualquer altura. Quando o agendamento de manutenção se torna disponível na sua região, a agenda identificada ficarão imediatamente Active Directory no seu armazém de dados.


## <a name="next-steps"></a>Passos Seguintes

- [Saiba mais](viewing-maintenance-schedule.md) sobre a visualização de uma agenda de manutenção 
- [Saiba mais](changing-maintenance-schedule.md) sobre como alterar uma agenda de manutenção
- [Saiba mais](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-usage) sobre como criar, visualizar e gerir alertas através do Azure Monitor
- [Saiba mais](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) sobre as ações de Webhook para regras de alerta de registo
- [Saiba mais](https://docs.microsoft.com/azure/service-health/service-health-overview) sobre o Azure Service Health







