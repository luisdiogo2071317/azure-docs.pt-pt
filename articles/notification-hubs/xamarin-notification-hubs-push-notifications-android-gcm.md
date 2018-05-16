---
title: Enviar notificações push para aplicações Xamarin.Android com Hubs de Notificação do Azure | Microsoft Docs
description: Neste tutorial, irá aprender a utilizar os Notification Hubs do Azure para enviar notificações push para uma aplicação Xamarin Android.
author: dimazaid
manager: kpiteira
editor: spelluru
services: notification-hubs
documentationcenter: xamarin
ms.assetid: 0be600fe-d5f3-43a5-9e5e-3135c9743e54
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: f75671e2e5511054f3db550a8c24e62d031492c3
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-push-notifications-to-xamarinandroid-apps-using-azure-notification-hubs"></a>Tutorial: Enviar notificações push para aplicações Xamarin.Android com Hubs de Notificação do Azure
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Descrição geral
Este tutorial mostra como utilizar os Notification Hubs do Azure para enviar notificações push para uma aplicação Xamarin Android. Vai criar uma aplicação Xamarin.Android em branco que recebe notificações push através do Firebase Cloud Messaging (FCM). Pode utilizar o hub de notificação para difundir notificações push para todos os dispositivos a executar a sua aplicação. O código concluído está disponível no exemplo [aplicação NotificationHubs][GitHub].

Neste tutorial, siga os passos seguintes:

> [!div class="checklist"]
> * Criar um projeto do Firebase e ativar o Firebase Cloud Messaging
> * Criar um hub de notificação
> * Criar uma aplicação Xamarin.Android e ligá-la ao hub de notificação
> * Enviar notificações de teste a partir do portal do Azure

## <a name="prerequisites"></a>Pré-requisitos

- **Subscrição do Azure**. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.
- [Visual Studio com Xamarin] no Windows ou [Visual Studio para Mac] no OS X.
- Conta Google ativa

## <a name="create-a-firebase-project-and-enable-firebase-cloud-messaging"></a>Criar um projeto do Firebase e ativar o Firebase Cloud Messaging
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="create-a-notification-hub"></a>Criar um hub de notificação
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-gcm-settings-for-the-notification-hub"></a>Configurar as definições do GCM para o hub de notificação

