---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.author: crdun
ms.openlocfilehash: 1d1d593b7305e0cd9899f4ec388cb441ced90b10
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50134269"
---
1. No Mac, visite o [Portal do Azure]. Clique em **Todos os Serviços** > **Serviço de Aplicações** > o back-end que acabou de criar. Nas definições da aplicação móvel, selecione o idioma que prefere:

    - Objective-C &ndash; **Início Rápido** > **iOS (Objective-C)**
    - SWIFT &ndash; **Início Rápido** > **iOS (Swift)**

    Em **3. Configurar a aplicação cliente**, clique em **Transferir**. Esta ação transfere um projeto Xcode concluído pré-configurado para ligar ao back-end. Abra o projeto com o Xcode.

1. Prima o botão **Executar** para criar o projeto e iniciar a aplicação no simulador do iOS.

1. Na aplicação, digite um texto significativo, tal como *Concluir o tutorial*, e clique no ícone de mais (**+**). Esta ação envia um pedido POST ao back-end do Azure que implementou anteriormente. Os dados de inserções de back-end do pedido encontram-se na tabela TodoItem SQL e devolve informações sobre os itens recentemente armazenados na aplicação móvel. A aplicação móvel apresenta estes dados na lista.

   ![Aplicação de início rápido em execução no iOS](./media/app-service-mobile-ios-quickstart/mobile-quickstart-startup-ios.png)

[Portal do Azure]: https://portal.azure.com/
