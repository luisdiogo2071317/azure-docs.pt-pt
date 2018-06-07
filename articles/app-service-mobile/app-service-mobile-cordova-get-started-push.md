---
title: Adicionar notificações push para uma aplicação Apache Cordova com a funcionalidade Mobile Apps do App Service do Azure | Microsoft Docs
description: Saiba como utilizar as Mobile Apps para enviar notificações push à aplicação Apache Cordova.
services: app-service\mobile
documentationcenter: javascript
manager: crdun
editor: ''
author: conceptdev
ms.assetid: 92c596a9-875c-4840-b0e1-69198817576f
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: crdun
ms.openlocfilehash: 13c1a53cfa3f998c9e3fa3ee1ee2dcec37357095
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34597999"
---
# <a name="add-push-notifications-to-your-apache-cordova-app"></a>Adicionar notificações push à aplicação Apache Cordova
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Descrição geral
Neste tutorial, adicionar notificações push para o [início rápido do Apache Cordova] [ 5] projeto para que uma notificação push é enviada para o dispositivo sempre que é inserido um registo.

Se não utilizar o projeto de servidor de início rápido transferido, terá do pacote de extensão de notificação push. Para obter mais informações, consulte [trabalhar com o servidor de back-end .NET SDK para Mobile Apps][1].

## <a name="prerequisites"></a>Pré-requisitos
Este tutorial parte do princípio de que tem uma aplicação Apache Cordova que foi desenvolvida com o Visual Studio 2015. Este dispositivo deve ser executada no emulador do Google Android, um dispositivo Android, um dispositivo Windows ou um dispositivo iOS.

Para concluir este tutorial, precisa de:

* Um PC com [Visual Studio Community 2015] [ 2] ou posterior
* [Visual Studio Tools para Apache Cordova][4]
* Um [conta ativa do Azure][3]
* Uma conclusão [início rápido do Apache Cordova] [ 5] projeto
* (Android) A [conta Google] [ 6] com um endereço de correio eletrónico verificado
* (iOS) Um [filiação do programa de programador Apple] [ 7] e um dispositivo iOS (iOS simulador não suporta notificações push)
* (Windows) A [conta de programador do Microsoft Store] [ 8] e um dispositivo Windows 10

## <a name="configure-hub"></a>Configurar um hub de notificação
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Ver um vídeo que mostra os passos nesta secção][9].

## <a name="update-the-server-project"></a>Atualizar o projeto de servidor
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="add-push-to-app"></a>Modificar a aplicação Cordova
Para garantir que o seu projeto de aplicação Apache Cordova está pronto para processar as notificações push, instale o plug-in do Cordova push plus quaisquer serviços push específicos da plataforma.

#### <a name="update-the-cordova-version-in-your-project"></a>Atualize a versão de Cordova no seu projeto.
Se o seu projeto utiliza uma versão do Apache Cordova que seja anterior à versão 6.1.1, atualize o projeto de cliente. Para atualizar o projeto, siga os passos seguintes: 

* Para abrir o estruturador de configuração, clique com botão direito `config.xml`.
* Selecione o **plataformas** separador.
* No **Cordova CLI** caixa de texto, selecione **6.1.1**. 
* Para atualizar o projeto, selecione **criar**e, em seguida, selecione **compilar solução**.

#### <a name="install-the-push-plugin"></a>Instalar o plug-in de push
Aplicações do Apache Cordova não nativamente lidar com capacidades de rede ou de dispositivo.  Estas funcionalidades são fornecidas pelo plug-ins que são publicadas um em [npm] [ 10] ou no GitHub. O `phonegap-plugin-push` Plug-in processa as notificações de push de rede.

Pode instalar o plug-in de push de uma das seguintes formas:

**A linha de comandos:**

Execute o seguinte comando:

    cordova plugin add phonegap-plugin-push

**Do Visual Studio:**

1. No Explorador de soluções, abra o `config.xml` ficheiro. Em seguida, selecione **plug-ins** > **personalizada**. Em seguida, selecione **Git** como origem de instalação. 
    
2. Introduza `https://github.com/phonegap/phonegap-plugin-push` como origem.

    ![Abra o ficheiro de config.xml no Explorador de soluções][img1]

3. Selecione a seta junto a origem de instalação.

4. No **SENDER_ID**, se já tiver um ID de projeto numérica para o projeto de consola de programador da Google, pode adicioná-lo aqui. Caso contrário, introduza um valor de marcador de posição, como 777777. Se estiver a segmentar Android, pode atualizar este valor no ficheiro config.xml mais tarde.

    >[!NOTE]
    >A partir da versão 2.0.0, google services.json tem de ser instalado na pasta raiz do seu projeto para configurar o ID do remetente. Para obter mais informações, consulte o [documentação de instalação.](https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md)
