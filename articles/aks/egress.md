---
title: Tráfego de saída da lista branca de cluster do serviço de Kubernetes do Azure (AKS)
description: Tráfego de saída da lista branca de um cluster do serviço de Kubernetes do Azure (AKS)
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/23/2018
ms.author: nepeters
ms.openlocfilehash: 0bafb62fcdc8a24084cf7170a0e0f72bfd7824b1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655355"
---
# <a name="azure-kubernetes-service-aks-egress"></a>Saída de serviço Kubernetes (AKS) do Azure

Por predefinição, o endereço de saída de um cluster do serviço de Kubernetes do Azure (AKS) aleatoriamente está atribuído. Esta configuração não é ideal quando necessitar de identificar um endereço IP para aceder aos serviços externos. Este documento fornece detalhes sobre como criar e manter um endereço IP estaticamente atribuídos saída de um cluster AKS.

## <a name="egress-overview"></a>Descrição geral de saída

Tráfego de saída de um cluster AKS acompanha convenções de Balanceador de carga do Azure, que estão documentadas [aqui][outbound-connections]. Antes do primeiro serviço Kubernetes do tipo `LoadBalancer` é criada, o agente de nós não fazem parte de nenhum conjunto de Balanceador de carga do Azure. Nesta configuração, os nós são sem um endereço IP público de nível de instância. Azure traduz o fluxo de saída para um endereço IP público de origem que não é configurável ou determinística.

Uma vez um serviço de Kubernetes do tipo `LoadBalancer` é criada, o agente de nós são adicionados a um conjunto de Balanceador de carga do Azure. Fluxo de saída, Azure traduz-se o primeiro endereço IP público configurado no balanceador de carga.

## <a name="create-a-static-public-ip"></a>Criar um IP público estático

Para impedir que aleatório endereços IP a ser utilizado, criar um endereço IP estático e certifique-se de que o Balanceador de carga utiliza este endereço. O endereço IP tem de ser criados no AKS **nó** grupo de recursos.

Obter o nome do grupo de recursos com o [mostrar de recurso az] [ az-resource-show] comando. Atualize o nome do grupo de recursos e o nome de cluster para corresponder ao seu ambiente.

```
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Em seguida, utilize o [az público-ip da rede criar] [ public-ip-create] comando para criar um endereço IP público estático. Atualize o nome do grupo de recursos para fazer corresponder o gatherred nome no último passo.

```console
$ az network public-ip create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --allocation-method static --query publicIp.ipAddress -o table

Result
-------------
23.101.128.81
```

## <a name="create-a-service-with-the-static-ip"></a>Criar um serviço com o IP estático

Agora que tem um endereço IP, criar um serviço de Kubernetes com o tipo `LoadBalancer` e atribuir o endereço IP para o serviço.

Crie um ficheiro denominado `egress-service.yaml` e copie o seguinte YAML. Atualize o endereço IP para corresponder ao seu ambiente.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: aks-egress
spec:
  loadBalancerIP: 23.101.128.81
  type: LoadBalancer
  ports:
  - port: 8080
```

Criar o serviço e a implementação com o `kubectl apply` comando.

```console
$ kubectl apply -f egress-service.yaml

service "aks-egress" created
```

Criar este serviço configura um novo IP de front-end do Balanceador de carga do Azure. Se não tiver quaisquer outros IPs configurada, em seguida, **todos os** tráfego de saída agora deve utilizar este endereço. Quando vários endereços são configurados no balanceador de carga do Azure, saída utiliza o primeiro IP em que Balanceador de carga.

## <a name="verify-egress-address"></a>Verifique o endereço de saída

Para verificar que está a ser utilizado o endereço IP público, utilize um serviço, tais como `checkip.dyndns.org`.

Iniciar e ligue a um pod:

```console
$ kubectl run -it --rm aks-ip --image=debian
```

Se necessário, instale o curl no contentor:

```console
$ apt-get update && apt-get install curl -y
```

Curl `checkip.dyndns.org`, que devolve o endereço IP de saída:

```console
$ curl -s checkip.dyndns.org

<html><head><title>Current IP Check</title></head><body>Current IP Address: 23.101.128.81</body></html>
```

Deverá ver que o endereço IP corresponde ao endereço IP estático, anexar ao balanceador de carga do Azure.

## <a name="ingress-controller"></a>Controlador de entrada

Para evitar a manutenção de vários endereços IP públicos no balanceador de carga do Azure, considere a utilização de um controlador de entrada. Controladores de entrada fornecem vantagens, como balanceamento de carga, terminação de SSL/TLS, suportem para reescritas URI e a montante encriptação SSL/TLS. Para mais informações sobre controladores de entrada no AKS, consulte o [controlador de entrada de configurar NGINX num AKS cluster] [ ingress-aks-cluster] guia.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o software demonstrado neste documento.

- [Helm CLI][helm-cli-install]
- [Controlador de entrada NGINX][nginx-ingress]
- [Ligações de saída do Balanceador de carga do Azure][outbound-connections]

<!-- LINKS - internal -->
[az-resource-show]: /cli/azure/resource#az-resource-show
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cloud-shell]: ../cloud-shell/overview.md
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[create-aks-cluster]: ./kubernetes-walkthrough.md
[helm-cli-install]: ./kubernetes-helm.md#install-helm-cli
[ingress-aks-cluster]: ./ingress.md
[outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md#scenarios
[public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create

<!-- LINKS - external -->
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
