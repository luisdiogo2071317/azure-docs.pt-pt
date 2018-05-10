---
title: Obter registos de kubelet do serviço do Azure Kubernetes (AKS)
description: Obter registos de kubelet de nós de cluster do serviço de Kubernetes do Azure (AKS)
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/08/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 0467be7e91fdbf4685fc41a375ea86a503e26009
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="get-kubelet-logs-from-azure-kubernetes-service-aks-cluster-nodes"></a>Obter registos de kubelet de nós de cluster do serviço de Kubernetes do Azure (AKS)

Ocasionalmente, poderá ter de obter registos de kubelet a partir de um nó de Azure Kubernetes serviço (AKS) para fins de resolução de problemas. Este documento fornece detalhes sobre uma opção para extrair estes registos.

## <a name="create-an-ssh-connection"></a>Criar uma ligação SSH

Em primeiro lugar, crie uma ligação SSH com o nó no qual tem de solicitar kubelet registos. Esta operação está detalhada no [SSH para nós de cluster do serviço de Kubernetes do Azure (AKS)] [ aks-ssh] documento.

## <a name="get-kubelet-logs"></a>Obter registos kubelet

Uma vez se ligou-se ao nó, execute o seguinte comando para extrair os registos de kubelet.

```azurecli-interactive
docker logs $(docker ps -a | grep "hyperkube kubele" | awk -F ' ' '{print $1}')
```

Saída de exemplo:

```console
2018-03-05 00:04:00.883195 I | proto: duplicate proto type registered: google.protobuf.Any
2018-03-05 00:04:00.883242 I | proto: duplicate proto type registered: google.protobuf.Duration
2018-03-05 00:04:00.883253 I | proto: duplicate proto type registered: google.protobuf.Timestamp
Flag --non-masquerade-cidr has been deprecated, will be removed in a future version
Flag --non-masquerade-cidr has been deprecated, will be removed in a future version
I0305 00:04:00.978560    8021 feature_gate.go:144] feature gates: map[Accelerators:true]
I0305 00:04:00.978996    8021 azure.go:174] azure: using client_id+client_secret to retrieve access token
I0305 00:04:00.979041    8021 server.go:439] Successfully initialized cloud provider: "azure" from the config file: "/etc/kubernetes/azure.json"
I0305 00:04:00.979058    8021 server.go:740] cloud provider determined current node name to be aks-nodepool1-42032720-0
```

<!-- LINKS - internal -->
[aks-ssh]: aks-ssh.md