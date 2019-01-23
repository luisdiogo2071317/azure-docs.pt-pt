---
title: incluir ficheiro
description: incluir ficheiro
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 01/04/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: f00ca7ddf44a9d5b850cd47520970a0396a0c1b5
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54453101"
---
1. Abra o Gestor do Android SDK clicando no ícone na barra de ferramentas do Android Studio ou em **Ferramentas** > **Android** > **Gestor de SDKs** no menu. Localize a versão de destino do Android SDK utilizada no projeto, abra-a clicando em **Mostrar Detalhes do Pacote** e escolha **APIs do Google**, se ainda não estiver instalada.
2. Clique no separador **Ferramentas SDK** . Se ainda não instalou o Serviço do Google Play, clique em **Serviços do Google Play**, como mostrado abaixo. Em seguida, clique em **Aplicar** para instalar. Anote o caminho do SDK, para utilização num passo posterior.

    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Abra o `build.gradle` ficheiro no diretório do aplicativo.

    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-add-google-play-dependency.png)
4. Adicione esta linha sob `dependencies`:

    ```text
    compile 'com.google.android.gms:play-services-gcm:12.0.0'
    ```
5. Clique no ícone **Sincronizar Projeto com os Ficheiros Gradle** na barra de ferramentas.
6. Abra **AndroidManifest.xml** e adicione esta etiqueta à etiqueta *aplicação*.

    ```xml
    <meta-data android:name="com.google.android.gms.version"
         android:value="@integer/google_play_services_version" />
    ```
