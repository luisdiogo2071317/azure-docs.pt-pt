---
title: Dimensionar um cluster do serviço de Kubernetes do Azure (AKS)
description: Dimensione um cluster do serviço de Kubernetes do Azure (AKS).
services: container-service
author: gabrtv
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 11/15/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 37f949e1e043e62b8bd8f73a2b40d0549f77adc3
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="scale-an-azure-kubernetes-service-aks-cluster"></a>Dimensionar um cluster do serviço de Kubernetes do Azure (AKS)

É fácil dimensionar um cluster do AKS para um número diferente de nós.  Selecione o número de nós pretendido e execute o comando `az aks scale`.  Quando o dimensionamento para baixo, nós será cuidadosamente [cordoned e drained] [ kubernetes-drain] para minimizar perturbações para as aplicações em execução.  Ao aumentar verticalmente, o comando `az` aguarda até que os nós estejam marcados `Ready` pelo cluster do Kubernetes.

## <a name="scale-the-cluster-nodes"></a>Dimensionar nós de cluster

Utilize o comando `az aks scale` para dimensionar os nós de cluster. O exemplo seguinte dimensiona um cluster com o nome *myAKSCluster* para um único nó.

```azurecli-interactive
az aks scale --name myAKSCluster --resource-group myResourceGroup --node-count 1
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
    "kubernetesVersion": "1.7.7",
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

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre como implementar e gerir AKS com os tutoriais de AKS.

> [!div class="nextstepaction"]
> [Tutorial do AKS][aks-tutorial]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md