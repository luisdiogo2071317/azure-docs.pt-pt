---
title: "Adicionar notificações push à aplicação xamarin. Android | Microsoft Docs"
description: "Saiba como utilizar o App Service do Azure e Notification Hubs do Azure para enviar notificações push à aplicação xamarin. Android"
services: app-service\mobile
documentationcenter: xamarin
author: conceptdev
manager: crdun
editor: 
ms.assetid: 6f7e8517-e532-4559-9b07-874115f4c65b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: crdun
ms.openlocfilehash: a6cdff68d63859c6a6612b606664d3e1fbaae375
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Adicionar notificações push à aplicação Xamarin.Android
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Descrição geral
Neste tutorial, adicionar notificações push para o [início rápido do xamarin. Android](app-service-mobile-windows-store-dotnet-get-started.md) projeto para que uma notificação push é enviada para o dispositivo sempre que é inserido um registo.

Se utilizar o projeto de servidor de início rápido transferido, terá do pacote de extensão de notificação push. Para obter mais informações, consulte o [trabalhar com o servidor de back-end .NET SDK de Mobile Apps do Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) guia.

## <a name="prerequisites"></a>Pré-requisitos
Este tutorial requer o programa de configuração:

* Uma conta Google ativa. Pode inscrever-se para uma conta do Google na [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).
* [Google Cloud Messaging o componente de cliente](http://components.xamarin.com/view/GCMClient/).

## <a name="configure-hub"></a>Configurar um Hub de notificação
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Ativar o Firebase Cloud Messaging
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-azure-to-send-push-requests"></a>Configurar o Azure para enviar pedidos de push
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a id="update-server"></a>Atualizar o projeto de servidor para enviar notificações push
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a id="configure-app"></a>Configurar o projeto de cliente para notificações push
[!INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <a id="add-push"></a>Adicionar o código de notificações push à aplicação
[!INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Notificações de push de teste na sua aplicação
Pode testar a aplicação através da utilização de um dispositivo virtual no emulador. Existem passos de configuração adicionais necessários quando em execução num emulador.

1. O dispositivo virtual tem de ter as APIs da Google definida como destino no Gestor de dispositivo Virtual Android (AVD).
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)
2. Adicionar uma conta do Google no dispositivo Android, clicando em **aplicações** > **definições** > **adicionar conta**, em seguida, siga as instruções.
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)
3. Execute a aplicação de todolist como antes e inserir um novo item de todo. Este tempo, um ícone de notificação é apresentado na área de notificação. Pode abrir a gaveta de notificação para ver o texto completo da notificação.
   
    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)

<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
