---
title: Atualizar um cluster do Azure Container Service (AKS)
description: Atualizar um cluster do Azure Container Service (AKS)
services: container-service
author: gabrtv
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 11/15/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 40b55309ee4c52743b30682d8751e6e432f9bb4a
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2017
---
# <a name="upgrade-an-azure-container-service-aks-cluster"></a>Atualizar um cluster do Azure Container Service (AKS)

O Azure Container Service (AKS) facilita a execução de tarefas de gestão comuns, incluindo a atualização de clusters do Kubernetes.

## <a name="upgrade-an-aks-cluster"></a>Atualizar um cluster do AKS

Antes de atualizar um cluster, utilize o comando `az aks get-versions` para verificar que versões do Kubernetes estão disponíveis para atualização.

```azurecli-interactive
az aks get-versions --name myK8sCluster --resource-group myResourceGroup --output table
```

Saída:

```console
Name     ResourceGroup    MasterVersion    MasterUpgrades       NodePoolVersion     NodePoolUpgrades
-------  ---------------  ---------------  -------------------  ------------------  -------------------
default  myResourceGroup  1.7.7            1.8.2, 1.7.9, 1.8.1  1.7.7               1.8.2, 1.7.9, 1.8.1
```

Existem três versões disponíveis para atualização: 1.7.9, 1.8.1 e 1.8.2. Podemos utilizar o comando `az aks upgrade` para atualizar para a versão mais recente disponível.  Durante o processo de atualização, os nós são cuidadosamente [isolados e drenados](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/) para minimizar a interrupção das aplicações em execução.  Antes de iniciar uma atualização dos clusters, certifique-se de que tem capacidade de computação adicional suficiente para processar a carga de trabalho à medida que são adicionados e removidos nós de cluster.

```azurecli-interactive
az aks upgrade --name myK8sCluster --resource-group myResourceGroup --kubernetes-version 1.8.2
```

Saída:

```json
{
  "id": "/subscriptions/4f48eeae-9347-40c5-897b-46af1b8811ec/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myK8sCluster",
  "location": "eastus",
  "name": "myK8sCluster",
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
        "name": "myK8sCluster",
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
    "kubernetesVersion": "1.8.2",
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

Agora, pode confirmar se a atualização foi concluída com êxito com o comando `az aks show`.

```azurecli-interactive
az aks show --name myK8sCluster --resource-group myResourceGroup --output table
```

Saída:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myK8sCluster  eastus     myResourceGroup  1.8.2                Succeeded            myk8sclust-myresourcegroup-3762d8-2f6ca801.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Passos seguintes

Saiba mais sobre como implementar e gerir AKS com os tutoriais de AKS.

> [!div class="nextstepaction"]
> [Tutorial de AKS](./tutorial-kubernetes-prepare-app.md)