---
title: Criar um volume estático para pods no Azure Kubernetes Service (AKS)
description: Saiba como criar manualmente um volume com discos do Azure para utilização com pods no Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 09/26/2018
ms.author: iainfou
ms.openlocfilehash: 20c7d20399392e653668953029bcb81886863ce4
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47404624"
---
# <a name="manually-create-and-use-kubernetes-volume-with-azure-disks-in-azure-kubernetes-service-aks"></a>Crie manualmente e utilizar o volume do Kubernetes com os discos do Azure no Azure Kubernetes Service (AKS)

Aplicações baseadas em contentores, muitas vezes, tem de aceder e manter os dados num volume de dados externa. Discos do Azure podem ser utilizados como este arquivo de dados externo. No AKS, volumes podem ser criados dinamicamente usando declarações de volume persistente ou pode criar e anexar um disco do Azure diretamente manualmente. Este artigo mostra-lhe como criar um disco do Azure e anexá-lo a um pod no AKS manualmente.

Para obter mais informações sobre volumes do Kubernetes, consulte [Kubernetes volumes][kubernetes-volumes].

## <a name="before-you-begin"></a>Antes de começar

Este artigo pressupõe que tem um cluster do AKS existente. Se precisar de um cluster do AKS, consulte o guia de introdução do AKS [com a CLI do Azure] [ aks-quickstart-cli] ou [no portal do Azure][aks-quickstart-portal].

Também precisa da versão 2.0.46 da CLI do Azure ou posterior instalado e configurado. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure CLI (Instalar o Azure CLI)][install-azure-cli].

## <a name="create-an-azure-disk"></a>Criar um disco do Azure

Quando cria um disco do Azure para utilização com o AKS, pode criar o recurso de disco no **nó** grupo de recursos. Essa abordagem permite que o cluster do AKS aceder e gerir o recurso de disco. Se criar o disco num grupo de recursos separado em vez disso, tem de conceder o principal de serviço do Azure Kubernetes Service (AKS) para o seu cluster a `Contributor` função para o grupo de recursos do disco.

Neste artigo, crie o disco no grupo de recursos de nó. Primeiro, obtenha o nome do grupo de recursos com o [show do az aks] [ az-aks-show] comando e adicione o `--query nodeResourceGroup` parâmetro de consulta. O exemplo seguinte obtém o grupo de recursos de nó para o nome de cluster do AKS *myAKSCluster* no nome do grupo de recursos *myResourceGroup*:

```azurecli
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Agora, crie um disco com o [criar disco de az] [ az-disk-create] comando. Especificar o nome de grupo de recursos de nó obtido no comando anterior e, em seguida, um nome para o recurso de disco, como *myAKSDisk*. O exemplo seguinte cria um *20*GiB disco e saídas o ID do disco, uma vez criada:

```azurecli-interactive
az disk create \
  --resource-group MC_myResourceGroup_myAKSCluster_eastus \
  --name myAKSDisk  \
  --size-gb 20 \
  --query id --output tsv
```

> [!NOTE]
> Discos do Azure são faturados ao SKU para um tamanho específico. Estes intervalo de SKUs de 32GiB para S4 ou P4 discos a 8TiB para S60 ou P60 discos. O débito e o desempenho de IOPS de uma Premium geridos disco depende do SKU e o tamanho das instâncias de nós do cluster do AKS. Ver [preços e o desempenho de discos geridos][managed-disk-pricing-performance].

O ID de recurso de disco é apresentado quando o comando foi concluída com êxito, conforme mostrado na seguinte saída de exemplo. Este ID de disco é usado para montar o disco no próximo passo.

```console
/subscriptions/<subscriptionID>/resourceGroups/MC_myAKSCluster_myAKSCluster_eastus/providers/Microsoft.Compute/disks/myAKSDisk
```

## <a name="mount-disk-as-volume"></a>Montar o disco como volume

Para montar o disco do Azure no seu pod, configure o volume na especificação do contentor. Crie um novo ficheiro designado `azure-disk-pod.yaml` com o seguinte conteúdo. Atualização `diskName` com o nome do disco criado no passo anterior, e `diskURI` com o ID de disco apresentado na saída do disco de comando de criação. Se assim o desejar, atualize o `mountPath`, que é o caminho onde o disco do Azure está montado no pod.

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

Utilize o `kubectl` comando para criar o pod.

```console
kubectl apply -f azure-disk-pod.yaml
```

Tem agora um pod em execução com um disco do Azure montado em `/mnt/azure`. Pode usar `kubectl describe pod azure-disk-pod` para verificar o disco está montado com êxito.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o AKS clusters interagir com os discos do Azure, consulte a [Plug-in do Kubernetes para os discos do Azure][kubernetes-disks].

<!-- LINKS - external -->
[kubernetes-disks]: https://github.com/kubernetes/examples/blob/master/staging/volumes/azure_disk/README.md
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[az-disk-list]: /cli/azure/disk#az-disk-list
[az-disk-create]: /cli/azure/disk#az-disk-create
[az-group-list]: /cli/azure/group#az-group-list
[az-resource-show]: /cli/azure/resource#az-resource-show
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[az-aks-show]: /cli/azure/aks#az-aks-show
[install-azure-cli]: /cli/azure/install-azure-cli