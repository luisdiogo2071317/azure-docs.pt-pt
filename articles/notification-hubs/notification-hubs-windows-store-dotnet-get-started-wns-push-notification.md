---
title: Enviar notificações para aplicações da Plataforma Universal do Windows com os Hubs de Notificação do Azure | Microsoft Docs
description: Neste tutorial, irá aprender a utilizar os Hubs de Notificação do Azure para enviar notificações push para uma aplicação da Plataforma Universal do Windows.
services: notification-hubs
documentationcenter: windows
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: cf307cf3-8c58-4628-9c63-8751e6a0ef43
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: c3bb170800508d5a546573850f445b2a8991ea8c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38597749"
---
# <a name="tutorial-send-notifications-to-universal-windows-platform-apps-by-using-azure-notification-hubs"></a>Tutorial: Enviar notificações para aplicações da Plataforma Universal do Windows com os Hubs de Notificação do Azure

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Neste tutorial, vai criar um hub de notificação para enviar notificações push para uma aplicação da Plataforma Universal do Windows (UWP). Vai criar uma aplicação da Loja Windows em branco que recebe notificações push com o Serviço de Notificações Push do Windows (WNS). Em seguida, pode utilizar o seu hub de notificação para difundir notificações push para todos os dispositivos que executem a sua aplicação.

> [!NOTE]
> O código concluído deste artigo está disponível no [GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).

Neste tutorial, siga os passos seguintes:

> [!div class="checklist"]
> * Criar uma aplicação na Loja Windows
> * Criar um hub de notificação
> * Criar uma aplicação do Windows de exemplo
> * Enviar notificações de teste


