---
title: Guardar as mensagens do hub IoT para o armazenamento de dados do Azure | Microsoft Docs
description: Utilize o encaminhamento de mensagens do IoT Hub para guardar as mensagens do hub IoT para o armazenamento de Blobs do Azure. As mensagens do hub IoT contêm informações, tais como os dados de sensor, que são enviados a partir do dispositivo IoT.
author: rangv
manager: ''
keywords: armazenamento de dados do IOT, armazenamento de dados de sensor iot
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 678c538a5d672826f74235d4ac415fccf5de13fe
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34635683"
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-to-your-azure-blob-storage"></a>Guardar IoT hub mensagens que contêm dados de sensor para o armazenamento de Blobs do Azure

![Diagrama de ponto a ponto](media/iot-hub-store-data-in-azure-table-storage/1_route-to-storage.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>O que irá aprender

Saiba como criar uma conta de armazenamento do Azure e uma aplicação de função do Azure para armazenar mensagens de hub IoT no seu armazenamento de Blobs.

## <a name="what-you-do"></a>O que fazer

- Crie uma conta do Storage do Azure.
- Prepare o seu IoT hub para encaminhar mensagens para o armazenamento.

## <a name="what-you-need"></a>Do que precisa

- [Configurar o seu dispositivo](iot-hub-raspberry-pi-kit-node-get-started.md) abranger os seguintes requisitos:
  - Uma subscrição do Azure Active Directory
  - Um IoT hub na sua subscrição 
  - Uma aplicação em execução que envia mensagens ao seu IoT hub

## <a name="create-an-azure-storage-account"></a>Criar uma conta do Storage do Azure

1. No [portal do Azure](https://portal.azure.com/), clique em **crie um recurso** > **armazenamento** > **conta de armazenamento**  >  **Criar**.

2. Introduza as informações necessárias para a conta de armazenamento:

   ![Criar uma conta do storage no portal do Azure](media\iot-hub-store-data-in-azure-table-storage\1_azure-portal-create-storage-account.png)

   * **Nome**: O nome da conta de armazenamento. O nome tem de ser globalmente exclusivo.

   * **Grupo de recursos**: utilizar o mesmo grupo de recursos que utiliza o seu IoT hub.

   * **Afixar ao dashboard**: selecione esta opção para facilitar o acesso ao hub IoT a partir do dashboard.

3. Clique em **Criar**.

## <a name="prepare-your-iot-hub-to-route-messages-to-storage"></a>Preparar o seu IoT hub para encaminhar mensagens para o armazenamento

IoT Hub suporta nativamente o encaminhamento de mensagens para o armazenamento do Azure como blobs. Para saber mais sobre os pontos finais personalizados de IoT Hub do Azure, pode consultar [lista de pontos finais de IoT Hub incorporadas](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-endpoints#custom-endpoints).

### <a name="add-storage-as-a-custom-endpoint"></a>Adicionar armazenamento como um ponto de final personalizado

Navegue até ao seu IoT hub no portal do Azure. Clique em **pontos finais** > **adicionar**. O ponto final de nome e selecione **contentor de armazenamento do Azure** como o tipo de ponto final. Utilize o seletor para selecionar a conta de armazenamento que criou na secção anterior. Criar um contentor de armazenamento e selecione-o, em seguida, clique em **OK**.

  ![Criar um ponto de final personalizado no IoT Hub](media\iot-hub-store-data-in-azure-table-storage\2_custom-storage-endpoint.png)

### <a name="add-a-route-to-route-data-to-storage"></a>Adicione uma rota para rota de dados para o armazenamento

Clique em **rotas** > **adicionar** e introduza um nome para a rota. Selecione **mensagens do dispositivo** como a origem de dados e selecione o ponto final de armazenamento que acabou de criar como o ponto final da rota. Introduza `true` como a cadeia de consulta, em seguida, clique em **guardar**.

  ![Criar uma rota no IoT Hub](media\iot-hub-store-data-in-azure-table-storage\3_create-route.png)
  
### <a name="add-a-route-for-hot-path-telemetry-optional"></a>Adicione uma rota de telemetria de caminho de acesso frequente (opcional)

Por predefinição, o IoT Hub encaminha todas as mensagens que não correspondam a qualquer outras rotas para o ponto final incorporado. Uma vez que todas as mensagens de telemetria agora corresponderem a regra que encaminha as mensagens para o armazenamento, terá de adicionar outra rotas para mensagens de escrita para o ponto final incorporado. Não há sem encargos adicionais para encaminhar mensagens para vários pontos finais.

> [!NOTE]
> Pode ignorar este passo se não está a fazer adicionais de processamento de mensagens de telemetria.

Clique em **adicionar** partir do painel de rotas e introduza um nome para a rota. Selecione **mensagens do dispositivo** como origem de dados e **eventos** como o ponto final. Introduza `true` como a cadeia de consulta, em seguida, clique em **guardar**.

  ![Criar uma rota-path no IoT Hub](media\iot-hub-store-data-in-azure-table-storage\4_hot-path-route.png)

## <a name="verify-your-message-in-your-storage-container"></a>Certifique-se a sua mensagem no contentor de armazenamento

1. Execute a aplicação de exemplo no seu dispositivo para enviar mensagens para o seu IoT hub.

2. [Transfira e instale o Explorador de armazenamento do Azure](http://storageexplorer.com/).

3. Abra o Explorador de armazenamento, clique em **adicionar uma conta do Azure** > **sessão**e, em seguida, inicie sessão na sua conta do Azure.

4. Clique em sua subscrição do Azure > **contas do Storage** > a conta do storage > **contentores de BLOBs** > o contentor.

   Deverá ver as mensagens enviadas a partir do seu dispositivo ao seu IoT hub com sessão iniciada no contentor do blob.

## <a name="next-steps"></a>Passos Seguintes

Com êxito criou a conta de armazenamento do Azure e encaminhadas mensagens do IoT Hub para um contentor de blob nessa conta de armazenamento.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
