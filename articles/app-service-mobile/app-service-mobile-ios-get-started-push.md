---
title: "Adicionar notificações Push à aplicação Mobile Apps do Azure com o iOS"
description: "Saiba como utilizar Mobile Apps do Azure para enviar notificações push para a sua aplicação iOS."
services: app-service\mobile
documentationcenter: ios
manager: crdun
editor: 
author: conceptdev
ms.assetid: fa503833-d23e-4925-8d93-341bb3fbab7d
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/10/2016
ms.author: crdun
ms.openlocfilehash: 063926a5a98aeab62e191d372831a00c29b9f6d7
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2018
---
# <a name="add-push-notifications-to-your-ios-app"></a>Adicionar notificações Push para aplicações iOS
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Descrição geral
Neste tutorial, adicionar notificações push para o [iOS rápidos iniciar] projeto para que uma notificação push é enviada para o dispositivo sempre que é inserido um registo.

Se utilizar o projeto de servidor de início rápido transferido, terá do pacote de extensão de notificação push. Consulte [trabalhar com o servidor de back-end .NET SDK de Mobile Apps do Azure](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) para obter mais informações.

O [simulador iOS não suporta notificações push](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Precisa de um dispositivo iOS físico e um [filiação do programa de programador Apple](https://developer.apple.com/programs/ios/).

## <a name="configure-hub"></a>Configurar Notification Hub
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Registar a aplicação para notificações push
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Configurar o Azure para enviar notificações push
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a id="update-server"></a>Atualizar o back-end para enviar notificações push
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Adicionar notificações push à aplicação
[!INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Notificações de push de teste
[!INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

## <a id="more"></a>Mais
* Modelos dão-lhe a flexibilidade para enviar pushes de plataforma e pushes localizadas. [Como utilizar iOS biblioteca de clientes para Mobile Apps do Azure](app-service-mobile-ios-how-to-use-client-library.md#templates) mostra-lhe como registar modelos.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS rápidos iniciar]: app-service-mobile-ios-get-started.md