## <a name="prerequisites"></a>Pré-requisitos
- **Subscrição do Azure**. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.
- [Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) ou posterior.
- [Ferramentas de desenvolvimento de aplicações do UWP instaladas](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
- Uma conta ativa da Loja Windows

A conclusão deste tutorial é um pré-requisito para todos os outros tutoriais dos Hubs de Notificação para aplicações do UWP.

## <a name="create-an-app-in-windows-store"></a>Criar uma aplicação na Loja Windows
Para enviar notificações push para aplicações do UWP, associe a aplicação à Loja Windows. Em seguida, configure o Hub de Notificação para se integrar no WNS.

1. Navegue para o [Windows Dev Center](https://dev.windows.com/overview), inicie sessão com a sua conta Microsoft e selecione **Criar uma nova aplicação**.

    ![Botão Nova aplicação](./media/notification-hubs-windows-store-dotnet-get-started/windows-store-new-app-button.png)
1. Escreva um nome para a aplicação e selecione **Reservar nome do produto**. Desta forma, é criado um registo novo da Loja Windows para a sua aplicação.

    ![Nome da aplicação na loja](./media/notification-hubs-windows-store-dotnet-get-started/store-app-name.png)
1. Expanda **Gestão de Aplicações**, selecione **WNS/MPNS**, **WNS/MPNS** e **Site dos Serviços Live**. Inicie sessão com a sua conta Microsoft. É aberto o **Portal de Registo da Aplicação** num novo separador. Em alternativa, pode navegar diretamente para o [Portal de Registo da Aplicação](http://apps.dev.microsoft.com) e selecionar o nome da aplicação para aceder a esta página.

    ![Página do WNS MPNS](./media/notification-hubs-windows-store-dotnet-get-started/wns-mpns-page.png)
1.   Anote a palavra-passe **Segredo da Aplicação** e o **Identificador de segurança de pacotes (SID)**.

        >[!WARNING]
        >O segredo da aplicação e o SID do pacote são credenciais de segurança importantes. Não partilhe estes valores com ninguém e não os distribua com a aplicação.

## <a name="create-a-notification-hub"></a>Criar um hub de notificação
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


### <a name="configure-wns-settings-for-the-hub"></a>Configurar as definições de WNS para o hub

1. Selecione **Windows (WNS)** na categoria **DEFINIÇÕES DE NOTIFICAÇÃO**. 
2. Introduza os valores para **SID do Pacote** e **Chave de Segurança** que anotou na secção anterior. 
3. Selecione **Guardar** na barra de ferramentas.

    ![Caixas do SID do Pacote e Chave de Segurança](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

O seu hub de notificação está agora configurado para funcionar com o WNS. Tem as cadeias de ligação para registar a sua aplicação e enviar notificações.

## <a name="create-a-sample-windows-app"></a>Criar uma aplicação do Windows de exemplo
1. No Visual Studio, selecione **Ficheiro**, aponte para **Novo** e selecione **Projeto**. 
2. Na caixa de diálogo **Novo Projeto**, siga os seguintes passos: 

    1. Expanda **Visual C#**.
    2. Selecione **Windows Universal**. 
    3. Selecione **Aplicação em branco (Universal Windows)**. 
    4. Introduza um **nome** para o projeto. 
    5. Selecione **OK**. 

        ![Caixa de diálogo Novo Projeto](./media/notification-hubs-windows-store-dotnet-get-started/new-project-dialog.png)
1. Aceite as predefinições para as versões de **destino** e **mínimas** da plataforma e selecione **OK**. 
2. No Explorador de Soluções, clique com o botão direito do rato no projeto de aplicação da Loja Windows, selecione **Loja** e, em seguida, selecione **Associar Aplicação à Loja**. É apresentado o assistente **Associar aplicação à Loja Windows**.
3. No assistente, inicie sessão com a sua conta Microsoft.
4. Selecione a aplicação que registou no passo 2, selecione **Seguinte** e, por fim, **Associar**. Desta forma, é adicionado ao manifesto da aplicação as informações de registo da Loja Windows necessárias.
5. No Visual Studio, clique com o botão direito do rato na solução e, em seguida, Selecione **Gerir Pacotes NuGet**. É aberta a janela **Gerir Pacotes NuGet**.
6. Na caixa de pesquisa, introduza **WindowsAzure.Messaging.Managed**, selecione **Instalar** e aceite os termos de utilização.
   
    ![Janela Gerir Pacotes NuGet][20]
   
    Esta ação transfere, instala e adiciona uma referência ao à biblioteca dos Hubs de Notificação do Azure para Windows mediante a utilização do [Pacote NuGet Microsoft.Azure.Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs).

3. Abra o ficheiro de projeto App.xaml.cs e adicione as seguintes instruções `using`: 
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;

4. No App.xaml.cs, adicione também a seguinte definição de método **InitNotificationsAsync** à classe **App**:
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("<your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
   
        }
   
    Este código obtém o URI do canal para a aplicação a partir do WNS e, em seguida, regista esse URI de canal no Notification Hub.
   
    >[!NOTE]
    >* Substitua o marcador de posição **nome do hub** pelo nome do hub de notificação que aparece no portal do Azure. 
    >* Substitua, também, o marcador de posição da cadeia de ligação pela cadeia de ligação **DefaultListenSharedAccessSignature**, que obteve na página **Políticas de Acesso** do seu hub de notificação numa secção anterior.
   > 
   > 
5. No topo do processador de eventos **OnLaunched** no App.xaml.cs, adicione a seguinte chamada ao novo método **InitNotificationsAsync**:
   
        InitNotificationsAsync();
   
    Esta ação garante que o URI do canal é registado no seu hub de notificações sempre que a aplicação for iniciada.

6. Para executar a aplicação, selecione a tecla **F5**. É apresentada uma caixa de diálogo que contém a chave de registo. Para fechar a caixa de diálogo, selecione **OK**. 

    ![Registado com êxito](./media/notification-hubs-windows-store-dotnet-get-started/registration-successful.png)

A aplicação já está pronta para receber notificações de alerta.

## <a name="send-test-notifications"></a>Enviar notificações de teste
Pode testar rapidamente a receção das notificações na sua aplicação ao enviar notificações no [portal do Azure](https://portal.azure.com/). 

1. No portal do Azure, mude para o separador Descrição Geral e selecione **Testar Envio** na barra de ferramentas.     

    ![Botão Testar Envio](./media/notification-hubs-windows-store-dotnet-get-started/test-send-button.png)
2. Na janela **Testar Envio**, efetue as ações abaixo: 
    1. Para **Plataformas**, selecione **Windows**.
    2. Para **Tipo de Notificação**, selecione **Alerta**. 
    3. Selecione **Enviar**. 
    
        ![Painel Testar Envio](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)
3. Veja o resultado da operação Enviar na lista **Resultado** na parte inferior da janela. Também pode ver uma mensagem de alerta. 

    ![Resultado da operação Enviar](./media/notification-hubs-windows-store-dotnet-get-started/result-of-send.png)
1. Veja a mensagem de notificação: **Mensagem de teste** no ambiente de trabalho. 

    ![Mensagem de notificação](./media/notification-hubs-windows-store-dotnet-get-started/test-notification-message.png)


## <a name="next-steps"></a>Passos seguintes
Neste tutorial, enviou notificações de difusão para todos os seus dispositivos Windows através do portal ou de uma aplicação de consola. Para saber como enviar notificações push para dispositivos específicos, avance para o tutorial seguinte: 

> [!div class="nextstepaction"]
>[Enviar notificações push para dispositivos específicos](
notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md)


<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->

[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md

[toast catalog]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[tile catalog]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[badge overview]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx
 
