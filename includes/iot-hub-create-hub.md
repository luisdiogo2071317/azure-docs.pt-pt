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
ms.openlocfilehash: 9a29406b92f7d2e2ce8171974efb5a264e112d1d
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34371155"
---
1. Inicie sessão no [Portal do Azure][lnk-portal].
1. Selecione **Criar um recurso** > **Internet das Coisas** > **Hub IoT**.
   
    ![Navegação do portal de captura de ecrã do Azure ao IoT Hub][1]

1. No painel **Hub IoT**, introduza as seguintes informações para o hub IoT:

   * **Subscrição**: selecione a subscrição que pretende utilizar para criar este hub IoT.

   * **Grupo de recursos**: crie um grupo de recursos para utilizar o hub IoT ou utilize um já existente. Para obter mais informações, consulte [utilizar grupos de recursos para gerir os recursos do Azure][lnk-resource-groups].

   * **Região**: selecione a localização mais próxima de si.

   * **Nome**: crie um nome para o seu hub IoT. Se o nome introduzido está disponível, aparece uma marca de verificação verde.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![Janela de noções básicas de IoT Hub][2]

2. Selecione **seguinte: tamanho e a escala** para continuar a criar o seu IoT hub. 

3. Escolha o **escalão de preço e escala**. Para este artigo, selecione o **F1 - livre** camada se, ainda está disponível na sua subscrição. Para obter mais informações, veja [Escalão de preços e dimensionamento][lnk-pricing].

   ![Janela de tamanho e a escala do IoT Hub][3]

4. Selecione **revisão + criar**.

1. Reveja as informações do hub IoT, em seguida, clique em **criar**. A criação do hub IoT pode demorar alguns minutos. Pode monitorizar o progresso no painel **Notificações**.
<!-- Images -->
[1]: ./media/iot-hub-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-create-hub/create-iot-hub3.png
<!-- Links -->
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md