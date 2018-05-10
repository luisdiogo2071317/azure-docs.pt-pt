---
title: incluir ficheiro
description: incluir ficheiro
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 04/02/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 752feca30fdca663aaf8bd88e6686781b9065682
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
Enviar notificações de modelo, apenas terá de fornecer um conjunto de propriedades. Neste cenário, o conjunto de propriedades contém a versão localizada das notícias atuais.

    {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin": "World News in Mandarin!"
    }



### <a name="send-notifications-using-a-c-console-app"></a>Enviar notificações através de uma aplicação de consola c#
Esta secção mostra como enviar notificações através de uma aplicação de consola. O código difunde notificações para dispositivos da loja Windows e iOS. Modificar o `SendTemplateNotificationAsync` método na aplicação de consola que criou anteriormente com o seguinte código:

```csharp
private static async void SendTemplateNotificationAsync()
{
    // Define the notification hub.
    NotificationHubClient hub = 
        NotificationHubClient.CreateClientFromConnectionString(
            "<connection string with full access>", "<hub name>");

    // Sending the notification as a template notification. All template registrations that contain 
    // "messageParam" or "News_<local selected>" and the proper tags will receive the notifications. 
    // This includes APNS, GCM, WNS, and MPNS template registrations.
    Dictionary<string, string> templateParams = new Dictionary<string, string>();

    // Create an array of breaking news categories.
    var categories = new string[] { "World", "Politics", "Business", "Technology", "Science", "Sports"};
    var locales = new string[] { "English", "French", "Mandarin" };

    foreach (var category in categories)
    {
        templateParams["messageParam"] = "Breaking " + category + " News!";

        // Sending localized News for each tag too...
        foreach( var locale in locales)
        {
            string key = "News_" + locale;

            // Your real localized news content would go here.
            templateParams[key] = "Breaking " + category + " News in " + locale + "!";
        }

        await hub.SendTemplateNotificationAsync(templateParams, category);
    }
}
```

O método SendTemplateNotificationAsync fornece a informação de notícias ao localizada **todos os** os seus dispositivos, independentemente da plataforma. O notification hub baseia-se e fornece o payload nativo correto para todos os dispositivos subscritos uma tag específica.

### <a name="sending-notification-with-mobile-services"></a>Enviar a notificação com os Mobile Services
No seu agendador de Mobile Services, utilize o seguinte script:

```csharp
var azure = require('azure');
var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string with full access>');
var notification = {
        "News_English": "World News in English!",
        "News_French": "World News in French!",
        "News_Mandarin", "World News in Mandarin!"
}
notificationHubService.send('World', notification, function(error) {
    if (!error) {
        console.warn("Notification successful");
    }
});
```

