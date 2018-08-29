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
ms.openlocfilehash: 08afdcf91499fdb6f2da6e9ccc82313286f5c964
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43111215"
---
## <a name="create-an-iot-hub"></a>Criar um hub IoT
Crie um hub IoT para ligação da aplicação do dispositivo simulado. Os passos que se seguem mostram como concluir esta tarefa com o Portal do Azure.

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).

2. Selecione **Criar um recurso** > **Internet das Coisas** > **Hub IoT**.
   
    ![Barra de atalhos do portal do Azure](./media/iot-hub-get-started-create-hub/create-iot-hub1.png)

3. No painel **Hub IoT**, introduza as seguintes informações para o hub IoT:

   * **Subscrição**: escolha a subscrição que quer utilizar para criar este hub IoT.

   * **Grupo de recursos**: crie um grupo de recursos para utilizar o hub IoT ou utilize um já existente. Para obter mais informações, veja [Utilizar grupos de recursos para gerir os recursos do Azure](../articles/azure-resource-manager/resource-group-portal.md).

   * **Região**: selecione a localização mais próxima de si.

   * **Nome**: crie um nome para o seu hub IoT. Se o nome introduzido estiver disponível, aparece uma marca de verificação verde.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   ![Janela de informações básicas do Hub IoT](./media/iot-hub-get-started-create-hub/create-iot-hub2.png)

4. Selecione **Seguinte: tamanho e dimensionamento** para continuar a criar o seu hub IoT. 

5. Selecione o seu **Escalão de preço e dimensionamento**. Neste artigo, selecione o escalão **F1 - Gratuito** se ainda estiver disponível na sua subscrição. Para obter mais informações, veja o [escalão de preços e dimensionamento](https://azure.microsoft.com/pricing/details/iot-hub/).

   ![Janela de tamanho e dimensionamento do Hub IoT](./media/iot-hub-get-started-create-hub/create-iot-hub3.png)

6. Selecione **Rever + criar**.

7. Reveja as informações do hub IoT e, em seguida, clique em **Criar**. A criação do hub IoT pode demorar alguns minutos. Pode monitorizar o progresso no painel **Notificações**.

8. Quando o seu novo hub IoT estiver pronto, clique no respetivo mosaico no portal do Azure para abrir a janela de propriedades. Agora que criou um hub IoT, localize as informações importantes que utiliza para ligar dispositivos e aplicações ao seu hub IoT. Clique em **Políticas de acesso partilhado**.
   
9. Em **Políticas de acesso partilhado**, selecione a política **iothubowner**. Copie a **Cadeia de ligação---chave primária** do Hub IoT para utilizar mais tarde. Para obter mais informações, veja [Controlo de acesso](../articles/iot-hub/iot-hub-devguide-security.md) no “Guia do programador do Hub IoT.”
   
    ![Políticas de acesso partilhado](./media/iot-hub-get-started-create-hub/create-iot-hub5.png)