---
title: "Introdução aos Hubs de Notificação do Azure para Aplicações Xamarin.iOS | Microsoft Docs"
description: "Neste tutorial, irá aprender a utilizar os Notification Hubs do Azure para enviar notificações push para uma aplicação Xamarin iOS."
services: notification-hubs
keywords: "notificações push ios, mensagens push, notificações push, mensagem push"
documentationcenter: xamarin
author: jwhiteDev
manager: kpiteira
editor: 
ms.assetid: 4d4dfd42-c5a5-4360-9d70-7812f96924d2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: edb48cd8de9b1f7357c40cea73fa4b9a422cb8fa
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
---
# <a name="get-started-with-azure-notification-hubs-for-xamarinios-apps"></a>Introdução aos Hubs de Notificação do Azure para Aplicações Xamarin.iOS
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Descrição geral
> [!NOTE]
> Para concluir este tutorial, tem de ter uma conta ativa do Azure. Se não tiver uma conta, pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter mais detalhes, consulte [Avaliação Gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).
> 
> 

Este tutorial mostra como utilizar os Notification Hubs do Azure para enviar notificações push para uma aplicação iOS. Irá criar uma aplicação Xamarin.iOS em branco que recebe notificações push, utilizando o [Serviço Apple Push Notification (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html). 

Quando tiver terminado, poderá utilizar o Notification Hub para difundir notificações push para todos os dispositivos a executar a sua aplicação. O código concluído está disponível no exemplo [aplicação NotificationHubs][GitHub].

Este tutorial demonstra o cenário de difusão simples de mensagens push com Notification Hubs.

## <a name="prerequisites"></a>Pré-requisitos
Para este tutorial, necessita do seguinte:

* Versão mais recente do [XCode][Install Xcode]
* Um dispositivo compatível iOS 10 (ou versão posterior)
* Associação ao [Programa de Programador da Apple](https://developer.apple.com/programs/)
* [Visual Studio para Mac]
  
  > [!NOTE]
  > Devido aos requisitos de configuração para as notificações push do iOS, deve implementar e testar a aplicação de exemplo num dispositivo iOS físico (iPhone ou iPad) em vez do simulador.
  > 
  > 

A conclusão deste tutorial é um pré-requisito para todos os outros tutoriais de Hubs de Notificação para aplicações Xamarin.iOS.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a>Configurar o Notification Hub para notificações push do iOS
Esta secção explica-lhe como criar um novo notification hub e configurar a autenticação com APNS através do certificado push **.p12** que criou anteriormente. Se pretender utilizar um Notification Hub que já tenha criado, pode avançar para o passo 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li>

<p>Clique no botão <b>Serviços de Notificação</b> e selecione <b>Apple (APNS)</b>. Certifique-se de que seleciona <b>Certificado</b>, clique no ícone de ficheiro e selecione o ficheiro <b>. p12</b> que exportou anteriormente. Não se esqueça de especificar também a palavra-passe correta.</p>

<p>Certifique-se de que selecionou o modo <b>Sandbox</b>, uma vez que se destina ao desenvolvimento. Utilize o modo <b>Produção</b> apenas se pretender enviar notificações push a utilizadores que já tenham adquirido a aplicação da loja.</p>
</li>
</ol>

&emsp;&emsp;&emsp;&emsp;![Configurar o APNS no portal do Azure][6]

&emsp;&emsp;&emsp;&emsp;![Configurar certificação APNS no portal do Azure][7]

O Notification Hub já está configurado para trabalhar com APNs e tem as cadeias de ligação para registar a sua aplicação e enviar notificações push.

## <a name="connect-your-app-to-the-notification-hub"></a>Ligar a aplicação ao Notification Hub
#### <a name="create-a-new-project"></a>Criar um novo projeto
1. No Visual Studio, crie um novo projeto do iOS e selecione o modelo **Aplicação de Vista Única** e clique em **Seguinte**
   
     ![Visual Studio - Selecione o Tipo de Aplicação][31]

2. Introduza o identificador do nome da aplicação e a organização e, em seguida, prima **Seguinte** e **Criar**

3. Na vista Solução, faça duplo clique em *Into.plist* e em **identidade** certifique-se de que o identificador do pacote corresponde ao utilizado ao criar o perfil de aprovisionamento. Em **Assinatura** certifique-se de que a sua conta de Programador está selecionada em **Equipa**, que "Gerir assinatura automaticamente" está selecionado e que o certificado de assinatura e perfil de aprovisionamento são automaticamente selecionados.

    ![Configuração de Aplicação do Visual Studio-iOS][32]

4. Adicione o pacote de Mensagens do Azure. Na vista Solução, clique com o botão direito no projeto e selecione **Adicionar** > **Adicionar pacotes de NuGet**. Procure **Xamarin.Azure.NotificationHubs.iOS** e adicione o pacote ao projeto.

5. Adicione um novo ficheiro à classe, nomeie-o **Constants.cs** e adicione as seguintes variáveis e substitua os marcadores de posição literais de cadeia pelo seu *nome do hub* e pela *DefaultListenSharedAccessSignature* anotada anteriormente.
   
    ```csharp
        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";
    ```

6. Em **AppDelegate.cs**, adicione o seguinte ao utilizar a instrução:
   
    ```csharp
        using WindowsAzure.Messaging;
    ```

7. Declare uma instância do **SBNotificationHub**:
   
    ```csharp
        private SBNotificationHub Hub { get; set; }
    ```

8. Em **AppDelegate.cs**, atualize **FinishedLaunching()** para fazer corresponder o seguinte:
   
    ```csharp
        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());
   
                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }
   
            return true;
        }
    ```

9. Substitua o método **RegisteredForRemoteNotifications()** em **AppDelegate.cs**:
   
    ```csharp
        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);
   
            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }
   
                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }
    ```

10. Substitua o método **ReceivedRemoteNotification()** em **AppDelegate.cs**:
   
    ```csharp
        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }
    ```

11. Crie o seguinte método **ProcessNotification()** em **AppDelegate.cs**:
   
    ```csharp
        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
   
                string alert = string.Empty;
   
                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();
   
                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }
    ```
   > [!NOTE]
   > Pode optar por substituir **FailedToRegisterForRemoteNotifications()** para processar situações como falha de ligação de rede. Isto é especialmente importante quando o utilizador inicia a aplicação no modo offline (por exemplo, Avião) e pretende processar cenários de mensagens push específicos para a sua aplicação.
  

12. Execute o aplicativo no seu dispositivo.

## <a name="sending-test-push-notifications"></a>Enviar Notificações Push de Teste
Pode testar a receção das notificações na aplicação com a opção *Envio de Teste* no [portal do Azure]. Esta ação envia uma notificação push de teste para o dispositivo.

![Portal do Azure – Envio de Teste][30]

Normalmente, as notificações push são enviadas num serviço de back-end como Mobile Apps ou ASP.NET com uma biblioteca compatível. Caso não esteja disponível uma biblioteca para o back-end, também pode utilizar a API REST diretamente para enviar mensagens de notificação.

Como passo seguinte é recomendado que veja o tutorial [Use Notification Hubs to push notifications to users ](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) (Utilizar os Hubs de Notificação para enviar notificações push aos utilizadores) como passo seguinte para enviar notificações de um back-end do ASP.NET. No entanto, as seguintes abordagens podem ser utilizadas para enviar notificações:

Apresentamos a seguir uma lista de outros tutoriais que pode querer rever acerca do envio de notificações:
* [Interface REST](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx): Pode suportar notificações push em qualquer plataforma de back-end utilizando a interface REST.
* **SDK .NET dos Notification Hubs do Microsoft Azure**: no Gestor de Pacotes Nuget para Visual Studio, execute [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).
* [Node.js](notification-hubs-nodejs-push-notification-tutorial.md): como utilizar os Hubs de Notificação do Node.js.
* Java/PHP**: para obter um exemplo sobre como enviar notificações push com as APIs REST, veja “How to use Notification Hubs from Java/PHP” (Como utilizar os Hubs de Notificação de Java/PHP) ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

## <a name="next-steps"></a>Passos seguintes
Neste exemplo simples, difundiu notificações push para todos os seus dispositivos iOS. Para abordar utilizadores específicos, consulte o tutorial [Utilizar Notification Hubs para notificações push a utilizadores]. Se pretender segmentar os utilizadores por grupos de interesse, pode ler [Utilizar Notification Hubs para enviar notícias de última hora]. Saiba mais sobre como utilizar Notification Hubso em [Documentação de Orientação dos Notification Hubs] e em [Procedimentos de Notification Hubs para iOS].

<!-- Images. -->
[6]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png
[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png
[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png
[32]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-app-settings.png



<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Windows Azure Messaging Framework]: http://go.microsoft.com/fwlink/?LinkID=799698&clcid=0x409
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-ios
[Documentação de Orientação dos Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Procedimentos de Notification Hubs para iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456
[Visual Studio para Mac]: https://www.visualstudio.com/vs/visual-studio-mac/
[Utilizar Notification Hubs para notificações push a utilizadores]: /manage/services/notification-hubs/notify-users-aspnet
[Utilizar Notification Hubs para enviar notícias de última hora]: /manage/services/notification-hubs/breaking-news-dotnet

[Local and Push Notification Programming Guide]:https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/HandlingRemoteNotifications.html#//apple_ref/doc/uid/TP40008194-CH6-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs
[Portal do Azure]: https://portal.azure.com
