---
title: "Tutorial do Kubernetes no Azure - Dimensionar aplicação"
description: "Tutorial de AKS – Dimensionar Aplicação"
services: container-service
author: dlepow
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 993a8b71b29952394a2ab6a2bdddd0fc5fd241ae
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="scale-application-in-azure-container-service-aks"></a>Dimensionar aplicação no Azure Container Service (AKS)

Se tiver seguido os tutoriais, terá um cluster do Kubernetes a trabalhar no AKS e implementado a aplicação do Azure Voting.

Neste tutorial, parte cinco de oito, aumenta horizontalmente pods na aplicação e tenta dimensionar automaticamente. Também irá aprender a dimensionar o número de nós da VM do Azure, para alterar a capacidade do cluster para alojar cargas de trabalho. As tarefas concluídas incluem:

> [!div class="checklist"]
> * Dimensionar os nós do Azure do Kubernetes
> * Dimensionar manualmente pods do Kubernetes
> * Configurar pods de Dimensionamento automático com o front-end da aplicação

Nos tutoriais subsequentes, a aplicação do Azure Vote é atualizada e o Operations Management Suite configurado para monitorizar o cluster de Kubernetes.

## <a name="before-you-begin"></a>Antes de começar

Nos tutoriais anteriores, foi compactada uma aplicação numa imagem de contentor, carregada esta imagem para o Azure Container Registry e criado um cluster de Kubernetes. A aplicação foi, em seguida, executada no cluster de Kubernetes.

Se ainda não concluiu estes passos e pretende acompanhar, regresse ao [Tutorial 1 – Criar imagens de contentor][aks-tutorial-prepare-app].

## <a name="scale-aks-nodes"></a>Dimensionar nós do AKS

Se criou o seu cluster Kubernetes com os comandos no tutorial anterior, tem um nó. Pode ajustar o número de nós manualmente, se pretender mais ou menos cargas de trabalho do contentor no seu cluster.

O exemplo seguinte aumenta o número de nós para três no cluster do Kubernetes denominado *myAKSCluster*. O comando demora poucos minutos a concluir.

```azurecli
az aks scale --resource-group=myResourceGroup --name=myAKSCluster --node-count 3
```

O resultado é semelhante a:

```
"agentPoolProfiles": [
  {
    "count": 3,
    "dnsPrefix": null,
    "fqdn": null,
    "name": "myAKSCluster",
    "osDiskSizeGb": null,
    "osType": "Linux",
    "ports": null,
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_D2_v2",
    "vnetSubnetId": null
  }
```

## <a name="manually-scale-pods"></a>Dimensionar pods manualmente

Deste modo, o front-end do Azure Vote e a instância de Redis foi implementada, cada um com uma réplica única. Para verificar, execute o comando [kubectl get][kubectl-get].

```azurecli
kubectl get pods
```

Saída:

```
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Altere manualmente o número de pods na implementação `azure-vote-front` com o comando [kubectl scale][kubectl-scale]. Este exemplo aumenta o número para 5.

```azurecli
kubectl scale --replicas=5 deployment/azure-vote-front
```

Execute [kubectl get pods][kubectl-get] para verificar se o Kubernetes está a criar os pods. Um ou dois minutos depois, os pods adicionais estão em execução:

```azurecli
kubectl get pods
```

Saída:

```
NAME                                READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Dimensionar pods automaticamente

O Kubernetes suporta [dimensionamento automático horizontal de pods][kubernetes-hpa] para ajustar o número de pods numa implementação, consoante a utilização da CPU ou de outras métricas selecionadas.

Para utilizar o dimensionamento automático, os seus pods têm de ter os pedidos de CPU e os limites definidos. Na implementação `azure-vote-front`, o contentor de front-end pede 0,25 de CPU, com um limite de 0,5 de CPU. As definições têm o seguinte aspeto:

```YAML
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

O exemplo seguinte utiliza o comando [kubectl autoscale][kubectl-autoscale] para dimensionar automaticamente o número de pods na implementação `azure-vote-front`. Aqui, se a utilização da CPU exceder 50%, o dimensionamento automático aumenta os pods para um máximo de 10.


```azurecli
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Para ver o estado do dimensionamento automático, execute o seguinte comando:

```azurecli
kubectl get hpa
```

Saída:

```
NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Após alguns minutos, com carga mínima na aplicação do Azure Vote, o número de réplicas de pod diminui automaticamente para 3.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, utilizou diferentes funcionalidades de dimensionamento no seu cluster Kubernetes. Tarefas abrangidas incluídas:

> [!div class="checklist"]
> * Dimensionar manualmente pods do Kubernetes
> * Configurar pods de Dimensionamento automático com o front-end da aplicação
> * Dimensionar os nós do Azure do Kubernetes

Avance para o próximo tutorial para saber mais sobre a atualização da aplicação no Kubernetes.

> [!div class="nextstepaction"]
> [Atualizar uma aplicação no Kubernetes][aks-tutorial-update-app]

<!-- LINKS - external -->
[kubectl-autoscale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-scale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale
[kubernetes-hpa]: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-update-app]: ./tutorial-kubernetes-app-update.md
