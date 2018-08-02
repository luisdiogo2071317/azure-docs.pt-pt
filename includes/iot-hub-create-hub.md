---
title: incluir ficheiro
description: incluir ficheiro
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 05/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: fce9e42e24be7f8678292a5d98a683ca4e579cd2
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39357123"
---
1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

2. Selecione **Criar um recurso** > **Internet das Coisas** > **Hub IoT**.
   
    ![Captura de ecrã da navegação do portal do Azure para o Hub IoT](./media/iot-hub-create-hub/create-iot-hub1.png)

3. No painel **Hub IoT**, introduza as seguintes informações para o hub IoT:

   * **Subscrição**: escolha a subscrição que quer utilizar para criar este hub IoT.

   * **Grupo de recursos**: crie um grupo de recursos para utilizar o hub IoT ou utilize um já existente. Para obter mais informações, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../articles/azure-resource-manager/resource-group-portal.md).

   * **Região**: selecione a localização mais próxima de si.

   * **Nome**: crie um nome para o seu hub IoT. Se o nome introduzido estiver disponível, aparece uma marca de verificação verde.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![Janela de informações básicas do Hub IoT](./media/iot-hub-create-hub/create-iot-hub2.png)

4. Selecione **Seguinte: tamanho e dimensionamento** para continuar a criar o seu hub IoT. 

5. Selecione o seu **Escalão de preço e dimensionamento**. Neste artigo, selecione o escalão **F1 - Gratuito** se ainda estiver disponível na sua subscrição. Para obter mais informações, veja o [escalão de preços e dimensionamento](https://azure.microsoft.com/pricing/details/iot-hub/).

   ![Janela de tamanho e dimensionamento do Hub IoT](./media/iot-hub-create-hub/create-iot-hub3.png)

6. Selecione **Rever + criar**.

7. Reveja as informações do hub IoT e, em seguida, clique em **Criar**. A criação do hub IoT pode demorar alguns minutos. Pode monitorizar o progresso no painel **Notificações**.