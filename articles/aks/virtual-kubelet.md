---
title: Executar virtual kubelet num cluster do Azure Kubernetes serviço (AKS)
description: Utilize virtual kubelet para executar Kubernetes contentores em instâncias de contentor do Azure.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/12/2018
ms.author: iainfou
ms.openlocfilehash: 04fdb1620dc6e7147ed10ae6eeeaeb3eeae14b62
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37097364"
---
# <a name="virtual-kubelet-with-aks"></a>Virtual Kubelet com AKS

Instâncias de contentor do Azure (ACI) fornece um ambiente alojado para a execução de contentores no Azure. Quando utilizar ACI, não é necessário para gerir a infraestrutura de computação subjacente, Azure processa esta gestão por si. Ao executar contentores no ACI, são-lhe cobrados por segundo para cada contentor em execução.

Quando utilizar o fornecedor de Virtual Kubelet para instâncias de contentor do Azure, os contentores de Linux e Windows podem ser agendados uma instância do contentor como se fosse um nó de Kubernetes padrão. Esta configuração permite-lhe tirar partido das capacidades de Kubernetes e o benefício de valor e o custo de gestão de instâncias de contentor.

> [!NOTE]
> Virtual Kubelet for um projeto de código aberto experimental e deve ser utilizada como tal. Para contribuir, problemas de ficheiros e ler mais sobre virtual kubelet, consulte o [Virtual Kubelet GitHub projeto][vk-github].

Esta configuração o Kubelet Virtual para instâncias de contentor num AKS detalhes de documento.

## <a name="prerequisite"></a>Pré-requisito

Este documento parte do princípio de que tem um cluster AKS. Se precisar de um cluster AKS, consulte o [início rápido do Azure Kubernetes serviço (AKS)][aks-quick-start].

Também precisa da versão da CLI do Azure **2.0.33** ou posterior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli).

[Helm](https://docs.helm.sh/using_helm/#installing-helm) também é necessário para instalar o Kubelet Virtual.

## <a name="installation"></a>Instalação

Utilize o [az aks install-conector] [ aks-install-connector] comando para instalar Virtual Kubelet. O exemplo seguinte implementa o conector de Linux e Windows.

```azurecli-interactive
az aks install-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet --os-type Both
```

Estes argumentos estão disponíveis para o `aks install-connector` comando.

| Argumento: | Descrição | Necessário |
|---|---|:---:|
| `--connector-name` | Nome do conector ACI.| Sim |
| `--name` `-n` | Nome do cluster gerido. | Sim |
| `--resource-group` `-g` | Nome do grupo de recursos. | Sim |
| `--os-type` | Tipo de sistema operativo de instâncias de contentor. Valores permitidos: ambos, Linux, Windows. Predefinição: Linux. | Não |
| `--aci-resource-group` | O grupo de recursos no qual pretende criar os grupos de contentor ACI. | Não |
| `--location` `-l` | A localização para criar os grupos de contentor ACI. | Não |
| `--service-principal` | Principal de serviço utilizado para autenticação de APIs do Azure. | Não |
| `--client-secret` | Segredo associado ao serviço principal. | Não |
| `--chart-url` | URL de um gráfico de Helm que instala o conector ACI. | Não |
| `--image-tag` | A tag de imagem da imagem de contentor virtual kubelet. | Não |

## <a name="validate-virtual-kubelet"></a>Validar Virtual Kubelet

Para validar que Virtual Kubelet foi instalado, devolver uma lista de nós de Kubernetes utilizando o [kubectl obter nós] [ kubectl-get] comando.

```console
$ kubectl get nodes

NAME                                    STATUS    ROLES     AGE       VERSION
aks-nodepool1-23443254-0                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-1                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-2                Ready     agent     16d       v1.9.6
virtual-kubelet-virtual-kubelet-linux   Ready     agent     4m        v1.8.3
virtual-kubelet-virtual-kubelet-win     Ready     agent     4m        v1.8.3
```

## <a name="run-linux-container"></a>Execute o contentor do Linux

Crie um ficheiro denominado `virtual-kubelet-linux.yaml` e copie o seguinte YAML. Substitua o `kubernetes.io/hostname` valor com o nome do nó Kubelet virtuais do Linux. Tome nota que um [nodeSelector] [ node-selector] e [toleration] [ toleration] estão a ser utilizadas para agendar o contentor no nó.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: aci-helloworld
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: aci-helloworld
    spec:
      containers:
      - name: aci-helloworld
        image: microsoft/aci-helloworld
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/hostname: virtual-kubelet-virtual-kubelet-linux
      tolerations:
      - key: azure.com/aci
        effect: NoSchedule
```

Execute a aplicação com o [kubectl criar] [ kubectl-create] comando.

```azurecli-interactive
kubectl create -f virtual-kubelet-linux.yaml
```

Utilize o [kubectl obter pods] [ kubectl-get] comando com o `-o wide` argumento para uma lista de pods com o nó agendado de saída. Tenha em atenção que o `aci-helloworld` pod agendado no `virtual-kubelet-virtual-kubelet-linux` nós.

```console
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
aci-helloworld-2559879000-8vmjw     1/1       Running   0          39s       52.179.3.180   virtual-kubelet-virtual-kubelet-linux
```

## <a name="run-windows-container"></a>Execute o contentor do Windows

Crie um ficheiro denominado `virtual-kubelet-windows.yaml` e copie o seguinte YAML. Substitua o `kubernetes.io/hostname` valor com o nome do nó Kubelet Virtual do Windows. Tome nota que um [nodeSelector] [ node-selector] e [toleration] [ toleration] estão a ser utilizadas para agendar o contentor no nó.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: nanoserver-iis
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: nanoserver-iis
    spec:
      containers:
      - name: nanoserver-iis
        image: nanoserver/iis
        ports:
        - containerPort: 80
      nodeSelector:
        kubernetes.io/hostname: virtual-kubelet-virtual-kubelet-win
      tolerations:
      - key: azure.com/aci
        effect: NoSchedule
```

Execute a aplicação com o [kubectl criar] [ kubectl-create] comando.

```azurecli-interactive
kubectl create -f virtual-kubelet-windows.yaml
```

Utilize o [kubectl obter pods] [ kubectl-get] comando com o `-o wide` argumento para uma lista de pods com o nó agendado de saída. Tenha em atenção que o `nanoserver-iis` pod agendado no `virtual-kubelet-virtual-kubelet-win` nós.

```console
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
nanoserver-iis-868bc8d489-tq4st     1/1       Running   8         21m       138.91.121.91   virtual-kubelet-virtual-kubelet-win
```

## <a name="remove-virtual-kubelet"></a>Remover Virtual Kubelet

Utilize o [az aks remove-conector] [ aks-remove-connector] comando para remover Virtual Kubelet. Substitua os valores de argumento com o nome do conector, AKS cluster e o grupo de recursos de cluster AKS.

```azurecli-interactive
az aks remove-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre Kubelet Virtual no [Virtual Kubelet Github projet][vk-github].

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-remove-connector]: /cli/azure/aks#az-aks-remove-connector
[az-container-list]: /cli/azure/aks#az_aks_list
[aks-install-connector]: /cli/azure/aks#az-aks-install-connector

<!-- LINKS - external -->
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/user-guide/kubectl/v1.8/#get
[node-selector]:https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[toleration]: https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/
[vk-github]: https://github.com/virtual-kubelet/virtual-kubelet
