---
title: Atualizar um cluster do Azure Container Service (AKS)
description: Atualizar um cluster do Azure Container Service (AKS)
services: container-service
author: gabrtv
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 04/05/2018
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 5f5cfc49bd7b5e1c06adf97bc6f6ba88fc81f77a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="upgrade-an-azure-container-service-aks-cluster"></a>Atualizar um cluster do Azure Container Service (AKS)

O Azure Container Service (AKS) facilita a execução de tarefas de gestão comuns, incluindo a atualização de clusters do Kubernetes.

## <a name="upgrade-an-aks-cluster"></a>Atualizar um cluster do AKS

Antes de atualizar um cluster, utilize o comando `az aks get-upgrades` para verificar que versões do Kubernetes estão disponíveis para atualização.

```azurecli-interactive
az aks get-upgrades --name myAKSCluster --resource-group myResourceGroup --output table
```

Saída:

```console
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  -------------------
default  mytestaks007     1.8.10           1.8.10             1.9.1, 1.9.2, 1.9.6
```

Temos três versões disponíveis para atualização: 1.9.1, 1.9.2 e 1.9.6. Podemos utilizar o comando `az aks upgrade` para atualizar para a versão mais recente disponível.  Durante o processo de atualização, nós são cuidadosamente [cordoned e drained] [ kubernetes-drain] para minimizar perturbações para as aplicações em execução.  Antes de iniciar uma atualização dos clusters, certifique-se de que tem capacidade de computação adicional suficiente para processar a carga de trabalho à medida que são adicionados e removidos nós de cluster.

> [!NOTE]
> Ao atualizar um cluster AKS, versões de secundárias Kubernetes não podem ser ignoradas. Por exemplo, atualiza entre 1.7.x > 1.8.x ou 1.8.x > 1.9.x são permitidos, no entanto 1.7 > 1.9 não é.

```azurecli-interactive
az aks upgrade --name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.9.6
```

Saída:

```json
{
  "id": "/subscriptions/<Subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "location": "eastus",
  "name": "myAKSCluster",
  "properties": {
    "accessProfiles": {
      "clusterAdmin": {
        "kubeConfig": "..."
      },
      "clusterUser": {
        "kubeConfig": "..."
      }
    },
    "agentPoolProfiles": [
      {
        "count": 1,
        "dnsPrefix": null,
        "fqdn": null,
        "name": "myAKSCluster",
        "osDiskSizeGb": null,
        "osType": "Linux",
        "ports": null,
        "storageProfile": "ManagedDisks",
        "vmSize": "Standard_D2_v2",
        "vnetSubnetId": null
      }
    ],
    "dnsPrefix": "myK8sClust-myResourceGroup-4f48ee",
    "fqdn": "myk8sclust-myresourcegroup-4f48ee-406cc140.hcp.eastus.azmk8s.io",
    "kubernetesVersion": "1.9.6",
    "linuxProfile": {
      "adminUsername": "azureuser",
      "ssh": {
        "publicKeys": [
          {
            "keyData": "..."
          }
        ]
      }
    },
    "provisioningState": "Succeeded",
    "servicePrincipalProfile": {
      "clientId": "e70c1c1c-0ca4-4e0a-be5e-aea5225af017",
      "keyVaultSecretRef": null,
      "secret": null
    }
  },
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

Confirme que a atualização foi concluída com êxito com o `az aks show` comando.

```azurecli-interactive
az aks show --name myAKSCluster --resource-group myResourceGroup --output table
```

Saída:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus     myResourceGroup  1.9.6                 Succeeded            myk8sclust-myresourcegroup-3762d8-2f6ca801.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre como implementar e gerir AKS com os tutoriais de AKS.

> [!div class="nextstepaction"]
> [Tutorial do AKS][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md