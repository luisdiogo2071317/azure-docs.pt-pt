---
title: incluir ficheiro
description: incluir ficheiro
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 10/16/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: e499a0f7bec47e672c599c729a15cc3e3d04a28a
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471665"
---
## <a name="create-a-storage-account-for-event-processor-host"></a>Criar uma conta de armazenamento para o Anfitrião do Processador de Eventos
O Anfitrião do Processador de Eventos é um agente inteligente que simplifica a receção de eventos provenientes dos Hubs de Eventos ao gerir pontos de verificação persistentes e receções em paralelo. Para o ponto de verificação, o Anfitrião do Processador de Eventos requer uma conta de armazenamento. O exemplo seguinte mostra como criar uma conta de armazenamento e como obter as chaves de acesso:

1. No portal do Azure e selecione **criar um recurso** na parte superior esquerda do ecrã.

2. Selecione **Armazenamento** e, em seguida, **Conta de Armazenamento – blob, ficheiro, tabela, fila**.
   
    ![Selecionar Conta de Armazenamento](./media/event-hubs-create-storage/create-storage1.png)

3. Sobre o **criar conta de armazenamento** página, siga os passos seguintes: 

    1. Introduza um nome para a conta de armazenamento. 
    2. Escolha uma subscrição do Azure que contém o hub de eventos.
    3. Selecione o grupo de recursos com o hub de eventos.
    4. Selecione uma localização na qual pretende criar o recurso. 
    5. Em seguida, clique em **rever + criar**.
   
    ![Criar conta de armazenamento - página](./media/event-hubs-create-storage/create-storage2.png)

4. Sobre o **rever + criar** página, reveja os valores e selecione **criar**. 

    ![Reveja as definições de conta de armazenamento e criar](./media/event-hubs-create-storage/review-create-storage-account.png)
5. Depois de confirmar a **implementações bem-sucedidas** mensagem, selecione **tem de recurso** na parte superior da página. Também pode iniciar a página de conta de armazenamento ao selecionar a sua conta de armazenamento na lista de recursos.  

    ![Selecione a conta de armazenamento da implementação](./media/event-hubs-create-storage/select-storage-deployment.png) 
7. Na **Essentials** janela, selecione **Blobs**. 

    ![Selecione o serviço de Blobs](./media/event-hubs-create-storage/select-blobs-service.png)
1. Selecione **+ contentor** na parte superior, introduza um **nome** para o contentor e selecione **OK**. 

    ![Criar um contentor de blobs](./media/event-hubs-create-storage/create-blob-container.png)
1. Selecione **chaves de acesso** no menu à esquerda e copie o valor de **chave1**. 

    Guarde os seguintes valores no bloco de notas ou noutra localização temporária.
    - Nome da conta de armazenamento
    - Chave de acesso para a conta de armazenamento
    - Nome do contentor