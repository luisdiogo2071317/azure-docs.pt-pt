---
title: Como enviar notificações agendadas | Microsoft Docs
description: Este tópico descreve a utilizar notificações agendada com Notification Hubs do Azure.
services: notification-hubs
documentationcenter: .net
keywords: notificações push, notificação push, notificações push de agendamento
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 6b718c75-75dd-4c99-aee3-db1288235c1a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: dotnet
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 0f4055a11d22604c0936685a7a2be3d56b259a5b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33776982"
---
# <a name="how-to-send-scheduled-notifications"></a>Como: Enviar notificações agendadas
## <a name="overview"></a>Descrição geral
Se tiver um cenário no qual pretende enviar uma notificação, a determinada altura no futuro mas não dispõe de uma forma fácil de reativação se o código de back-end para enviar a notificação. Hubs de notificação de escalão Standard suportam uma funcionalidade que permite-lhe agendar notificações de segurança para sete dias no futuro.

Quando enviar uma notificação, basta utilizar o [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) classe no SDK dos Notification Hubs, conforme mostrado no exemplo seguinte:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Além disso, pode cancelar uma notificação anteriormente agendada utilizando o respetivo notificationId:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Não existem não existem limites no número de notificações agendadas, que pode enviar.

