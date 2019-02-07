---
title: incluir ficheiro
description: incluir ficheiro
services: notification-hubs
author: jwargo
ms.service: notification-hubs
ms.topic: include
ms.date: 01/17/2019
ms.author: jowargo
ms.custom: include file
ms.openlocfilehash: 00b7ffcba876b6abea59cff170331c7413a61d39
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823322"
---
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** no menu à esquerda e selecione **os Hubs de notificação** no **Mobile** secção. Selecione a estrela (`*`) junto ao nome do serviço para adicioná-lo para o **Favoritos** secção no menu da esquerda. Após **os Hubs de notificação** é adicionado ao **Favoritos**, selecione-o no menu da esquerda. 

      ![Portal do Azure – selecione os Hubs de notificação](./media/notification-hubs-portal-create-new-hub/all-services-select-notification-hubs.png)
3. Sobre o **os Hubs de notificação** página, selecione **Add** na barra de ferramentas. 

      ![Os Hubs de notificação - adicionar botão de barra de ferramentas](./media/notification-hubs-portal-create-new-hub/add-toolbar-button.png)
4. Sobre o **Hub de notificação** página, efetue os seguintes passos: 
    1. Especifique um **name** para a notificação **hub**.  
    2. Especifique um **name** para o **espaço de nomes**.
    3. Selecione um **localização** no qual pretende criar o hub de notificação. 
    4. Selecione um grupo de recursos existente ou introduza um nome para a nova **grupo de recursos**.
    5. Selecione **Criar**. 

        ![Portal do Azure – definir as propriedades do hub de notificação](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)
4. Selecione **notificações** (ícone de sino) e selecione **Ir para recurso**, ou atualizar a lista no **Hubs de notificação** página e selecione o hub de notificação. 

      ![Portal do Azure - notificações -> Ir para o recurso](./media/notification-hubs-portal-create-new-hub/go-to-notification-hub.png)
5. Selecione **Políticas de Acesso** na lista. Anote as duas cadeias de ligação que tem à sua disposição. Precisará destas para processar as notificações push mais tarde.

      >[!IMPORTANT]
      >**NÃO** utilize DefaultFullSharedAccessSignature na sua aplicação. Isto destina-se a ser utilizado apenas no seu back-end.
      >

      ![Portal do Azure – cadeias de ligação do hub de notificação](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)
