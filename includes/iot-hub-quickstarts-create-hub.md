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
ms.openlocfilehash: ddecf2f75e80f95291b12d9c139a8da85f45ac94
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36288071"
---
O primeiro passo consiste em utilizar o portal do Azure para criar um hub IoT na sua subscrição. O Hub IoT permite-lhe ingerir elevados volumes de telemetria na cloud a partir de vários dispositivos. Depois, o hub ativa um ou mais serviços de back-end em execução na cloud, para ler e processar essa telemetria.

1. Inicie sessão no [portal do Azure](http://portal.azure.com).

1. Selecione **Criar um recurso** > **Internet das Coisas** > **Hub IoT**.

    ![Selecione para instalar o Hub IoT][1]

1. No painel **Hub IoT**, introduza as seguintes informações para o hub IoT:

   * **Subscrição**: escolha a subscrição que quer utilizar para criar este hub IoT.

   * **Grupo de recursos**: crie um grupo de recursos para utilizar o hub IoT ou utilize um já existente. Ao colocar todos os recursos relacionados num grupo, pode geri-los em conjunto. Por exemplo, eliminar o grupo de recursos elimina todos os recursos que se encontram nesse grupo. Para obter mais informações, veja [Utilizar grupos de recursos para gerir os recursos do Azure][lnk-resource-groups].

   * **Região**: selecione a localização mais próxima de si.

   * **Nome**: crie um nome para o seu hub IoT. Se o nome introduzido estiver disponível, aparece uma marca de verificação verde.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![Janela de informações básicas do Hub IoT][2]

2. Selecione **Seguinte: tamanho e dimensionamento** para continuar a criar o seu hub IoT. 

3. Selecione o seu **Escalão de preço e dimensionamento**. Neste artigo, selecione o escalão **F1 - Gratuito** se ainda estiver disponível na sua subscrição. Para obter mais informações, veja [Escalão de preços e dimensionamento][lnk-pricing].

   ![Janela de tamanho e dimensionamento do Hub IoT][3]

4. Selecione **Rever + criar**.

1. Reveja as informações do hub IoT e, em seguida, clique em **Criar**. A criação do hub IoT pode demorar alguns minutos. Pode monitorizar o progresso no painel **Notificações**.


<!-- Images -->
[1]: ./media/iot-hub-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-create-hub/create-iot-hub3.png
<!-- Links -->
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md