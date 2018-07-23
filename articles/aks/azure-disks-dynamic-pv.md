---
title: Criar volumes persistentes com o Azure Kubernetes Service
description: Saiba como utilizar os discos do Azure para criar volumes persistentes para pods no Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/20/2018
ms.author: iainfou
ms.openlocfilehash: 129a39294d52a312c91fc6a4fd009d76e88b4ff7
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185227"
---
# <a name="create-persistent-volumes-with-azure-disks-for-azure-kubernetes-service-aks"></a>Criar volumes persistentes com discos do Azure para o Azure Kubernetes Service (AKS)

Um volume persistente representa uma parte do armazenamento aprovisionado para utilização com pods do Kubernetes. Um volume persistente pode ser utilizado por um ou vários pods e pode ser dinamicamente ou estaticamente aprovisionado. Para obter mais informações sobre volumes persistentes do Kubernetes, consulte [volumes persistentes do Kubernetes][kubernetes-volumes]. Este artigo mostra-lhe como utilizar volumes persistentes com discos do Azure num cluster do Azure Kubernetes Service (AKS).

> [!NOTE]
> Um disco do Azure só pode ser montado com *modo de acesso* tipo *ReadWriteOnce*, que disponibiliza a apenas um único nó AKS. Se precisar de partilhar um volume persistente em vários nós, considere utilizar [ficheiros do Azure][azure-files-pvc].

## <a name="built-in-storage-classes"></a>Criado em classes de armazenamento

Uma classe de armazenamento é utilizada para definir como uma unidade de armazenamento é dinamicamente criada com um volume persistente. Para obter mais informações sobre classes de armazenamento do Kubernetes, consulte [Classes de armazenamento do Kubernetes][kubernetes-storage-classes].

Cada cluster do AKS inclui duas classes de armazenamento pré-criados, ambos configurado para trabalhar com os discos do Azure:

* O *predefinição* classe de armazenamento aprovisiona um disco standard do Azure.
    * Armazenamento Standard está protegido por HDDs e fornece armazenamento económico, mantendo o alto desempenho. Os discos Standard são ideais para uma carga de trabalho de desenvolvimento e teste económica.
* O *premium geridos* classe de armazenamento Aprovisiona uma premium disco do Azure.
    * Os discos Premium são apoiados por um disco de elevado desempenho baseado em SSD e de baixa latência. São perfeitos para as VMs com carga de trabalho de produção. Se os nós do AKS no seu cluster utilizam o armazenamento premium, selecione o *premium geridos* classe.

Utilize o [kubectl obter sc] [ kubectl-get] comando para ver as classes de armazenamento pré-criados. A exemplo a seguir mostra a pré-criar classes de armazenamento disponíveis dentro de um cluster do AKS:

```
$ kubectl get sc

NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

> [!NOTE]
> Volume persistente afirmações são especificadas em GiB mas Azure managed disks são faturados ao SKU para um tamanho específico. Estes intervalo de SKUs de 32GiB para S4 ou P4 discos a 4TiB para S50 ou P50 discos. O débito e o desempenho de IOPS de uma Premium geridos disco depende tanto o SKU e o tamanho das instâncias de nós do cluster do AKS. Para obter mais informações, consulte [preço e desempenho dos Managed Disks][managed-disk-pricing-performance].

## <a name="create-a-persistent-volume-claim"></a>Criar uma afirmação de volume persistente

Uma afirmação de volume persistente (PVC) é utilizada para aprovisionar automaticamente o armazenamento com base numa classe de armazenamento. Neste caso, um PVC pode utilizar uma das classes de armazenamento pré-criados para criar um standard ou premium do Azure disco gerido.

Crie um ficheiro denominado `azure-premium.yaml`e a cópia no manifesto do seguinte. A afirmação solicita um disco chamado `azure-managed-disk` isto é *5GB* de tamanho com *ReadWriteOnce* acesso. O *premium geridos* classe de armazenamento está especificada como a classe de armazenamento.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  storageClassName: managed-premium
  resources:
    requests:
      storage: 5Gi
```

