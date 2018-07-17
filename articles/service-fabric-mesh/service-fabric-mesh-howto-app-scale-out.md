---
title: Dimensionar os serviços num aplicativo de malha do Azure Service Fabric | Documentos da Microsoft
description: Saiba como dimensionar serviços dentro de um aplicativo em execução no serviço de recursos de infraestrutura malha com a CLI do Azure de forma independente.
services: service-fabric
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
ms.openlocfilehash: c0350b767b65aee0c4611bb8fa6f635a651d33dc
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076307"
---
# <a name="scale-services-within-an-application-running-on-service-fabric-mesh"></a>Serviços de dimensionamento dentro de um aplicativo em execução no Service Fabric em malha

Este artigo mostra como dimensionar de forma independente microsserviços dentro de um aplicativo. Neste exemplo, aplicação de objetos visuais consiste em dois microsserviços; `web` e `worker`. 

O `web` service é uma aplicação ASP.NET Core com uma página da web que mostra os triângulos no browser. O browser apresenta um triângulo para cada instância do `worker` serviço. 

O `worker` movem o triângulo de um intervalo predefinido no espaço de serviço e envia a localização do triângulo para `web` serviço. Ele usa o DNS para resolver o endereço do `web` serviço.

## <a name="set-up-service-fabric-mesh-cli"></a>Configurar a CLI do Service Fabric de malha 
Pode utilizar o Azure Cloud Shell ou uma instalação local da CLI do Azure para concluir esta tarefa. Instalar o módulo de extensão de CLI de malha do Azure Service Fabric através destas [instruções](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure
Inicie sessão no Azure e definir a subscrição.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-resource-group"></a>Criar grupo de recursos
Crie um grupo de recursos para implementar a aplicação. Pode utilizar um grupo de recursos existente e ignorar este passo. 

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-application-with-one-worker-service"></a>Implementar a aplicação com o serviço de uma função de trabalho
Criar a aplicação no grupo de recursos utilizando o `deployment create` comando.

```azurecli-interactive
az mesh deployment create --resource-group <resourceGroupName> --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
  
```
O comando anterior implementa um através do Linux [mesh_rp.base.linux.json modelo](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.linux.json). Se pretender implementar uma aplicação do Windows, utilize [mesh_rp.base.windows.json modelo](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.base.windows.json). Imagens de contentor do Windows são maiores do que as imagens de contentor do Linux e podem demorar mais tempo para implementar.

Dentro de alguns minutos, o comando deverá devolver com:

`visualObjectsApp has been deployed successfully on visualObjectsNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Abra a aplicação
Assim que a aplicação é implementada com êxito, obtenha o endereço IP público para o ponto final de serviço e abra-o num browser. Ele deve exibir uma página da web com um triângulo movendo o espaço.

O comando de implementação devolve o endereço IP público do ponto de extremidade de serviço. Também pode consultar o recurso de rede para encontrar o endereço IP público do ponto de extremidade de serviço.
 
O nome de recurso de rede para esta aplicação está `visualObjectsNetwork`, obter informações sobre ele usando o seguinte comando. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name visualObjectsNetwork
```

## <a name="scale-worker-service"></a>Dimensionamento `worker` serviço

Dimensionar o `worker` serviço três instâncias com o seguinte comando. 

```azurecli-interactive
az mesh deployment create --resource-group <resourceGroupName> --template-uri https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
  
```
O comando anterior implementa um através do Linux [mesh_rp.scaleout.linux.json modelo](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.linux.json). Se pretender implementar uma aplicação do Windows, utilize [mesh_rp.scaleout.windows.json modelo](https://sfmeshsamples.blob.core.windows.net/templates/visualobjects/mesh_rp.scaleout.windows.json). Imagens de contentor do Windows são maiores do que as imagens de contentor do Linux e podem demorar mais tempo para implementar.

Assim que a aplicação é implementada com êxito, o navegador deve ser apresentar uma página da web com três triângulos movendo o espaço.

## <a name="delete-the-resources"></a>Eliminar os recursos

De modo a preservar recursos limitados, atribuídos para o programa de pré-visualização, elimine os recursos com frequência. Para eliminar os recursos relacionados a este exemplo, elimine o grupo de recursos no qual eles foram implantados.

```azurecli-interactive
az group delete --resource-group myResourceGroup 
```

## <a name="next-steps"></a>Passos Seguintes
- Ver a aplicação de exemplo de objetos visuais num [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects).
- Para saber mais sobre o modelo de recursos do Service Fabric, veja [modelo de recursos de malha do Service Fabric](service-fabric-mesh-service-fabric-resources.md).
- Para saber mais sobre a malha de recursos de infraestrutura do serviço, leia os [descrição geral do Service Fabric em malha](service-fabric-mesh-overview.md).