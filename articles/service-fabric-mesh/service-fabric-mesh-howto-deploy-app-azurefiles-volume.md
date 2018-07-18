---
title: Store estado ao montar o volume de ficheiros do Azure com base no interior do contentor no aplicativo de malha do Service Fabric | Documentos da Microsoft
description: Saiba como armazenar o estado ao montar o volume de ficheiros do Azure com base no interior do contentor no aplicativo de malha do Service Fabric com a CLI do Azure.
services: service-fabric-mesh
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: 94a4e17e6285893520a2f6482b32a69b1229e2fa
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090637"
---
# <a name="store-state-by-mounting-azure-files-based-volume-in-service-fabric-mesh-application"></a>Store estado ao montar o volume de ficheiros do Azure com base na aplicação de malha do Service Fabric

Este artigo mostra como armazenar o Estado nos ficheiros do Azure ao montar um volume no interior do contentor de um aplicativo de malha do Service Fabric. Neste exemplo, o aplicativo de contador tem um serviço ASP.NET Core com uma página da web que mostra o valor do contador num browser. 

O `counterService` perodically lê um valor de contador de um arquivo, incrementa ele e a escrita de volta para o ficheiro. O ficheiro é armazenado numa pasta que está montada no volume apoiado por partilha de ficheiros do Azure. 

## <a name="set-up-service-fabric-mesh-cli"></a>Configurar a CLI do Service Fabric de malha 
Pode utilizar o Azure Cloud Shell ou uma instalação local da CLI do Azure para concluir esta tarefa. Instalar o módulo de extensão de CLI de malha do Azure Service Fabric através destas [instruções](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure
Inicie sessão no Azure e definir a subscrição.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-file-share"></a>Criar a partilha de ficheiros 
Criar uma partilha de ficheiros do Azure através destas [instruções](/azure/storage/files/storage-how-to-create-file-share). O nome da conta de armazenamento, a chave de conta de armazenamento e o nome da partilha de ficheiros que são referenciadas como `<storageAccountName>`, `<storageAccountKey>`, e `<fileShareName>` nas seguintes instruções.

## <a name="create-resource-group"></a>Criar grupo de recursos
Crie um grupo de recursos para implementar a aplicação. Pode utilizar um grupo de recursos existente e ignorar este passo. 

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-template"></a>Implementar o modelo

Criar a aplicação e os recursos relacionados com o seguinte comando e forneça os valores para `storageAccountName`, `storageAccountKey` e `fileShareName` do passo anterior.

O `storageAccountKey` parâmetro no modelo é um `securestring`. Não será apresentado o estado de implementação e `az mesh service show` comandos. Certifique-se de que ele está corretamente especificado no comando seguinte.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.linux.json  --parameters "{\"location\": {\"value\": \"eastus\"}, \"fileShareName\": {\"value\": \"<fileShareName>\"}, \"storageAccountName\": {\"value\": \"<storageAccountName>\"}, \"storageAccountKey\": {\"value\": \"<storageAccountKey>\"}}"
```

O comando anterior implementa uma aplicação do Linux utilizar [mesh_rp.linux.json modelo](https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.linux.json). Se pretender implementar uma aplicação do Windows, utilize [mesh_rp.windows.json modelo](https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.windows.json). Imagens de contentor do Windows são maiores do que as imagens de contentor do Linux e podem demorar mais tempo para implementar.

Dentro de alguns minutos, o comando deverá devolver com:

`counterApp has been deployed successfully on counterAppNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Abra a aplicação
Assim que a aplicação é implementada com êxito, obtenha o endereço IP público para o ponto final de serviço e abra-o num browser. Apresenta uma página da web com o valor do contador que está a ser atualizado a cada segundo.

O comando de implementação devolve o endereço IP público do ponto de extremidade de serviço. Opcionalmente, também pode consultar o recurso de rede para encontrar o endereço IP público do ponto de extremidade de serviço. 
 
O nome de recurso de rede para esta aplicação está `counterAppNetwork`, obter informações sobre ele usando o seguinte comando. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name counterAppNetwork
```

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Certifique-se de que o aplicativo é capaz de usar o volume
O aplicativo cria um ficheiro denominado `counter.txt` no ficheiro partilhar dentro `counter/counterService` pasta. O conteúdo deste ficheiro é o valor de contador que está a ser apresentado na página da web.

O ficheiro pode ser transferido através de qualquer ferramenta que permite a navegação de uma partilha de ficheiros de ficheiros do Azure. O [Explorador de armazenamento do Microsoft Azure](https://azure.microsoft.com/features/storage-explorer/) é um exemplo de uma ferramenta desse tipo.

## <a name="delete-the-resources"></a>Eliminar os recursos

De modo a preservar recursos limitados, atribuídos para o programa de pré-visualização, elimine os recursos com frequência. Para eliminar os recursos relacionados a este exemplo, elimine o grupo de recursos no qual eles foram implantados.

```azurecli-interactive
az group delete --resource-group myResourceGroup 
```

## <a name="next-steps"></a>Passos Seguintes

- Ver a aplicação de exemplo do volume de ficheiros do Azure no [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter).
- Para saber mais sobre o modelo de recursos do Service Fabric, veja [modelo de recursos de malha do Service Fabric](service-fabric-mesh-service-fabric-resources.md).
- Para saber mais sobre a malha de recursos de infraestrutura do serviço, leia os [descrição geral do Service Fabric em malha](service-fabric-mesh-overview.md).
