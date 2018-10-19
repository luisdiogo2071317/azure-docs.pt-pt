---
title: Tutorial do Kubernetes no Azure - Atualizar cluster para versão superior
description: Neste tutorial do Azure Kubernetes Service (AKS), saiba como atualizar um cluster do AKS existente para a versão do Kubernetes mais recente disponível.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 1c0710be11b95b66d16661b5aff9cbf739ccda92
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901944"
---
# <a name="tutorial-upgrade-kubernetes-in-azure-kubernetes-service-aks"></a>Tutorial: Atualizar o Kubernetes no Serviço Kubernetes do Azure (AKS)

Como parte da aplicação e do ciclo de vida do cluster, pode querer atualizar para a versão mais recente disponível do Kubernetes e utilizar novas funcionalidades. Um cluster do Serviço Kubernetes do Azure (AKS) poder ser atualizado através da CLI do Azure. Para minimizar a interrupção das aplicações em execução, os nós do Kubernetes são cuidadosamente [isolados e drenados][kubernetes-drain] durante o processo de atualização.

Neste tutorial, parte sete de sete, é atualizado um cluster do Kubernetes. Saiba como:

> [!div class="checklist"]
> * Identificar versões do Kubernetes atuais e disponíveis
> * Atualizar os nós do Kubernetes
> * Validar uma atualização com êxito

## <a name="before-you-begin"></a>Antes de começar

Nos tutoriais anteriores, foi compactada uma aplicação numa imagem de contentor, carregada esta imagem para o Azure Container Registry e criado um cluster de Kubernetes. A aplicação foi, em seguida, executada no cluster de Kubernetes. Se ainda não concluiu estes passos e pretende acompanhar, regresse ao [Tutorial 1 – Criar imagens de contentor][aks-tutorial-prepare-app].

Este tutorial requer a execução da versão 2.0.44 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure CLI (Instalar o Azure CLI)][azure-cli-install].

## <a name="get-available-cluster-versions"></a>Obter versões de cluster disponíveis

Antes de atualizar um cluster, utilize o comando [az aks get-upgrades][] para verificar que versões do Kubernetes estão disponíveis para atualização:

```azurecli
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

No exemplo seguinte, a versão atual é *1.9.6* e as versões disponíveis são apresentadas na coluna *Atualizações*.

```
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  ----------------------
default  myResourceGroup  1.9.9            1.9.9              1.10.3, 1.10.5, 1.10.6
```

## <a name="upgrade-a-cluster"></a>Atualizar um cluster

Utilize o comando [az aks upgrade][] para atualizar o cluster do AKS. O exemplo seguinte atualiza o cluster para a versão *1.10.6* do Kubernetes.

> [!NOTE]
> Pode atualizar apenas uma versão secundária de cada vez. Por exemplo, pode atualizar da *1.9.6* para a *1.10.3*, mas não pode atualizar da *1.9.6* para a *1.11.x* diretamente. Para atualizar da *1.9.6* para a *1.11.x*, primeiro atualize da *1.9.6* para a *1.10.3* e, em seguida, execute outra atualização da  *1.10.3* para a *1.11.x*.

```azurecli
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.10.6
```

A saída do exemplo seguinte condensado mostra *kubernetesVersion* e apresenta agora *1.10.6*:

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
  "kubernetesVersion": "1.10.6",
  "location": "eastus",
  "name": "myAKSCluster",
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

## <a name="validate-an-upgrade"></a>Validar uma atualização

Confirme se a atualização foi concluída com êxito com o comando [az aks show][], da seguinte forma:

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

O resultado do exemplo seguinte mostra o cluster do AKS a executar a *KubernetesVersion 1.10.6*:

```
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.10.6               Succeeded            myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io
```

## <a name="delete-the-cluster"></a>Eliminar o cluster

Como esta é a última parte da série do tutorial, pode querer eliminar o cluster do AKS. Uma vez que os nós do Kubernetes são executados em máquinas virtuais (VMs) do Azure, continuam a incorrer em custos, mesmo que não utilize o cluster. Utilize o comando [az group delete][az-group-delete] para remover o grupo de recursos, o serviço de contentores e todos os recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Quando elimina o cluster, o principal de serviço do Azure Active Directory utilizado pelo cluster do AKS não é removido. Para obter passos sobre como remover o principal de serviço, consulte [Considerações sobre e eliminação do principal de serviço AKS][sp-delete].

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, atualizou o Kubernetes num cluster do AKS. Aprendeu a:

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
[azure-cli-install]: /cli/azure/install-azure-cli
[az-group-delete]: /cli/azure/group#az-group-delete
[sp-delete]: kubernetes-service-principal.md#additional-considerations
