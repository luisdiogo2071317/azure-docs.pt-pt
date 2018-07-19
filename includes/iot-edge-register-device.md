---
title: incluir ficheiro
description: incluir ficheiro
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 06/25/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: bacafdc8f7fd8e206335f3be0a086df1c54f1081
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38746714"
---
Crie uma identidade de dispositivo para o seu dispositivo simulado para que este consiga comunicar com o seu hub IoT. Uma vez que os dispositivos do IoT Edge se comportam e podem ser geridos de forma diferente dos dispositivos do IoT típicos, declare esta opção como um dispositivo do IoT Edge desde o início. 

1. No portal do Azure, navegue para o seu hub IoT.
1. Selecione **IoT Edge** e, em seguida, selecione **Adicionar Dispositivo do IoT Edge**.

   ![Adicionar Dispositivo do IoT Edge](./media/iot-edge-register-device/add-device.png)

1. Dê um ID de dispositivo único ao seu dispositivo simulado.
1. Selecione **Guardar** para adicionar o seu dispositivo.
1. Selecione o seu novo dispositivo na lista de dispositivos.
1. Copie o valor para **Cadeia de ligação – chave primária** e guarde-o. Irá utilizar este valor para configurar o runtime do IoT Edge na secção seguinte. 

