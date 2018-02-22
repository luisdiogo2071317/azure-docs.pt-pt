---
title: "Comportamento de alerta de SMS em grupos de ação | Microsoft Docs"
description: "Formato de mensagem SMS e responder a mensagens SMS para anular a subscrição, resubscribe ou pedir ajuda."
author: dkamstra
manager: chrad
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2018
ms.author: dukek
ms.openlocfilehash: ce6908de0f6bcc30d1ee846fe92171a0cb589cbb
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
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
| STOP | Desativa o SMS adicional de todos os grupos de ação |
| INICIAR | Volta a ativar SMS de todos os grupos de ação |
| AJUDA | É enviada uma resposta para o utilizador com uma ligação a este artigo. |

>[!NOTE]
>Se um utilizador anulou a subscrição do SMS alertas, mas, em seguida, é adicionado a um novo grupo de ação; estes irão receber alertas SMS para este novo grupo de ação, mas permanecem unsubscribed de todos os grupos de ação anterior.

## <a name="next-steps"></a>Próximos Passos
Obter um [descrição geral dos alertas de registo de atividade](monitoring-overview-alerts.md) e saber como receber alertas  
Saiba mais sobre [limitação de taxa SMS](monitoring-alerts-rate-limiting.md)  
Saiba mais sobre [grupos de ação](monitoring-action-groups.md)
