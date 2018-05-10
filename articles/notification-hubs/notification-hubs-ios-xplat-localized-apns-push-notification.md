---
title: Localizado notificações push para dispositivos iOS com Notification Hubs do Azure | Microsoft Docs
description: Saiba como utilizar notificações de push localizado em dispositivos iOS com Notification Hubs do Azure.
services: notification-hubs
documentationcenter: ios
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 484914b5-e081-4a05-a84a-798bbd89d428
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: b3d74086ee233da50138aff00d8da78aa0243a75
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-push-localized-notifications-to-ios-devices-using-azure-notification-hubs"></a>Tutorial: Push localizado notificações para dispositivos iOS com Notification Hubs do Azure 
> [!div class="op_single_selector"]
> * [Loja Windows c#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 

Este tutorial mostra como utilizar o [modelos](notification-hubs-templates-cross-platform-push-messages.md) funcionalidade dos Hubs de notificação do Azure para difundir notificações de notícias de última que foram localizadas por idioma e o dispositivo. Neste tutorial, começa com a aplicação iOS que criou no [utilizar Notification Hubs para enviar notícias de última hora]. Quando terminar, pode registar categorias que está interessado, especifique um idioma no qual pretende receber as notificações e receber notificações de push apenas para as categorias selecionadas nesse idioma.

Existem duas partes para este cenário:

* aplicação iOS permite aos clientes dispositivos para especificar um idioma e subscrever a categorias de notícias de última diferente;
* Back-end difunde as notificações, utilizando o **tag** e **modelo** funcionalidades dos Notification Hubs do Azure.

Neste tutorial, siga os passos seguintes:

> [!div class="checklist"]
> * Atualizar a interface de utilizador de aplicação
> * Criar a aplicação iOS
> * Enviar notificações de localizada do modelo de aplicação de consola .NET
> * Enviar notificações de localizada do modelo do dispositivo


## <a name="overview"></a>Descrição geral
No [utilizar Notification Hubs para enviar notícias de última hora], criou uma aplicação que utilizado **etiquetas** subscrever notificações para as categorias de notícias de última hora diferente. Muitas aplicações, no entanto, vários mercados de destino e que necessitam de localização. Significa que o conteúdo das notificações próprios tem de estar localizadas e enviada para o conjunto correto de dispositivos. Este tutorial mostra como utilizar o **modelo** funcionalidade dos Hubs de notificação facilmente entregar notificações de notícias de última localizada.

> [!NOTE]
> Uma forma para enviar notificações localizadas consiste em criar várias versões de cada etiqueta. Por exemplo, para suportar o inglês, francês e Mandarim, terá três etiquetas diferentes para notícias mundo: "world_en", "world_fr" e "world_ch". Em seguida, terá de enviar uma versão localizada das notícias de mundo para cada um destas etiquetas. Neste tópico, utilize modelos para evitar a proliferação de etiquetas e o requisito de várias mensagens a enviar.

Um nível elevado, os modelos são uma forma para especificar a forma como um dispositivo específico deverá receber uma notificação. O modelo especifica o formato de payload exato ao referir-se para propriedades que fazem parte da mensagem enviada pelo seu back-end da aplicação. No seu caso, envie uma mensagem de região agnóstico que contém todas as linguagens de:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Em seguida, certifique-se de que os dispositivos registar com um modelo que refere-se para a propriedade correta. Por exemplo, uma aplicação iOS que pretende registar de notícias francês regista utilizando a seguinte sintaxe:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Para obter mais informações sobre modelos, consulte [modelos](notification-hubs-templates-cross-platform-push-messages.md) artigo.

## <a name="prerequisites"></a>Pré-requisitos

- Concluir o [notificações Push para dispositivos iOS específico](notification-hubs-ios-xplat-segmented-apns-push-notification.md) tutorial e tem o código disponível, porque este tutorial de mensagens em fila baseia-se diretamente nesse código.
- Visual Studio 2012 ou posterior é opcional.

## <a name="update-the-app-user-interface"></a>Atualizar a interface de utilizador de aplicação
Nesta secção, modificar a aplicação de notícias de última hora que criou no tópico [utilizar Notification Hubs para enviar notícias de última hora] enviar localizado utilizando modelos de notícias de última hora.

Na sua MainStoryboard_iPhone.storyboard, adicionar um controlo segmentados com os três idiomas: inglês, francês e Mandarim.

![][13]

Em seguida, certifique-se de que adiciona um IBOutlet no seu viewcontroller. H, conforme mostrado na imagem seguinte:

![][14]

## <a name="build-the-ios-app"></a>Criar a aplicação iOS
1. Na sua Notification.h adicionar o *retrieveLocale* método e modificar o arquivo e subscrever métodos, conforme mostrado no seguinte código:
   
    ```obj-c
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;
   
        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;
   
        - (NSSet*) retrieveCategories;
   
        - (int) retrieveLocale;
   
    ```
    Na sua Notification.m, modifique o *storeCategoriesAndSubscribe* método, adicionando o parâmetro de região e armazenar nas predefinições do utilizador:
   
    ```obj-c
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];
   
            [self subscribeWithLocale: locale categories:categories completion:completion];
        }
    ````
    Em seguida, modifique o *subscrever* método para incluir a região:
   
    ```obj-c
        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];
   
            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }
   
            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];
   
            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }
       ```
    You use the method *registerTemplateWithDeviceToken*, instead of *registerNativeWithDeviceToken*. When you register for a template, you have to provide the json template and also a name for the template (as the app might want to register different templates). Make sure to register your categories as tags, as you want to make sure to receive the notifciations for those news.
   
    Add a method to retrieve the locale from the user default settings:
   
    ```obj-c
        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
   
            int locale = [defaults integerForKey:@"BreakingNewsLocale"];
   
            return locale < 0?0:locale;
        }
    ```
2. Agora que alterou a classe de notificações, tem de certificar-se de que o ViewController torna a utilizar o novo UISegmentControl. Adicione a seguinte linha no *viewDidLoad* método para se certificar-se de que mostram a região atualmente selecionado:
   
    ```obj-c
        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];
     ```  
    Em seguida, no seu *subscrever* método, alterar a chamada para o *storeCategoriesAndSubscribe* para o seguinte código:
   
    ```obj-c
        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];
    ```
3. Por fim, é necessário atualizar o *didRegisterForRemoteNotificationsWithDeviceToken* método no seu Appdelegate, para que corretamente, pode atualizar o seu registo quando a aplicação for iniciada. Alterar a chamada para o *subscrever* método de notificações com o seguinte código:
   
    ```obj-c
        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];
    ```

## <a name="optional-send-localized-template-notifications-from-net-console-app"></a>(opcional) Enviar notificações de localizada do modelo de aplicação de consola .NET
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

## <a name="optional-send-localized-template-notifications-from-the-device"></a>(opcional) Enviar notificações de localizada do modelo do dispositivo
Se não tiver acesso para Visual Studio, ou apenas pretende teste enviar as notificações de modelo localizada diretamente a partir da aplicação no dispositivo. Pode adicionar os parâmetros do modelo localizada para a `SendNotificationRESTAPI` método definido no tutorial anterior.

    ```obj-c
        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }
    ```


## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, enviadas notificações localizadas em dispositivos iOS. Para saber como notificações push a utilizadores específicos de aplicações iOS, avançar para o tutorial seguinte: 

> [!div class="nextstepaction"]
>[Notificações push a utilizadores específicos](notification-hubs-aspnet-backend-ios-apple-apns-notification.md)

<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[utilizar Notification Hubs para enviar notícias de última hora]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
