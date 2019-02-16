---
title: Configurar o hub de notificação do Azure com as definições de PNS | Documentos da Microsoft
description: Neste guia de introdução, saiba como configurar um hub de notificação no portal do Azure com definições de sistema (PNS) de notificação de plataforma.
services: notification-hubs
author: jwargo
manager: patniko
editor: spelluru
ms.service: notification-hubs
ms.workload: mobile
ms.topic: quickstart
ms.date: 02/14/2019
ms.author: jowargo
ms.openlocfilehash: 7f7e4a4d75a8e118da6f026817bc4ecfcc7a60db
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/15/2019
ms.locfileid: "56314133"
---
# <a name="configure-an-azure-notification-hub-with-platform-notification-system-settings-in-the-azure-portal"></a>Configurar um hub de notificação do Azure com definições de sistema de notificação de plataforma no portal do Azure 
Os Hubs de Notificação do Azure oferecem um motor de envio de escalamento horizontal e utilização fácil que lhe permite enviar notificações para qualquer plataforma (iOS, Android, Windows, Kindle, Baidu, etc.) a partir de qualquer back-end (cloud ou no local). Para obter mais informações sobre o serviço, consulte [o que é o Notification Hubs do Azure?](notification-hubs-push-notification-overview.md).

[Criar um hub de notificação do Azure com o portal do Azure](create-notification-hub-portal.md) se ainda não fez isso. Neste guia de introdução, saiba como configurar um hub de notificação no portal do Azure com definições de sistema (PNS) de notificação de plataforma.

## <a name="apple-push-notification-service-apns"></a>Serviço Apple Push Notification (APNS)
1. Na **Hub de notificação** página no portal do Azure, selecione **Apple (APNS)** sob **definições** no menu da esquerda.
2. Se selecionou **certificado**, e fazer as seguintes ações:
    1. Selecione o **ícone de ficheiros**e selecione o **p12** ficheiro a carregar. 
    2. Especifique a **palavra-passe**.
    3. Selecione o modo **Sandbox**. Utilize o modo **Produção** apenas se pretender enviar notificações push a utilizadores que já tenham adquirido a aplicação da loja.

        ![Configurar certificação APNS no portal do Azure](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)
3. Se selecionou **Token**e siga estes passos: 
    1. Introduza os valores para **ID da chave**, **ID de pacote**, **ID da equipa**, e **token**.
    2. Selecione o modo **Sandbox**. Utilize o modo **Produção** apenas se pretender enviar notificações push a utilizadores que já tenham adquirido a aplicação da loja.

        ![Configurar o token do APNS no portal do Azure](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-token.png)

Para um tutorial completo sobre como enviar notificações push para dispositivos iOS com Notification Hubs do Azure e o Apple Push Notification Service (APNS), consulte [deste tutorial](notification-hubs-ios-apple-push-notification-apns-get-started.md).

