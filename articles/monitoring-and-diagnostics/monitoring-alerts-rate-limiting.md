---
title: Taxa de limitação de SMS, mensagens de correio eletrónico, notificações push da aplicação do Azure e webhooks
description: Compreenda a forma como o Azure limita o número de SMS possíveis, e-mail, as notificações de push ou webhook aplicação do Azure de um grupo de ação.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 3/12/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 1acea47638c75971bad62f28dcd7e96823d84c1d
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35262961"
---
# <a name="rate-limiting-for-voice-sms-emails-azure-app-push-notifications-and-webhook-posts"></a>Taxa de limitação de voz, SMS e os e-mails, notificações push da aplicação do Azure webhook publicações
Limitação de taxa é um suspensão de notificações que ocorre quando existem demasiados são enviados para um número de telefone específico, o endereço de e-mail ou o dispositivo. Limitação de taxa garante que os alertas são geríveis e passíveis de ação.

Os limiares de limite de taxa são:

 - **SMS**: mais do que 1 SMS a cada 5 minutos.
 - **Voz**: a cada 5 minutos de chamada de voz mais do que 1.
 - **E-mail**: mais do que 100 e-mails numa hora.
 
 Outras ações não são taxa limitada.

## <a name="rate-limit-rules"></a>Regras de limite de taxa
- Um número de telefone específico ou o e-mail é limitada quando recebe mensagens mais do que permite que o limiar de taxa.
- Um número de telefone ou e-mail pode fazer parte de grupos de ação entre várias subscrições. Limitação de taxa aplica-se em todas as subscrições. Aplica-se, assim que o limiar for atingido, mesmo se as mensagens são enviadas a partir de várias subscrições.
- Quando um endereço de e-mail é limitado de velocidade, é enviada uma notificação adicional para comunicar a limitação de taxa. Os Estados de correio eletrónico quando expira a limitação de taxa.

## <a name="next-steps"></a>Passos Seguintes ##
* Saiba mais sobre [SMS alerta comportamento](monitoring-sms-alert-behavior.md).
* Obter um [descrição geral dos alertas de registo de atividade](monitoring-overview-alerts.md)e saber como receber alertas.  
* Saiba como [configurar alertas sempre que uma notificação de estado de funcionamento do serviço é publicada](monitoring-activity-log-alerts-on-service-notifications.md).
