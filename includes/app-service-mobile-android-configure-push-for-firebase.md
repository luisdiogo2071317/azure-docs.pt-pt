---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: bb03e4b5b04a0272d8fa9b032da5adb50878b620
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42811567"
---
1. Na [portal do Azure](https://portal.azure.com/), clique em **Procurar tudo** > **serviços aplicacionais**e, em seguida, clique em seu back-end de aplicações móveis. Sob **configurações**, clique em **Push do serviço de aplicações**e, em seguida, clique no nome do hub de notificação.
2. Aceda a **Google (GCM)**, introduza o **chave de servidor de** valor obtido a partir do Firebase no procedimento anterior, e, em seguida, clique em **guardar**.

    ![Defina a chave de API no portal](./media/app-service-mobile-android-configure-push/mobile-push-api-key.png)

O back-end de aplicações móveis está agora configurado para utilizar o Firebase Cloud Messaging. Isto permite-lhe enviar notificações push para a aplicação em execução num dispositivo Android, ao utilizar o hub de notificação.
