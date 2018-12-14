---
title: Taxa de limitação para mensagens de e-mail, SMS e webhooks e notificações push da aplicação do Azure
description: Compreenda como o Azure limita o número de possíveis SMS, e-mail e as notificações de push ou webhook de aplicação do Azure a partir de um grupo de ação.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 3/12/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 6f60e7c6e6a053e3c563fb1e0850d65311b9baba
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53346232"
---
# <a name="rate-limiting-for-voice-sms-emails-azure-app-push-notifications-and-webhook-posts"></a>Taxa de limitação para voz, SMS, mensagens de correio eletrónico, notificações push da aplicação do Azure e webhook de mensagens
Limitação de velocidade é uma suspensão de notificações que ocorre quando demasiados são enviados para um número de telefone específico, o endereço de e-mail ou o dispositivo. Limitação de velocidade garante que os alertas são gerenciáveis e passíveis de ação.

Os limiares de limite de taxa são:

 - **SMS**: SMS não mais de 1 a cada 5 minutos.
 - **Voz**: Chamada de voz não mais de 1 a cada 5 minutos.
 - **e-mail**: Não mais do que 100 mensagens de e-mail numa hora.
 
 Outras ações não são o limite de taxa.

## <a name="rate-limit-rules"></a>Regras de limite de taxa
- Um número de telefone específico ou o e-mail é limitada ao receber mais mensagens do que permite o limiar de taxa.
- Um número de telefone ou e-mail pode ser parte de grupos de ação em várias subscrições. Limitação de velocidade aplica-se em todas as subscrições. Aplica-se assim que o limiar for atingido, mesmo que as mensagens são enviadas a partir de várias subscrições.
- Quando um endereço de e-mail é taxa limitada, é enviada uma notificação adicional para comunicar a limitação de velocidade. Os Estados de e-mail quando a limitação de velocidade expira.

## <a name="next-steps"></a>Passos Seguintes ##
* Saiba mais sobre [comportamento de alertas do SMS](alerts-sms-behavior.md).
* Obter um [descrição geral dos alertas de registo de atividade](alerts-overview.md)e saiba como receber alertas.  
* Saiba como [configurar alertas sempre que uma notificação de estado de funcionamento do serviço é lançada](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).