5. Selecione **Adicionar**.

O plug-in de push está agora instalado.

#### <a name="install-the-device-plugin"></a>Instalar o plug-in do dispositivo
Siga o mesmo procedimento utilizado para instalar o plug-in de push. Adicione o plug-in do dispositivo na lista de plug-ins Core. (Para encontrá-lo, selecione **plug-ins** > **Core**.) É necessário este plug-in para obter o nome da plataforma.

#### <a name="register-your-device-when-the-application-starts"></a>Registar o seu dispositivo quando iniciar a aplicação 
Inicialmente, incluímos alguns código mínimas para Android. Mais tarde, pode modificar a aplicação para ser executada no iOS ou Windows 10.

1. Adicione uma chamada para **registerForPushNotifications** durante a chamada de retorno para o processo de início de sessão. Em alternativa, pode adicioná-lo na parte inferior do **onDeviceReady** método:

        // Log in to the service.
        client.login('google')
            .then(function () {
                // Create a table reference.
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems.
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item.
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added to register for push notifications.
                registerForPushNotifications();

            }, handleError);

    Este exemplo mostra chamar **registerForPushNotifications** após a autenticação é concluída com êxito. Pode chamar `registerForPushNotifications()` as vezes que é necessário.

2. Adicione os novos **registerForPushNotifications** método da seguinte forma:

        // Register for push notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle the registration event.
        pushRegistration.on('registration', function (data) {
          // Get the native platform of the device.
          var platform = device.platform;
          // Get the handle returned during registration.
          var handle = data.registrationId;
          // Set the device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }
3. (Android) No código anterior, substitua `Your_Project_ID` com a numérica projeto ID para a sua aplicação do [consola de programador da Google][18].

## <a name="optional-configure-and-run-the-app-on-android"></a>(Opcional) Configurar e executar a aplicação no Android
Conclua esta secção para ativar as notificações push para Android.

#### <a name="enable-gcm"></a>Ativar o Firebase Cloud Messaging
Uma vez que está a filtrar a plataforma Google Android inicialmente, tem de ativar Firebase Cloud Messaging.

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

#### <a name="configure-backend"></a>Configurar o aplicação móvel de back-end para enviar pedidos de push utilizando FCM
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

#### <a name="configure-your-cordova-app-for-android"></a>Configure a sua aplicação Cordova para Android
Na aplicação Cordova, abra config.xml. Em seguida, substitua `Your_Project_ID` com a numérica projeto ID para a sua aplicação do [consola de programador da Google][18].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

Abra index.js. Em seguida, atualize o código para utilizar o ID numérico do projeto.

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

#### <a name="configure-device"></a>Configurar o seu dispositivo Android para a depuração USB
Antes de poder implementar a aplicação para o seu dispositivo Android, terá de ativar a depuração de USB. Siga os passos seguintes no seu telemóvel Android:

1. Aceda a **definições** > **acerca do telemóvel**. Em seguida, toque no **número de compilação** até que o modo de programador está ativado (cerca de sete vezes).
2. Novamente no **definições** > **opções de programador**, ativar **depuração USB**. Em seguida, ligar o seu telemóvel Android ao seu PC de desenvolvimento com um cabo USB.

Testamos isto através de um dispositivo de X Google 1000v 5 com o Android 6.0 (Marshmallow). No entanto, as técnicas são comuns a qualquer versão Android moderna.

#### <a name="install-google-play-services"></a>Instalar serviços do Google Play
O plug-in de push baseia-se no Android serviços Google Play para notificações push.

1. No Visual Studio, selecione **ferramentas** > **Android** > **Gestor do Android SDK**. Em seguida, expanda o **Extras** pasta. Marque as caixas adequadas para se certificar de que cada um dos SDKs seguintes está instalada:

   * Android 2.3 ou superior
   * Repositório Google revisão 27 ou superior
   * Serviços do Google Play 9.0.2 ou superior

2. Selecione **instalar pacotes**. Em seguida, aguarde a conclusão da instalação.

As bibliotecas necessárias atuais constam o [documentação de instalação push de plug-in de phonegap][19].

#### <a name="test-push-notifications-in-the-app-on-android"></a>Notificações de push de teste na aplicação no Android
Notificações de push de teste agora, pode executar a aplicação e a inserção itens na tabela TodoItem. Pode testar a partir do mesmo dispositivo ou de um segundo dispositivo, enquanto estiver a utilizar o mesmo back-end. Teste a aplicação Cordova na plataforma Android de uma das seguintes formas:

* *Num dispositivo físico:* anexar o seu dispositivo Android ao seu computador de desenvolvimento com um cabo USB.  Em vez de **emulador do Google Android**, selecione **dispositivo**. Visual Studio implementa a aplicação no dispositivo e executa a aplicação. Em seguida, pode interagir com a aplicação no dispositivo.

  Ecrã de partilha de aplicações, tais como [Mobizen] [ 20] podem ajudá-lo a desenvolver aplicações Android. Mobizen projetos Android ecrã para um web browser no seu PC.

* *No emulador do Android:* existem passos de configuração adicionais que são necessários quando estiver a utilizar um emulador.

    Certifique-se que estiver a implementar um dispositivo virtual que tenha as APIs da Google definida como destino, conforme mostrado no Gestor de dispositivo Virtual Android (AVD).

    ![Gestor de dispositivo Virtual Android](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Se pretender utilizar um x86 mais rápida do emulador, [instalar o controlador HAXM][11]e, em seguida, configure o emulador utilizá-la.

    Adicionar uma conta do Google no dispositivo Android, selecionando **aplicações** > **definições** > **adicionar conta**. Em seguida, siga as instruções.

    ![Adicionar uma conta do Google no dispositivo Android](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Execute a aplicação de todolist como antes e inserir um novo item de todo. Este tempo, um ícone de notificação é apresentado na área de notificação. Pode abrir a gaveta de notificação para ver o texto completo da notificação.

    ![Vista de notificação](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

## <a name="optional-configure-and-run-on-ios"></a>(Opcional) Configurar e executar em dispositivos iOS
Esta secção destina-se à execução do projeto Cordova em dispositivos iOS. Se não estão a trabalhar com dispositivos iOS, pode ignorar esta secção.

#### <a name="install-and-run-the-ios-remote-build-agent-on-a-mac-or-cloud-service"></a>Instalar e executar o agente de compilação remoto iOS num serviço de nuvem ou Mac
Antes de poder executar uma aplicação Cordova em dispositivos iOS com o Visual Studio, percorrer os passos a [guia de configuração de iOS] [ 12] para instalar e executar o agente de compilação remoto.

Certifique-se de que pode criar a aplicação para iOS. Os passos no guia de configuração são necessários para criar a aplicação para iOS a partir do Visual Studio. Se não tiver um Mac, pode criar para iOS, utilizando o agente de compilação remoto num serviço como MacInCloud. Para obter mais informações, consulte [executar a aplicação do iOS na nuvem][21].

> [!NOTE]
> XCode 7 ou superior, é necessário para utilizar o plug-in de push no iOS.

#### <a name="find-the-id-to-use-as-your-app-id"></a>Localizar o ID para utilizar como o seu ID de aplicação
Antes de registar a sua aplicação para notificações push, config.xml abertos na aplicação Cordova, localizar o `id` valor do elemento widget de atributos e, em seguida, copie-a para utilização posterior. No XML seguinte, o ID é `io.cordova.myapp7777777`.

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
          version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

Mais tarde, utilize este identificador quando criar um ID de aplicação no portal de programador da Apple. Se criar um ID de aplicação diferente no portal de programador, tem de colocar alguns passos adicionais mais tarde no tutorial. O ID do elemento widget tem de corresponder ao ID de aplicação no portal de programador.

#### <a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Registar a aplicação para notificações push no portal de programador da Apple
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Veja um vídeo que mostra os passos semelhantes](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

#### <a name="configure-azure-to-send-push-notifications"></a>Configurar o Azure para enviar notificações push
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

#### <a name="verify-that-your-app-id-matches-your-cordova-app"></a>Certifique-se de que o ID da aplicação corresponde à sua aplicação Cordova
Se o ID da aplicação que criou na sua conta de programador Apple já corresponde ao ID do elemento widget no ficheiro config.xml, pode ignorar este passo. No entanto, se os IDs não corresponderem, siga os passos seguintes:

1. Elimine a pasta de plataformas do seu projeto.
2. Elimine a pasta de plug-ins do seu projeto.
3. Elimine a pasta de node_modules do seu projeto.
4. Atualize o atributo de id do elemento widget no ficheiro config.xml para utilizar o ID da aplicação que criou na sua conta de programador da Apple.
5. Reconstrua o projeto.

##### <a name="test-push-notifications-in-your-ios-app"></a>Notificações de push de teste na sua aplicação iOS
1. No Visual Studio, certifique-se de que **iOS** é selecionado como o destino da implementação. Em seguida, selecione **dispositivo** para executar as notificações push no seu dispositivo iOS ligado.

    Pode executar as notificações push num dispositivo iOS que está ligado ao seu PC com o iTunes. O simulador iOS não suporta notificações push.

2. Selecione o **executar** botão ou **F5** no Visual Studio para compilar o projeto e iniciar a aplicação no dispositivo iOS. Em seguida, selecione **OK** para aceitar as notificações push.

   > [!NOTE]
   > A aplicação pede a confirmação para notificações push durante a primeira execução.

3. Na aplicação, digite uma tarefa e, em seguida, selecione o sinal de adição **(+)** ícone.
4. Certifique-se de que foi recebida uma notificação. Em seguida, selecione **OK** a dispensa da notificação.

## <a name="optional-configure-and-run-on-windows"></a>(Opcional) Configurar e executar no Windows
Esta secção descreve como executar o projeto de aplicação Apache Cordova em dispositivos Windows 10 (o plug-in do PhoneGap push é suportado no Windows 10). Se não estiver a trabalhar com dispositivos Windows, pode ignorar esta secção.

#### <a name="register-your-windows-app-for-push-notifications-with-wns"></a>Registar a aplicação do Windows para notificações push no WNS
Para utilizar as opções de arquivo no Visual Studio, selecione um destino de Windows na lista de plataformas de solução, tais como **Windows x64** ou **Windows x86**. (Evitar **Windows AnyCPU** para as notificações push.)

[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Ver um vídeo que mostra os passos semelhantes][13]

#### <a name="configure-the-notification-hub-for-wns"></a>Configurar o notification hub para WNS
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>Configure a sua aplicação Cordova para suportar notificações de push do Windows
Abra o estruturador de configuração clicando **config.xml**. Em seguida, selecione **estruturador de vistas**. Em seguida, selecione o **Windows** separador e, em seguida, selecione **Windows 10** em **versão de destino do Windows**.

Para suportar notificações push na sua compilações predefinido (debug), abra o ficheiro build.json. Em seguida, copie a configuração de "release" para a configuração de depuração.

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

Após a atualização, o ficheiro build.json deve conter o seguinte código:

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

Criar a aplicação e certifique-se de que tem sem erros. A aplicação cliente deve agora registar-se para as notificações de back-end das Mobile Apps. Repita esta secção para cada projeto Windows na sua solução.

#### <a name="test-push-notifications-in-your-windows-app"></a>Notificações de push de teste na sua aplicação do Windows
No Visual Studio, certifique-se de que uma plataforma do Windows se encontra selecionada como o destino da implementação, tais como **Windows x64** ou **Windows x86**. Para executar a aplicação em PCs Windows 10 que está a alojar o Visual Studio, escolha **máquina Local**.

1. Selecione o **executar** botão para compilar o projeto e iniciar a aplicação.

2. Na aplicação, escreva um nome para um todoitem novo e, em seguida, selecione o sinal de adição **(+)** ícone para adicioná-lo.

Certifique-se de que é recebida uma notificação quando o item foi adicionado.

## <a name="next-steps"></a>Passos seguintes
* Leia sobre [Notification Hubs] [ 17] para saber mais sobre as notificações push.
* Se ainda não o tiver feito, continuar o tutorial por [adicionar autenticação] [ 14] à aplicação Apache Cordova.

Saiba como utilizar os SDKs do seguintes:

* [SDK do Apache Cordova][15]
* [SDK do servidor ASP.NET][1]
* [SDK do servidor node.js][16]

<!-- Images -->
[img1]: ./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png

<!-- URLs -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: http://www.visualstudio.com/
[3]: https://azure.microsoft.com/pricing/free-trial/
[4]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[5]: app-service-mobile-cordova-get-started.md
[6]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[7]: https://developer.apple.com/programs/
[8]: https://developer.microsoft.com/en-us/store/register
[9]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub
[10]: https://www.npmjs.com/
[11]: https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM
[12]: http://taco.visualstudio.com/en-us/docs/ios-guide/
[13]: https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push
[14]: app-service-mobile-cordova-get-started-users.md
[15]: app-service-mobile-cordova-how-to-use-client-library.md
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[17]: ../notification-hubs/notification-hubs-push-notification-overview.md
[18]: https://console.developers.google.com/home/dashboard
[19]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[20]: https://www.mobizen.com/
[21]: http://taco.visualstudio.com/en-us/docs/build_ios_cloud/
