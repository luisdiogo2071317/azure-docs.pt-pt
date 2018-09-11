---
title: Tráfego de saída de lista branca do cluster do Azure Kubernetes Service (AKS)
description: Tráfego de saída de lista de permissões de um cluster do Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/23/2018
ms.author: iainfou
ms.openlocfilehash: e2793a72fcbc20b79bdd564e331426fedf1ae34b
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44347805"
---
# <a name="azure-kubernetes-service-aks-egress"></a>Saída do Azure Kubernetes Service (AKS)

Por predefinição, o endereço de saída de um cluster do Azure Kubernetes Service (AKS) é atribuído aleatoriamente. Esta configuração não é ideal quando precisar de identificar um endereço IP para aceder a serviços externos. Este documento fornece detalhes sobre como criar e manter um endereço de IP de saída atribuído estaticamente num cluster do AKS.

## <a name="egress-overview"></a>Descrição geral de saída

Tráfego de saída de um cluster do AKS segue as convenções do Balanceador de carga do Azure, que estão documentadas [aqui][outbound-connections]. Antes do primeiro serviço Kubernetes do tipo `LoadBalancer` é criado, o agente de nós não fazem parte de qualquer conjunto de Balanceador de carga do Azure. Nesta configuração, os nós são sem um endereço IP público de nível de instância. Azure traduz-se o fluxo de saída para um endereço IP público de origem que não seja configurável ou determinística.

Uma vez um serviço de Kubernetes do tipo `LoadBalancer` é criado, o agente de nós são adicionados a um conjunto de Balanceador de carga do Azure. Fluxo de saída, Azure traduz-se o primeiro endereço IP público configurado no balanceador de carga.

## <a name="create-a-static-public-ip"></a>Criar um IP público estático

Para impedir que os endereços IP aleatórios a ser utilizado, crie um endereço IP estático e certifique-se de que o Balanceador de carga utiliza este endereço. O endereço IP tem de ser criado do AKS **nó** grupo de recursos.

Obtenha o nome do grupo de recursos com o [show de recursos de az] [ az-resource-show] comando. Atualize o nome do grupo de recursos e o nome do cluster para corresponder ao seu ambiente.

```
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Em seguida, utilize o [criar a rede de az public-ip] [ public-ip-create] comando para criar um endereço IP público estático. Atualize o nome do grupo de recursos de acordo com o nome gatherred no último passo.

```console
$ az network public-ip create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --allocation-method static --query publicIp.ipAddress -o table

Result
-------------
23.101.128.81
```

## <a name="create-a-service-with-the-static-ip"></a>Criar um serviço com o IP estático

Agora que tem um endereço IP, criar um serviço do Kubernetes com o tipo de `LoadBalancer` e atribuir o endereço IP para o serviço.

Crie um ficheiro denominado `egress-service.yaml` e copie o YAML seguinte. Atualize o endereço IP para corresponder ao seu ambiente.

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

Criar o serviço e a implantação com o `kubectl apply` comando.

```console
$ kubectl apply -f egress-service.yaml

service "aks-egress" created
```

Criar este serviço configura um novo IP de front-end do Balanceador de carga do Azure. Se não tiver qualquer outro IPs configurada, em seguida, **todos os** o tráfego de saída deve, agora, utilize este endereço. Quando vários endereços são configurados no balanceador de carga do Azure, saída utiliza o IP primeiro nesse Balanceador de carga.

## <a name="verify-egress-address"></a>Verifique se o endereço de saída

Para verificar que está sendo usado o endereço IP público, utilize um serviço como `checkip.dyndns.org`.

Iniciar e anexar a um pod:

```console
$ kubectl run -it --rm aks-ip --image=debian
```

Se necessário, instale o curl no contentor:

```console
$ apt-get update && apt-get install curl -y
```

Curl `checkip.dyndns.org`, que retorna o endereço de IP de saída:

```console
$ curl -s checkip.dyndns.org

<html><head><title>Current IP Check</title></head><body>Current IP Address: 23.101.128.81</body></html>
```

Deverá ver que o endereço IP corresponde ao endereço IP estático, anexado ao balanceador de carga do Azure.

## <a name="ingress-controller"></a>Controlador de entradas

Para evitar a manutenção de vários endereços IP públicos no balanceador de carga do Azure, considere a utilização de um controlador de entrada. Controladores de entrada fornecem benefícios, como balanceamento de carga, terminação de SSL/TLS, suportam para reescritas URI e a encriptação de SSL/TLS a montante. Para obter mais informações sobre controladores de entrada no AKS, consulte a [controlador de entradas de configurar o NGINX num cluster do AKS] [ ingress-aks-cluster] guia.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o software demonstrado neste documento.

- [Helm CLI][helm-cli-install]
- [Controlador de entradas do NGINX][nginx-ingress]
- [Ligações de saída do Balanceador de carga do Azure][outbound-connections]

<!-- LINKS - internal -->
[az-resource-show]: /cli/azure/resource#az-resource-show
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cloud-shell]: ../cloud-shell/overview.md
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[create-aks-cluster]: ./kubernetes-walkthrough.md
[helm-cli-install]: ./kubernetes-helm.md#install-helm-cli
[ingress-aks-cluster]: ./ingress-basic.md
[outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md#scenarios
[public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create

<!-- LINKS - external -->
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
