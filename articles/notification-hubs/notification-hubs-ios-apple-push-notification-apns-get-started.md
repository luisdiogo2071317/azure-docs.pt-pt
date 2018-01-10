---
title: "Introdução aos Hubs de Notificação do Azure para Aplicações iOS | Microsoft Docs"
description: "Neste tutorial, irá aprender a utilizar os Notification Hubs do Azure para enviar notificações push para uma aplicação iOS."
services: notification-hubs
documentationcenter: ios
keywords: "notificação push, notificações push, notificações push do ios"
author: jwhitedev
manager: kpiteira
editor: 
ms.assetid: b7fcd916-8db8-41a6-ae88-fc02d57cb914
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: 0e9e7ab196eef790b74074be319cd8122cf3ff5c
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
---
# <a name="get-started-with-azure-notification-hubs-for-ios-apps"></a>Introdução aos Hubs de Notificação do Azure para Aplicações iOS
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Descrição geral
> [!NOTE]
> Para concluir este tutorial, tem de ter uma conta ativa do Azure. Se não tiver uma conta, pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter mais detalhes, consulte [Avaliação Gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).
> 
> 

Este tutorial mostra como utilizar os Notification Hubs do Azure para enviar notificações push para uma aplicação iOS. Deverá criar uma aplicação iOS em branco que possa receber notificações push utilizando o [Serviço Apple Push Notification (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1). 

Quando tiver terminado, poderá utilizar o Notification Hub para difundir notificações push para todos os dispositivos a executar a sua aplicação.

## <a name="before-you-begin"></a>Antes de começar
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

O código de conclusão deste tutorial pode ser encontrado [no GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

## <a name="prerequisites"></a>Pré-requisitos
Para este tutorial, necessita do seguinte:

* [Framework de Mensagens do Windows Azure]
* Versão mais recente do [Xcode].
* Um dispositivo compatível com iOS 10 (ou versão posterior)
* Associação ao [Programa de Programador da Apple](https://developer.apple.com/programs/)
  
  > [!NOTE]
  > Devido aos requisitos de configuração das notificações push, deve implementar e testar as notificações push num dispositivo iOS físico (iPhone ou iPad) em vez do simulador iOS.
  > 
  > 

A conclusão deste tutorial é um pré-requisito para todos os outros tutoriais dos Notification Hubs para aplicações iOS.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a>Configurar o Notification Hub para notificações push do iOS
Esta secção explica-lhe como criar um novo notification hub e configurar a autenticação com APNs através do certificado push **.p12** que criou anteriormente. Se pretender utilizar um Notification Hub que já tenha criado, pode avançar para o passo 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li>

<p>Em <b>Serviços de Notificação</b> selecione <b>Apple (APNS)</b>. Certifique-se de que seleciona <b>Certificado</b>, clique no ícone de ficheiro e selecione o ficheiro <b>. p12</b> que exportou anteriormente. Não se esqueça de especificar também a palavra-passe correta.</p>

<p>Certifique-se de que selecionou o modo <b>Sandbox</b>, uma vez que se destina ao desenvolvimento. Utilize o modo <b>Produção</b> apenas se pretender enviar notificações push a utilizadores que já tenham adquirido a aplicação da loja.</p>
</li>
</ol>
&emsp;&emsp;&emsp;&emsp;![Configurar o APNS no portal do Azure][6]

&emsp;&emsp;&emsp;&emsp;![Configurar certificação APNS no portal do Azure][7]

O Notification Hub já está configurado para trabalhar com APNs e tem as cadeias de ligação para registar a sua aplicação e enviar notificações push.O Notification Hub já está configurado para trabalhar com APNs e tem as cadeias de ligação para registar a sua aplicação e enviar notificações push.

## <a name="connect-your-ios-app-to-notification-hubs"></a>Ligar a aplicação iOS aos Notification Hubs
1. No Xcode, crie um novo projeto do iOS e selecione o modelo **Aplicação de Vista Única**.
   
    ![Xcode – Aplicação de Vista Única][8]
    
2. Ao definir as opções para o seu novo projeto, não se esqueça de utilizar o mesmo **Nome do Produto** e **Identificador de Organização** que utilizou quando definiu o identificador de pacote no portal de Programador da Apple.
   
    ![Xcode – opções do projeto][11]
    
3. Em Navegador de Projeto, clique no nome do projeto, clique no separador **Geral** e localize **Assinatura**. Certifique-se de que seleciona a Equipa adequada para a conta de Programador da Apple. XCode deve obter automaticamente o Perfil de Aprovisionamento que criou anteriormente com base no identificador de pacote.
   
    Se não vir o novo perfil de aprovisionamento que criou no Xcode, tente atualizar os perfis para a sua identidade de assinatura. Clique em **Xcode** na barra de menus, clique em **Preferências**, clique em **Conta** clique no botão **Ver Detalhes**, clique na sua identidade de assinatura e clique no botão atualizar no canto inferior direito.

    ![Xcode – perfil de aprovisionamento][9]

4. Selecione o separador **Capacidades** e certifique-se de ativar as Notificações Push

    ![Xcode – capacidades de push][12]
   
5. Transfira o [Framework de Mensagens do Windows Azure] e deszipe o ficheiro. No Xcode, clique com o botão direito do rato no projeto e clique na opção **Adicionar Ficheiros a** para adicionar a pasta **WindowsAzureMessaging.framework** ao seu projeto Xcode. Selecione **Opções** e certifique-se de que **Copiar itens, se necessário** está selecionado e, em seguida, clique em **Adicionar**.

    ![Deszipar o Azure SDK][10]

6. Adicione um novo ficheiro de cabeçalho ao projeto com o nome **HubInfo.h**. Este ficheiro vai deter as constantes para o Notification Hub. Adicione as seguintes definições e substitua os marcadores de posição literais de cadeia pelo seu *nome do hub* e pela *DefaultListenSharedAccessSignature* anotado anteriormente.

    ```obj-c
        #ifndef HubInfo_h
        #define HubInfo_h
   
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
   
        #endif /* HubInfo_h */
    ```
    
7. Abra o ficheiro **AppDelegate.h** e adicione as diretivas de importação seguintes:

    ```obj-c
        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
        #import <UserNotifications/UserNotifications.h> 
        #import "HubInfo.h"
    ```
8. No **ficheiro Appdelegate**, adicione o seguinte código em **didFinishLaunchingWithOptions** com base no método na versão do iOS. Este código regista o seu identificador de dispositivo com APNs:

    ```obj-c
        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
            UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];
   
        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];
    ```
   
9. No mesmo ficheiro, adicione os seguintes métodos:

    ```obj-c
         - (void) application:(UIApplication *) application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
           SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];
   
            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
               if (error != nil) {
                   NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                   [self MessageBox:@"Registration Status" message:@"Registered"];
              }
          }];
         }
   
        -(void)MessageBox:(NSString *) title message:(NSString *)messageText
        {
         UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }
    ```

    Este código liga-se ao Notification Hub utilizando as informações de ligação que especificou em HubInfo.h. Em seguida, atribui o token do dispositivo Notification Hub para que este possa enviar notificações.

10. No mesmo ficheiro, adicione o método seguinte para apresentar um **UIAlert** caso a notificação seja recebida enquanto a aplicação está ativa:

    ```obj-c
            - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
               NSLog(@"%@", userInfo);
               [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
           }
    ```

11. Para confirmar que não existem falhas, crie e execute a aplicação no dispositivo.

## <a name="send-test-push-notifications"></a>Enviar notificações push de teste
Pode testar a receção das notificações na aplicação com a opção *Envio de Teste* no [portal do Azure]. Isto irá enviar uma notificação push de teste para o dispositivo.

![Portal do Azure – Envio de Teste][30]

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="checking-if-your-app-can-receive-push-notifications"></a>Verificar se a aplicação pode receber notificações push
Para testar as notificações push no iOS, tem de implementar a aplicação para um dispositivo iOS físico. Não pode enviar notificações push da Apple utilizando o Simulator iOS.

1. Execute a aplicação, verifique se o registo é concluído com êxito e prima **OK**.
   
    ![Teste do Registo de Notificações Push da Aplicação iOS][33]
2. Em seguida, enviará uma notificação push de teste a partir do [portal do Azure], como descrito acima. 

3. A notificação push é enviada a todos os dispositivos que estejam registados para receber as notificações do Notification Hub específico.
   
    ![Teste de Receção de Notificações Push da Aplicação iOS][35]

## <a name="next-steps"></a>Passos seguintes
Neste exemplo simples, difundiu notificações push para todos os dispositivos iOS. Um passo seguinte recomendado na aprendizagem seria iniciar o tutorial [Azure Notification Hubs Notifica os Utilizadores do iOS com back-end .NET]. Este guia irá guiá-lo através da criação de um back-end para enviar notificações push utilizando etiquetas. 

Se pretende segmentar os utilizadores por grupos de interesse, pode, adicionalmente, ler o tutorial [Utilizar Notification Hubs para enviar notícias de última hora]. 

Para obter informações gerais sobre os Notification Hubs, consulte a nossa [Documentação de Orientação dos Notification Hubs].

<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png
[12]: ./media/notification-hubs-ios-get-started/notification-hubs-enable-push.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Framework de Mensagens do Windows Azure]: http://go.microsoft.com/fwlink/?LinkID=799698&clcid=0x409
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Documentação de Orientação dos Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure Notification Hubs Notifica os Utilizadores do iOS com back-end .NET]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Utilizar Notification Hubs para enviar notícias de última hora]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Local and Push Notification Programming Guide]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[portal do Azure]: https://portal.azure.com
