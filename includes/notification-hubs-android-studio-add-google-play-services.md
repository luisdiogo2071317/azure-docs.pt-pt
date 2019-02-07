---
title: incluir ficheiro
description: incluir ficheiro
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 02/05/2019
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: bc920493b32d500602a5b683c098d23aff855150
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55823301"
---
1. Na **Android Studio**, selecione **ferramentas** no menu e selecione **SDK Manager**. 
2. Selecione a versão de destino do Android SDK utilizada no seu projeto e selecione **Mostrar detalhes do pacote**. 

    ![Android SDK Manager – versão de destino selecione](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Selecione **APIs do Google**, se ainda não estiver instalado.

    ![Android SDK Manager - selecionado de APIs do Google](./media/notification-hubs-android-studio-add-google-play-services/googole-apis-selected.png)
4. Mude para o **ferramentas do SDK** separador. Se ainda não tiver instalado o serviço do Google Play, selecione **serviços do Google Play** conforme mostrado na imagem seguinte. Em seguida, clique em **Aplicar** para instalar. Anote o caminho do SDK, para utilização num passo posterior.

    ![Android SDK Manager – serviços do Google Play selecionado](./media/notification-hubs-android-studio-add-google-play-services/google-play-services-selected.png)
3. Se vir a **Confirmar alteração** caixa de diálogo, selecione **OK**. O componente de instalador instala os componentes solicitados. Selecione **concluir** depois dos componentes são instalados.
4. Selecione **OK** para fechar a **definições para novos projetos** caixa de diálogo.  
5. Abra o `build.gradle` de ficheiros a **aplicação** directory e adicione esta linha sob `dependencies`. 

    ```text
    implementation 'com.google.android.gms:play-services-gcm:16.0.0'
    ```
5. Selecione **sincronizar agora** ícone na barra de ferramentas.

    ![Sincronização com o Gradle](./media/notification-hubs-android-studio-add-google-play-services/gradle-sync.png)
1. Abra **AndroidManifest.xml** e adicione esta etiqueta à etiqueta *aplicação*.

    ```xml
    <meta-data android:name="com.google.android.gms.version"
         android:value="@integer/google_play_services_version" />
    ```
