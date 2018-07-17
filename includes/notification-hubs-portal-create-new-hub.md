---
title: incluir ficheiro
description: incluir ficheiro
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 03/28/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 22c1d24042072de5d57d41da379a5fad18180de7
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972519"
---
1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. Selecione **Criar um recurso** > **Móvel** > **Hub de Notificação**.
   
      ![Portal do Azure – criar hubs de notificação](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-create.png)
      
3. Na caixa **Hub de Notificação**, escreva um nome exclusivo. Selecione a **Região**, a **Subscrição** e o **Grupo de Recursos** (se já tiver um). 
   
      Se ainda não tiver um espaço de nomes do service bus, pode utilizar o nome predefinido, que é criado tendo por base o nome do hub (se esse nome do espaço de nomes estiver disponível).
    
      Se já tiver um espaço de nomes do service bus no qual quer criar o hub, siga estes passos

    a. Na área **Espaço de nomes**, selecione a ligação **Selecionar Existente**. 
   
    b. Selecione **Criar**.
   
      ![Portal do Azure – definir as propriedades do hub de notificação](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

4. Selecione **Notificações** (ícone de campainha) e selecione **Ir para recurso**. 

      ![Portal do Azure - notificações -> Ir para o recurso](./media/notification-hubs-portal-create-new-hub/notification-go-to-resource.png)    
5. Selecione **Políticas de Acesso** na lista. Anote as duas cadeias de ligação que tem à sua disposição. Precisará destas para processar as notificações push mais tarde.

      >[!IMPORTANT]
      >**NÃO** utilize DefaultFullSharedAccessSignature na sua aplicação. Isto destina-se a ser utilizado apenas no seu back-end.
      >
   
      ![Portal do Azure – cadeias de ligação do hub de notificação](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)

