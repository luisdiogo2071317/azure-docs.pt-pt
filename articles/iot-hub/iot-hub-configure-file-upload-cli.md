---
title: Configurar o carregamento de ficheiro para o IoT Hub com a CLI do Azure) (az.py) | Documentos da Microsoft
description: Como configurar fileuploads para o IoT Hub do Azure com a várias plataformas do Azure CLI 2.0 (az.py).
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 0eac620d44967827f7703da9cf409703a123ab07
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39460199"
---
# <a name="configure-iot-hub-file-uploads-using-azure-cli"></a>Configurar o IoT Hub, carregamentos de ficheiros com a CLI do Azure

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

Para utilizar o [funcionalidade de carregamento de ficheiros no IoT Hub][lnk-upload], primeiro tem de associar uma conta de armazenamento do Azure com o seu hub IoT. Pode utilizar uma conta de armazenamento existente ou crie um novo.

Para concluir este tutorial, precisa do seguinte:

* Uma conta ativa do Azure. Se não tiver uma conta, pode criar uma [conta gratuita][lnk-free-trial] em apenas alguns minutos.
* [CLI 2.0 do Azure][lnk-CLI-install].
* Um hub IoT do Azure. Se não tiver um hub IoT, pode utilizar o `az iot hub create` [comando] [ lnk-cli-create-iothub] para criar ou utilizar o portal para [criar um hub IoT] [lnk-portal-hub].
* Uma conta do Armazenamento do Azure. Se não tiver uma conta de armazenamento do Azure, pode utilizar o [CLI 2.0 do Azure - gerir contas de armazenamento] [ lnk-manage-storage] para criar ou utilizar o portal para [criar uma conta de armazenamento] [ lnk-portal-storage].

## <a name="sign-in-and-set-your-azure-account"></a>Iniciar sessão e definir a sua conta do Azure

Inicie sessão na sua conta do Azure e selecione a sua subscrição.

1. Na linha de comandos, execute o [comando login][lnk-login-command]:

    ```azurecli
    az login
    ```

    Siga as instruções para se autenticar com o código e inicie sessão na sua conta do Azure através de um browser.

1. Se tiver várias subscrições do Azure, iniciar sessão no Azure dá-lhe acesso a todas as contas do Azure associadas às suas credenciais. Utilize o comando [ para listar as contas do Azure][lnk-az-account-command] disponíveis e que pode utilizar:

    ```azurecli
    az account list
    ```

    Utilize o comando seguinte para selecionar a subscrição que pretende utilizar para executar os comandos para criar o seu hub IoT. Pode utilizar o nome ou o ID da subscrição da saída do comando anterior:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="retrieve-your-storage-account-details"></a>Obter os detalhes da conta de armazenamento

Os passos seguintes partem do princípio de que criou a sua conta de armazenamento com o **Resource Manager** modelo de implementação e não a **clássico** modelo de implementação.

Para configurar os carregamentos de ficheiros a partir dos seus dispositivos, precisa da cadeia de ligação para uma conta de armazenamento do Azure. A conta de armazenamento tem de ser na mesma subscrição que o seu hub IoT. Também precisa do nome de um contentor de BLOBs na conta de armazenamento. Utilize o seguinte comando para obter as chaves de conta de armazenamento:

```azurecli
az storage account show-connection-string --name {your storage account name} --resource-group {your storage account resource group}
```

Anote o **connectionString** valor. Precisa nos passos seguintes.

Pode utilizar um contentor de BLOBs existentes para carregamentos de ficheiros ou crie uma nova:

* Para listar os contentores de BLOBs existentes na sua conta de armazenamento, utilize o seguinte comando:

    ```azurecli
    az storage container list --connection-string "{your storage account connection string}"
    ```

* Para criar um contentor de BLOBs na sua conta de armazenamento, utilize o seguinte comando:

    ```azurecli
    az storage container create --name {container name} --connection-string "{your storage account connection string}"
    ```

## <a name="file-upload"></a>Carregamento de ficheiros

Agora pode configurar o seu IoT hub para habilitar [funcionalidade de carregamento de ficheiros] [ lnk-upload] com os detalhes da conta de armazenamento.

A configuração requer os seguintes valores:

**Contentor de armazenamento**: um contentor de BLOBs numa conta de armazenamento do Azure na sua subscrição do Azure atual para associar o seu hub IoT. Obter as informações de conta de armazenamento necessário na secção anterior. IoT Hub gera automaticamente os URIs de SAS com permissões de escrita para este contentor de BLOBs para os dispositivos a utilizar quando eles carregam ficheiros.

**Receber notificações sobre os ficheiros carregados**: Ativar ou desativar notificações de carregamento do ficheiro.

**TTL de SAS**: esta definição é o tempo de vida dos URIs de SAS retornado para o dispositivo ao IoT Hub. Definido como uma hora por predefinição.

**TTL de padrão de definições de notificação de ficheiros**: O tempo de vida de um ficheiro a carregar notificação antes de expirar. Definido como um dia por predefinição.

**Contagem máxima de entrega de notificação de ficheiros**: O número de vezes o IoT Hub tentará entregar um arquivo carregar notificação. Definido como 10, por predefinição.

Utilize os seguintes comandos do CLI do Azure para configurar as definições de carregamento de ficheiros no seu hub IoT:

Num uso de shell do bash:

```azurecli
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.connectionString="{your storage account connection string}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.containerName="{your storage container name}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.sasTtlAsIso8601=PT1H0M0S

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

Numa utilização de linha de comandos do Windows:

```azurecli
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.connectionString="{your storage account connection string}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.containerName="{your storage container name}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.sasTtlAsIso8601=PT1H0M0S"

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

Pode rever a configuração de carregamento de ficheiros no seu hub IoT com o seguinte comando:

```azurecli
az iot hub show --name {your iot hub name}
```

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre as capacidades de carregamento de arquivo do IoT Hub, veja [carregar ficheiros a partir de um dispositivo][lnk-upload].

Siga estas ligações para saber mais sobre como gerir o IoT Hub do Azure:

* [Em massa gerir dispositivos IoT][lnk-bulk]
* [Métricas do IoT Hub][lnk-metrics]
* [Monitorização de operações][lnk-monitor]

Para explorar ainda mais os recursos do IoT Hub, veja:

* [Guia do programador do IoT Hub][lnk-devguide]
* [Implementar o AI em dispositivos de ponta com o Azure IoT Edge][lnk-iotedge]
* [Proteger a sua solução de IoT desde o backup][lnk-securing]

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-securing]: /azure/iot-fundamentals/iot-security-ground-up


[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-manage-storage]:../storage/common/storage-azure-cli.md#manage-storage-accounts
[lnk-portal-storage]:../storage/common/storage-create-storage-account.md
[lnk-cli-create-iothub]: https://docs.microsoft.com/cli/azure/iot/hub#az-iot-hub-create