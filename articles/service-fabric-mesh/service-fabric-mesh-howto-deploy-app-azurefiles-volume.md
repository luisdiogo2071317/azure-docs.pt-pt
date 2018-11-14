---
title: Store estado num aplicativo de malha do Azure Service Fabric ao montar um volume de ficheiros do Azure com base no interior do contentor | Documentos da Microsoft
description: Saiba como armazenar o estado num aplicativo de malha do Azure Service Fabric ao montar um volume de ficheiros do Azure com base no interior do contentor com a CLI do Azure.
services: service-fabric-mesh
documentationcenter: .net
author: rwike77
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: cb5b421c1bcfe888d65335f3ab7f67bed80eec34
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614264"
---
# <a name="store-state-in-an-azure-service-fabric-mesh-application-by-mounting-an-azure-files-based-volume-inside-the-container"></a>Estado de Store num aplicativo de malha do Azure Service Fabric ao montar uma ficheiros do Azure com base em volume no interior do contentor

Este artigo mostra como armazenar o Estado nos ficheiros do Azure ao montar um volume no interior do contentor de um aplicativo de malha do Service Fabric. Neste exemplo, o aplicativo de contador tem um serviço ASP.NET Core com uma página da web que mostra o valor do contador num browser. 

O `counterService` periodicamente lê um valor de contador de um arquivo, incrementa-lo e a escrita de volta para o ficheiro. O ficheiro é armazenado numa pasta que está montada no volume apoiado por partilha de ficheiros do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Pode utilizar o Azure Cloud Shell ou uma instalação local da CLI do Azure para concluir esta tarefa. Para utilizar a CLI do Azure com este artigo, certifique-se de que `az --version` retorna, pelo menos, `azure-cli (2.0.43)`.  Instalar (ou atualizar) o módulo de extensão de CLI de malha do Azure Service Fabric através destas [instruções](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Azure e defina a sua subscrição.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-a-file-share"></a>Criar uma partilha de ficheiros

Criar uma partilha de ficheiros do Azure através destas [instruções](/azure/storage/files/storage-how-to-create-file-share). O nome da conta de armazenamento, a chave de conta de armazenamento e o nome da partilha de ficheiros que são referenciadas como `<storageAccountName>`, `<storageAccountKey>`, e `<fileShareName>` nas seguintes instruções. Estes valores estão disponíveis no portal do Azure:
* <storageAccountName> – Em **contas de armazenamento**, é o nome da conta de armazenamento que utilizou quando criou a partilha de ficheiros.
* <storageAccountKey> -Selecione a sua conta de armazenamento sob **contas de armazenamento** e, em seguida, selecione **chaves de acesso** e utilizar o valor sob **chave1**.
* <fileShareName> -Selecione a sua conta de armazenamento sob **contas de armazenamento** e, em seguida, selecione **ficheiros**. O nome a utilizar é o nome da partilha de ficheiros que acabou de criar.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos no qual quer implementar a aplicação. O comando seguinte cria um grupo de recursos com o nome `myResourceGroup` numa localização nos EUA Leste.

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-template"></a>Implementar o modelo

Criar a aplicação e os recursos relacionados com o seguinte comando e forneça os valores para `storageAccountName`, `storageAccountKey` e `fileShareName` partir anteriormente [criar uma partilha de ficheiros](#create-a-file-share) passo.

O `storageAccountKey` parâmetro no modelo é uma cadeia segura. Não será apresentado o estado de implementação e `az mesh service show` comandos. Certifique-se de que ele está corretamente especificado no comando seguinte.

O seguinte comando implementa uma aplicação do Linux utilizar o [counter.azurefilesvolume.linux.json modelo](https://sfmeshsamples.blob.core.windows.net/templates/counter/counter.azurefilesvolume.linux.json). Para implementar uma aplicação do Windows, utilize o [counter.azurefilesvolume.windows.json modelo](https://sfmeshsamples.blob.core.windows.net/templates/counter/counter.azurefilesvolume.windows.json). Lembre-se de que a maior imagens de contentor podem demorar mais tempo a implementar.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/counter/counter.azurefilesvolume.linux.json  --parameters "{\"location\": {\"value\": \"eastus\"}, \"fileShareName\": {\"value\": \"<fileShareName>\"}, \"storageAccountName\": {\"value\": \"<storageAccountName>\"}, \"storageAccountKey\": {\"value\": \"<storageAccountKey>\"}}"
```

Dentro de alguns minutos, o comando deverá devolver com `counterApp has been deployed successfully on counterAppNetwork with public ip address <IP Address>`

## <a name="open-the-application"></a>Abrir a aplicação

O comando de implementação irá devolver o endereço IP público do ponto de extremidade de serviço. Assim que a aplicação é implementada com êxito, obtenha o endereço IP público para o ponto final de serviço e abra-a partir de um browser. Esta será apresentada uma página da web com o valor do contador que está a ser atualizado a cada segundo.

O nome de recurso de rede para esta aplicação está `counterAppNetwork`. Pode ver informações sobre a aplicação, como a descrição, localização, grupo de recursos, etc., utilizando o seguinte comando:

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name counterAppNetwork
```

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Certifique-se de que o aplicativo é capaz de usar o volume

O aplicativo cria um ficheiro denominado `counter.txt` no ficheiro partilhar dentro `counter/counterService` pasta. O conteúdo deste ficheiro é o valor de contador que está a ser apresentado na página da web.

O ficheiro pode ser transferido através de qualquer ferramenta que permite a navegação de uma partilha de ficheiros de ficheiros do Azure, tais como o [Explorador de armazenamento do Microsoft Azure](https://azure.microsoft.com/features/storage-explorer/).

## <a name="delete-the-resources"></a>Eliminar os recursos

Com frequência, elimine os recursos que já não estiver a utilizar no Azure. Para eliminar os recursos relacionados a este exemplo, elimine o grupo de recursos no qual eles foram implantados (o que elimina tudo associado o grupo de recursos) com o seguinte comando:

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

## <a name="next-steps"></a>Passos Seguintes

- Ver a aplicação de exemplo do volume de ficheiros do Azure no [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter).
- Para saber mais sobre o Modelo de Recursos do Service Fabric, consulte [Modelo de Recursos do Service Fabric Mesh](service-fabric-mesh-service-fabric-resources.md).
- Para saber mais sobre o Service Fabric Mesh, consulte [Descrição geral do Service Fabric Mesh](service-fabric-mesh-overview.md).