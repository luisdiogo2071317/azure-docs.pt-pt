---
title: Comportamento de alerta de SMS em grupos de ação
description: Formato de mensagem SMS e responder a mensagens SMS para anular a subscrição, resubscribe ou pedir ajuda.
author: dkamstra
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: f2f463f6c428ce6c72e2640472376fa17a2bfe5a
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263012"
---
# <a name="sms-alert-behavior-in-action-groups"></a>Comportamento de grupos de ação de alerta de SMS
## <a name="overview"></a>Descrição geral ##
Grupos de ação permitem-lhe configurar uma lista de ações. Estes grupos são utilizados quando definir alertas; garantir que um grupo de ação específica é notificado quando o alerta é acionado. Uma das ações suportadas é SMS; Notificações do SMS suportam comunicação bidirecional. Um utilizador pode responder a um SMS para:

- **Anular a subscrição de alertas:** um utilizador pode anular todos os alertas SMS para todos os grupos de ação ou um grupo de única ação.
- **Resubscribe alertas:** um utilizador pode resubscribe para todos os alertas SMS para todos os grupos de ação ou um grupo de única ação.  
- **Pedir ajuda:** um utilizador pode pedir para obter mais informações sobre o SMS. Será redirecionado para este artigo.

Este artigo abrange o comportamento dos alertas SMS e as ações de resposta o utilizador pode efetuar com base na região do utilizador:

## <a name="receiving-an-sms-alert"></a>Receber um alerta SMS
Um recetor SMS configurado como parte de um grupo de ação recebe um SMS quando um alerta é acionado. O SMS contém as seguintes informações:
* Shortname do grupo de ação que este alerta foi enviado para
- Título do alerta

| RESPOSTA | Descrição |
| ----- | ----------- |
| DESATIVAR <Action Group Short name> | Desativa o SMS adicional do grupo de ação |
| ATIVAR <Action Group Short name> | Volta a ativar SMS do grupo de ação |
| PARAR | Desativa o SMS adicional de todos os grupos de ação |
| INICIAR | Volta a ativar SMS de todos os grupos de ação |
| AJUDA | É enviada uma resposta para o utilizador com uma ligação a este artigo. |

>[!NOTE]
>Se um utilizador anulou a subscrição do SMS alertas, mas, em seguida, é adicionado a um novo grupo de ação; estes irão receber alertas SMS para este novo grupo de ação, mas permanecem unsubscribed de todos os grupos de ação anterior.

## <a name="next-steps"></a>Próximos Passos
Obter um [descrição geral dos alertas de registo de atividade](monitoring-overview-alerts.md) e saber como receber alertas  
Saiba mais sobre [limitação de taxa SMS](monitoring-alerts-rate-limiting.md)  
Saiba mais sobre [grupos de ação](monitoring-action-groups.md)
