---
title: Executar o Virtual Kubelet num cluster do Azure Kubernetes Service (AKS)
description: Saiba como utilizar o Virtual Kubelet com o Azure Kubernetes Service (AKS) para executar contentores de Linux e Windows no Azure Container Instances.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/12/2018
ms.author: iainfou
ms.openlocfilehash: 0466f416568b2a1a82e264a8508697fc9de87287
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952483"
---
# <a name="use-virtual-kubelet-with-azure-kubernetes-service-aks"></a>Utilizar o Virtual Kubelet com o serviço Kubernetes do Azure (AKS)

O Azure Container Instances (ACI) fornecem um ambiente de hospedagem para executar contentores no Azure. Ao utilizar o ACI, não é necessário para gerir a infraestrutura de computação subjacentes, o Azure trata esse gerenciamento para. Ao executar contentores no ACI, são cobradas por segundo para cada contentor em execução.

Ao usar o Virtual Kubelet fornecedor do Azure Container Instances, contentores do Linux e Windows podem ser agendadas numa instância de contentor, como se fosse um nó de Kubernetes padrão. Esta configuração permite-lhe tirar partido das capacidades do Kubernetes e o benefício de valor e o custo de gestão de instâncias do contentor.

> [!NOTE]
> Virtual Kubelet é um projeto de código-fonte aberto experimental e deve ser utilizado como tal. Para contribuir, problemas de ficheiros e ler mais sobre o virtual kubelet, consulte a [projeto Virtual Kubelet GitHub][vk-github].

Este detalhes de documento, configurar o Virtual Kubelet para instâncias de contentor num AKS.

## <a name="prerequisite"></a>Pré-requisito

Este documento parte do princípio de que tem um cluster do AKS. Se precisar de um cluster do AKS, consulte a [início rápido do Azure Kubernetes Service (AKS)][aks-quick-start].

Também tem a versão da CLI do Azure **2.0.33** ou posterior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli).

Para instalar o Virtual Kubelet [Helm](https://docs.helm.sh/using_helm/#installing-helm) também é necessário.

### <a name="for-rbac-enabled-clusters"></a>Para clusters habilitados em RBAC

Se o seu cluster do AKS tem a capacidade de RBAC, tem de criar uma conta de serviço e o enlace de função para utilização com Tiller. Para obter mais informações, consulte [controlo de acesso baseado em funções do Helm][helm-rbac].

R *ClusterRoleBinding* também tem de ser criada para o Virtual Kubelet. Para criar uma ligação, crie um ficheiro denominado *rbac virtualkubelet.yaml* e cole a seguinte definição:

```yaml
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: virtual-kubelet
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: default
  namespace: default
```

Aplicar o vínculo com o [kubectl aplicam-se] [ kubectl-apply] e especifique seus *rbac virtualkubelet.yaml* ficheiro, conforme mostrado no exemplo a seguir:

```
$ kubectl apply -f rbac-virtual-kubelet.yaml

clusterrolebinding.rbac.authorization.k8s.io/virtual-kubelet created
```

Pode continuar a instalar o Virtual Kubelet no seu cluster do AKS.

## <a name="installation"></a>Instalação

Utilize o [az aks install-connector] [ aks-install-connector] comando para instalar o Virtual Kubelet. O exemplo seguinte implementa o conector do Linux e do Windows.

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
| `--aci-resource-group` | O grupo de recursos no qual pretende criar os grupos de contentor do ACI. | Não |
| `--location` `-l` | A localização para criar os grupos de contentor do ACI. | Não |
| `--service-principal` | Principal de serviço utilizado para autenticação para APIs do Azure. | Não |
| `--client-secret` | Segredo associado ao principal de serviço. | Não |
| `--chart-url` | URL de um gráfico do Helm que instala o conector ACI. | Não |
| `--image-tag` | A tag de imagem da imagem de contentor virtual kubelet. | Não |

## <a name="validate-virtual-kubelet"></a>Validar o Virtual Kubelet

Para validar que o Virtual Kubelet foi instalado, retornar uma lista de nós do Kubernetes com o [kubectl obter nós] [ kubectl-get] comando.

```
$ kubectl get nodes

NAME                                    STATUS    ROLES     AGE       VERSION
aks-nodepool1-23443254-0                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-1                Ready     agent     16d       v1.9.6
aks-nodepool1-23443254-2                Ready     agent     16d       v1.9.6
virtual-kubelet-virtual-kubelet-linux   Ready     agent     4m        v1.8.3
virtual-kubelet-virtual-kubelet-win     Ready     agent     4m        v1.8.3
```

## <a name="run-linux-container"></a>Executar o contentor do Linux

Crie um ficheiro denominado `virtual-kubelet-linux.yaml` e copie o YAML seguinte. Substitua o `kubernetes.io/hostname` valor com o nome do nó Virtual Kubelet de Linux. Observe que uma [nodeSelector] [ node-selector] e [toleration] [ toleration] estão a ser utilizadas para agendar o contentor no nó.

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

Execute a aplicação com o [criar kubectl] [ kubectl-create] comando.

```console
kubectl create -f virtual-kubelet-linux.yaml
```

Utilize o [kubectl obter pods] [ kubectl-get] comando com o `-o wide` argumento para uma lista de pods com o nó agendado de saída. Tenha em atenção que o `aci-helloworld` pod foi agendada ao `virtual-kubelet-virtual-kubelet-linux` nó.

```
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
aci-helloworld-2559879000-8vmjw     1/1       Running   0          39s       52.179.3.180   virtual-kubelet-virtual-kubelet-linux
```

## <a name="run-windows-container"></a>Execute contentores do Windows

Crie um ficheiro denominado `virtual-kubelet-windows.yaml` e copie o YAML seguinte. Substitua o `kubernetes.io/hostname` valor com o nome do nó Virtual Kubelet do Windows. Observe que uma [nodeSelector] [ node-selector] e [toleration] [ toleration] estão a ser utilizadas para agendar o contentor no nó.

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

Execute a aplicação com o [criar kubectl] [ kubectl-create] comando.

```console
kubectl create -f virtual-kubelet-windows.yaml
```

Utilize o [kubectl obter pods] [ kubectl-get] comando com o `-o wide` argumento para uma lista de pods com o nó agendado de saída. Tenha em atenção que o `nanoserver-iis` pod foi agendada ao `virtual-kubelet-virtual-kubelet-win` nó.

```
$ kubectl get pods -o wide

NAME                                READY     STATUS    RESTARTS   AGE       IP             NODE
nanoserver-iis-868bc8d489-tq4st     1/1       Running   8         21m       138.91.121.91   virtual-kubelet-virtual-kubelet-win
```

## <a name="remove-virtual-kubelet"></a>Remover o Virtual Kubelet

Utilize o [az aks remove-connector] [ aks-remove-connector] comando para remover o Virtual Kubelet. Substitua os valores de argumento com o nome do conector do cluster do AKS e o grupo de recursos de cluster do AKS.

```azurecli-interactive
az aks remove-connector --resource-group myAKSCluster --name myAKSCluster --connector-name virtual-kubelet
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o Virtual Kubelet, o [Github de Virtual Kubelet projet][vk-github].

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
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