## <a name="google-firebase-cloud-messaging-fcm"></a>Google Firebase Cloud Messaging (FCM)
1. Sobre o **Hub de notificação** página no portal do Azure, selecione **Google (GCM/FCM)** sob **definições** no menu da esquerda. 
2. Colar o **chave de servidor** para o projeto do FCM que guardou anteriormente. 
3. Selecione **Guardar** na barra de ferramentas. 

    ![Hubs de notificação do Azure – Google (FCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)
4. Verá uma mensagem nos alertas que o hubs de notificação foi atualizada com êxito. O **guardar** botão está desativado. 

Para um tutorial completo sobre como enviar notificações push para dispositivos Android com Notification Hubs do Azure e o Google Firebase Cloud Messaging, consulte [deste tutorial](notification-hubs-android-push-notification-google-fcm-get-started.md).

## <a name="windows-push-notification-service-wns"></a>Serviço de notificações de Push do Windows (WNS)
1. Sobre o **Hub de notificação** página no portal do Azure, selecione **Windows (WNS)** sob **definições** no menu da esquerda.
2. Introduza os valores para **SID do pacote** e **chave de segurança**.
3. Selecione **Guardar** na barra de ferramentas.

    ![Caixas do SID do Pacote e Chave de Segurança](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)


Para um tutorial completo sobre como enviar notificações para uma aplicação plataforma Universal do Windows (UWP) em execução num dispositivo Windows, consulte [deste tutorial](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).

## <a name="windows-phone---microsoft-push-notification-service"></a>Windows Phone - serviço de notificações Push da Microsoft
1. Sobre o **Hub de notificação** página no portal do Azure, selecione **Windows Phone (MPNS)** sob **definições**.
2. Se pretender ativar push não autenticado, selecione **ativar push não autenticado**e selecione **guardar** na barra de ferramentas.

    ![Portal do Azure – Ativar notificações push não autenticadas](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)
3. Se pretender utilizar o **autenticado** push, siga estes passos:
    1. Selecione **carregar certificado** na barra de ferramentas.
    2. Selecione o **ícone de arquivo** e selecione o ficheiro de certificado.
    3. Introduza o **palavra-passe** para o certificado. 
    4. Selecione **OK** para fechar a **carregar certificado** página. 
    5. Sobre o **Windows Phone(MPNS)** página, selecione **guardar** na barra de ferramentas.

Para um tutorial completo sobre como enviar notificações push para um aplicativo Windows Phone 8 utilizando o Microsoft Push Notification Service (MPNS), consulte [deste tutorial](notification-hubs-windows-mobile-push-notifications-mpns.md).
      
## <a name="amazon-device-messaging-adm"></a>Amazon Device Messaging (ADM)
1. Sobre o **Hub de notificação** página no portal do Azure, selecione **Amazon (ADM)** sob **definições** no menu da esquerda.
2. Introduza os valores para **ID de cliente** e **segredo do cliente**.
3. Selecione **Guardar** na barra de ferramentas.
    
    ![Hubs de notificação do Azure – definições do ADM](./media/notification-hubs-kindle-get-started/notification-hub-adm-settings.png)

Para obter um tutorial completo sobre como utilizar notificações de push Notification Hubs do Azure para uma aplicação Kindle, consulte [deste tutorial](notification-hubs-kindle-amazon-adm-push-notification.md).

## <a name="baidu-android-china"></a>Baidu (Android China)
1. Sobre o **Hub de notificação** página no portal do Azure, selecione **Baidu (Android China)** sob **definições** no menu da esquerda. 
2. Introduza o **chave de API** que obtém a partir da consola do Baidu, no projeto Baidu cloud push. 
3. Introduza o **chave secreta** que obteve na consola do Baidu, no projeto Baidu cloud push. 
4. Selecione **Guardar** na barra de ferramentas. 

    ![Hubs de notificação do Azure – Baidu (Android China)](./media/notification-hubs-baidu-get-started/AzureNotificationServicesBaidu.png)
4. Verá uma mensagem nos alertas que o hubs de notificação foi atualizada com êxito. O **guardar** botão está desativado. 

Para um tutorial completo sobre como enviar notificações de push com Notification Hubs do Azure e o Baidu cloud push, consulte [deste tutorial](notification-hubs-baidu-china-android-notifications-get-started.md).

## <a name="next-steps"></a>Passos Seguintes
Neste início rápido, aprendeu a configurar os sistemas de notificação de plataforma diferente para um hub de notificação no portal do Azure. 

Para obter instruções passo a passo completas para enviar notificações para estas plataformas diferentes, veja os tutoriais na **tutoriais** secção.

- [Enviar notificações push para dispositivos iOS com Notification Hubs do Azure e o Apple Push Notification Service (APNS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).
- [Enviar notificações push para dispositivos Android com Notification Hubs do Azure e o Google Firebase Cloud Messaging](notification-hubs-android-push-notification-google-fcm-get-started.md).
- [Notificações push para uma aplicação plataforma Universal do Windows (UWP) em execução num dispositivo Windows](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
- [Enviar notificações push para um aplicativo Windows Phone 8 utilizando o Microsoft Push Notification Service (MPNS)](notification-hubs-windows-mobile-push-notifications-mpns.md).
- [Notificações push para uma aplicação Kindle](notification-hubs-kindle-amazon-adm-push-notification.md).
- [Notificações push utilizando a emissão de cloud de Notification Hubs do Azure e Baidu](notification-hubs-baidu-china-android-notifications-get-started.md).
