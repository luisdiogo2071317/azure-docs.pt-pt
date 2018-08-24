---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: c86210208b9f747cbef1d9231528fa6cedbdb5d2
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42814534"
---
1. Com o botão direito no projeto da Windows Store, clique em **Set as StartUp Project**, em seguida, prima a tecla F5 para executar a aplicação da Windows Store.

    Depois da aplicação for iniciada, o dispositivo está registado para as notificações push.
2. Pare a aplicação da Windows Store e repita o passo anterior para a aplicação do Windows Phone Store.

    Neste momento, ambos os dispositivos estão registados para receber notificações push.

3. Execute novamente a aplicação da Windows Store e digite um texto no **inserir um TodoItem**e, em seguida, clique em **guardar**.

    Tenha em atenção que depois de concluída a inserção, a Windows Store e as aplicações do Windows Phone recebem uma notificação push do WNS. A notificação é apresentada no Windows Phone, mesmo quando a aplicação não está em execução.

    ![](./media/app-service-mobile-windows-universal-test-push/mobile-quickstart-push5-wp8.png)
