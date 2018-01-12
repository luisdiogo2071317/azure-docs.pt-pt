---
title: "Adicionar notificações push à aplicação xamarin. Forms | Microsoft Docs"
description: "Saiba como utilizar os serviços do Azure para enviar notificações push de várias plataformas para aplicações xamarin. Forms."
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: 
ms.assetid: d9b1ba9a-b3f2-4d12-affc-2ee34311538b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: crdun
ms.openlocfilehash: a9c7c5dbbc50ccf8c5383be28e96dfb82af48559
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2018
---
# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Adicionar notificações push à aplicação xamarin. Forms
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Descrição geral
Neste tutorial, adicionar notificações push para todos os projetos resultaram da [início rápido do xamarin. Forms](app-service-mobile-xamarin-forms-get-started.md). Isto significa que é enviada uma notificação push para todos os clientes de plataforma, sempre que é inserido um registo.

Se utilizar o projeto de servidor de início rápido transferido, terá do pacote de extensão de notificação push. Para obter mais informações, consulte [trabalhar com o servidor de back-end .NET SDK de Mobile Apps do Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Pré-requisitos
Para iOS, precisa de um [filiação do programa de programador Apple](https://developer.apple.com/programs/ios/) e um dispositivo iOS físico. O [simulador iOS não suporta notificações push](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

## <a name="configure-hub"></a>Configurar um hub de notificação
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a>Atualizar o projeto de servidor para enviar notificações push
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-and-run-the-android-project-optional"></a>Configurar e executar o projeto Android (opcional)
Conclua esta secção para ativar as notificações push para o projeto Droid xamarin. Forms para Android.

### <a name="enable-firebase-cloud-messaging-fcm"></a>Ativar a nuvem Firebase Messaging (FCM)
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

### <a name="configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm"></a>Configurar Mobile Apps back-end para enviar pedidos de push utilizando FCM
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

### <a name="add-push-notifications-to-the-android-project"></a>Adicionar notificações push para o projeto de Android
Com o back-end configurado com FCM, pode adicionar componentes e códigos no cliente para registar o FCM. Também pode registar-se para notificações push com Notification Hubs do Azure através de back-end das Mobile Apps e receber notificações.

1. No **Droid** do projeto, clique com botão direito **referências > Gerir pacotes NuGet...** .
1. Na janela do Gestor de pacotes NuGet, procure o **Xamarin.Firebase.Messaging** do pacote e adicione-ao projeto.
1. No properies projeto para a **Droid** do projeto, defina a aplicação para compilar a utilizar a versão Android 7.0 ou superior.
1. Adicionar o **google services.json** transferido a partir da consola Firebase, na raiz do ficheiro a **Droid** projeto e definir a ação de compilação **GoogleServicesJson**. Para obter mais informações, consulte [adicione o ficheiro de JSON de serviços do Google](https://developer.xamarin.com/guides/android/data-and-cloud-services/google-messaging/remote-notifications-with-fcm/#Add_the_Google_Services_JSON_File).

#### <a name="registering-with-firebase-cloud-messaging"></a>Registar o Firebase Cloud Messaging

1. Abra o **AndroidManifest.xml** de ficheiros e insira o seguinte `<receiver>` elementos para o `<application>` elemento:

        <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
        <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
          <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
            <category android:name="${applicationId}" />
          </intent-filter>
        </receiver>

#### <a name="implementing-the-firebase-instance-id-service"></a>Implementar o serviço de ID de instância Firebase

1. Adicionar uma nova classe para o **Droid** projeto com o nome `FirebaseRegistrationService`e certifique-se de que o seguinte `using` instruções estão presentes na parte superior do ficheiro:

        using System.Threading.Tasks;
        using Android.App;
        using Android.Util;
        using Firebase.Iid;
        using Microsoft.WindowsAzure.MobileServices;

1. Substitua o vazio `FirebaseRegistrationService` classe com o seguinte código:

        [Service]
        [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
        public class FirebaseRegistrationService : FirebaseInstanceIdService
        {
            const string TAG = "FirebaseRegistrationService";

            public override void OnTokenRefresh()
            {
                var refreshedToken = FirebaseInstanceId.Instance.Token;
                Log.Debug(TAG, "Refreshed token: " + refreshedToken);
                SendRegistrationTokenToAzureNotificationHub(refreshedToken);
            }

            void SendRegistrationTokenToAzureNotificationHub(string token)
            {
                // Update notification hub registration
                Task.Run(async () =>
                {
                    await AzureNotificationHubService.RegisterAsync(TodoItemManager.DefaultManager.CurrentClient.GetPush(), token);
                });
            }
        }

    O `FirebaseRegistrationService` classe é responsável pela criação de tokens de segurança que autorizar a aplicação aceda FCM. O `OnTokenRefresh` método é invocado quando a aplicação recebe um token de registo do FCM. O método obtém o token do `FirebaseInstanceId.Instance.Token` propriedade, o que é atualizada no modo assíncrono através do FCM. O `OnTokenRefresh` método é invocado com pouca frequência, porque o token só é atualizado quando a aplicação é instalada ou desinstalada, quando o utilizador elimina dados de aplicações, quando a aplicação apague o ID de instância, ou quando a segurança do token tiver sido comprometida. Além disso, o serviço de ID de instância do FCM irá solicitar que a aplicação atualiza o token periodicamente, normalmente, a cada 6 meses.

    O `OnTokenRefresh` método também invoca o `SendRegistrationTokenToAzureNotificationHub` método, o que é utilizado para associar o token de registo do utilizador com o Notification Hub do Azure.

#### <a name="registering-with-the-azure-notification-hub"></a>Registo no Hub de notificação do Azure

1. Adicionar uma nova classe para o **Droid** projeto com o nome `AzureNotificationHubService`e certifique-se de que o seguinte `using` instruções estão presentes na parte superior do ficheiro:

        using System;
        using System.Threading.Tasks;
        using Android.Util;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;

1. Substitua o vazio `AzureNotificationHubService` classe com o seguinte código:

        public class AzureNotificationHubService
        {
            const string TAG = "AzureNotificationHubService";

            public static async Task RegisterAsync(Push push, string token)
            {
                try
                {
                    const string templateBody = "{\"data\":{\"message\":\"$(messageParam)\"}}";
                    JObject templates = new JObject();
                    templates["genericMessage"] = new JObject
                    {
                        {"body", templateBody}
                    };

                    await push.RegisterAsync(token, templates);
                    Log.Info("Push Installation Id: ", push.InstallationId.ToString());
                }
                catch (Exception ex)
                {
                    Log.Error(TAG, "Could not register with Notification Hub: " + ex.Message);
                }
            }
        }

    O `RegisterAsync` método cria um modelo de mensagem de notificação simples como JSON e regista para receber notificações de modelo do notification hub, utilizando o token de registo Firebase. Isto garante que qualquer notificações enviadas pelo Hub de notificação do Azure destina-se o dispositivo representado pelo token de registo.

#### <a name="displaying-the-contents-of-a-push-notification"></a>Apresentar o conteúdo de uma notificação Push

1. Adicionar uma nova classe para o **Droid** projeto com o nome `FirebaseNotificationService`e certifique-se de que o seguinte `using` instruções estão presentes na parte superior do ficheiro:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Util;
        using Firebase.Messaging;

1. Substitua o vazio `FirebaseNotificationService` classe com o seguinte código:

        [Service]
        [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
        public class FirebaseNotificationService : FirebaseMessagingService
        {
            const string TAG = "FirebaseNotificationService";

            public override void OnMessageReceived(RemoteMessage message)
            {
                Log.Debug(TAG, "From: " + message.From);

                // Pull message body out of the template
                var messageBody = message.Data["message"];
                if (string.IsNullOrWhiteSpace(messageBody))
                    return;

                Log.Debug(TAG, "Notification message body: " + messageBody);
                SendNotification(messageBody);
            }

            void SendNotification(string messageBody)
            {
                var intent = new Intent(this, typeof(MainActivity));
                intent.AddFlags(ActivityFlags.ClearTop);
                var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

                var notificationBuilder = new Notification.Builder(this)
                    .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
                    .SetContentTitle("New Todo Item")
                    .SetContentText(messageBody)
                    .SetContentIntent(pendingIntent)
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))
                    .SetAutoCancel(true);

                var notificationManager = NotificationManager.FromContext(this);
                notificationManager.Notify(0, notificationBuilder.Build());
            }
        }

    O `OnMessageReceived` método, o que é invocado quando uma aplicação recebe uma notificação de FCM, extrai a conteúdo de mensagem e chama o `SendNotification` método. Este método converte o conteúdo da mensagem de notificação de local é iniciada enquanto a aplicação está em execução, com a notificação de apresentação na área de notificação.

Agora, está pronto teste notificações de push na aplicação em execução no dispositivo Android ou o emulador.

### <a name="test-push-notifications-in-your-android-app"></a>Notificações de push de teste na sua aplicação Android
Os primeiros dois passos são necessários apenas quando estiver a testar um emulador.

1. Certifique-se de que estão a implementar ou depuração num dispositivo virtual que tenha as APIs da Google definida como destino, conforme mostrado abaixo no Gestor de dispositivo Virtual Android.
2. Adicionar uma conta do Google no dispositivo Android, clicando em **aplicações** > **definições** > **adicionar conta**. Em seguida, siga as instruções para adicionar uma conta do Google existente para o dispositivo ou para criar um novo.
3. No Visual Studio ou no Xamarin Studio, clique com botão direito do **Droid** projeto e clique em **definir como projeto de arranque**.
4. Clique em **executar** para compilar o projeto e iniciar a aplicação no seu dispositivo Android ou emulador.
5. Na aplicação, digite uma tarefa e, em seguida, clique o sinal de adição (**+**) ícone.
6. Certifique-se de que é recebida uma notificação quando é adicionado um item.

## <a name="configure-and-run-the-ios-project-optional"></a>Configurar e executar o projeto iOS (opcional)
Esta secção destina-se à execução do projeto iOS Xamarin para dispositivos iOS. Pode ignorar esta secção se não estiver a trabalhar com dispositivos iOS.

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

#### <a name="configure-the-notification-hub-for-apns"></a>Configurar o notification hub para APNS
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Em seguida, irá configurar a definição de projeto do iOS no Xamarin Studio ou Visual Studio.

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

#### <a name="add-push-notifications-to-your-ios-app"></a>Adicionar notificações push à aplicação iOS
1. No **iOS** do projeto, abra appdelegate. cs e adicione a seguinte instrução na parte superior do ficheiro de código.

        using Newtonsoft.Json.Linq;
2. No **AppDelegate** classe, adicione uma substituição para o **RegisteredForRemoteNotifications** eventos para se registar para notificações:

        public override void RegisteredForRemoteNotifications(UIApplication application,
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }
3. No **AppDelegate**, também adicione a seguinte substituição para o **DidReceiveRemoteNotification** processador de eventos:

        public override void DidReceiveRemoteNotification(UIApplication application,
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    Este método processa as notificações recebidas enquanto a aplicação está em execução.
4. No **AppDelegate** classe, adicione o seguinte código para o **FinishedLaunching** método:

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    Isto permite que o suporte para notificações remotas e pedidos de registo de push.

A aplicação agora é atualizada para suportar notificações push.

#### <a name="test-push-notifications-in-your-ios-app"></a>Notificações de push de teste na sua aplicação iOS
1. Clique com o botão direito no projeto iOS e clique em **definir como projeto de arranque**.
2. Prima a **executar** botão ou **F5** no Visual Studio para compilar o projeto e iniciar a aplicação no dispositivo iOS. Em seguida, clique em **OK** para aceitar as notificações push.

   > [!NOTE]
   > Tem de aceitar explicitamente notificações push da sua aplicação. Este pedido só ocorre na primeira vez que a aplicação é executada.
   >
   >
3. Na aplicação, digite uma tarefa e, em seguida, clique o sinal de adição (**+**) ícone.
4. Certifique-se de que uma notificação é recebida e, em seguida, clique em **OK** a dispensa da notificação.

## <a name="configure-and-run-windows-projects-optional"></a>Configurar e executar o projeto Windows (opcional)
Esta secção destina-se a executar a WinApp xamarin. Forms e projetos de WinPhone81 para dispositivos Windows. Estes passos também suportam projetos da plataforma Universal do Windows (UWP). Pode ignorar esta secção se não estiver a trabalhar com dispositivos Windows.

#### <a name="register-your-windows-app-for-push-notifications-with-windows-notification-service-wns"></a>Registar a aplicação do Windows para notificações push com o serviço de notificação de Windows (WNS)
[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

#### <a name="configure-the-notification-hub-for-wns"></a>Configurar o notification hub para WNS
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="add-push-notifications-to-your-windows-app"></a>Adicionar notificações push à aplicação do Windows
1. No Visual Studio, abra **App.xaml.cs** em janelas de projeto e adicione as seguintes declarações.

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Substitua `<your_TodoItemManager_portable_class_namespace>` com o espaço de nomes do seu projeto portátil que contém o `TodoItemManager` classe.
2. No App.xaml.cs, adicione a seguinte **InitNotificationsAsync** método:

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS =
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    Este método obtém o canal de notificação push e regista um modelo para receber notificações de modelo do seu hub de notificação. Uma notificação de modelo que suporta *messageParam* irá ser entregue ao cliente.
3. No App.xaml.cs, atualize o **OnLaunched** definição de método do processador de eventos, adicionando o `async` modificador. Em seguida, adicione a seguinte linha de código no final do método:

        await InitNotificationsAsync();

    Isto garante que o registo da notificação push é criado ou atualizado sempre que a aplicação é iniciada. É importante fazê-lo para garantir que o canal de push do WNS sempre está ativo.  
4. No Explorador de soluções para o Visual Studio, abra o **Package. appxmanifest** de ficheiros e defina **com capacidade de alerta** para **Sim** em **notificações**.
5. Criar a aplicação e certifique-se de que tem sem erros. A aplicação cliente deve agora registar-se para as notificações de modelo de back-end das Mobile Apps. Repita esta secção para cada projeto Windows na sua solução.

#### <a name="test-push-notifications-in-your-windows-app"></a>Notificações de push de teste na sua aplicação do Windows
1. No Visual Studio, clique com o botão direito um projeto do Windows e clique em **definir como projeto de arranque**.
2. Prima o botão **Executar** para compilar o projeto e iniciar a aplicação.
3. Na aplicação, escreva um nome para um todoitem novo e, em seguida, clique no sinal de adição (**+**) ícone para adicioná-lo.
4. Certifique-se de que é recebida uma notificação quando o item foi adicionado.

## <a name="next-steps"></a>Passos Seguintes
Pode saber mais sobre as notificações push:

* [Enviar notificações Push a partir de aplicações móveis do Azure](https://developer.xamarin.com/guides/xamarin-forms/cloud-services/push-notifications/azure/)
* [Firebase Cloud Messaging](https://developer.xamarin.com/guides/android/data-and-cloud-services/google-messaging/firebase-cloud-messaging/)
* [Notificações remotas com o Firebase Cloud Messaging](https://developer.xamarin.com/guides/android/data-and-cloud-services/google-messaging/remote-notifications-with-fcm/)
* [Diagnosticar problemas de notificação push](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  Existem vários motivos por que razão as notificações podem obter removidas ou não terminar em dispositivos. Este tópico mostra como analisar e descobrir a causa de raiz de falhas de notificação push.

Também pode continuar a sessão de um dos seguintes tutoriais:

* [Adicionar autenticação à aplicação ](app-service-mobile-xamarin-forms-get-started-users.md)  
  Saiba como autenticar os utilizadores da aplicação com um fornecedor de identidade.
* [Permitir sincronização offline para a sua aplicação](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Saiba como adicionar suporte offline à aplicação com um back-end de Aplicações Móveis. Com a sincronização offline, os utilizadores poderão interagir com uma aplicação móvel&mdash;visualizar, adicionar ou modificar dados&mdash;, mesmo quando não existe nenhuma ligação de rede.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
