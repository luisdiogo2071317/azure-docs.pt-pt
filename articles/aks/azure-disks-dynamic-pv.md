---
title: Utilizar o disco do Azure com AKS
description: Utilizar os discos do Azure com AKS
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/06/2018
ms.author: nepeters
ms.openlocfilehash: 36e25d7e5f1e5c6e1cf72442b73ac081810d216a
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="persistent-volumes-with-azure-disks"></a>Volumes persistentes com discos do Azure

Um volume persistente representa um conjunto de armazenamento que tenha sido aprovisionado para utilização com pods de Kubernetes. Um volume persistente pode ser utilizado por um ou vários pods e pode ser dinamicamente ou estaticamente aprovisionado. Para obter mais informações sobre volumes persistentes Kubernetes, consulte [volumes persistentes Kubernetes][kubernetes-volumes].

Este detalhes de documento com persistentes volumes de discos do Azure de um cluster do serviço de contentor do Azure (AKS).

## <a name="built-in-storage-classes"></a>Criado em classes de armazenamento

Uma classe de armazenamento é utilizada para definir como uma unidade de armazenamento dinamicamente é criada com um volume persistente. Para obter mais informações sobre Kubernetes classes de armazenamento, consulte [Kubernetes armazenamento Classes][kubernetes-storage-classes].

Cada cluster AKS inclui duas classes de armazenamento pré-criada, ambos configurado para trabalhar com discos do Azure. O `default` classe de armazenamento aprovisiona um disco do Azure standard. O `managed-premium` classe de armazenamento aprovisiona um premium disco do Azure. Se os nós AKS no seu cluster utilizam o armazenamento premium, selecione o `managed-premium` classe.

Utilize o [kubectl obter sc] [ kubectl-get] comando para ver as classes de armazenamento criada previamente.

```console
NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

## <a name="create-persistent-volume-claim"></a>Criar afirmações volume persistente

Uma afirmação de volume persistente (PVC) é utilizada para aprovisionar automaticamente o armazenamento baseado numa classe de armazenamento. Neste caso, um PVC pode utilizar uma das classes de armazenamento previamente criada para criar um Azure standard ou premium disco gerido.

Crie um ficheiro denominado `azure-premimum.yaml`e copie o manifesto seguinte.

Tome nota que o `managed-premium` classe de armazenamento é especificado em que a anotação e a afirmação está a pedir um disco `5GB` tamanho com `ReadWriteOnce` acesso. 

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
  annotations:
    volume.beta.kubernetes.io/storage-class: managed-premium
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

Criar a afirmação de volume persistente com o [kubectl criar] [ kubectl-create] comando.

```azurecli-interactive
kubectl create -f azure-premimum.yaml
```

> [!NOTE]
> Um disco do Azure só pode ser montado com o tipo de modo de acesso ReadWriteOnce, o que torna a mesma disponível para um único nó AKS. Se precisar de partilhar um volume persistente em vários nós, considere utilizar [ficheiros do Azure][azure-files-pvc].

## <a name="using-the-persistent-volume"></a>Utilizar o volume persistente

Depois da afirmação persistente volume foi criada, e o disco aprovisionado com êxito, um pod pode ser criado com acesso ao disco. O manifesto seguinte cria um pod que utiliza a afirmação de volume persistente `azure-managed-disk` montar o disco do Azure, o `/mnt/azure` caminho. 

Crie um ficheiro denominado `azure-pvc-disk.yaml`e copie o manifesto seguinte.

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

Criar pod com o [kubectl criar] [ kubectl-create] comando.

```azurecli-interactive
kubectl create -f azure-pvc-disk.yaml
```

Tem agora um pod em execução com o Azure disco montado no `/mnt/azure` diretório. Pode ver o quando inspecionar o pod através de montagem do volume `kubectl describe pod mypod`.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre os volumes de persistentes Kubernetes utilizando discos do Azure.

> [!div class="nextstepaction"]
> [Plug-in do Kubernetes para discos do Azure][kubernetes-disk]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-disk]: https://kubernetes.io/docs/concepts/storage/storage-classes/#new-azure-disk-storage-class-starting-from-v172
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/premium-storage.md