---
title: "Guia de início rápido – Cluster do Azure Kubernetes para Linux"
description: Aprenda rapidamente a criar um cluster do Kubernetes para contentores do Linux no AKS com a CLI do Azure.
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: quickstart
ms.date: 11/15/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc, devcenter
ms.openlocfilehash: 4d4c45728134853d2eabc838d40068250d90d6e0
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2017
---
# <a name="deploy-an-azure-container-service-aks-cluster"></a>Implementar um cluster do Azure Container Service (AKS)

Neste guia de introdução, é implementado um cluster do AKS com a CLI do Azure. Depois, é executada no cluster uma aplicação de vários contentores que consiste num front-end da Web e numa instância de Redis. Depois de concluída, a aplicação está acessível através da Internet.

![Imagem de navegação para o Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

Este guia de introdução pressupõe um conhecimento básico dos conceitos do Kubernetes. Para obter informações detalhadas sobre o Kubernetes, consulte a [documentação do Kubernetes][kubernetes-documentation].

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se optar por instalar e usar a CLI localmente, este guia de introdução requer a execução da versão 2.0.21 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Install Azure CLI (Instalar o Azure CLI)][azure-cli-install].

## <a name="enabling-aks-preview-for-your-azure-subscription"></a>Ativar a pré-visualização do AKS para a sua subscrição do Azure
Enquanto o AKS está em pré-visualização, a criação de novos clusters exige um sinalizador de funcionalidade na sua subscrição. Pode pedir esta funcionalidade para qualquer número de subscrições que pretende utilizar. Utilize o comando `az provider register` para registar o fornecedor do AKS:

```azurecli-interactive
az provider register -n Microsoft.ContainerService
```

Depois de registar, está pronto para criar um cluster do Kubernetes com o AKS.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o comando [az group create][az-group-create]. Um grupo de recursos do Azure é um grupo lógico, no qual os recursos do Azure são implementados e geridos.

O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Saída:

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-aks-cluster"></a>Criar um cluster do AKS

O exemplo seguinte cria um cluster com o nome *myK8sCluster* com um nó.

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myK8sCluster --node-count 1 --generate-ssh-keys
```

Ao fim de vários minutos, o comando é concluído e devolve informações sobre o cluster no formato JSON.

## <a name="connect-to-the-cluster"></a>Ligar ao cluster

Para gerir um cluster de Kubernetes, utilize [kubectl][kubectl], o cliente de linha de comandos do Kubernetes.

Se estiver a utilizar o Azure CloudShell, o kubectl já está instalado. Se pretender instalá-lo localmente, execute o seguinte comando.


```azurecli
az aks install-cli
```

Para configurar o kubectl para se ligar ao seu cluster do Kubernetes, execute o comando seguinte. Este passo transfere credenciais e configura a CLI do Kubernetes para as utilizar.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myK8sCluster
```

Para verificar a ligação ao cluster, utilize o comando [kubectl get][kubectl-get] para devolver uma lista de nós do cluster.

```azurecli-interactive
kubectl get nodes
```

Saída:

```
NAME                          STATUS    ROLES     AGE       VERSION
k8s-myk8scluster-36346190-0   Ready     agent     2m        v1.7.7
```

## <a name="run-the-application"></a>Executar a aplicação

Um ficheiro de manifesto do Kubernetes define um estado pretendido para o cluster, incluindo que imagens de contentor devem estar em execução. Neste exemplo, é utilizado um manifesto para criar todos os objetos necessários para executar a aplicação Azure Vote.

Crie um ficheiro com o nome `azure-vote.yaml` e copie-o para o código YAML seguinte. Se estiver a trabalhar no Azure Cloud Shell, este ficheiro pode ser criado através de vi ou Nano, como se estivesse a trabalhar num sistema físico ou virtual.

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      containers:
      - name: azure-vote-back
        image: redis
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:redis-v1
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Utilize o comando [kubectl create][kubectl-create] para executar a aplicação.

```azurecli-interactive
kubectl create -f azure-vote.yaml
```

Saída:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>Testar a aplicação

À medida que a aplicação é executada, é criado um [serviço do Kubernetes][kubernetes-service] que expõe o front-end da aplicação na Internet. Este processo pode demorar alguns minutos a concluir.

Para monitorizar o progresso, utilize o comando [kubectl get service][kubectl-get] com o argumento `--watch`.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Inicialmente, o *EXTERNAL-IP* do serviço *azure-vote-front* aparece como *pendente*.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

Quando o endereço *EXTERNAL-IP* mudar de *pendente* para *Endereço IP*, utilize `CTRL-C` para parar o processo de observação do kubectl.

```
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Agora, pode navegar para o endereço IP externo para ver a aplicação Azure Vote.

![Imagem de navegação para o Azure Vote](media/container-service-kubernetes-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a>Eliminar o cluster
Quando o cluster já não for necessário, pode utilizar o comando [az group delete][az-group-delete] para remover o grupo de recursos, o serviço de contentores e todos os recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-the-code"></a>Obter o código

Neste guia de introdução, foram utilizadas imagens de contentores pré-criadas para criar uma implementação de Kubernetes. O código da aplicação relacionado, o Dockerfile, e o ficheiro de manifesto do Kubernetes, estão disponíveis no GitHub.

[https://github.com/Azure-Samples/azure-voting-app-redis][azure-vote-app]

## <a name="next-steps"></a>Passos seguintes

Neste guia de início rápido, implementou um cluster do Kubernetes e implementou uma aplicação de vários contentores no mesmo.

Para saber mais sobre o AKS e ver um exemplo completo de código para implementação, avance para o tutorial dos clusters de Kubernetes.

> [!div class="nextstepaction"]
> [Gerir um cluster do AKS][aks-tutorial]:

<!-- LINKS - external -->
[azure-vote-app]: https://github.com/Azure-Samples/azure-voting-app-redis.git
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-documentation]: https://kubernetes.io/docs/home/
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az_aks_get_credentials
[az-group-create]: /cli/azure/group#az_group_create
[az-group-delete]: /cli/azure/group#az_group_delete
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md

