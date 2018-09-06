---
title: incluir ficheiro
description: incluir ficheiro
services: service-bus-messaging
author: spelluru
ms.service: service-bus-messaging
ms.topic: include
ms.date: 06/29/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 7ed298fc8f13685c4872c4c54ba1e447debea79f
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43702641"
---
Certifique-se que já criou um espaço de nomes do Service Bus, como mostrado [aqui][namespace-how-to].

1. Inicie sessão no [Portal do Azure][azure-portal].
2. No painel de navegação à esquerda do portal, clique em **Service Bus** (se não vir **Service Bus**, clique em **Todos os serviços**).
3. Clique no espaço de nomes no qual gostaria de criar a fila. Neste caso, é **sbnstest1**.
   
    ![Criar uma fila][createqueue1]
4. Na janela do espaço de nomes, clique em **Filas** e, em seguida, na janela **Filas**, clique em **+ Fila**.
   
    ![Selecionar Filas][createqueue2]
5. Introduza o **Nome** da fila e deixe os outros valores com as respetivas predefinições.
   
    ![Selecionar Novo][createqueue3]
6. Na parte inferior da janela, clique em **Criar**.

[createqueue1]: ./media/service-bus-create-queue-portal/create-queue1.png
[createqueue2]: ./media/service-bus-create-queue-portal/create-queue2.png
[createqueue3]: ./media/service-bus-create-queue-portal/create-queue3.png

[namespace-how-to]: ../articles/service-bus-messaging/service-bus-create-namespace-portal.md
[azure-portal]: https://portal.azure.com
