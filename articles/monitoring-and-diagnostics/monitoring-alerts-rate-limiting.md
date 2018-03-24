---
title: Taxa de limitação de SMS, mensagens de correio eletrónico, notificações push da aplicação do Azure e webhooks | Microsoft Docs
description: Compreenda a forma como o Azure limita o número de SMS possíveis, e-mail, as notificações de push ou webhook aplicação do Azure de um grupo de ação.
author: dkamstra
manager: chrad
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/12/2018
ms.author: dukek
ms.openlocfilehash: 9216a64dbd8201ff09ea5c9283b4db465682a3bd
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
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
