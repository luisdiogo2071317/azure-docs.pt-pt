---
title: SSH para nós de cluster do serviço de Kubernetes do Azure (AKS)
description: Criar uma ligação SSH com um cluster do serviço de Kubernetes do Azure (AKS) nós
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/06/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 95b385e9847a7809492bbb74bd1eba616df90d72
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="ssh-into-azure-kubernetes-service-aks-cluster-nodes"></a>SSH para nós de cluster do serviço de Kubernetes do Azure (AKS)

Ocasionalmente, poderá ter de aceder a um nó de Azure Kubernetes serviço (AKS) para manutenção, recolha de registos ou outras operações de resolução de problemas. Nós AKS não são expostos à internet. Utilize os passos detalhados neste documento para criar uma ligação SSH com um nó AKS.

## <a name="reset-ssh-keys"></a>Repor chaves SSH

Se tiver implementado um AKS sem chaves SSH, ou não tem acesso às chaves SSH adequadas, estes podem ser repostos utilizando o portal do Azure.

Navegue para o cluster AKS, selecione um nó AKS (máquina virtual) e selecione **Repor palavra-passe** para repor a chave pública SSH.

![AKS VM com o botão do reposição de palavra-passe](media/aks-ssh/reset-password.png)

Selecione **repor chave pública SSH**, introduza o AKS cluster nome de utilizador, que é **azueruser** por predefinição e a cópia de uma chave pública SSH. Selecione **atualização** quando concluir.

![Portal AKS VM com o botão do reposição de palavra-passe](media/aks-ssh/reset-password-2.png)

Assim que a chave SSH foi reposta, pode criar uma ligação de SSH utilizando a chave privada correspondente.

## <a name="get-aks-node-address"></a>Obter o endereço do nó AKS

Obter o endereço IP de uma utilização de nó de cluster do AKS o `az vm list-ip-addresses` comando. Substitua o nome do grupo de recursos com o nome do grupo de recursos AKS.

```console
$ az vm list-ip-addresses --resource-group MC_myAKSCluster_myAKSCluster_eastus -o table

VirtualMachine            PrivateIPAddresses
------------------------  --------------------
aks-nodepool1-42032720-0  10.240.0.6
aks-nodepool1-42032720-1  10.240.0.5
aks-nodepool1-42032720-2  10.240.0.4
```

## <a name="create-ssh-connection"></a>Criar a ligação SSH

Execute o `debian` contentor imagem e anexar uma sessão de terminal. O contentor, em seguida, pode ser utilizado para criar uma sessão SSH com qualquer nó no AKS cluster.

```console
kubectl run -it --rm aks-ssh --image=debian
```

Instale um cliente SSH no contentor.

```console
apt-get update && apt-get install openssh-client -y
```

Abra o terminal um segundo e lista todos os pods de obter o nome de pod recentemente criado.

```console
$ kubectl get pods

NAME                       READY     STATUS    RESTARTS   AGE
aks-ssh-554b746bcf-kbwvf   1/1       Running   0          1m
```

Copie a chave SSH privada para o pod, substitua o nome de pod com o valor adequado.

```console
kubectl cp ~/.ssh/id_rsa aks-ssh-554b746bcf-kbwvf:/id_rsa
```

Atualização do `id_rsa` para que seja utilizador só de leitura de ficheiros.

```console
chmod 0600 id_rsa
```

Crie uma ligação SSH para o nó AKS. O nome de utilizador predefinido para um cluster AKS é `azureuser`. Se esta conta foi alterada no momento de criação do cluster, substitua o nome de utilizador de admin adequado.

```console
$ ssh -i id_rsa azureuser@10.240.0.6

Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.11.0-1016-azure x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

48 packages can be updated.
0 updates are security updates.


*** System restart required ***
Last login: Tue Feb 27 20:10:38 2018 from 167.220.99.8
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@aks-nodepool1-42032720-0:~$
```

## <a name="remove-ssh-access"></a>Remover o acesso SSH

Quando terminar, saia da sessão SSH e, em seguida, a sessão interativa de contentor. Esta ação elimina pod utilizado para acesso SSH do AKS cluster.
