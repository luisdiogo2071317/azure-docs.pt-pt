---
title: Atualizar o Hub IoT do Azure | Microsoft Docs
description: Altere o escalão de preço e escala para o IoT Hub obter mais capacidades de gestão de dispositivos e de mensagens.
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
ms.assetid: ''
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2018
ms.author: kgremban
ms.openlocfilehash: d383d26b406c012b6b76225faf89f4b5dbd6bb9c
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="how-to-upgrade-your-iot-hub"></a>Como atualizar o seu IoT hub

À medida que aumenta a sua solução de IoT, o IoT Hub do Azure está pronto para o ajudar a aumentar verticalmente. IoT Hub do Azure oferece duas camadas, basic (B) e standard (S) para acomodar os clientes que pretendem utilizam várias funcionalidades. Em cada camada está três tamanhos (1, 2 e 3) que determinam o número de mensagens que podem ser enviados por dia. 

Quando tiver mais dispositivos e precisar de mais capacidades, existem três formas para ajustar o seu IoT hub para se adequarem às suas necessidades:

* Adicione unidades dentro do IoT hub. Por exemplo, cada unidade adicional num hub B1 IoT permite um mensagens de um máximo de 400.000 adicionais por dia. 
* Altere o tamanho do IoT hub. Por exemplo, migre a partir da camada B1 à camada B2 para aumentar a quantidade de mensagens em fila que consiga suportar cada unidade por dia.
* Atualizar para um escalão superior. Por exemplo, atualize a partir da camada B1 para o escalão S1 para a mesma capacidade de mensagens, mas com as funcionalidades avançadas que vêm no escalão standard.

Estas alterações podem ocorrer sem interromper as operações existentes.

Se pretender mudar o seu IoT hub, pode remover unidades e reduzir o tamanho do IoT hub. No entanto, não é possível mudar para um escalão inferior. Por exemplo, pode mover a partir da camada de S2 para o escalão S1, mas não a partir da camada S2 à camada B1. 

Estes exemplos destinam-se para o ajudar a compreender como ajustar o seu IoT hub como as alterações de solução. Para obter informações específicas sobre as capacidades de cada camada sempre devem consultar a [preços do IoT Hub do Azure](https://azure.microsoft.com/pricing/details/iot-hub/). 

## <a name="upgrade-your-existing-iot-hub"></a>Atualizar o seu IoT hub existente 

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com/) e navegue até ao seu IoT hub. 
2. Selecione **preços e dimensionamento**. 

   ![Preço e escala](./media/iot-hub-upgrade/pricing-scale.png)

3. Para alterar a camada para o seu hub, selecione **escalão de preço e escala**. Escolher o escalão de nova, em seguida, clique em **selecione**.

   ![Preço e escala](./media/iot-hub-upgrade/select-tier.png)

4. Para alterar o número de unidades do seu hub, introduza um novo valor em **unidades do IoT Hub**. 
5. Selecione **guardar** para guardar as alterações. 

Agora é ajustado o seu IoT hub e as configurações são iguais. 

## <a name="next-steps"></a>Passos Seguintes

Obter mais detalhes [como escolher o escalão de IoT Hub direita](iot-hub-scaling.md). 

