---
title: incluir ficheiro
description: incluir ficheiro
services: service-bus-messaging
author: sethmanheim
ms.service: service-bus-messaging
ms.topic: include
ms.date: 06/29/2018
ms.author: sethm
ms.custom: include file
ms.openlocfilehash: bad7e87cb194fb7d8846e7e675881658a1d33b1e
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37138165"
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
