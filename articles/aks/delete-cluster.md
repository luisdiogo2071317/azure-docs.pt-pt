---
title: "Eliminar um cluster do serviço de contentor do Azure (AKS)"
description: Eliminar e AKS cluster com o portal CLI ou do Azure.
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/05/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 78056288f45616eda427f8e708efc679f8a5202c
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2018
---
# <a name="delete-an-azure-container-service-aks-cluster"></a>Eliminar um cluster do serviço de contentor do Azure (AKS)

Se eliminar um cluster do serviço de contentor do Azure, o grupo de recursos no qual o cluster foi implementado continua a ser, no entanto recursos de todos os relacionados com AKS são eliminados. Este documento mostra como eliminar um cluster AKS utilizando a CLI do Azure e o portal do Azure. 

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
| `--resource-group` `-g` | Nome do grupo de recursos do serviço de contentor do Azure. | sim |
| `--no-wait` | Não aguarde a operação de execução longa concluir. | não |
| `--yes` `-y` | Não solicita confirmação. | não |

## <a name="azure-portal"></a>Portal do Azure

No portal do Azure, navegue para o grupo de recursos que contém o recurso de serviço de contentor do Azure (AKS), selecione o recurso e clique em **eliminar**. Lhe for pedido para confirmar a operação de eliminação.

![Eliminar o portal de cluster AKS](media/container-service-delete-cluster/delete-aks-portal.png)

<!-- LINKS - internal -->
[az-aks-delete]: /cli/azure/aks?view=azure-cli-latest#az_aks_delete