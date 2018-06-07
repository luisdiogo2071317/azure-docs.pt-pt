---
title: Soluções do Azure para a Internet das Coisas (IoT Edge) | Microsoft Docs
description: Descrição geral de uma amostra da arquitetura de solução IoT e como se relaciona com dispositivos, o serviço do Hub IoT do Azure, SDKs do dispositivo Azure IoT, SDKs de serviço do Azure IoT e outros serviços do Azure.
author: dominicbetts
manager: timlt
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 09/15/2017
ms.author: dobett
ms.openlocfilehash: bd59e740803f8f0e6f5f542805d615772efba913
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34630345"
---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Passos Seguintes

O Azure IoT Edge é um serviço do Azure que permite o processamento de análises e dados na periferia. Com o IoT Edge, pode capacitar os seus dispositivos com código baseado em contentores que inclui lógica extraída diretamente dos serviços do Azure que já utiliza ou com o código específico da sua própria solução. Permite que os dispositivos:

* Ajam como dispositivos gateway, agregando e processando os dados de vários dispositivos de folha.
* Executem deteção de anomalias e reajam a alterações no ambiente, sem terem de aguardar por instruções da cloud.
* Minimizem os custos de largura de banda e armazenamento, ao pré-processar os dados e enviar os resultados. 

O IoT Edge também inclui uma interface na cloud que possibilidade a gestão remota de dispositivos. Sem ter de aceder fisicamente aos seus dispositivos, pode implementar código, monitorizar o estado e atualizá-lo. Pode gerir remotamente dispositivos individuais ou criar implementações que afetam grandes conjuntos de dispositivos que definir. Para obter mais informações, veja [Understand IoT Edge deployments for single devices or at scale][lnk-deployment] (Compreender as implementações do IoT Edge para dispositivos individuais ou em escala).

Para saber mais sobre os componentes que permitem o IoT Edge, veja [How Azure IoT Edge work7][lnk-overview] (Como funciona o Azure IoT Edge).

Se nunca utilizou o Hub IoT do Azure, poderá querer começar por ver a [Descrição geral do serviço Hub IoT do Azure][lnk-iot-hub].

[lnk-deployment]: module-deployment-monitoring.md
[lnk-overview]: how-iot-edge-works.md
[lnk-iot-hub]: ../iot-hub/iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: ../iot-hub/iot-hub-device-management-overview.md
