---
title: Utilizar os discos do Azure com AKS
description: Utilizar os discos do Azure com AKS
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/08/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: e6b2d6e31a843259001a36ffb8c588c879a3f2b9
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="volumes-with-azure-disks"></a>Volumes de discos do Azure

Aplicações baseadas no contentor muitas vezes necessitam de acesso e manter os dados de um volume de dados externas. Discos do Azure podem ser utilizados como este arquivo de dados externas. Este detalhes de artigo utilizando um disco do Azure como um volume Kubernetes de um cluster do serviço de contentor do Azure (AKS).

Para obter mais informações sobre Kubernetes volumes, consulte [Kubernetes volumes][kubernetes-volumes].

## <a name="create-an-azure-disk"></a>Criar um disco do Azure

Antes de montar um Azure geridos disco como um volume Kubernetes, o disco tem de existir no mesmo grupo de recursos, como os recursos de cluster AKS. Para localizar este grupo de recursos, utilize o [lista de grupos de az] [ az-group-list] comando.

```azurecli-interactive
az group list --output table
```

Está à procura de um grupo de recursos com um nome semelhante ao `MC_clustername_clustername_locaton`, em que clustername é o nome do AKS cluster e localização é a região do Azure onde o cluster ter sido implementado.

```console
Name                                 Location    Status
-----------------------------------  ----------  ---------
MC_myAKSCluster_myAKSCluster_eastus  eastus      Succeeded
myAKSCluster                         eastus      Succeeded
```

Utilize o [criar disco de az] [ az-disk-create] comando para criar o disco do Azure. 

Utilizar este exemplo, atualizar `--resource-group` com o nome do grupo de recursos, e `--name` para um nome à sua escolha.

```azurecli-interactive
az disk create \
  --resource-group MC_myAKSCluster_myAKSCluster_eastus \
  --name myAKSDisk  \
  --size-gb 20 \
  --query id --output tsv
```

Depois de criar o disco, deverá ver um resultado semelhante ao seguinte. Este valor é o ID de disco, o que é utilizado ao montar o disco para um pod Kubernetes.

```console
/subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```

## <a name="mount-file-share-as-volume"></a>Montar a partilha de ficheiros como volume

Monte o disco do Azure na sua pod configurando o volume na especificação do contentor. 

Criar um novo ficheiro designado `azure-disk-pod.yaml` com o seguinte conteúdo. Atualização `diskName` com o nome do disco recém-criado e `diskURI` com o ID de disco. Além disso, tome nota do `mountPath`, este é o caminho no qual o disco do Azure está montado no pod.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: azure-disk-pod
spec:
 containers:
  - image: microsoft/sample-aks-helloworld
    name: azure
    volumeMounts:
      - name: azure
        mountPath: /mnt/azure
 volumes:
      - name: azure
        azureDisk:
          kind: Managed
          diskName: myAKSDisk
          diskURI: /subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```

Utilize kubectl para criar o pod.

```azurecli-interactive
kubectl apply -f azure-disk-pod.yaml
```

Tem agora um pod em execução com um disco do Azure montado o `/mnt/azure`.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre os volumes de Kubernetes utilizando discos do Azure.

> [!div class="nextstepaction"]
> [Plug-in do Kubernetes para discos do Azure][kubernetes-disks]

<!-- LINKS - external -->
[kubernetes-disks]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_disk/README.md
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/

<!-- LINKS - internal -->
[az-disk-list]: /cli/azure/disk#az_disk_list
[az-disk-create]: /cli/azure/disk#az_disk_create
[az-group-list]: /cli/azure/group#az_group_list