> [!TIP]
> Para criar um disco que utiliza o armazenamento standard, utilize `storageClassName: default` em vez *premium geridos*.

Criar a afirmação de volume persistente com o [kubectl criar] [ kubectl-create] de comandos e especificar seu *azure-premium.yaml* ficheiro:

```
$ kubectl create -f azure-premium.yaml

persistentvolumeclaim/azure-managed-disk created
```

## <a name="use-the-persistent-volume"></a>Utilizar o volume persistente

Assim que tiver sido criada a afirmação de volume persistente e o disco foi aprovisionado com êxito, um pod pode ser criado com acesso ao disco. O manifesto seguinte cria um pod NGINX básico que utiliza com o nome de afirmações de volume persistente *disco do azure geridos* montar o disco do Azure no caminho `/mnt/azure`.

Crie um ficheiro denominado `azure-pvc-disk.yaml`e a cópia no manifesto do seguinte.

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```

Criar o pod com o [criar kubectl] [ kubectl-create] de comando, conforme mostrado no exemplo a seguir:

```
$ kubectl create -f azure-pvc-disk.yaml

pod/mypod created
```

Tem agora um pod em execução com o seu disco do Azure montado no `/mnt/azure` diretório. Esta configuração pode ser vista quando inspecionar o seu pod via `kubectl describe pod mypod`, conforme mostrado no seguinte exemplo condensado:

```
$ kubectl describe pod mypod

[...]
Volumes:
  volume:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  azure-managed-disk
    ReadOnly:   false
  default-token-smm2n:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-smm2n
    Optional:    false

Events:
  Type    Reason                 Age   From                               Message
  ----    ------                 ----  ----                               -------
  Normal  Scheduled              2m    default-scheduler                  Successfully assigned mypod to aks-nodepool1-79590246-0
  Normal  SuccessfulMountVolume  2m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "default-token-smm2n"
  Normal  SuccessfulMountVolume  1m    kubelet, aks-nodepool1-79590246-0  MountVolume.SetUp succeeded for volume "pvc-faf0f176-8b8d-11e8-923b-deb28c58d242"
[...]
```

## <a name="back-up-a-persistent-volume"></a>Fazer uma cópia de segurança de um volume persistente

Para fazer backup dos dados no seu volume persistente, tire um instantâneo do disco gerido para o volume. Em seguida, pode utilizar este instantâneo para criar um disco restaurado e anexar a pods como um meio de restauro dos dados.

Em primeiro lugar, obtenha o nome de volume com o `kubectl get pvc` comando, tal como para PVC com o nome *disco do azure gerido*:

```
$ kubectl get pvc azure-managed-disk

NAME                 STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS      AGE
azure-managed-disk   Bound     pvc-faf0f176-8b8d-11e8-923b-deb28c58d242   5Gi        RWO            managed-premium   3m
```

Este nome de volume forma o nome de disco do Azure subjacente. Consulta para o ID de disco com [lista de disco de az] [ az-disk-list] e indique o seu nome de volume PVC, conforme mostrado no exemplo a seguir:

```
$ az disk list --query '[].id|[?contains(@,`pvc-faf0f176-8b8d-11e8-923b-deb28c58d242`)]' -o tsv

