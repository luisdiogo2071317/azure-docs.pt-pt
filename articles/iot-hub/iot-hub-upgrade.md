---
title: Atualizar o Hub IoT do Azure | Documentos da Microsoft
description: Altere o escalão de preço e escala do IoT Hub para obter mais recursos de gerenciamento de mensagens e de dispositivo.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/02/2018
ms.author: kgremban
ms.openlocfilehash: e1342ed574d84ed5b4edd5060c2d6d3ec8bca1a8
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003116"
---
# <a name="how-to-upgrade-your-iot-hub"></a>Como atualizar o seu hub IoT

À medida que aumenta a sua solução de IoT, IoT Hub do Azure está pronto para o ajudar a aumentar verticalmente. O IoT Hub do Azure disponibiliza duas camadas, básico (B) e standard (S), para acomodar os clientes que pretendem utilizam as funcionalidades diferentes. Em cada escalão são três tamanhos (1, 2 e 3) que determina o número de mensagens que podem ser enviados por dia. 

Quando tiver mais dispositivos e precisar de mais recursos, existem três formas para ajustar o seu hub IoT para satisfazer as suas necessidades:

* Adicione unidades no IoT hub. Por exemplo, cada unidade adicional de um hub B1 IoT permite um adicionais 400 000 mensagens por dia. 
* Altere o tamanho do IoT hub. Por exemplo, migre da camada B1 para o escalão de B2 para aumentar a quantidade de mensagens que pode oferecer suporte a cada unidade por dia.
* Atualizar para um escalão mais elevado. Por exemplo, atualize a partir da camada de B1 para o escalão S1 para a mesma capacidade de mensagens, mas com os recursos avançados que são fornecidos no escalão standard.

Estas alterações podem ocorrer sem interromper operações existentes.

Se quiser mudar o seu hub IoT, pode remover unidades e reduzir o tamanho do IoT hub. No entanto, não pode mudar para um escalão mais baixo. Por exemplo, pode mover do escalão S2 para o escalão S1, mas não a partir da camada de S2 para a camada B1. Tenha também em atenção que apenas um tipo de [edition](https://azure.microsoft.com/pricing/details/iot-hub/) dentro de uma camada pode ser selecionado por IoT Hub. Por exemplo, pode criar um IoT Hub com múltiplas unidades de S1, mas não com uma mistura de unidades de edições diferentes, por exemplo, S1 e B3, ou S1 e S2.

Esses exemplos destinam-se para o ajudar a compreender como ajustar o seu hub IoT como as alterações de solução. Para obter informações específicas sobre as capacidades de cada camada deve sempre consultar [preços do IoT Hub do Azure](https://azure.microsoft.com/pricing/details/iot-hub/). 

## <a name="upgrade-your-existing-iot-hub"></a>Atualizar o seu hub IoT existente 

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) e navegue até ao seu hub IoT. 
2. Selecione **preços e dimensionamento**. 

   ![Preço e escala](./media/iot-hub-upgrade/pricing-scale.png)

3. Para alterar o escalão do hub, selecione **escalão de preços e dimensionamento**. Selecione o novo escalão, em seguida, clique em **selecione**.

   ![Preço e escala](./media/iot-hub-upgrade/select-tier.png)

4. Para alterar o número de unidades do seu hub, introduza um novo valor sob **unidades do IoT Hub**. 
5. Selecione **guardar** para guardar as alterações. 

Agora é ajustado o seu hub IoT e suas configurações permanecem inalteradas. Tenha em atenção que o limite máximo de partição para o IoT Hub de escalão básico é 8 e para o escalão standard é 32. A maioria dos Hubs de IoT precisa apenas de 4 partições. O limite de partição é escolhido ao IoT Hub é criado e se relaciona com as mensagens do dispositivo para a cloud para o número de leitores simultâneos dessas mensagens. Este valor permanece inalterado durante a migração de escalão básico para o escalão standard. 

## <a name="next-steps"></a>Passos Seguintes

Obter mais detalhes [como escolher o escalão certo do IoT Hub](iot-hub-scaling.md). 

