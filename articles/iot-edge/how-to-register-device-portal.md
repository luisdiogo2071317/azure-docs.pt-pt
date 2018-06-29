---
title: Registar um novo dispositivo de limite de IoT do Azure (portal) | Microsoft Docs
description: Utilizar o portal do Azure para registar um dispositivo de limite de IoT novo
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/05/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: b61594469df33e11c23c9cbe0b9542da374fefa3
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036305"
---
# <a name="register-a-new-azure-iot-edge-device-from-the-azure-portal"></a>Registar um novo dispositivo de limite de IoT do Azure do portal do Azure

Antes de poder utilizar os seus dispositivos de IoT com limite de IoT do Azure, terá de registá-los no seu IoT hub. Depois de registar um dispositivo, receberá uma cadeia de ligação que pode ser utilizada para configurar o seu dispositivo para cargas de trabalho de limite. 

Este artigo mostra como registar um novo dispositivo de limite de IoT no portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

* Um [IoT hub](../iot-hub/iot-hub-create-through-portal.md) na sua subscrição do Azure. 

## <a name="create-a-device"></a>Criar um dispositivo

No portal do Azure, os dispositivos de limite de IoT são criados e geridos separadamente em dispositivos que se ligar ao seu IoT hub, mas não estão preparados para o limite. 

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) e navegue até ao seu IoT hub. 
2. Selecione **IoT Edge** no menu.
3. Selecione **adicionar dispositivo de limite de IoT**. 
4. Forneça um ID de dispositivo descritivo. 
5. Selecione **Guardar**. 

## <a name="view-all-devices"></a>Ver todos os dispositivos

São listados todos os edge preparados dispositivos que ligam ao seu IoT hub no **IoT Edge** página. 

## <a name="retrieve-the-connection-string"></a>Obter a cadeia de ligação

Quando estiver pronto para configurar o seu dispositivo, terá da cadeia de ligação que liga o dispositivo físico com a respetiva identidade do IoT hub.

1. Do **IoT Edge** página no portal, clique no ID de dispositivo da lista de dispositivos de limite. 
2. Copie o valor do **cadeia de ligação — chave primária** ou **cadeia de ligação — chave secundária**. 

## <a name="next-steps"></a>Passos Seguintes

Saiba como [implementar módulos num dispositivo com o portal do Azure](how-to-deploy-modules-portal.md)