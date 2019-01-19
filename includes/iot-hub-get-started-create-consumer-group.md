---
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.author: dobett
ms.openlocfilehash: 2d041ccbcf85f1931ca8ebc8b17595c9221c03cc
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2019
ms.locfileid: "54414600"
---
## <a name="add-a-consumer-group-to-your-iot-hub"></a>Adicionar um grupo de consumidores ao seu hub IoT

Grupos de consumidores são utilizados por aplicações para obter dados do IoT Hub do Azure. Neste tutorial, vai criar um grupo de consumidores a ser utilizado por um serviço do Azure próximo para ler dados a partir do seu hub IoT.

Para adicionar um grupo de consumidores ao seu hub IoT, siga estes passos:

1. No [portal do Azure](https://portal.azure.com/), abra o hub IoT.

2. No painel esquerdo, clique em **pontos finais incorporados**, selecione **eventos** no painel superior e introduza um nome sob **grupos de consumidores** no painel direito. Clique em **salvar** depois de alterar a **TTL predefinido** valor e retorná-lo novamente para o valor original.

   ![Criar um grupo de consumidores do seu hub IoT](./media/iot-hub-get-started-create-consumer-group/iot-hub-create-consumer-group-azure.png)
