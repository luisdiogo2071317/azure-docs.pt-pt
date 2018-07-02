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
ms.openlocfilehash: d586ca18953b12045fbbaa4a656d78a7192eb88e
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "34371240"
---
## <a name="create-an-iot-hub"></a>Criar um hub IoT
Crie um hub IoT para ligação da aplicação do dispositivo simulado. Os passos que se seguem mostram como concluir esta tarefa com o Portal do Azure.

1. Inicie sessão no [Portal do Azure][lnk-portal].

1. Selecione **Criar um recurso** > **Internet das Coisas** > **Hub IoT**.
   
    ![Barra de atalhos do portal do Azure][1]

1. No painel **Hub IoT**, introduza as seguintes informações para o hub IoT:

   * **Subscrição**: escolha a subscrição que quer utilizar para criar este hub IoT.

   * **Grupo de recursos**: crie um grupo de recursos para utilizar o hub IoT ou utilize um já existente. Para obter mais informações, veja [Utilizar grupos de recursos para gerir os recursos do Azure][lnk-resource-groups].

   * **Região**: selecione a localização mais próxima de si.

   * **Nome**: crie um nome para o seu hub IoT. Se o nome introduzido estiver disponível, aparece uma marca de verificação verde.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![Janela de informações básicas do Hub IoT][2]

2. Selecione **Seguinte: tamanho e dimensionamento** para continuar a criar o seu hub IoT. 

3. Selecione o seu **Escalão de preço e dimensionamento**. Neste artigo, selecione o escalão **F1 - Gratuito** se ainda estiver disponível na sua subscrição. Para obter mais informações, veja [Escalão de preços e dimensionamento][lnk-pricing].

   ![Janela de tamanho e dimensionamento do Hub IoT][3]

4. Selecione **Rever + criar**.

1. Reveja as informações do hub IoT e, em seguida, clique em **Criar**. A criação do hub IoT pode demorar alguns minutos. Pode monitorizar o progresso no painel **Notificações**.

1. Quando o seu novo hub IoT estiver pronto, clique no respetivo mosaico no portal do Azure para abrir a janela de propriedades. Agora que criou um hub IoT, localize as informações importantes que utiliza para ligar dispositivos e aplicações ao seu hub IoT. Clique em **Políticas de acesso partilhado**.
   
1. Em **Políticas de acesso partilhado**, selecione a política **iothubowner**. Copie a **Cadeia de ligação---chave primária** do Hub IoT para utilizar mais tarde. Para obter mais informações, veja [Controlo de acesso][lnk-access-control] no “Guia do programador do Hub IoT.”
   
    ![Políticas de acesso partilhado][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
