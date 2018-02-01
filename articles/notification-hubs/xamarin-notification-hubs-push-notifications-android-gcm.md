---
title: "Introdução aos Hubs de Notificação para aplicações Xamarin Android | Microsoft Docs"
description: "Neste tutorial, irá aprender a utilizar os Notification Hubs do Azure para enviar notificações push para uma aplicação Xamarin Android."
author: jwhitedev
manager: kpiteira
editor: 
services: notification-hubs
documentationcenter: xamarin
ms.assetid: 0be600fe-d5f3-43a5-9e5e-3135c9743e54
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: 1cb6fbc82c493e17815dc60ddcff183a47513bc6
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="get-started-with-notification-hubs-for-xamarinandroid-apps"></a>Introdução aos Hubs de Notificação para aplicações Xamarin Android
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Descrição geral
Este tutorial mostra como utilizar os Notification Hubs do Azure para enviar notificações push para uma aplicação Xamarin Android. Irá criar uma aplicação Xamarin.Android em branco que recebe notificações push através do Firebase Cloud Messaging (FCM). Quando tiver terminado, poderá utilizar o Notification Hub para difundir notificações push para todos os dispositivos a executar a sua aplicação. O código concluído está disponível no exemplo [aplicação NotificationHubs][GitHub].

Este tutorial demonstra o cenário de difusão simples utilizando Notification Hubs.

## <a name="before-you-begin"></a>Antes de começar
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

O código de conclusão para este tutorial pode ser encontrado [aqui][GitHub] no GitHub.

## <a name="prerequisites"></a>Pré-requisitos
Para este tutorial, necessita do seguinte:

* [Visual Studio com Xamarin] no Windows ou [Visual Studio para Mac] no OS X.
* Conta Google ativa

A conclusão deste tutorial é um pré-requisito para todos os outros tutoriais de Notification Hubs para aplicações Xamarin Android.

> [!IMPORTANT]
> Para concluir este tutorial, tem de ter uma conta ativa do Azure. Se não tiver uma conta, pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter mais detalhes, consulte [Avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).
> 
> 

## <a name="enable-firebase-cloud-messaging"></a>Ativar Firebase Cloud Messaging
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-your-notification-hub"></a>Configurar o Notification Hub
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li><p>Escolha o separador <b>Configurar</b> na parte superior, introduza o valor da <b>Chave de API</b> obtido na secção anterior e, em seguida, selecione <b>Guardar</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

O Notification Hub está configurado para trabalhar com FCM e tem as cadeias de ligação para registar a sua aplicação e para receber notificações e enviar notificações push.

## <a name="connect-your-app-to-the-notification-hub"></a>Ligar a aplicação ao Notification Hub
Em primeiro lugar, crie um novo projeto. 

1. No Visual Studio, escolha **Nova Solução** > **Aplicação Android** e, em seguida, selecione **Seguinte**.
   
      ![Visual Studio - Criar novo projeto Android][22]

2. Introduza o seu **Nome da Aplicação** e **Identificador**. Escolha as **Plataformas de Destino** que pretende suportar e, em seguida, escolha **Seguinte** e **Criar**.
   
      ![Visual Studio - Configuração de aplicação Android][23]

    Este processo cria um novo projeto Android.

3. Abra as propriedades do projeto clicando com o botão direito no seu novo projeto na vista Solução e selecione **Opções**. Selecione o item **Aplicação Android** na secção **Compilar**.
   
    Certifique-se de que a primeira letra do seu **Nome do pacote** está em minúscula.
   
   > [!IMPORTANT]
   > A primeira letra do nome do pacote tem de estar em minúscula. Caso contrário, irá receber erros de manifesto da aplicação quando registar a aplicação nas notificações push abaixo.
   > 
   > 
   
      ![Visual Studio - Opções do projeto Android][24]
4. Opcionalmente, defina a **Versão mínima do Android** para outro nível da API.
5. Opcionalmente, defina a **Versão de destino Android** para outra versão da API que pretende utilizar como destino (tem de ser um API de nível 8 ou posterior).
6. Escolha **OK** e feche a caixa de diálogo Opções do Projeto.

