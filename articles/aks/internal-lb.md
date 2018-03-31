---
title: Criar um balanceador de carga interno do serviço de contentor do Azure (AKS)
description: Utilize um balanceador de carga interno com o serviço de contentor do Azure (AKS).
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 3/29/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7b9ecdb5364f7c0f5bb68ce693e53bc2c5327337
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2018
---
# <a name="use-an-internal-load-balancer-with-azure-container-service-aks"></a>Utilizar um balanceador de carga interno com o serviço de contentor do Azure (AKS)

Balanceamento de carga interna faz com que um serviço Kubernetes acessíveis para as aplicações em execução na mesma rede virtual do cluster Kubernetes. Este detalhes de documento criação de um balanceador de carga interno com o serviço de contentor do Azure (AKS).

## <a name="create-internal-load-balancer"></a>Criar Balanceador de carga interno

Para criar um balanceador de carga interno, criar um manifesto de serviço com o tipo de serviço `LoadBalancer` e `azure-load-balancer-internal` anotação, como mostrado no seguinte exemplo.

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

Depois de implementada, um balanceador de carga do Azure é criado e disponibilizado na mesma rede virtual do cluster AKS. 

![Imagem do Balanceador de carga interno AKS](media/internal-lb/internal-lb.png)

Quando o serviço a obter detalhes, o endereço IP a `EXTERNAL-IP` coluna é o endereço IP do Balanceador de carga interno. 

```console
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.248.59   10.240.0.7    80:30555/TCP   10s
```

## <a name="specify-an-ip-address"></a>Especifique um endereço IP

Se gostaria de utilizar um endereço IP específico com o Balanceador de carga interno, adicione o `loadBalancerIP` propriedade para a especificação de Balanceador de carga. O endereço IP especificado tem de residir na mesma sub-rede que o cluster AKS e já não deve ser atribuído a um recurso.

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

Ao obter os detalhes do serviço, o endereço IP no `EXTERNAL-IP` deve refletir o endereço IP especificado. 

```console
$ kubectl get service azure-vote-front

NAME               TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.184.168   10.240.0.25   80:30225/TCP   4m
```

## <a name="delete-the-load-balancer"></a>Eliminar o Balanceador de carga

Quando tiverem sido eliminados todos os serviços com Balanceador de carga interno, o Balanceador de carga próprio também é eliminado.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre serviços Kubernetes o [Kubernetes dos serviços de documentação][kubernetes-services].

<!-- LINKS - External -->
[kubernetes-services]: https://kubernetes.io/docs/concepts/services-networking/service/