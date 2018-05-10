---
title: Utilizar um endereço IP estático com o Balanceador de carga do serviço de Kubernetes do Azure (AKS)
description: Utilize um endereço IP estático com o Balanceador de carga do serviço de Kubernetes do Azure (AKS).
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/12/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: da1fc4cd31a4b4f13da5b527e2a62e147799bfef
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="use-a-static-ip-address-with-the-azure-kubernetes-service-aks-load-balancer"></a>Utilizar um endereço IP estático com o Balanceador de carga do serviço de Kubernetes do Azure (AKS)

Em alguns casos, tal como quando o serviço de Kubernetes do Azure (AKS) carregar balanceador é recriado ou Kubernetes serviços com um tipo de LoadBalancer são recriados, pode alterar o endereço IP público do serviço Kubernetes. Este configurar um endereço IP estático para os serviços de Kubernetes detalhes de documento.

## <a name="create-static-ip-address"></a>Criar endereço IP estático

Crie um endereço IP público estático para o serviço de Kubernetes. O endereço IP tem de ser criada no grupo de recursos que foi criado automaticamente durante a implementação de cluster. Para obter informações sobre os diferentes grupos de recursos AKS e como identificar o grupo de recursos criada automaticamente, consulte o [AKS FAQ][aks-faq-resource-group].

Utilize o [ip público de rede az criar] [ az-network-public-ip-create] comando para criar o endereço IP.

```azurecli-interactive
az network public-ip create --resource-group MC_myResourceGRoup_myAKSCluster_eastus --name myAKSPublicIP --allocation-method static
```

Tome nota do endereço IP.

```json
{
  "publicIp": {
    "dnsSettings": null,
    "etag": "W/\"6b6fb15c-5281-4f64-b332-8f68f46e1358\"",
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myAKSPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": "40.121.183.52",
    "ipConfiguration": null,
    "ipTags": [],
    "location": "eastus",
    "name": "myAKSPublicIP",
    "provisioningState": "Succeeded",
    "publicIpAddressVersion": "IPv4",
    "publicIpAllocationMethod": "Static",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "56ec8760-a3b8-4aeb-a89d-42e68d2cbc8c",
    "sku": {
      "name": "Basic"
    },
    "tags": null,
    "type": "Microsoft.Network/publicIPAddresses",
    "zones": null
  }
````

 Se for necessário, o endereço pode ser obtido utilizando o [lista de ip público de rede az] [ az-network-public-ip-list] comando.

```azurecli-interactive
az network public-ip list --resource-group MC_myResourceGRoup_myAKSCluster_eastus --query [0].ipAddress --output tsv
```

```console
40.121.183.52
```

## <a name="create-service-with-ip-address"></a>Criar serviço com o endereço IP

Assim que tiver sido aprovisionado o endereço IP estático, um serviço Kubernetes pode ser criado com o `loadBalancerIP` propriedade e um valor do endereço IP estático.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

## <a name="troubleshooting"></a>Resolução de problemas

Se o endereço IP estático não foi criado ou foi criado no grupo de recursos incorreto, a criação do serviço falha. Para resolver problemas, devolver eventos de criação de serviço com o [kubectl descrevem] [ kubectl-describe] comando.

```azurecli-interactive
kubectl describe service azure-vote-front
```

```console
Name:                     azure-vote-front
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=azure-vote-front
Type:                     LoadBalancer
IP:                       10.0.18.125
IP:                       40.121.183.52
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  32582/TCP
Endpoints:                <none>
Session Affinity:         None
External Traffic Policy:  Cluster
Events:
  Type     Reason                      Age               From                Message
  ----     ------                      ----              ----                -------
  Normal   CreatingLoadBalancer        7s (x2 over 22s)  service-controller  Creating load balancer
  Warning  CreatingLoadBalancerFailed  6s (x2 over 12s)  service-controller  Error creating load balancer (will retry): Failed to create load balancer for service default/azure-vote-front: user supplied IP Address 40.121.183.52 was not found
```

<!-- LINKS - External -->
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/

<!-- LINKS - Internal -->
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[az-network-public-ip-create]: /cli/azure/network/public-ip#az_network_public_ip_create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az_network_public_ip_list
