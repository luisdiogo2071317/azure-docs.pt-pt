---
title: Eliminar um cluster do Azure Kubernetes Service (AKS)
description: Cluster de eliminação e AKS com o portal do Azure ou da CLI.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 2/05/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: c8eab17a5c635560d9a5274eb038845238968e02
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39439940"
---
# <a name="delete-an-azure-kubernetes-service-aks-cluster"></a>Eliminar um cluster do Azure Kubernetes Service (AKS)

Ao eliminar um cluster do serviço Kubernetes do Azure, o grupo de recursos no qual o cluster foi implementado como for, recursos de todos os relacionados com o AKS são eliminados. Este documento mostra como eliminar um cluster do AKS com a CLI do Azure e o portal do Azure.

Para além de eliminar o cluster, pode ser eliminado o grupo de recursos em que foi implementada, que também elimina o cluster do AKS.

## <a name="azure-cli"></a>CLI do Azure

Utilize o [eliminar az aks] [ az-aks-delete] comando para eliminar o cluster do AKS.

```azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster
```

As seguintes opções estão disponíveis com o `az aks delete` comando.

| Argumento | Descrição | Necessário |
|---|---|:---:|
| `--name` `-n` | Nome do recurso para o cluster gerido. | sim |
| `--resource-group` `-g` | Nome do grupo de recursos do Azure Kubernetes Service. | sim |
| `--no-wait` | Não aguarde a conclusão da operação de longa execução. | não |
| `--yes` `-y` | Não pedir confirmação. | não |

## <a name="azure-portal"></a>Portal do Azure

Enquanto estiver no portal do Azure, navegue para o grupo de recursos que contém o recurso do Azure Kubernetes Service (AKS), selecione o recurso e clique em **eliminar**. São-lhe pedido para confirmar a operação de eliminação.

![Eliminar o portal de cluster do AKS](media/container-service-delete-cluster/delete-aks-portal.png)

<!-- LINKS - internal -->
[az-aks-delete]: /cli/azure/aks?view=azure-cli-latest#az-aks-delete