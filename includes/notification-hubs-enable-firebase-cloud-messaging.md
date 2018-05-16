---
title: incluir ficheiro
description: incluir ficheiro
services: notification-hubs
author: spelluru
ms.service: notification-hubs
ms.topic: include
ms.date: 04/05/2018
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 4dc67175f2ccc569ab6dfe5338607fa6bebe3882
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
1. Inicie sessão na [consola Firebase](https://firebase.google.com/console/). Crie um novo projeto Firebase se ainda não tiver um.
2. Depois de criar o projeto, selecione **Adicionar Firebase à aplicação Android**. Em seguida, siga as instruções fornecidas. Transfira o ficheiro **google-services.json**. 

    ![Adicionar Firebase à aplicação Android](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Na consola da Firebase, selecione o cog do seu projeto. Em seguida, selecione **Definições do projeto**.

    ![Selecione Definições do projeto](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Selecione o separador **Geral** nas definições do projeto. Em seguida, transfira o ficheiro **google-services.json** que contém a chave de API do Servidor e o ID de Cliente.
5. Selecione o separador **Cloud Messaging** nas definições do projeto e, em seguida, copie o valor da **Chave do servidor do legado**. Utilize este valor para configurar a política de acesso do hub de notificação.