/subscriptions/<guid>/resourceGroups/MC_MYRESOURCEGROUP_MYAKSCLUSTER_EASTUS/providers/MicrosoftCompute/disks/kubernetes-dynamic-pvc-faf0f176-8b8d-11e8-923b-deb28c58d242
```

Utilize o ID do disco para criar um disco de instantâneo com [criar instantâneo de az][az-snapshot-create]. O exemplo seguinte cria um instantâneo com o nome *pvcSnapshot* no mesmo grupo de recursos que o cluster do AKS (*MC_myResourceGroup_myAKSCluster_eastus*). Se criar instantâneos e restaurar discos em grupos de recursos que o cluster do AKS não tem acesso a, pode encontrar problemas de permissão.

```azurecli
$ az snapshot create \
    --resource-group MC_myResourceGroup_myAKSCluster_eastus \
    --name pvcSnapshot \
    --source /subscriptions/<guid>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/MicrosoftCompute/disks/kubernetes-dynamic-pvc-faf0f176-8b8d-11e8-923b-deb28c58d242
```

Dependendo da quantidade de dados no disco, poderá demorar alguns minutos para criar o instantâneo.

## <a name="restore-and-use-a-snapshot"></a>Restaurar e utilizar um instantâneo

Para restaurar o disco e utilizá-lo com um pod de Kubernetes, utilize o instantâneo como uma origem ao criar um disco com [criar disco de az][az-disk-create]. Esta operação preserva o recurso original se, em seguida, precisa acessar o instantâneo de dados original. O exemplo seguinte cria um disco chamado *pvcRestored* partir do instantâneo com o nome *pvcSnapshot*:

```azurecli
az disk create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name pvcRestored --source pvcSnapshot
```

Para utilizar o disco restaurado com um pod, especifica o ID do disco no manifesto. Obter o ID de disco com o [show de disco de az] [ az-disk-show] comando. O exemplo seguinte obtém o ID do disco para *pvcRestored* criado no passo anterior:

```azurecli
az disk show --resource-group MC_myResourceGroup_myAKSCluster_eastus --name pvcRestored --query id -o tsv
```

Criar um manifesto de pod com o nome `azure-restored.yaml` e especifique o disco URI que obteve no passo anterior. O exemplo seguinte cria um servidor de web NGINX básico, com o disco restaurado montado como um volume em */mnt/azure*:

```yaml
kind: Pod
apiVersion: v1
metadata:
  name: mypodrestored
spec:
  containers:
    - name: myfrontendrestored
      image: nginx
      volumeMounts:
      - mountPath: "/mnt/azure"
        name: volume
  volumes:
    - name: volume
      azureDisk:
        kind: Managed
        diskName: pvcRestored
        diskURI: /subscriptions/<guid>/resourceGroups/MC_myResourceGroupAKS_myAKSCluster_eastus/providers/Microsoft.Compute/disks/pvcRestored
```

Criar o pod com o [criar kubectl] [ kubectl-create] de comando, conforme mostrado no exemplo a seguir:

```
$ kubectl create -f azure-restored.yaml

pod/mypodrestored created
```

Pode usar `kubectl describe pod mypodrestored` para ver os detalhes de pod, como o seguinte exemplo condensado, que mostra as informações de volume:

```
$ kubectl describe pod mypodrestored

[...]
Volumes:
  volume:
    Type:         AzureDisk (an Azure Data Disk mount on the host and bind mount to the pod)
    DiskName:     pvcRestored
    DiskURI:      /subscriptions/19da35d3-9a1a-4f3b-9b9c-3c56ef409565/resourceGroups/MC_myResourceGroupAKS_myAKSCluster_eastus/providers/Microsoft.Compute/disks/pvcRestored
    Kind:         Managed
    FSType:       ext4
    CachingMode:  ReadWrite
    ReadOnly:     false
[...]
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre os volumes de persistentes Kubernetes com os discos do Azure.

> [!div class="nextstepaction"]
> [Plug-in do Kubernetes para os discos do Azure][azure-disk-volume]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[azure-disk-volume]: azure-disk-volume.md
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/premium-storage.md
[az-disk-list]: /cli/azure/disk#az-disk-list
[az-snapshot-create]: /cli/azure/snapshot#az-snapshot-create
[az-disk-create]: /cli/azure/disk#az-disk-create
[az-disk-show]: /cli/azure/disk#az-disk-show
