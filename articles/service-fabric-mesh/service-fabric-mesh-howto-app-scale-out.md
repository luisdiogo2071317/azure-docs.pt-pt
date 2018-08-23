---
title: Dimensionar os serviços num aplicativo de malha do Azure Service Fabric | Documentos da Microsoft
description: Saiba como dimensionar serviços dentro de um aplicativo em execução no serviço de recursos de infraestrutura malha com a CLI do Azure de forma independente.
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
ms.date: 08/08/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: e68bcd135c33c7fd83908b8fed0fd098a698fd36
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/10/2018
ms.locfileid: "42054752"
---
# <a name="scale-services-within-an-application-running-on-service-fabric-mesh"></a>Serviços de dimensionamento dentro de um aplicativo em execução no Service Fabric em malha

Este artigo mostra como dimensionar de forma independente microsserviços dentro de um aplicativo. Neste exemplo, o aplicativo de objetos visuais consiste em dois microsserviços: `web` e `worker`.

O `web` service é uma aplicação ASP.NET Core com uma página da web que mostra os triângulos no browser. O browser apresenta um triângulo para cada instância do `worker` serviço.

O `worker` movem o triângulo de um intervalo predefinido no espaço de serviço e envia a localização do triângulo para `web` serviço. Ele usa o DNS para resolver o endereço do `web` serviço.

## <a name="set-up-service-fabric-mesh-cli"></a>Configurar a CLI do Service Fabric mesh

Pode utilizar o Azure Cloud Shell ou uma instalação local da CLI do Azure para concluir esta tarefa. Instale o módulo de extensão da CLI do Azure Service Fabric Mesh através destas [instruções](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Azure e defina a sua subscrição.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-resource-group"></a>Criar grupo de recursos

Crie um grupo de recursos no qual quer implementar a aplicação.

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-application-with-one-worker-service"></a>Implementar a aplicação com o serviço de uma função de trabalho

Criar a aplicação no grupo de recursos utilizando o `deployment create` comando.

O exemplo seguinte implementa uma aplicação do Linux utilizar o [mesh_rp.base.linux.json modelo](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json). Para implementar uma aplicação do Windows, utilize o [[mesh_rp.base.windows.json modelo](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.windows.json). As imagens de contentor do Windows são maiores do que as imagens de contentor do Linux e podem demorar mais tempo a implementar.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
```

Dentro de alguns minutos, o comando deverá devolver com:

`visualObjectsApp has been deployed successfully on visualObjectsNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Abrir a aplicação

O comando de implementação irá devolver o endereço IP público do ponto de extremidade de serviço. Assim que a aplicação é implementada com êxito, obtenha o endereço IP público para o ponto final de serviço e abra-a partir de um browser. Esta será apresentada uma página da web com um triângulo movimentação.

O nome de recurso de rede para esta aplicação está `visualObjectsNetwork`. Pode ver informações sobre a aplicação, como a descrição, localização, grupo de recursos, etc., utilizando o seguinte comando:

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name visualObjectsNetwork
```

## <a name="scale-worker-service"></a>Dimensionamento `worker` serviço

Dimensionar o `worker` serviço três instâncias com o seguinte comando. O exemplo seguinte implementa uma aplicação do Linux utilizar o [mesh_rp.scaleout.linux.json modelo](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json). Para implementar uma aplicação do Windows, utilize o [mesh_rp.scaleout.windows.json modelo](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.windows.json). Lembre-se de que a maior imagens de contentor podem demorar mais tempo a implementar.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
  
```

Assim que a aplicação é implementada com êxito, o browser deve apresentar uma página da web com três triângulos movimentação.

## <a name="delete-the-resources"></a>Eliminar os recursos

Com frequência, elimine os recursos que já não estiver a utilizar no Azure. Para eliminar os recursos relacionados a este exemplo, elimine o grupo de recursos no qual eles foram implantados (o que elimina tudo associado o grupo de recursos) com o seguinte comando:

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

## <a name="next-steps"></a>Passos Seguintes

- Ver a aplicação de exemplo de objetos visuais num [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects).
- Para saber mais sobre o modelo de recursos do Service Fabric, veja [modelo de recursos de malha do Service Fabric](service-fabric-mesh-service-fabric-resources.md).
- Para saber mais sobre a malha de recursos de infraestrutura do serviço, leia os [descrição geral do Service Fabric em malha](service-fabric-mesh-overview.md).