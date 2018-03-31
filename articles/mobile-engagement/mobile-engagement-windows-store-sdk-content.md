---
title: Conteúdo do SDK de aplicações universais do Windows
description: Saiba mais sobre os conteúdos do SDK Windows Universal aplicações do Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: 8fa1b701-1c2b-4aec-940c-06c974ef5405
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 7b520adcc6af24f7b092580ea82a787a120668bf
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2018
---
# <a name="windows-universal-apps-sdk-content"></a>Conteúdo do SDK de aplicações universais do Windows
> [!IMPORTANT]
> O Azure Mobile Engagement ter extinguido em 3/31/2018. Esta página será eliminada pouco tempo depois.
> 

Este documento apresenta e descreve o conteúdo implementado pelo SDK na sua aplicação.

## <a name="the-resources-folder"></a>O `/Resources` pasta
Esta pasta contém todos os recursos que necessita de Mobile Engagement. Também pode personalizá-las para ajustar à sua aplicação.

* `EngagementConfiguration.xml` : Ficheiro de configuração o Mobile Engagement, este é onde pode personalizar as definições de Mobile Engagement (cadeia de ligação do Mobile Engagement, falhas de relatório...).

### <a name="html-folder"></a>pasta /HTML
* `EngagementNotification.html` : O `Notification` web estrutura de html da vista de faixas na aplicação.
* `EngagementAnnouncement.html` : O `Announcement` web estrutura da vista html para vistas interstitial na aplicação.

### <a name="images-folder"></a>pasta /Images
* `EngagementIconNotification.png` : O ícone de marca apresentado à esquerda de uma notificação, substituir este pelo ícone de marca.
* `EngagementIconOk.png` : O `Ok` ícone das páginas de conteúdo de alcance para o botão de ação ou de validação.
* `EngagementIconNOK.png` : O `NOK` ícone utilizado quando o botão de validação das páginas de conteúdo de alcance está desativado.
* `EngagementIconClose.png` : O `Close` ícone de notificações de alcance e conteúdo do botão de dispensa.

### <a name="overlay-folder"></a>pasta /Overlay
* `EngagementPageOverlay.cs` : A página de sobreposição responsável pela adição o Engagement alcançar na aplicação IU para o subordinado.

