---
title: incluir ficheiro
description: incluir ficheiro
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 08/20/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 892dd050829dce242035e2b875ea43ed13910d4a
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/21/2018
ms.locfileid: "40246079"
---
## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Associar uma conta de armazenamento do Azure para o IoT Hub

Uma vez que a aplicação de dispositivo simulado carrega um ficheiro para um blob, tem de ter uma [armazenamento do Azure](../articles/storage/common/storage-quickstart-create-account.md) conta associada ao IoT Hub. Quando associa uma conta de armazenamento do Azure com um hub IoT, o hub IoT gera um URI de SAS. Um dispositivo pode utilizar este URI de SAS para carregar com segurança um ficheiro para um contentor de Blobs. O serviço IoT Hub e os SDKs de dispositivo coordenam o processo que gera o URI de SAS e disponibiliza-os para um dispositivo para utilizar para carregar um ficheiro.

Siga as instruções em [configurar ficheiro carrega no portal do Azure](../articles/iot-hub/iot-hub-configure-file-upload.md) para associar uma conta de armazenamento do Azure ao seu hub IoT. Certifique-se de que um contentor de Blobs está associado ao seu hub IoT e que as notificações de ficheiros estão ativadas.

![Ativar as notificações de ficheiro no portal](media/iot-hub-associate-storage/enable-file-notifications.png)