1. Selecione **Google (GCM)** na secção **DEFINIÇÕES DE NOTIFICAÇÃO**. 
2. Introduza a **Chave do servidor do legado** que anotou a partir da consola Google Firebase. 
3. Selecione **Guardar** na barra de ferramentas. 

    ![](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

O Notification Hub está configurado para trabalhar com FCM e tem as cadeias de ligação para registar a sua aplicação e para receber notificações e enviar notificações push.

## <a name="create-xamainandroid-app-and-connect-it-to-notification-hub"></a>Criar uma aplicação Xamarin.Android e ligá-la ao hub de notificação

### <a name="create-visual-studio-project-and-add-nuget-packages"></a>Criar um projeto do Visual Studio e adicionar pacotes NuGet
1. No Visual Studio, aponte para **Ficheiro**, selecione **Novo** e, em seguida, selecione **Projeto**. 
   
      ![Visual Studio - Criar novo projeto Android](./media/partner-xamarin-notification-hubs-android-get-started/new-project-dialog.png)
2. Na janela **Explorador de Soluções**, expanda **Propriedades** e clique em **AndroidManifest.xml**. Atualize o nome do pacote para corresponder ao nome do pacote que introduziu ao adicionar o Firebase Cloud Messaging ao seu projeto na consola Google Firebase. 

      ![Nome do pacote no GCM](./media/partner-xamarin-notification-hubs-android-get-started/package-name-gcm.png)
3. Clique com o botão direito do rato no projeto e selecione **Gerir Pacotes NuGet...**. 
4. Selecione o separador **Procurar**. Procure **Xamarin.GooglePlayServices.Base**. Selecione **Xamarin.GooglePlayServices.Base** na lista de resultados. Em seguida, selecione **Instalar**. 

      ![NuGet dos Serviços do Google Play](./media/partner-xamarin-notification-hubs-android-get-started/google-play-services-nuget.png)
5. Na janela **Gestor de Pacotes NuGet**, procure **Xamarin.Firebase.Messaging**. Selecione **Xamarin.Firebase.Messaging** na lista de resultados. Em seguida, selecione **Instalar**. 
6. Agora, procure **Xamarin.Azure.NotificationHubs.Android**. Selecione **Xamarin.Azure.NotificationHubs.Android** na lista de resultados. Em seguida, selecione **Instalar**. 

### <a name="add-the-google-services-json-file"></a>Adicionar o Ficheiro JSON dos Serviços Google

1. Copie **google-services.json** que transferiu a partir da consola Google Firebase para a pasta do projeto.
2. Adicione **google-services.json** ao projeto.
3. Selecione **google-services.json** na janela **Explorador de Soluções**.
4. No painel **Propriedades**, defina a Ação de Compilação como **GoogleServicesJson**. Se não vir **GoogleServicesJson**, feche o Visual Studio, reinicie-o, reabra o projeto e repita. 

      ![Ação de compilação GoogleServicesJson](./media/partner-xamarin-notification-hubs-android-get-started/google-services-json-build-action.png)

### <a name="set-up-notification-hubs-in-your-project"></a>Configurar Notification Hubs no seu projeto

#### <a name="registering-with-firebase-cloud-messaging"></a>Registar no Firebase Cloud Messaging

Abra o ficheiro **AndroidManifest.xml** e insira os seguintes elementos `<receiver>` no elemento `<application>`:

        <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
        <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
          <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
            <category android:name="${applicationId}" />
          </intent-filter>
        </receiver>

1. Recolha as seguintes informações para a sua aplicação Android e Notification Hub:
   
   * **Escutar a cadeia de ligação**: no dashboard, no [portal do Azure], escolha **Ver cadeias de ligação**. Copie a cadeia de ligação *DefaultListenSharedAccessSignature* para este valor.
   * **Nome do hub**: nome do hub a partir do [portal do Azure]. Por exemplo, *mynotificationhub2*.
     
2. Clique com o botão direito do rato em **projeto** na janela **Explorador de Soluções**, aponte para **Adicionar** e selecione **Classe**. 
4. Crie uma classe **Constants.cs** para o seu projeto Xamarin e defina os seguintes valores constantes na classe. Substitua os marcadores de posição com os seus valores.
    
    ```csharp
        public static class Constants
        {
           public const string ListenConnectionString = "<Listen connection string>";
           public const string NotificationHubName = "<hub name>";
        }
    ```
3. Adicione o seguinte ao utilizar as instruções **MainActivity.cs**:
   
    ```csharp
        using Android.Util;
    ```
4. Adicione uma variável de instância a **MainActivity.cs** que será utilizada para mostrar uma caixa de diálogo de alerta quando a aplicação está em execução:
   
    ```csharp
        public const string TAG = "MainActivity";
    ```
5. Adicione o seguinte código para `OnCreate` a **mainactivity. cs** após `base.OnCreate(savedInstanceState)`:

    ```csharp   
        if (Intent.Extras != null)
        {
            foreach (var key in Intent.Extras.KeySet())
            {
                if(key!=null)
                {
                    var value = Intent.Extras.GetString(key);
                    Log.Debug(TAG, "Key: {0} Value: {1}", key, value);
                }
            }
        }
    ```
7. Crie uma nova classe, **MyFirebaseIIDService** como criou a classe **Constants**. 
8. Adicione o seguinte ao utilizar as instruções **MyFirebaseIIDService.cs**:
   
    ```csharp
    using Android.Util;
    using WindowsAzure.Messaging;
    using Firebase.Iid;
    ```

9. Em **MyFirebaseIIDService.cs**, adicione a seguinte declaração de **classe** e faça com que a classe herde de **FirebaseInstanceIdService**:
   
    ```csharp
        [Service]
        [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
        public class MyFirebaseIIDService : FirebaseInstanceIdService
    ```
10. Em **MyFirebaseIIDService.cs**, adicione o seguinte código:
   
    ```csharp
        const string TAG = "MyFirebaseIIDService";
        NotificationHub hub;

        public override void OnTokenRefresh()
        {
            var refreshedToken = FirebaseInstanceId.Instance.Token;
            Log.Debug(TAG, "FCM token: " + refreshedToken);
            SendRegistrationToServer(refreshedToken);
        }

        void SendRegistrationToServer(string token)
        {
            // Register with Notification Hubs
            hub = new NotificationHub(Constants.NotificationHubName,
                                      Constants.ListenConnectionString, this);

            var tags = new List<string>() { };
            var regID = hub.Register(token, tags.ToArray()).RegistrationId;

            Log.Debug(TAG, $"Successful registration of ID {regID}");
        }
    ```
11. Crie outra classe nova para o projeto e nomeie-o **MyFirebaseMessagingService**.
12. Adicione o seguinte através de instruções a **MyFirebaseMessagingService.cs**.
    
    ```csharp
        using Android.Util;
        using Firebase.Messaging;
    ```
13. Adicione o seguinte acima da declaração de classe e faça com a classe herde a partir de **FirebaseMessagingService**:
    
    ```csharp
        [Service]
        [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
        public class MyFirebaseMessagingService : FirebaseMessagingService
    ```    
14. Adicione o seguinte código a **MyFirebaseMessagingService.cs**:
    
    ```csharp
        const string TAG = "MyFirebaseMsgService";
        public override void OnMessageReceived(RemoteMessage message)
        {
            Log.Debug(TAG, "From: " + message.From);
            if(message.GetNotification()!= null)
            {
                //These is how most messages will be received
                Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
                SendNotification(message.GetNotification().Body);
            }
            else 
            {
                //Only used for debugging payloads sent from the Azure portal
                SendNotification(message.Data.Values.First());

            }

        }

        void SendNotification(string messageBody)
        {
            var intent = new Intent(this, typeof(MainActivity));
            intent.AddFlags(ActivityFlags.ClearTop);
            var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

            var notificationBuilder = new Notification.Builder(this)
                        .SetContentTitle("FCM Message")
                        .SetSmallIcon(Resource.Drawable.ic_launcher)
                        .SetContentText(messageBody)
                        .SetAutoCancel(true)
                        .SetContentIntent(pendingIntent);

            var notificationManager = NotificationManager.FromContext(this);

            notificationManager.Notify(0, notificationBuilder.Build());
        }
    ```
15. **Crie** o projeto. 
16. **Execute** a aplicação no dispositivo ou emulador carregado

## <a name="send-test-notification-from-the-azure-portal"></a>Enviar notificação de teste a partir do portal do Azure
Pode testar a receção das notificações na aplicação com a opção *Envio de Teste* no [portal do Azure]. Esta ação envia uma notificação push de teste para o dispositivo.

![Portal do Azure – Envio de Teste](media/partner-xamarin-notification-hubs-android-get-started/send-test-notification.png)

Normalmente, as notificações push são enviadas num serviço de back-end, como Mobile Services ou ASP.NET, através de uma biblioteca compatível. Caso não esteja disponível uma biblioteca para o back-end, também pode utilizar a API REST diretamente para enviar mensagens de notificação.

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, enviou notificações de difusão para todos os dispositivos Android registados no back-end. Para saber como enviar notificações push para dispositivos Android específicos, avance para o tutorial seguinte: 

> [!div class="nextstepaction"]
>[Enviar notificações push para dispositivos específicos](notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md)


<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png
[24]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-xamarin-android-app-options.png
[25]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-google-services-json.png

[30]: ./media/notification-hubs-android-get-started/notification-hubs-test-send.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js
[Visual Studio com Xamarin]: https://docs.microsoft.com/visualstudio/install/install-visual-studio
[Visual Studio para Mac]: https://www.visualstudio.com/vs/visual-studio-mac/

[portal do Azure]: https://portal.azure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Use Notification Hubs to push notifications to users]: /manage/services/notification-hubs/notify-users-aspnet
[Use Notification Hubs to send breaking news]: /manage/services/notification-hubs/breaking-news-dotnet
[GitHub]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid
