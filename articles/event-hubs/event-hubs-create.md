---
title: Criar um hub de eventos do Azure | Documentos da Microsoft
description: Criar um espaço de nomes de Hubs de eventos do Azure e um hub de eventos com o portal do Azure
services: event-hubs
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: 5b468e1758d752cd3001c85b328d064369429499
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/17/2018
ms.locfileid: "42060723"
---
# <a name="create-an-event-hubs-namespace-and-an-event-hub-using-the-azure-portal"></a>Criar um espaço de nomes de Hubs de eventos e um hub de eventos com o portal do Azure

## <a name="create-an-event-hubs-namespace"></a>Criar um espaço de nomes dos Hubs de Eventos

1. Inicie sessão no [portal do Azure][Azure portal]e clique em **criar um recurso** na parte superior esquerda do ecrã.
2. Clique em **Internet das Coisas** e, em seguida, clique em **Hubs de Eventos**.
   
    ![](./media/event-hubs-create/create-event-hub9.png)

3. Em **Criar espaço de nomes**, introduza um nome de espaço de nomes. O sistema verifica imediatamente a disponibilidade do nome.  

4. Após se certificar de que o espaço de nomes está disponível, selecione o escalão de preço (Básico ou Standard). Escolha também uma subscrição do Azure, um grupo de recursos e a localização na qual quer criar o recurso.
 
5. Clique em **Criar** para criar o espaço de nomes. Poderá ter de aguardar alguns minutos para que o sistema aprovisione totalmente os recursos.

    ![](./media/event-hubs-create/create-event-hub1.png)

6. Na lista de espaços de nomes do portal, clique no nome do espaço de nomes criado recentemente.

7. Clique em **Políticas de acesso partilhado** e, em seguida, clique em **RootManageSharedAccessKey**.
    
    ![](./media/event-hubs-create/create-event-hub7.png)

8. Clique no botão Copiar para copiar a cadeia de ligação **RootManageSharedAccessKey** para a área de transferência. Guarde esta cadeia de ligação numa localização temporária, como o Bloco de Notas, para utilizar mais tarde.
    
    ![](./media/event-hubs-create/create-event-hub8.png)

## <a name="create-an-event-hub"></a>Criar um hub de eventos

1. Na lista de espaços de nomes dos Hubs de Eventos, clique no espaço de nome criado recentemente.      
   
    ![](./media/event-hubs-create/create-event-hub2.png) 

2. No painel de espaço de nomes, clique em **Hubs de Eventos**.
   
    ![](./media/event-hubs-create/create-event-hub3.png)

3. Na parte superior do painel, clique em **+ Hub de eventos**.
   
    ![](./media/event-hubs-create/create-event-hub4.png)
4. Escreva um nome para o seu hub de eventos e, em seguida, clique em **Criar**. 

O hub de eventos é agora criado e tem as cadeias de ligação que tem de enviar e receber eventos.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre os Hubs de eventos, visite estas ligações:

* [Descrição geral dos Hubs de Eventos](event-hubs-what-is-event-hubs.md)
* [Descrição geral da API dos Hubs de Eventos](event-hubs-api-overview.md)

[Azure portal]: https://portal.azure.com/