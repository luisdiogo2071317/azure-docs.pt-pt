---
title: Implementar uma aplicação no Azure Service Fabric em malha através de um modelo | Documentos da Microsoft
description: Saiba como implementar uma aplicação a malha de recursos de infraestrutura do serviço .NET Core a partir de um modelo com a CLI do Azure.
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
ms.date: 07/12/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: 356e8019f9a4a64cb1c1c113d0f44990aa4e0f95
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/10/2018
ms.locfileid: "42059537"
---
# <a name="deploy-a-service-fabric-mesh-application-to-service-fabric-mesh-using-a-template"></a>Implementar uma aplicação de malha de recursos de infraestrutura do serviço de mensagens em fila para a malha de recursos de infraestrutura de serviço com um modelo
Este artigo mostra como implementar uma aplicação .NET Core para a malha de recursos de infraestrutura do serviço através de um modelo. Quando tiver terminado, terá uma aplicação de voto com um front-end de que guarda os resultados das votações num serviço de back-end do cluster da web ASP.NET Core. O front-end utiliza o DNS para resolver o endereço do serviço de back-end.

## <a name="set-up-service-fabric-mesh-cli"></a>Configurar a CLI do Service Fabric mesh 
Pode utilizar o Azure Cloud Shell ou uma instalação local da CLI do Azure para concluir esta tarefa. Instale o módulo de extensão da CLI do Azure Service Fabric Mesh através destas [instruções](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure
Inicie sessão no Azure e defina a sua subscrição.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-resource-group"></a>Criar grupo de recursos
Crie um grupo de recursos no qual quer implementar a aplicação. Pode utilizar um grupo de recursos existente e ignorar este passo. 

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-application"></a>Implementar a aplicação
Criar a aplicação no grupo de recursos utilizando o `deployment create` comando.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/voting/mesh_rp.windows.json --parameters "{\"location\": {\"value\": \"eastus\"}}"
```

O comando anterior implementa um aplicativo do Windows usando [mesh_rp.windows.json modelo](https://sfmeshsamples.blob.core.windows.net/templates/voting/mesh_rp.windows.json). Se pretender implementar uma aplicação do Linux, utilize [mesh_rp.linux.json modelo](https://sfmeshsamples.blob.core.windows.net/templates/voting/mesh_rp.linux.json). As imagens de contentor do Windows são maiores do que as imagens de contentor do Linux e podem demorar mais tempo a implementar.

Dentro de alguns minutos, o comando deverá devolver com:

`VotingApp has been deployed successfully on VotingAppNetwork with public ip address <IP address>` 

## <a name="open-the-application"></a>Abrir a aplicação
Assim que a aplicação é implementada com êxito, obtenha o endereço IP público para o ponto final de serviço e abra-o num browser. Ele exibe após a página da web. 

![Aplicação Voting](./media/service-fabric-mesh-howto-deploy-app-template/VotingApplication.png)

Agora pode adicionar opções de votos para o aplicativo e vote nela ou eliminar opções de votos.

O comando de implementação devolve o endereço IP público do ponto de extremidade de serviço. Opcionalmente, também pode consultar o recurso de rede para encontrar o endereço IP público do ponto de extremidade de serviço. 

O nome de recurso de rede para esta aplicação está `VotingAppNetwork`, obter informações sobre ele usando o seguinte comando. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name VotingAppNetwork
```

## <a name="check-the-application-details"></a>Verificar os detalhes da aplicação
Pode verificar o estado da aplicação com o comando `app show`. O nome da aplicação para a aplicação implementada é "VotingApp", então, obter os detalhes. 

```azurecli-interactive
az mesh app show --resource-group myResourceGroup --name VotingApp
```

## <a name="list-the-deployed-applications"></a>Lista as aplicações implementadas
Pode utilizar o comando "app list" para obter uma lista das aplicações implementadas na sua subscrição. 

```azurecli-interactive
az mesh app list -o table
```

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar do aplicativo e ele está relacionado com recursos, elimine o grupo de recursos contendo-os. 

```azurecli-interactive
az group delete --resource-group myResourceGroup  
``` 

## <a name="next-steps"></a>Passos Seguintes
- Ver a aplicação de votação de exemplo na [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp).
- Para saber mais sobre a malha de recursos de infraestrutura do serviço, leia os [descrição geral do Service Fabric em malha](service-fabric-mesh-overview.md).


