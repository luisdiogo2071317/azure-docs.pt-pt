---
title: Utilizar o disco do Azure com AKS
description: Utilizar os discos do Azure com AKS
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 1/25/2018
ms.author: nepeters
ms.openlocfilehash: e1f5b68d5d39dd846ebec525d1e83a6c0ef4971a
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="persistent-volumes-with-azure-disks---dynamic-provisioning"></a>Aprovisionamento de volumes persistentes com discos do Azure - dinâmicos

Um volume persistente representa um conjunto de armazenamento que tenha sido aprovisionado para utilização num Kubernetes cluster. Um volume persistente pode ser utilizado por um ou vários pods e pode ser dinamicamente ou estaticamente aprovisionado. Este documento fornece detalhes sobre o aprovisionamento dinâmico de um disco do Azure como um volume persistente Kubernetes num AKS cluster. 

Para obter mais informações sobre volumes persistentes Kubernetes, consulte [volumes persistentes Kubernetes][kubernetes-volumes].

## <a name="built-in-storage-classes"></a>Criado em classes de armazenamento

Uma classe de armazenamento é utilizada para definir como um volume persistente dinamicamente criado está configurado. Para obter mais informações sobre Kubernetes classes de armazenamento, consulte [Kubernetes armazenamento Classes][kubernetes-storage-classes].

Cada cluster AKS inclui duas classes de armazenamento pré-criada, ambos configurado para trabalhar com discos do Azure. Utilize o `kubectl get storageclass` comando para vê-los.

```console
NAME                PROVISIONER                AGE
default (default)   kubernetes.io/azure-disk   1h
managed-premium     kubernetes.io/azure-disk   1h
```

Se estas classes de armazenamento funcionam para as suas necessidades, não terá de criar um novo.

## <a name="create-storage-class"></a>Criar a classe de armazenamento

Se gostaria de criar uma nova classe de armazenamento configurada para discos do Azure, pode fazê-lo ao utilizar o manifesto de exemplo seguinte. 

O `storageaccounttype` valor `Standard_LRS` indica que é criado um disco padrão. Este valor pode ser alterado para `Premium_LRS` para criar um [disco premium][premium-storage]. Para utilizar discos premium, a máquina de virtual do nó AKS tem de ter um tamanho compatível com discos premium. Consulte [neste documento] [ premium-storage] para obter uma lista de tamanhos compatíveis.

```yaml
apiVersion: storage.k8s.io/v1beta1
kind: StorageClass
metadata:
  name: azure-managed-disk
provisioner: kubernetes.io/azure-disk
parameters:
  kind: Managed
  storageaccounttype: Standard_LRS
```

## <a name="create-persistent-volume-claim"></a>Criar afirmações volume persistente

Uma afirmação de volume persistente utiliza um objeto de classe de armazenamento para aprovisionar dinamicamente um conjunto de armazenamento. Quando utilizar um disco do Azure, o disco é criado no mesmo grupo de recursos, como os recursos AKS.

O manifesto de exemplo cria uma afirmação de volume persistente utilizando o `azure-managed-disk` classe de armazenamento, para criar um disco `5GB` tamanho com `ReadWriteOnce` acesso. Para obter mais informações sobre modos de acesso de PVC, consulte [modos de acesso][access-modes].

> [!NOTE]
> Um disco do Azure só pode ser montado com o tipo de modo de acesso ReadWriteOnce, o que torna a mesma disponível para um único nó AKS. Se precisar de partilhar um volume persistente em vários nós, considere utilizar [ficheiros do Azure][azure-files-pvc]. 

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
  annotations:
    volume.beta.kubernetes.io/storage-class: azure-managed-disk
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

## <a name="using-the-persistent-volume"></a>Utilizar o volume persistente

Depois da afirmação persistente volume foi criada, e o disco aprovisionado com êxito, um pod pode ser criado com acesso ao disco. O manifesto seguinte cria um pod que utiliza a afirmação de volume persistente `azure-managed-disk` montar o disco do Azure, o `/var/www/html` caminho. 

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
      - mountPath: "/var/www/html"
        name: volume
  volumes:
    - name: volume
      persistentVolumeClaim:
        claimName: azure-managed-disk
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre os volumes de persistentes Kubernetes utilizando discos do Azure.

> [!div class="nextstepaction"]
> [Plug-in do Kubernetes para discos do Azure][kubernetes-disk]

<!-- LINKS - external -->
[access-modes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#access-modes
[kubernetes-disk]: https://kubernetes.io/docs/concepts/storage/storage-classes/#new-azure-disk-storage-class-starting-from-v172
[kubernetes-storage-classes]: https://kubernetes.io/docs/concepts/storage/storage-classes/
[kubernetes-volumes]: https://kubernetes.io/docs/concepts/storage/persistent-volumes/

<!-- LINKS - internal -->
[azure-files-pvc]: azure-files-dynamic-pv.md
[premium-storage]: ../virtual-machines/windows/premium-storage.md