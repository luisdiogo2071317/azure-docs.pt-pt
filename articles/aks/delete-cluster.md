---
title: Eliminar um cluster do serviço de Kubernetes do Azure (AKS)
description: Eliminar e AKS cluster com o portal CLI ou do Azure.
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/05/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: e006466d1450471900a8635c49d3bc6c3a73d476
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="delete-an-azure-kubernetes-service-aks-cluster"></a>Eliminar um cluster do serviço de Kubernetes do Azure (AKS)

Se eliminar um cluster do serviço de Kubernetes do Azure, o grupo de recursos no qual o cluster foi implementado continua a ser, no entanto recursos de todos os relacionados com AKS são eliminados. Este documento mostra como eliminar um cluster AKS utilizando a CLI do Azure e o portal do Azure.

Para além de eliminar o cluster, é possível eliminar o grupo de recursos em que foi implementada, que também elimina o cluster AKS.

## <a name="azure-cli"></a>CLI do Azure

Utilize o [az aks eliminar] [ az-aks-delete] comando para eliminar o cluster AKS.

```azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster
```

As seguintes opções estão disponíveis com o `az aks delete` comando.

| Argumento | Descrição | Necessário |
|---|---|:---:|
| `--name` `-n` | Nome do recurso para o cluster gerido. | sim |
| `--resource-group` `-g` | Nome do grupo de recursos do Azure Kubernetes serviço. | sim |
| `--no-wait` | Não aguarde a operação de execução longa concluir. | não |
| `--yes` `-y` | Não solicita confirmação. | não |

## <a name="azure-portal"></a>Portal do Azure

No portal do Azure, navegue para o grupo de recursos que contém o recurso de serviço de Kubernetes do Azure (AKS), selecione o recurso e clique em **eliminar**. Lhe for pedido para confirmar a operação de eliminação.

![Eliminar o portal de cluster AKS](media/container-service-delete-cluster/delete-aks-portal.png)

<!-- LINKS - internal -->
[az-aks-delete]: /cli/azure/aks?view=azure-cli-latest#az_aks_delete