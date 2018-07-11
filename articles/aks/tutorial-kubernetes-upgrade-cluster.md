---
title: Tutorial do Kubernetes no Azure - atualizar cluster
description: Tutorial do Kubernetes no Azure - atualizar cluster
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d66197b69a0804a49fabb72e9b97c77e000bdf88
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131648"
---
# <a name="tutorial-upgrade-kubernetes-in-azure-kubernetes-service-aks"></a>Tutorial: Atualizar o Kubernetes no Serviço Kubernetes do Azure (AKS)

Um cluster do Serviço Kubernetes do Azure (AKS) poder ser atualizado através da CLI do Azure. Para minimizar a interrupção das aplicações em execução, os nós do Kubernetes são cuidadosamente [isolados e drenados][kubernetes-drain] durante o processo de atualização.

Neste tutorial, parte sete de sete, é atualizado um cluster do Kubernetes. As tarefas que concluir incluem:

> [!div class="checklist"]
> * Identificar versões do Kubernetes atuais e disponíveis
> * Atualizar os nós do Kubernetes
> * Validar uma atualização com êxito

## <a name="before-you-begin"></a>Antes de começar

Nos tutoriais anteriores, foi compactada uma aplicação numa imagem de contentor, carregada esta imagem para o Azure Container Registry e criado um cluster de Kubernetes. A aplicação foi, em seguida, executada no cluster de Kubernetes.

Se ainda não concluiu estes passos e pretende acompanhar, regresse ao [Tutorial 1 – Criar imagens de contentor][aks-tutorial-prepare-app].

## <a name="get-cluster-versions"></a>Obter versões de cluster

Antes de atualizar um cluster, utilize o comando [az aks get-upgrades][] para verificar que versões do Kubernetes estão disponíveis para atualização.

```azurecli
az aks get-upgrades --name myAKSCluster --resource-group myResourceGroup --output table
```

No exemplo seguinte, a versão atual do nó é *1.9.6* e as versões disponíveis são apresentadas na coluna *Atualizações*.

```
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  ----------
default  myResourceGroup  1.9.6            1.9.6              1.10.3
```

## <a name="upgrade-cluster"></a>Atualizar o cluster

Utilize o comando [az aks upgrade][] para atualizar os nós de cluster. O exemplo seguinte atualiza o cluster para a versão *1.10.3*.

```azurecli
az aks upgrade --name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.10.3
```

A saída do exemplo seguinte condensado mostra *kubernetesVersion* e apresenta agora *1.10.3*:

```json
{
  "agentPoolProfiles": [
    {
      "count": 3,
      "maxPods": 110,
      "name": "nodepool1",
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS1_v2",
    }
  ],
  "dnsPrefix": "myAKSClust-myResourceGroup-19da35",
  "enableRbac": false,
  "fqdn": "myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io",
  "id": "/subscriptions/<Subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "kubernetesVersion": "1.10.3",
  "location": "eastus",
  "name": "myAKSCluster",
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

## <a name="validate-upgrade"></a>Validar atualização

Confirme se a atualização foi concluída com êxito com o comando [az aks show][].

```azurecli
az aks show --name myAKSCluster --resource-group myResourceGroup --output table
```

Saída:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.10.3               Succeeded            myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, atualizou o Kubernetes num cluster do AKS. Foram realizadas as seguintes tarefas:

> [!div class="checklist"]
> * Identificar versões do Kubernetes atuais e disponíveis
> * Atualizar os nós do Kubernetes
> * Validar uma atualização com êxito

Siga esta ligação para saber mais sobre o AKS.

> [!div class="nextstepaction"]
> [Descrição geral do AKS][aks-intro]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-intro]: ./intro-kubernetes.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[az aks show]: /cli/azure/aks#az-aks-show
[az aks get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az aks upgrade]: /cli/azure/aks#az-aks-upgrade