---
title: Utilizar os discos do Azure com AKS
description: Utilizar os discos do Azure com AKS
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/21/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: cfad5ebd1212df03cee86b71340d8a73c2594f57
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096098"
---
# <a name="volumes-with-azure-disks"></a>Volumes de discos do Azure

Aplicações baseadas no contentor muitas vezes necessitam de acesso e manter os dados de um volume de dados externas. Discos do Azure podem ser utilizados como este arquivo de dados externas. Este detalhes de artigo utilizando um disco do Azure como um volume Kubernetes num cluster do Azure Kubernetes serviço (AKS).

Para obter mais informações sobre Kubernetes volumes, consulte [Kubernetes volumes][kubernetes-volumes].

## <a name="create-an-azure-disk"></a>Criar um disco do Azure

Antes de montar um disco gerida do Azure como um volume Kubernetes, o disco tem de existir o AKS **nó** grupo de recursos. Obter o nome do grupo de recursos com o [mostrar de recurso az] [ az-resource-show] comando.

```azurecli-interactive
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Utilize o [criar disco de az] [ az-disk-create] comando para criar o disco do Azure.

Atualização `--resource-group` com o nome do grupo de recursos recolhidos no último passo, e `--name` para um nome à sua escolha.

```azurecli-interactive
az disk create \
  --resource-group MC_myResourceGroup_myAKSCluster_eastus \
  --name myAKSDisk  \
  --size-gb 20 \
  --query id --output tsv
```

Depois de criar o disco, deverá ver um resultado semelhante ao seguinte. Este valor é o ID de disco, o que é utilizado ao montar o disco.

```console
/subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```

## <a name="mount-disk-as-volume"></a>Montar o disco como volume

Monte o disco do Azure na sua pod configurando o volume na especificação do contentor.

Criar um novo ficheiro designado `azure-disk-pod.yaml` com o seguinte conteúdo. Atualização `diskName` com o nome do disco recém-criado e `diskURI` com o ID de disco. Além disso, tome nota do `mountPath`, que é o caminho onde o disco do Azure está montado no pod.

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
[az-resource-show]: /cli/azure/resource#az-resource-show
