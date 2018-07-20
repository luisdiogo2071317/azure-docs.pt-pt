---
title: Criar volumes persistentes com o Azure Kubernetes Service
description: Saiba como utilizar os discos do Azure para criar volumes persistentes para pods no Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/10/2018
ms.author: iainfou
ms.openlocfilehash: 34d3a5dbccf2cad7873bf6166e406c7c4817ac09
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/19/2018
ms.locfileid: "39158714"
---
# <a name="create-persistent-volumes-with-azure-disks-for-azure-kubernetes-service-aks"></a>Criar volumes persistentes com discos do Azure para o Azure Kubernetes Service (AKS)

Um volume persistente representa uma parte do armazenamento aprovisionado para utilização com pods do Kubernetes. Um volume persistente pode ser utilizado por um ou vários pods e pode ser dinamicamente ou estaticamente aprovisionado. Para obter mais informações sobre volumes persistentes do Kubernetes, consulte [volumes persistentes do Kubernetes][kubernetes-volumes]. Este artigo mostra-lhe como utilizar volumes persistentes com discos do Azure num cluster do Azure Kubernetes Service (AKS).

> [!NOTE]
> Um disco do Azure só pode ser montado com *modo de acesso* tipo *ReadWriteOnce*, que disponibiliza a apenas um único nó AKS. Se precisar de partilhar um volume persistente em vários nós, considere utilizar [ficheiros do Azure][azure-files-pvc].

## <a name="built-in-storage-classes"></a>Criado em classes de armazenamento

Uma classe de armazenamento é utilizada para definir como uma unidade de armazenamento é dinamicamente criada com um volume persistente. Para obter mais informações sobre classes de armazenamento do Kubernetes, consulte [Classes de armazenamento do Kubernetes][kubernetes-storage-classes].

Cada cluster do AKS inclui duas classes de armazenamento pré-criados, ambos configurado para trabalhar com os discos do Azure. O *predefinição* classe de armazenamento aprovisiona um disco standard do Azure. O *premium geridos* classe de armazenamento Aprovisiona uma premium disco do Azure. Se os nós do AKS no seu cluster utilizam o armazenamento premium, selecione o *premium geridos* classe.

Utilize o [kubectl obter sc] [ kubectl-get] comando para ver as classes de armazenamento pré-criados. A exemplo a seguir mostra a pré-criar classes de armazenamento disponíveis dentro de um cluster do AKS:

```
$ kubectl get sc

NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

> [!NOTE]
> Volume persistente afirmações são especificadas em GiB mas Azure managed disks são faturados ao SKU para um tamanho específico. Estes intervalo de SKUs de 32GiB para S4 ou P4 discos a 4TiB para S50 ou P50 discos. Além disso, o débito e o desempenho de IOPS de uma Premium geridos disco depende tanto o SKU e o tamanho das instâncias de nós do cluster do AKS. Para obter mais informações, consulte [preço e desempenho dos Managed Disks][managed-disk-pricing-performance].

## <a name="create-a-persistent-volume-claim"></a>Criar uma afirmação de volume persistente

Uma afirmação de volume persistente (PVC) é utilizada para aprovisionar automaticamente o armazenamento com base numa classe de armazenamento. Neste caso, um PVC pode utilizar uma das classes de armazenamento pré-criados para criar um standard ou premium do Azure disco gerido.

Crie um ficheiro denominado `azure-premium.yaml`e a cópia no manifesto do seguinte.

Observe que o *premium geridos* classe de armazenamento é especificada a anotação, e a afirmação está a solicitar um disco *5GB* tamanho com *ReadWriteOnce* acesso.

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

Criar a afirmação de volume persistente com o [kubectl aplicam-se] [ kubectl-apply] de comandos e especificar seu *azure-premium.yaml* ficheiro:

```console
kubectl apply -f azure-premium.yaml
```

## <a name="use-the-persistent-volume"></a>Utilizar o volume persistente

Assim que tiver sido criada a afirmação de volume persistente e o disco foi aprovisionado com êxito, um pod pode ser criado com acesso ao disco. O manifesto seguinte cria um pod que utiliza a afirmação de volume persistente *disco do azure geridos* montar o disco do Azure no `/mnt/azure` caminho.

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

Criar o pod com o [aplicam-se de kubectl] [ kubectl-apply] comando.

```console
kubectl apply -f azure-pvc-disk.yaml
```

Tem agora um pod em execução com o seu disco do Azure montado no `/mnt/azure` diretório. Esta configuração pode ser vista quando inspecionar o seu pod via `kubectl describe pod mypod`.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre os volumes de persistentes Kubernetes com os discos do Azure.

> [!div class="nextstepaction"]
> [Plug-in do Kubernetes para os discos do Azure][azure-disk-volume]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/
[managed-disk-pricing-performance]: https://azure.microsoft.com/pricing/details/managed-disks/

<!-- LINKS - internal -->
[azure-disk-volume]: azure-disk-volume.md
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/premium-storage.md