### <a name="add-the-required-packages-to-your-project"></a>Adicione os pacotes necessários ao projeto

1. Clique com o botão direito do rato no projeto e selecione **Adicionar** > **Adicionar Pacotes Nuget**
2. Procure **Xamarin.Azure.NotificationHubs.Android** e adicione-ao projeto.
3. Procure **Xamarin.Firebase.Messaging** e adicione-ao projeto.

### <a name="set-up-notification-hubs-in-your-project"></a>Configurar Notification Hubs no seu projeto
1. Recolha as seguintes informações para a sua aplicação Android e Notification Hub:
   
   * **Escutar a cadeia de ligação**: no dashboard, no [portal do Azure], escolha **Ver cadeias de ligação**. Copie a cadeia de ligação *DefaultListenSharedAccessSignature* para este valor.
   * **Nome do hub**: este é o nome do seu hub a partir do [portal do Azure]. Por exemplo, *mynotificationhub2*.
     
2. Crie uma classe **Constants.cs** para o seu projeto Xamarin e defina os seguintes valores constantes na classe. Substitua os marcadores de posição com os seus valores.
    
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

6. Clique no projeto e adicione o `google-services.json` que transferiu a partir do projeto Firebase anteriormente. Faça duplo clique no ficheiro adicionado e defina a ação de compilação para `GoogleServicesJson`

    ![Visual Studio - Configurar google-services.json][25]

7. Criar uma nova classe, **MyFirebaseIIDService**.

8. Adicione o seguinte ao utilizar as instruções **MyFirebaseIIDService.cs**:
   
    ```csharp
        using System;
        using Android.App;
        using Firebase.Iid;
        using Android.Util;
        using WindowsAzure.Messaging;
        using System.Collections.Generic;
    ```

9. Em **MyFirebaseIIDService.cs**, adicione o seguinte acima da declaração de **classe**, e faça com que a classe de herde de **FirebaseInstanceIdService**:
   
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
        using System;
        using System.Linq;
        using Android;
        using Android.App;
        using Android.Content;
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

15. Execute a aplicação no dispositivo ou emulador carregado

## <a name="send-notifications-from-the-portal"></a>Enviar notificações a partir do portal
Pode testar a receção das notificações na aplicação com a opção *Envio de Teste* no [portal do Azure]. Esta ação envia uma notificação push de teste para o dispositivo.

![Portal do Azure – Envio de Teste][30]

Normalmente, as notificações push são enviadas num serviço de back-end, como Mobile Services ou ASP.NET, através de uma biblioteca compatível. Caso não esteja disponível uma biblioteca para o back-end, também pode utilizar a API REST diretamente para enviar mensagens de notificação.

Aqui está uma lista de outros tutoriais que pode querer rever sobre o envio de notificações:

* ASP.NET: Consulte [Utilizar Notification Hubs para notificações push a utilizadores].
* SDK Java dos Hubs de Notificação do Azure: veja [Como utilizar Hubs de Notificação de Java](notification-hubs-java-push-notification-tutorial.md) para enviar notificações de Java. Isto foi testado em Eclipse para Desenvolvimento do Android.
* PHP: veja [How to use Notification Hubs from PHP (Como utilizar Hubs de Notificação de PHP)](notification-hubs-php-push-notification-tutorial.md).

## <a name="next-steps"></a>Passos seguintes
Neste exemplo simples, difundiu notificações para todos os seus dispositivos Android. Para abordar utilizadores específicos, consulte o tutorial [Utilizar Notification Hubs para notificações push a utilizadores]. Se pretender segmentar os utilizadores por grupos de interesse, pode ler [Utilizar Notification Hubs para enviar notícias de última hora]. Saiba mais sobre como utilizar Notification Hubs em [Documentação de Orientação dos Notification Hubs] e em [Procedimentos de Notification Hubs para Android].

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
[Documentação de Orientação dos Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Procedimentos de Notification Hubs para Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Utilizar Notification Hubs para notificações push a utilizadores]: /manage/services/notification-hubs/notify-users-aspnet
[Utilizar Notification Hubs para enviar notícias de última hora]: /manage/services/notification-hubs/breaking-news-dotnet
[GitHub]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid
