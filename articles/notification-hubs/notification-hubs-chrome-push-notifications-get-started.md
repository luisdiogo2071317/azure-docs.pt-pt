---
title: Enviar notificações push para aplicações do Chrome com Hubs de Notificação do Azure | Microsoft Docs
description: Aprenda a utilizar os Notification Hubs do Azure para enviar notificações push para uma Aplicação do Chrome.
services: notification-hubs
keywords: notificações push móveis, notificações push, notificação push, notificações push do chrome
documentationcenter: ''
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 75d4ff59-d04a-455f-bd44-0130a68e641f
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-chrome
ms.devlang: JavaScript
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 248fd094a8655af2a21035267a6b8f69f268683d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51262170"
---
# <a name="tutorial-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Tutorial: Enviar notificações push para aplicações do Chrome com Hubs de Notificação do Azure
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Este tutorial explica-lhe como criar um hub de notificação e enviar notificações push para uma aplicação do Google Chrome de exemplo através do [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/). A aplicação do Chrome é executada no contexto de um browser Google Chrome e registada no hub de notificação. 

> [!NOTE]
> As notificações push da aplicação do Chrome não são notificações genéricas no browser, são específicas para o modelo de extensibilidade do browser (consulte [Descrição Geral das Aplicações do Chrome] para obter mais detalhes). Para além do browser do ambiente de trabalho, as aplicações do Chrome também podem ser executadas em telemóveis (Android e iOS) através do Apache Cordova. Para obter mais informações, veja [Aplicações do Chrome no Telemóvel].

Neste tutorial, siga os passos seguintes:

> [!div class="checklist"]
> * [Ativar o Google Cloud Messaging](#register)
> * [Configurar o hub de notificação](#configure-hub)
> * [Ligar a Aplicação do Chrome ao hub de notificação](#connect-app)
> * [Enviar uma notificação push para a Aplicação do Chrome](#send)

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a id="register"></a>Ativar o Google Cloud Messaging
1. Navegue até ao site da [Google Cloud Console](https://console.cloud.google.com/cloud-resource-manager) e inicie sessão com as credenciais da sua conta Google
2. Selecione **Criar Projeto** na barra de ferramentas. 

    ![Botão Criar Projeto](media/notification-hubs-chrome-get-started/create-project-button.png)
1. Forneça um **Nome do Projeto** adequado e clique no botão **Criar**.
2. Selecione o ícone de notificações (sino) na barra de ferramentas e selecione a mensagem **Criar Projeto**. 

    ![Notificação de criação do projeto](media/notification-hubs-chrome-get-started/project-creation-notification.png)
1. Anote o **Número do Projeto** na página **Projetos** para o projeto que criou. Vai utilizar o número do projeto como o **ID do Remetente do GCM** na Aplicação do Chrome para se registar no GCM.
   
    ![Google Cloud Console – Número do Projeto](media/notification-hubs-chrome-get-started/gcm-project-number.png)
3. No dashboard, selecione **Aceder à descrição geral das APIs**. 

    ![Botão Aceder à descrição geral das APIs](media/notification-hubs-chrome-get-started/go-to-apis-overview-button.png)
1. Na página API e Serviços, selecione **Ativar APIs e Serviços**. 

    ![Ativar APIs e Serviços](media/notification-hubs-chrome-get-started/enable-apis-and-services.png)
1. Pesquise na lista com a palavra-chave **cloud messaging**. Selecione **Google Cloud Messaging** na lista filtrada. 

    ![API do Google Cloud Messaging](media/notification-hubs-chrome-get-started/google-cloud-messaging-api.png)
1. Na página **Google Cloud Messaging**, selecione **Ativar**.

    ![Ativar o GCM](media/notification-hubs-chrome-get-started/enable-gcm.png)
1. Na página **API e Serviços**, mude para o separador **Credenciais**. Selecione **Criar credenciais** e, em seguida, selecione **Chave de API**. 

    ![Botão Criar chave de API](media/notification-hubs-chrome-get-started/create-api-key-button.png)
1. Na caixa de diálogo **Chave de API criada**, selecione o botão Copiar para copiar a chave para a área de transferência. Guarde-a algures. Utilize este valor para configurar o hub de notificação na secção seguinte para permitir que envie notificações push para o GCM.

    ![Página API](media/notification-hubs-chrome-get-started/api-created-page.png)
12. Selecione a chave de API na lista **Chaves de API**. Na página Chave de API, selecione **Endereços IP (servidores Web, tarefas Cron, etc.)** , introduza **0.0.0.0/0** para o endereço IP e clique em Guardar. 

    ![Ativar endereços IP](media/notification-hubs-chrome-get-started/enable-ip-addresses.png)

## <a id="configure-hub"></a>Criar o hub de notificação
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

6. Selecione **Google (GCM)** na categoria **DEFINIÇÕES DE NOTIFICAÇÃO**, introduza a **Chave de API** para o projeto do GCM e clique em **Guardar**.

      ![Notification Hubs do Azure – Google (GCM)](media/notification-hubs-chrome-get-started/configure-gcm-api-key.png)

## <a id="connect-app"></a>Ligar a Aplicação do Chrome ao hub de notificação
O Notification Hub está agora configurado para trabalhar com o GCM e tem as cadeias de ligação para registar a aplicação para receber e enviar notificações push.

### <a name="create-a-new-chrome-app"></a>Criar uma nova Aplicação do Chrome
O exemplo seguinte baseia-se no [Exemplo de GCM para Aplicações do Chrome] e utiliza o modo recomendado para criar uma Aplicação do Chrome. Esta secção destaca os passos específicos dos Hubs de Notificação do Azure. 

> [!NOTE]
> Recomendamos que transfira a origem para esta Aplicação do Chrome a partir do [Exemplo de Notification Hub da Aplicação do Chrome]. 

A Aplicação do Chrome é criada através de JavaScript e pode utilizar qualquer um dos seus editores de texto preferidos para a sua criação. A imagem seguinte mostra o aspeto da aplicação do Chrome:

![Aplicação do Google Chrome][15]

1. Crie uma pasta e dê-lhe nome `ChromePushApp`. O nome é arbitrário. Se lhe der um nome diferente, certifique-se de que substitui o caminho nos segmentos de código necessários.
2. Transfira a [biblioteca crypto-js]para a pasta que criou no segundo passo. Esta pasta de biblioteca contém duas subpastas: `components` e `rollups`.
3. Crie um ficheiro `manifest.json`. Todas as Aplicações do Chrome são acompanhadas por um ficheiro de manifesto que contém os metadados da aplicação e, mais importante ainda, todas as permissões que são concedidas à aplicação quando o utilizador a instala.
   
        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }
   
    Repare no elemento `permissions`, que especifica que esta Aplicação do Chrome está autorizada a receber notificações push do GCM. O exemplo de aplicação utiliza também um ficheiro de ícones, `gcm_128.png`, que pode encontrar na origem e que é reutilizado do exemplo de GCM original. Pode substitui-lo por qualquer imagem que esteja em conformidade com os [critérios de ícones](https://developer.chrome.com/apps/manifest/icons).
4. Crie um ficheiro chamado `background.js` com o seguinte código:
   
        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }
   
        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.
   
          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);
   
          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }
   
        var registerWindowCreated = false;
   
        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {
   
            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }
   
        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);
   
        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);
   
    Este ficheiro faz aparecer o HTML da janela da Aplicação do Chrome (**register.html**) e define também o processador **messageReceived** para processar a notificação push recebida.
5. Crie um ficheiro chamado `register.html`, que define a IU da Aplicação do Chrome. 
   
   > [!NOTE]
   > Este exemplo utiliza **CryptoJS v3.1.2**. Se tiver transferido outra versão da biblioteca, certifique-se de que substitui corretamente a versão no caminho `src`.
   > 
   > 
   
        <html>
   
        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>
   
        <body>
   
        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>
   
        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>
   
        <br/>
   
        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>
   
        <br/>
        <br/>
   
        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>
   
        </body>
   
        </html>
6. Crie um ficheiro chamado `register.js` com o seguinte indicado neste passo. Este ficheiro especifica o script por detrás de `register.html`. As Aplicações do Chrome não permitem a execução inline, terá pois de criar um script de segurança separado para a IU.
   
        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";
   
        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }
   
        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }
   
          document.getElementById("console").innerHTML = currentStatus  + status;
        }
   
        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);
   
          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }
   
        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;
   
          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }
   
          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;
   
          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }
   
        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }
   
        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";
   
          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });
   
          originalUri = endpoint + hubName;
        }
   
        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration
   
          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;
   
          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);
   
          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }
   
        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";
   
          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);
   
          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();
   
          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration successful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };
   
          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }
   
          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");
   
          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }
   
    O script tem os seguintes parâmetros chave:
   
   * **window.onload** define os eventos de clique no botão dos dois botões na IU. O primeiro processador de eventos de clique de botão é registado com o GCM e o outro utiliza o ID de registo que é devolvido após o registo no GCM para se registar nos Hubs de Notificação do Azure.
   * **updateLog** é a função que permite as informações de registo de código. 
   * **registerWithGCM** é o processador do primeiro clique no botão, o que faz a chamada `chrome.gcm.register` para o GCM para registar a instância atual da Aplicação do Chrome.
   * **registerCallback** é a função de chamada de retorno que é chamada quando a chamada de registo do GCM é devolvida.
   * **registerWithNH** é o processador do segundo clique no botão, que regista com Notification Hubs. Obtém `hubName` e `connectionString` (que o utilizador especificou) e cria a chamada da API REST de Registo dos Notification Hubs.
   * **splitConnectionString** e **generateSaSToken** são programas de ajuda que representam a implementação JavaScript de um processo de criação de token SaS, que tem de ser utilizado em todas as chamadas da API REST. Para obter mais informações, consulte [Conceitos Comuns](https://msdn.microsoft.com/library/dn495627.aspx).
   * **sendNHRegistrationRequest** é a função que faz uma chamada REST de HTTP para os Hubs de Notificação do Azure.
   * **registrationPayload** define o payload XML de registo. Para obter mais informações, consulte [Criar API REST dos HN de Registo]. Atualize o ID de registo com o valor recebido do GCM.
   * **client** é uma instância de **XMLHttpRequest** que a aplicação utiliza para fazer o pedido POST de HTTP. Atualize o cabeçalho `Authorization` com `sasToken`. A conclusão com êxito desta chamada regista esta instância da Aplicação do Chrome nos Hubs de Notificação do Azure.

        A estrutura geral das pastas deste projeto deve assemelhar-se à seguinte:    ![Aplicação do Google Chrome – Estrutura de Pastas][21]

### <a name="set-up-and-test-your-chrome-app"></a>Configurar e testar a Aplicação do Chrome
1. Abra o browser Chrome. Abra **Extensões do Chrome** e ative **Modo de programador**.
   
    ![Google Chrome – Ativar o Modo de Programador][16]
2. Clique em **Carregar a extensão descompactada** e navegue para a pasta onde criou os ficheiros. Opcionalmente, pode também utilizar a **Ferramenta do Programador de Aplicações e Extensões do Chrome**. Esta ferramenta é ela própria uma Aplicação do Chrome (instalada a partir do Loja Online do Chrome) e fornece capacidades avançadas de depuração para o desenvolvimento da Aplicação do Chrome.
   
    ![Google Chrome –Carregar a Extensão Descompactada][17]
3. Se a Aplicação do Chrome for criada sem erros, verá a sua Aplicação do Chrome a ser apresentada.
   
    ![Google Chrome – Visualização da Aplicação do Chrome][18]
4. Introduza o **Número de Projeto** que obteve anteriormente da **Google Cloud Console** como ID do remetente e clique em **Registar com o GCM**. Deverá ver a mensagem **Registo com o GCM concluído com êxito.**
   
    ![Google Chrome – Personalização da Aplicação do Chrome][19]
5. Introduza o **Nome do Notification Hub** e o **DefaultListenSharedAccessSignature** que obteve anteriormente do portal e clique em **Registar Notification Hub do Azure**. Deverá ver a mensagem **Registo do Notification Hub concluído com êxito!** e os detalhes da resposta do registo, que contém o ID de registo dos Notification Hubs do Azure.
   
    ![Google Chrome – Especificar os Detalhes do Notification Hub][20]  

## <a name="send"></a>Enviar uma notificação à Aplicação do Chrome
Para efeitos de teste, envie notificações push do Chrome utilizando uma aplicação de consola .NET. 

> [!NOTE]
> Pode enviar notificações push com Hubs de Notificação de qualquer back-end através da <a href="https://msdn.microsoft.com/library/windowsazure/dn223264.aspx">interface REST</a> pública. Consulte o [Portal de documentação](https://azure.microsoft.com/documentation/services/notification-hubs/) para obter mais exemplos em diferentes plataformas.
> 
> 

1. No Visual Studio, no menu **Ficheiro**, selecione **Novo** e, em seguida, **Projeto**. Em **Visual c#**, clique em **Windows**, **Aplicação de Consola** e em **OK**.  Este passo cria um novo projeto de aplicação de consola.
2. No menu **Ferramentas**, clique em **Gestor de Pacotes NuGet** e, em seguida, em **Consola do Gestor de Pacotes**. A Consola do Gestor de Pacotes é apresentada na janela inferior.
3. Na janela da consola, execute o seguinte comando:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
   É adicionada automaticamente uma referência ao SDK do Azure Service Bus com o pacote <a href="http://nuget.org/packages/WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet ao projeto.</a>
4. Abra `Program.cs` e adicione a seguinte instrução `using`:
   
        using Microsoft.Azure.NotificationHubs;
5. Na classe `Program`, adicione o método seguinte:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }
   
    Não se esqueça de substituir o marcador de posição `<hub name>` pelo nome do hub de notificação que aparece no [portal](https://portal.azure.com) na página Hub de Notificação. Substitua, também, o marcador de posição da cadeia de ligação pela cadeia de ligação denominada `DefaultFullSharedAccessSignature`, que obteve na secção de configuração do Notification Hub.
   
    > [!NOTE]
    > Certifique-se de que utiliza a cadeia de ligação com acesso **Completo** e não com acesso de **Escuta**. A cadeia de ligação de acesso de **Escuta** não concede permissões para enviar notificações push.
6. Adicionar o seguinte, chama o método `Main`:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Certifique-se de que o Chrome está em execução e execute a aplicação de consola.
8. Deverá ver o seguinte pop-up de notificação no ambiente de trabalho.
   
    ![Google Chrome – Notificação][13]
9. Poderá ver também todas as suas notificações utilizando a janela de Notificações do Chrome na barra de tarefas (no Windows) quando estiver a executar o Chrome.
   
    ![Google Chrome – Lista de Notificações](./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png)

> [!NOTE]
> Não precisa de ter a Aplicação do Chrome em execução ou aberta no browser (mas o browser Chrome em si deve estar em execução). Pode, também, ter uma vista consolidada de todas as suas notificações na janela de Notificações do Chrome.
> 
> 

## <a name="next-steps"> </a>Passos seguintes
Neste tutorial, enviou notificações de difusão para todos os clientes registados no back-end. Para saber como enviar notificações push para dispositivos específicos, avance para o tutorial seguinte: 

> [!div class="nextstepaction"]
>[Enviar notificações push para dispositivos específicos](notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md)

<!-- Images. -->
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[Exemplo de Notification Hub da Aplicação do Chrome]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Notification Hubs Overview]: notification-hubs-push-notification-overview.md
[Descrição Geral das Aplicações do Chrome]: https://developer.chrome.com/apps/about_apps
[Exemplo de GCM para Aplicações do Chrome]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Aplicações do Chrome no Telemóvel]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Criar API REST dos HN de Registo]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[biblioteca crypto-js]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud Messaging for Chrome]: https://developer.chrome.com/apps/cloudMessagingV1
[Azure Notification Hubs Notify Users]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Azure Notification Hubs breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
