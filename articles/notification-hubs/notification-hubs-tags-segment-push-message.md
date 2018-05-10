---
title: Encaminhamento e expressões de etiqueta
description: Este tópico explica as expressões de encaminhamento e a etiqueta de notification hubs do Azure.
services: notification-hubs
documentationcenter: .net
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 0fffb3bb-8ed8-4e0f-89e8-0de24a47f644
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: e08fca0b6b57d654f2b2ff7b935f38d8c517487b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="routing-and-tag-expressions"></a>Expressões de encaminhamento e etiqueta
## <a name="overview"></a>Descrição geral
Expressões de etiqueta permitem-lhe a conjuntos específicos do destino de dispositivos ou registos mais especificamente, ao enviar uma notificação push através dos Notification Hubs.

## <a name="targeting-specific-registrations"></a>Filtragem de registos específicos
A única forma de destino de notificação específica registos é para associar etiquetas com os mesmos, em seguida, destino dessas etiquetas. Tal como explicado [registo gestão](notification-hubs-push-notification-registration-management.md), para receber notificações de uma aplicação tem de registar um dispositivo identificador no hub de notificação de push. Assim que for criado um registo no hub de notificação, o back-end de aplicação pode enviar notificações push para o mesmo.
Back-end da aplicação pode escolher os registos de destino com uma notificação específica das seguintes formas:

1. **Difusão**: todos os registos no notification hub recebem a notificação.
2. **Etiqueta**: todos os registos que contêm a etiqueta especificada recebem a notificação.
3. **Etiqueta expressão**: todos os registos cujo conjunto de etiquetas corresponde a expressão especificada ao recebem a notificação.

## <a name="tags"></a>Etiquetas
Uma etiqueta pode ser qualquer cadeia, até 120 carateres, alfanuméricos e os seguintes carateres não alfanuméricos: '_', ' @', '#', '. ',':', '-'. O exemplo seguinte mostra uma aplicação a partir da qual pode receber notificações de alerta sobre os grupos de leitores de música específico. Neste cenário, uma forma simples de notificações de rota é registos de etiqueta com etiquetas que representam as bandas diferentes, como a imagem seguinte:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags.png)

Esta imagem, a mensagem etiquetados **Beatles** atingir apenas o tablet registado com a etiqueta **Beatles**.

Para obter mais informações sobre a criação de registos para etiquetas, consulte [registo gestão](notification-hubs-push-notification-registration-management.md).

Pode enviar notificações para etiquetas utilizando os métodos de notificações de envio do `Microsoft.Azure.NotificationHubs.NotificationHubClient` classe no [os Notification Hubs do Microsoft Azure](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) SDK. Também pode utilizar o Node.js ou as APIs de REST de notificações Push.  Eis um exemplo utilizando o SDK.

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You requested a Beatles notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Beatles");

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You requested a Wailers notification</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, "Wailers");




As etiquetas não têm de ser previamente aprovisionadas e podem fazer referência a várias conceitos de específico da aplicação. Por exemplo, os utilizadores desta aplicação de exemplo podem comentar bandas e pretende receber os seus amigos, independentemente da banda na qual são comentar toasts, não apenas para os comentários no respetivos bandas Favoritos, mas também para todos os seus comentários. A imagem seguinte mostra um exemplo deste cenário:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags2.png)

Esta imagem, Alice está interessada nas atualizações para o Beatles e João está interessado nas atualizações para os Wailers. João também está interessado em comentários de Charlie, não sendo Charlie no interessados os Wailers. Quando é enviada uma notificação para o comentário de Charlie o Beatles, Alice e Bernardo recebe-lo.

Enquanto pode codificar múltiplas preocupações em etiquetas (por exemplo, "band_Beatles" ou "follows_Charlie"), as etiquetas são cadeias simples e não propriedades com valores. Um registo é correspondido apenas na presença ou ausência de uma tag específica.

Para um tutorial completa passo a passo sobre como utilizar etiquetas para enviar para grupos de interesses, consulte [notícias de última hora](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md).

## <a name="using-tags-to-target-users"></a>Utilizar etiquetas para utilizadores de destino
Outra forma, a utilização de etiquetas é identificar todos os dispositivos que um utilizador específico. Registos podem ser etiquetados com uma tag que contém um ID de utilizador, como a imagem seguinte:

![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags3.png)

Esta imagem, a mensagem etiquetados uid: Alice atinge todos os registos marcado uid:Alice; Por conseguinte, todos os dispositivos de Alice.

## <a name="tag-expressions"></a>Expressões de etiqueta
Existem casos em que uma notificação tem um conjunto de registos, que é identificado não por uma única tag, mas por uma expressão booleana em tags de destino.

Considere um desporto de aplicação que envia um lembrete para todas as pessoas Boston sobre um jogo entre o vermelho Sox e Cardinals. Se a aplicação cliente regista etiquetas sobre interesse no equipas e a localização, a notificação deve ser direcionada para todos os utilizadores Boston que está interessado no Sox vermelha ou as Cardinals. Esta condição pode ser expressada com a seguinte expressão booleana:

    (follows_RedSox || follows_Cardinals) && location_Boston


![](./media/notification-hubs-routing-tag-expressions/notification-hubs-tags4.png)

Expressões de etiqueta podem conter todos os operadores booleanos, tais como AND (& &), ou (|) e não (!). Também podem conter parênteses. Expressões de etiqueta estão limitadas a 20 etiquetas se contêm apenas ORs; caso contrário, estão limitadas a 6 etiquetas.

Eis um exemplo para enviar notificações com expressões de etiqueta com o SDK.

    Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;

    String userTag = "(location_Boston && !follows_Cardinals)";    

    // Windows 8.1 / Windows Phone 8.1
    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
    "You want info on the Red Sox</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

    // Windows 10
    toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
    "You want info on the Red Sox</text></binding></visual></toast>";
    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
