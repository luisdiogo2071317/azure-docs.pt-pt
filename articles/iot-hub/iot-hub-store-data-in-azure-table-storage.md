---
title: Salvar suas mensagens do hub IoT no armazenamento de dados do Azure | Documentos da Microsoft
description: Utilize o encaminhamento de mensagens do IoT Hub para poupar suas mensagens do hub IoT para o armazenamento de Blobs do Azure. As mensagens do hub IoT contenham informações, tais como dados de sensores, que são enviados a partir do seu dispositivo IoT.
author: rangv
manager: ''
keywords: armazenamento de dados de IOT, armazenamento de dados de sensores de iot
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 84355453a5cb8d8f42abdcbde5432651c9c035b0
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856295"
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-to-your-azure-blob-storage"></a>Guardar mensagens do hub IoT que contêm dados de sensores no armazenamento de Blobs do Azure

![Diagrama de ponto a ponto](./media/iot-hub-store-data-in-azure-table-storage/1_route-to-storage.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>O que irá aprender

Saiba como criar uma conta de armazenamento do Azure e uma aplicação de função do Azure para armazenar mensagens do hub IoT no armazenamento de Blobs do Azure.

## <a name="what-you-do"></a>O que fazer

- Crie uma conta do Storage do Azure.
- Configure o seu hub IoT para rotear mensagens para o armazenamento.

## <a name="what-you-need"></a>Do que precisa

- [Configurar o dispositivo](iot-hub-raspberry-pi-kit-node-get-started.md) para abranger os seguintes requisitos:
  - Uma subscrição do Azure Active Directory
  - Um hub IoT com a sua subscrição 
  - Um aplicativo em execução que envia mensagens para o seu hub IoT

## <a name="create-an-azure-storage-account"></a>Criar uma conta do Storage do Azure

1. Na [portal do Azure](https://portal.azure.com/), clique em **criar um recurso** > **armazenamento** > **conta de armazenamento**.

2. Introduza as informações necessárias para a conta de armazenamento:

   ![Criar uma conta de armazenamento no portal do Azure](./media/iot-hub-store-data-in-azure-table-storage/1_azure-portal-create-storage-account.png)

   * **Nome**: O nome da conta de armazenamento. O nome tem de ser globalmente exclusivo.

   * **Tipo de conta**: escolha `Storage (general purpose v1)`.

   * **Localização**: Escolha a mesma localização que utiliza o seu hub IoT.

   * **A replicação**: escolha `Locally-redundant storage (LRS)`.

   * **Desempenho**: escolha `Standard`.

   * **Transferência segura necessária**: escolha `Disabled`.

   * **Subscrição**: selecione a sua subscrição do Azure.

   * **Grupo de recursos**: utilizar o mesmo grupo de recursos que utiliza o seu hub IoT.

   * **Afixar ao dashboard**: selecione esta opção para facilitar o acesso ao hub IoT a partir do dashboard.

3. Clique em **Criar**.

## <a name="prepare-your-iot-hub-to-route-messages-to-storage"></a>Preparar o seu hub IoT para rotear mensagens para o armazenamento

IoT Hub suporta nativamente encaminhar mensagens para o armazenamento do Azure, como blobs. Para saber mais sobre os pontos de extremidade personalizados do IoT Hub do Azure, pode consultar [lista de pontos finais do IoT Hub incorporados](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-endpoints#custom-endpoints).

### <a name="add-storage-as-a-custom-endpoint"></a>Adicionar armazenamento como um ponto final personalizado

1. Navegue até ao seu hub IoT no portal do Azure. 

2. Clique em **pontos de extremidade** > **adicionar**. 

3. O ponto final de nome e selecione **contentor de armazenamento do Azure** como o tipo de ponto final. 

4. Utilize o seletor para selecionar a conta de armazenamento que criou na secção anterior. Criar um contentor de armazenamento e selecione-o, em seguida, clique em **OK**.

   ![Criar um ponto final personalizado no IoT Hub](./media/iot-hub-store-data-in-azure-table-storage/2_custom-storage-endpoint.png)

### <a name="add-a-route-to-route-data-to-storage"></a>Adicionar um rota para encaminhar os dados ao armazenamento

1. Clique em **rotas** > **Add** e introduza um nome para a rota. 

2. Selecione **mensagens de dispositivo** como a origem de dados e selecione o ponto final de armazenamento que acabou de criar como o ponto final na rota. 

3. Introduza `true` como a cadeia de consulta, em seguida, clique em **guardar**.

   ![Criar uma rota no IoT Hub](./media/iot-hub-store-data-in-azure-table-storage/3_create-route.png)
  
### <a name="add-a-route-for-hot-path-telemetry-optional"></a>Adicione uma rota para a telemetria de caminho de acesso frequente (opcional)

Por predefinição, o IoT Hub encaminha todas as mensagens que não correspondam a quaisquer outras rotas para o ponto final incorporado. Uma vez que todas as mensagens de telemetria agora corresponderem à regra que encaminha as mensagens para o armazenamento, terá de adicionar outra rota para mensagens a ser escrito para o ponto final incorporado. Não existe nenhum custo adicional para rotear mensagens para vários pontos de extremidade.

> [!NOTE]
> Pode ignorar este passo se não estiver a fazer processamento adicional na suas mensagens de telemetria.

1. Clique em **adicionar** partir do painel de rotas e introduza um nome para a rota. 

2. Selecione **mensagens do dispositivo** como a origem de dados e **eventos** como o ponto final. 

3. Introduza `true` como a cadeia de consulta, em seguida, clique em **guardar**.

  ![Criar uma rota de caminhos recorrentes no IoT Hub](./media/iot-hub-store-data-in-azure-table-storage/4_hot-path-route.png)

## <a name="verify-your-message-in-your-storage-container"></a>Certifique-se a sua mensagem no seu contentor de armazenamento

1. Execute o aplicativo de exemplo no seu dispositivo para enviar mensagens ao seu hub IoT.

2. [Baixe e instale o Explorador de armazenamento do Azure](http://storageexplorer.com/).

3. Abra o Explorador de armazenamento, clique em **adicionar uma conta do Azure** > **entrar**e, em seguida, inicie sessão na sua conta do Azure.

4. Clique na sua subscrição do Azure > **contas de armazenamento** > sua conta de armazenamento > **contentores de BLOBs** > seu contentor.

   Deverá ver as mensagens enviadas a partir do seu dispositivo ao seu hub IoT com sessão iniciada no contentor de Blobs.

## <a name="clean-up-resources"></a>Limpar recursos 

Neste tutorial, adicionado uma conta de armazenamento e, em seguida, adicionou o encaminhamento para mensagens do IoT Hub ser escrito para a conta de armazenamento. Para limpar os recursos que criou, remove as informações de encaminhamento e, em seguida, eliminar a conta de armazenamento. 

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. Clique em **grupos de recursos** e selecione o grupo de recursos que utilizou. É apresentada a lista de recursos no grupo. 

   > [!NOTE]
   > Se pretender remover todos os recursos no grupo de recursos, clique em **eliminar** para eliminar o grupo de recursos, em seguida, siga as instruções. Esta ação irá remover tudo nesse grupo de recursos, para que terminou de limpar os recursos e pode avançar para a secção seguinte.

3. Clique no hub IoT que utilizou para este tutorial. 

4. No painel do IoT Hub, clique em **rotas**. Clique na caixa de verificação junto a regra de encaminhamento adicionado, em seguida, clique em **eliminar**. Quando lhe for perguntado se tiver a certeza de que pretende eliminar esse caminho, clique em **Sim**.

   ![Remover a regra de encaminhamento](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-routing.png)

   Feche o painel de encaminhamento. É reencaminhado para o painel do grupo de recursos.

5. Clique novamente no IoT hub. 

6. No painel do IoT Hub, clique em **pontos de extremidade**. Clique na caixa de verificação junto ao ponto de extremidade adicionou para o contentor de armazenamento, em seguida, clique em **eliminar**. Quando lhe for perguntado se tiver a certeza de que pretende eliminar o ponto de extremidade selecionado, clique em **Sim**.

    ![Remover ponto final](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-endpoint.png)

    Feche o painel de pontos de extremidade. É reencaminhado para o painel do grupo de recursos. 

7.  Clique na conta de armazenamento que configurou para este tutorial. 

8.  No painel de conta de armazenamento, clique em **eliminar** para remover a conta de armazenamento. É direcionado para o **eliminar conta de armazenamento** painel.

   ![Remover conta de armazenamento](./media/iot-hub-store-data-in-azure-table-storage/cleanup-remove-storageaccount.png)

8.  Escreva o nome de conta de armazenamento, em seguida, clique em **eliminar** na parte inferior do painel. 

## <a name="next-steps"></a>Passos Seguintes

Conseguiu criar a conta de armazenamento do Azure e roteadas mensagens do IoT Hub a um contentor de BLOBs nessa conta de armazenamento.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
