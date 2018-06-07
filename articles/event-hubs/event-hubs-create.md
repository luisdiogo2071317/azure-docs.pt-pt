---
title: Criar um hub de eventos do Azure | Microsoft Docs
description: Criar um espaço de nomes de Event Hubs do Azure e um hub de eventos no portal do Azure
services: event-hubs
author: sethmanheim
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 05/29/2017
ms.author: sethm
ms.openlocfilehash: 9b466d4e727c1511ca2318c0da3ec2807a965a5d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34625547"
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-the-azure-portal"></a>Criar um espaço de nomes de Event Hubs e um hub de eventos no portal do Azure

## <a name="create-an-event-hubs-namespace"></a>Criar um espaço de nomes de Event Hubs

1. Inicie sessão no [portal do Azure][Azure portal]e clique em **crie um recurso** na parte superior esquerda do ecrã.
2. Clique em **Internet das coisas**e, em seguida, clique em **Event Hubs**.
   
    ![](./media/event-hubs-create/create-event-hub9.png)

3. No **criar espaço de nomes**, introduza um nome de espaço de nomes. O sistema verifica imediatamente a disponibilidade do nome.  

4. Após se certificar de que o espaço de nomes está disponível, selecione o escalão de preço (Básico ou Standard). Escolha também uma subscrição do Azure, um grupo de recursos e a localização na qual pretende criar o recurso.
 
5. Clique em **Criar** para criar o espaço de nome. Poderá ter de aguardar alguns minutos para que o sistema aprovisione totalmente os recursos.

    ![](./media/event-hubs-create/create-event-hub1.png)

6. Na lista de espaços de nomes portal, clique o espaço de nomes criado recentemente.

7. Clique em **políticas de acesso partilhado**e, em seguida, clique em **RootManageSharedAccessKey**.
    
    ![](./media/event-hubs-create/create-event-hub7.png)

8. Clique no botão copiar para copiar a cadeia de ligação **RootManageSharedAccessKey** para a área de transferência. Guarde esta cadeia de ligação numa localização temporária, como o Notepad, para utilizar mais tarde.
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a>Criar um hub de eventos

1. Na lista de espaço de nomes de Event Hubs, clique o espaço de nomes criado recentemente.      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. No painel de espaço de nomes, clique em **Hubs de Eventos**.
   
    ![](./media/event-hubs-create/create-event-hub3.png)

3. Na parte superior do painel, clique em **+ Hub de eventos**.
   
    ![](./media/event-hubs-create/create-event-hub4.png)
4. Escreva um nome para o hub de eventos, em seguida, clique em **criar**. 

O hub de eventos está agora criado e tem as cadeias de ligação que terá de enviar e receber eventos.

## <a name="next-steps"></a>Passos Seguintes

Para mais informações sobre os Event Hubs, visite estas ligações:

* [Descrição geral dos Hubs de Eventos](event-hubs-what-is-event-hubs.md)
* [Descrição geral da API dos Hubs de Eventos](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/