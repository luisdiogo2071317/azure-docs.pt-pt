---
title: "Taxa de limitação de SMS, mensagens de correio eletrónico, notificações push da aplicação do Azure e webhooks | Microsoft Docs"
description: "Compreenda a forma como o Azure limita o número de SMS possíveis, e-mail, as notificações de push ou webhook aplicação do Azure de um grupo de ação."
author: dukek
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
ms.date: 12/8/2017
ms.author: dukek
ms.openlocfilehash: c76bf5cf51f18a32b33060d528c64d119e31dbbd
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/09/2017
---
# <a name="rate-limiting-for-sms-messages-emails-azure-app-push-notifications-and-webhook-posts"></a>Taxa de limitação para mensagens SMS e os e-mails, notificações push da aplicação do Azure webhook publicações
Limitação de taxa é um suspensão de notificações que ocorre quando existem demasiados notificações são enviadas para um número de telefone específico, o endereço de e-mail ou o dispositivo. Limitação de taxa garante que os alertas são geríveis e passíveis de ação.

Os limiares de limite de taxa são:

 - **SMS**: mais do que 1 SMS a cada 5 minutos.
 - **E-mail**: 100 mensagens numa hora.
 - **Notificações Push da aplicação do Azure**: não há nenhum taxa de limitação para notificações push.
 - **Webhooks**: não há nenhum taxa de limitação para webhooks.

## <a name="rate-limit-rules"></a>Regras de limite de taxa
- Um número de telefone específico ou o e-mail é limitada quando recebe mensagens mais do que permite que o limiar de taxa.
- Um número de telefone ou e-mail pode fazer parte de grupos de ação entre várias subscrições. Limitação de taxa aplica-se em todas as subscrições. Aplica-se, assim que o limiar for atingido, mesmo se as mensagens são enviadas a partir de várias subscrições.  
- Quando um endereço de e-mail é limitado de velocidade, é enviada uma notificação adicional para comunicar a limitação de taxa. Os Estados de notificação quando expira a limitação de taxa.

## <a name="next-steps"></a>Passos seguintes ##
* Saiba mais sobre [SMS alerta comportamento](monitoring-sms-alert-behavior.md).
* Obter um [descrição geral dos alertas de registo de atividade](monitoring-overview-alerts.md)e saber como receber alertas.  
* Saiba como [configurar alertas sempre que uma notificação de estado de funcionamento do serviço é publicada](monitoring-activity-log-alerts-on-service-notifications.md).
