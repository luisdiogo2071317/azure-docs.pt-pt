---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 42c961b81a254adef5e42c3c8916c9c081f548c8
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42811588"
---
1. Na vista solução (ou **Explorador de soluções** no Visual Studio), com o botão direito a **componentes** pasta, clique em **obter mais componentes...** , procure o **cliente Google Cloud Messaging** componente e adicioná-lo ao projeto.
2. Abra o ficheiro de projeto ToDoActivity.cs e adicione o seguinte usando a instrução a classe:

    ```csharp
    using Gcm.Client;
    ```

3. Na **ToDoActivity** , adicione o seguinte código novo: 

    ```csharp
    // Create a new instance field for this activity.
    static ToDoActivity instance = new ToDoActivity();

    // Return the current activity instance.
    public static ToDoActivity CurrentActivity
    {
        get
        {
            return instance;
        }
    }
    // Return the Mobile Services client.
    public MobileServiceClient CurrentClient
    {
        get
        {
            return client;
        }
    }
    ```

    Isto permite-lhe aceder à instância de cliente móvel do processo de serviço do manipulador de push.
4. Adicione o seguinte código para o **OnCreate** método, após o **MobileServiceClient** é criado:

    ```csharp
    // Set the current instance of TodoActivity.
    instance = this;

    // Make sure the GCM client is set up correctly.
    GcmClient.CheckDevice(this);
    GcmClient.CheckManifest(this);

    // Register the app for push notifications.
    GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);
    ```

Sua **ToDoActivity** está agora preparado para adicionar notificações push.
