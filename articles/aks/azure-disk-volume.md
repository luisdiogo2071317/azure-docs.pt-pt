---
title: Utilizar discos do Azure com o AKS
description: Utilizar discos do Azure com o AKS
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/21/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: f807264dc2c2e07ccd175fb1b0427b7ce9e9f524
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868250"
---
# <a name="volumes-with-azure-disks"></a>Volumes com os discos do Azure

Aplicações baseadas em contentores, muitas vezes, tem de aceder e manter os dados num volume de dados externa. Discos do Azure podem ser utilizados como este arquivo de dados externo. Este artigo descreve a utilização de um disco do Azure como um volume do Kubernetes num cluster do Azure Kubernetes Service (AKS).

Para obter mais informações sobre volumes do Kubernetes, consulte [Kubernetes volumes][kubernetes-volumes].

## <a name="create-an-azure-disk"></a>Criar um disco do Azure

Antes de montar um disco gerido do Azure como um volume do Kubernetes, o disco tem de existir do AKS **nó** grupo de recursos. Obtenha o nome do grupo de recursos com o [show de recursos de az] [ az-resource-show] comando.

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

Quando o disco tiver sido criado, deverá ver um resultado semelhante ao seguinte. Este valor é o ID de disco, o que é utilizado quando a montagem do disco.

```console
/subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```
> [!NOTE]
> Managed disks do Azure são faturados ao SKU para um tamanho específico. Estes intervalo de SKUs de 32GiB para S4 ou P4 discos a 4TiB para S50 ou P50 discos. Além disso, o débito e o desempenho de IOPS de uma Premium geridos disco depende do SKU e o tamanho das instâncias de nós do cluster do AKS. Ver [preços e o desempenho de discos geridos][managed-disk-pricing-performance].

> [!NOTE]
> Se precisar de criar o disco no grupo de recursos separado, terá também de adicionar o principal de serviço do Azure Kubernetes Service (AKS) para o seu cluster para o grupo de recursos que contém o disco com o `Contributor` função. 
>

## <a name="mount-disk-as-volume"></a>Montar o disco como volume

Monte o disco do Azure no seu pod ao configurar o volume na especificação do contentor.

Crie um novo ficheiro designado `azure-disk-pod.yaml` com o seguinte conteúdo. Atualização `diskName` com o nome do disco recém-criado e `diskURI` com o ID de disco. Além disso, observe o `mountPath`, que é o caminho onde o disco do Azure está montado no pod.

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

Utilize o kubectl para criar o pod.

```azurecli-interactive
kubectl apply -f azure-disk-pod.yaml
```

Tem agora um pod em execução com um disco do Azure montado no `/mnt/azure`.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre os volumes de Kubernetes com os discos do Azure.

> [!div class="nextstepaction"]
> [Plug-in do Kubernetes para os discos do Azure][kubernetes-disks]

<!-- LINKS - external -->
[kubernetes-disks]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_disk/README.md
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[az-disk-list]: /cli/azure/disk#az_disk_list
[az-disk-create]: /cli/azure/disk#az_disk_create
[az-group-list]: /cli/azure/group#az_group_list
[az-resource-show]: /cli/azure/resource#az-resource-show
