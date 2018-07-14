---
title: Criar um balanceador de carga interno do Azure Kubernetes Service (AKS)
description: Saiba como criar e utilizar um balanceador de carga interno para expor os seus serviços com o Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/12/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 123fc08995416e0ff9c7e12a526deadc34b3a4a2
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001400"
---
# <a name="use-an-internal-load-balancer-with-azure-kubernetes-service-aks"></a>Utilizar um balanceador de carga interno com o Azure Kubernetes Service (AKS)

Balanceamento de carga interno torna um serviço do Kubernetes acessível a aplicações em execução na mesma rede virtual do cluster de Kubernetes. Este artigo mostra-lhe como criar e utilizar um balanceador de carga interno com o Azure Kubernetes Service (AKS). O Balanceador de carga do Azure está disponível em dois SKUs: básico e Standard. AKS utiliza o SKU básico.

## <a name="create-an-internal-load-balancer"></a>Criar um balanceador de carga interno

Para criar um balanceador de carga interno, criar um manifesto de serviço com o tipo de serviço *LoadBalancer* e o *interno do azure-load-balancer* anotação, conforme mostrado no exemplo a seguir:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Depois de implantados com `kubetctl apply`, um balanceador de carga do Azure é criado e disponibilizado na mesma rede virtual que o cluster do AKS.

![Imagem do Balanceador de carga interno do AKS](media/internal-lb/internal-lb.png)

Ao visualizar os detalhes do serviço, o endereço IP no *EXTERNAL-IP* coluna é o endereço IP do Balanceador de carga interno, conforme mostrado no exemplo a seguir:

```
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.248.59   10.240.0.7    80:30555/TCP   10s
```

## <a name="specify-an-ip-address"></a>Especifique um endereço IP

Se gostaria de utilizar um endereço IP específico com o Balanceador de carga interno, adicione a *loadBalancerIP* propriedade para a especificação de Balanceador de carga. O endereço IP especificado tem de residir na mesma sub-rede que o cluster do AKS e já não deve ser atribuído a um recurso.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
spec:
  type: LoadBalancer
  loadBalancerIP: 10.240.0.25
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

Ao visualizar os detalhes do serviço, o endereço IP no *EXTERNAL-IP* reflete o endereço IP especificado:

```
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.184.168   10.240.0.25   80:30225/TCP   4m
```

## <a name="use-private-networks"></a>Utilizar redes privadas

Ao criar o cluster do AKS, pode especificar definições de rede avançadas. Esta abordagem permite-lhe implementar o cluster numa rede virtual do Azure existente e sub-redes. Um cenário é implementar o seu cluster do AKS para uma rede privada ligada ao seu ambiente no local e executar serviços acessíveis apenas internamente. Para obter mais informações, consulte [configuração de rede avançada no AKS][advanced-networking].

Sem alterações para os passos anteriores são necessários para implementar um balanceador de carga interno num cluster do AKS que utiliza uma rede privada. O Balanceador de carga é criado no mesmo grupo de recursos do seu cluster do AKS mas ligado à sua rede privada virtual e sub-rede, conforme mostrado no exemplo a seguir:

```
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.1.15.188   10.0.0.35     80:31669/TCP   1m
```

> [!NOTE]
> Poderá ter de conceder o principal de serviço para o seu cluster do AKS a *contribuinte de rede* função para o grupo de recursos onde os recursos de rede virtual do Azure são implementados. Ver o principal de serviço com [show do az aks][az-aks-show], tais como `az aks show --resource-group myResourceGroup --name myAKSCluster --query "servicePrincipalProfile.clientId"`. Para criar uma atribuição de função, utilize o [criação da atribuição de função de az] [ az-role-assignment-create] comando.

## <a name="specify-a-different-subnet"></a>Especifique uma sub-rede diferente

Para especificar uma sub-rede para o Balanceador de carga, adicione a *azure-carga-balanceador--sub-rede interna* anotação ao seu serviço. A sub-rede especificada tem de ser na mesma rede virtual que o cluster do AKS. Quando implementada, o Balanceador de carga *EXTERNAL-IP* endereço faz parte da sub-rede especificada.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-internal: "true"
    service.beta.kubernetes.io/azure-load-balancer-internal-subnet: "apps-subnet"
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

## <a name="delete-the-load-balancer"></a>Eliminar o Balanceador de carga

Quando são eliminados todos os serviços que utilizam o Balanceador de carga interno, o próprio Balanceador de carga também é eliminado.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre os serviços do Kubernetes com o [documentação dos serviços de Kubernetes][kubernetes-services].

<!-- LINKS - External -->
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - Internal -->
[advanced-networking]: networking-overview.md
[deploy-advanced-networking]: networking-overview.md#configure-networking---cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create