---
title: Implementar contentores com Helm no Kubernetes no Azure
description: Utilize a ferramenta de empacotamento de Helm para implementar contentores num cluster Kubernetes no AKS
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 39c6de1ce2443cf027d7cde067281355ea0b7207
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="use-helm-with-azure-container-service-aks"></a>Utilizar Helm com o serviço de contentor do Azure (AKS)

[Helm] [ helm] é uma ferramenta de empacotamento de open source que o ajuda a instalar e gerir o ciclo de vida das aplicações Kubernetes. Semelhante para os gestores de pacote do Linux, tais como *APT* e *Yum*, Helm é utilizado para gerir Kubernetes gráficos, que são pacotes de recursos de Kubernetes pré-configurada.

Passos neste documento através de configurar e utilizar Helm num cluster de Kubernetes no AKS.

## <a name="before-you-begin"></a>Antes de começar

Os passos detalhados neste documento partem do princípio de que já criou um cluster do AKS e estabeleceu uma ligação de kubectl com o cluster. Se precisar destes itens ver, a [início rápido AKS][aks-quickstart].

## <a name="install-helm-cli"></a>Instalar Helm CLI

A CLI Helm for um cliente que é executado no seu sistema de desenvolvimento e permite-lhe iniciar, parar e gerir aplicações com gráficos Helm.

Se estiver a utilizar o Azure CloudShell, a CLI Helm já está instalada. Para instalar a CLI Helm num utilize Mac `brew`. Para obter informações adicionais, consulte opções [instalar Helm][helm-install-options].

```console
brew install kubernetes-helm
```

Saída:

```
==> Downloading https://homebrew.bintray.com/bottles/kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/kubernetes-helm/2.6.2: 50 files, 132.4MB
```

## <a name="configure-helm"></a>Configurar Helm

O [helm init] [ helm-init] comando é utilizado para instalar componentes de Helm num Kubernetes cluster e efetuar configurações do lado do cliente. Execute o seguinte comando para instalar Helm no seu cluster AKS e configurar o cliente Helm.

```azurecli-interactive
helm init
```

Saída:

```
$HELM_HOME has been configured at /home/user/.helm.

Tiller (the Helm server-side component) has been installed into your Kubernetes Cluster.
Happy Helming!
```

## <a name="find-helm-charts"></a>Localizar gráficos Helm

Gráficos de Helm são utilizados para implementar aplicações num Kubernetes cluster. Para procurar em gráficos de Helm previamente criados, utilize o [pesquisa helm] [ helm-search] comando.

```azurecli-interactive
helm search
```

A procura de resultado semelhante ao seguinte, no entanto, com muitas mais gráficos.

```
NAME                            VERSION DESCRIPTION
stable/acs-engine-autoscaler    2.0.0   Scales worker nodes within agent pools
stable/artifactory              6.1.0   Universal Repository Manager supporting all maj...
stable/aws-cluster-autoscaler   0.3.1   Scales worker nodes within autoscaling groups.
stable/buildkite                0.2.0   Agent for Buildkite
stable/centrifugo               2.0.0   Centrifugo is a real-time messaging server.
stable/chaoskube                0.5.0   Chaoskube periodically kills random pods in you...
stable/chronograf               0.3.0   Open-source web application written in Go and R...
stable/cluster-autoscaler       0.2.0   Scales worker nodes within autoscaling groups.
stable/cockroachdb              0.5.0   CockroachDB is a scalable, survivable, strongly...
stable/concourse                0.7.0   Concourse is a simple and scalable CI system.
stable/consul                   0.4.1   Highly available and distributed service discov...
stable/coredns                  0.5.0   CoreDNS is a DNS server that chains middleware ...
stable/coscale                  0.2.0   CoScale Agent
stable/dask-distributed         2.0.0   Distributed computation in Python
stable/datadog                  0.8.0   DataDog Agent
...
```

Para atualizar a lista de gráficos, utilize o [atualização do repositório de helm] [ helm-repo-update] comando.

```azurecli-interactive
helm repo update
```

Saída:

```
Hang tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

## <a name="run-helm-charts"></a>Executar gráficos Helm

Para implementar um controlador de entrada NGINX, utilize o [helm instalação] [ helm-install] comando.

```azurecli-interactive
helm install stable/nginx-ingress
```

O resultado semelhante ao seguinte, mas inclui informações adicionais, tais como as instruções sobre como utilizar a implementação de Kubernetes.

```
NAME:   tufted-ocelot
LAST DEPLOYED: Thu Oct  5 00:48:04 2017
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                                    DATA  AGE
tufted-ocelot-nginx-ingress-controller  1     5s

==> v1/Service
NAME                                         CLUSTER-IP   EXTERNAL-IP  PORT(S)                     AGE
tufted-ocelot-nginx-ingress-controller       10.0.140.10  <pending>    80:30486/TCP,443:31358/TCP  5s
tufted-ocelot-nginx-ingress-default-backend  10.0.34.132  <none>       80/TCP                      5s

==> v1beta1/Deployment
NAME                                         DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
tufted-ocelot-nginx-ingress-controller       1        1        1           0          5s
tufted-ocelot-nginx-ingress-default-backend  1        1        1           1          5s
...
```

Para obter mais informações sobre a utilização de um controlador de entrada NGINX com Kubernetes, consulte [NGINX entrada controlador][nginx-ingress].

## <a name="list-helm-charts"></a>Lista Helm gráficos

Para ver uma lista dos gráficos instalado no seu cluster, utilize o [lista helm] [ helm-list] comando.

```azurecli-interactive
helm list
```

Saída:

```
NAME            REVISION    UPDATED                     STATUS      CHART               NAMESPACE
bilging-ant     1           Thu Oct  5 00:11:11 2017    DEPLOYED    nginx-ingress-0.8.7 default
```

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre a gestão Kubernetes gráficos, consulte a documentação de Helm.

> [!div class="nextstepaction"]
> [Documentação de Helm][helm-documentation]

<!-- LINKS - external -->
[helm]: https://github.com/kubernetes/helm/
[helm-documentation]: https://github.com/kubernetes/helm/blob/master/docs/index.md
[helm-init]: https://docs.helm.sh/helm/#helm-init
[helm-install]: https://docs.helm.sh/helm/#helm-install
[helm-install-options]: https://github.com/kubernetes/helm/blob/master/docs/install.md
[helm-list]: https://docs.helm.sh/helm/#helm-list
[helm-repo-update]: https://docs.helm.sh/helm/#helm-repo-update
[helm-search]: https://docs.helm.sh/helm/#helm-search
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md