---
title: Implementar contentores com Helm no Kubernetes no Azure
description: Utilize a ferramenta de empacotamento de Helm para implementar contentores num cluster Kubernetes no AKS
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/13/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 531e6d9368b2bf91c48fd41b1e9330879b0df49a
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102484"
---
# <a name="use-helm-with-azure-kubernetes-service-aks"></a>Utilizar Helm serviço Kubernetes do Azure (AKS)

[Helm] [ helm] é uma ferramenta de empacotamento de open source que o ajuda a instalar e gerir o ciclo de vida das aplicações Kubernetes. Semelhante para os gestores de pacote do Linux, tais como *APT* e *Yum*, Helm é utilizado para gerir Kubernetes gráficos, que são pacotes de recursos de Kubernetes pré-configurada.

Passos neste documento através de configurar e utilizar Helm num cluster de Kubernetes no AKS.

## <a name="before-you-begin"></a>Antes de começar

Os passos detalhados neste documento partem do princípio de que já criou um cluster do AKS e estabeleceu uma ligação de kubectl com o cluster. Se precisar destes itens ver, a [início rápido AKS][aks-quickstart].

## <a name="install-helm-cli"></a>Instalar Helm CLI

A CLI Helm for um cliente que é executado no seu sistema de desenvolvimento e permite-lhe iniciar, parar e gerir aplicações com Helm.

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

## <a name="create-service-account"></a>Criar conta de serviço

Configurar Helm num RBAC ativado o cluster, terá de uma conta de serviço e a função de enlace para o serviço de Tiller. Para obter mais informações sobre como proteger Helm / Tiller num RBAC ativada cluster, consulte [Tiller, espaços de nomes e RBAC][tiller-rbac]. Tenha em atenção, se o cluster não for RBAC ativada, ignore este passo.

Crie um ficheiro denominado `helm-rbac.yaml` e copie o seguinte YAML.

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Criar a conta de serviço e o enlace de função com o `kubectl create` comando.

```
kubectl create -f helm-rbac.yaml
```

Quando utilizar um RBAC ativado o cluster, tem opções no nível de acesso que tiller tem do cluster. Consulte [Helm: controlos de acesso baseado em funções] [ helm-rbac] para obter mais informações sobre as opções de configuração.

## <a name="configure-helm"></a>Configurar Helm

Agora instalar tiller utilizando o [helm init] [ helm-init] comando. Se o cluster não for o RBAC ativada, remova o `--service-account` argumento e o valor.

```
helm init --service-account tiller
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

Para implementar o Wordpress através de um gráfico de Helm, utilize o [helm instalação] [ helm-install] comando.

```azurecli-interactive
helm install stable/wordpress
```

O resultado semelhante ao seguinte, mas inclui informações adicionais, tais como as instruções sobre como utilizar a implementação de Kubernetes.

```
NAME:   bilging-ibex
LAST DEPLOYED: Tue Jun  5 14:31:49 2018
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                                     READY  STATUS   RESTARTS  AGE
bilging-ibex-mariadb-7557b5474-dmdxn     0/1    Pending  0         1s
bilging-ibex-wordpress-7494c545fb-tskhz  0/1    Pending  0         1s

==> v1/Secret
NAME                    TYPE    DATA  AGE
bilging-ibex-mariadb    Opaque  2     1s
bilging-ibex-wordpress  Opaque  2     1s

==> v1/ConfigMap
NAME                        DATA  AGE
bilging-ibex-mariadb        1     1s
bilging-ibex-mariadb-tests  1     1s

==> v1/PersistentVolumeClaim
NAME                    STATUS   VOLUME   CAPACITY  ACCESS MODES  STORAGECLASS  AGE
bilging-ibex-mariadb    Pending  default  1s
bilging-ibex-wordpress  Pending  default  1s

==> v1/Service
NAME                    TYPE          CLUSTER-IP    EXTERNAL-IP  PORT(S)                     AGE
bilging-ibex-mariadb    ClusterIP     10.0.76.164   <none>       3306/TCP                    1s
bilging-ibex-wordpress  LoadBalancer  10.0.215.250  <pending>    80:30934/TCP,443:31134/TCP  1s

==> v1beta1/Deployment
NAME                    DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
bilging-ibex-mariadb    1        1        1           0          1s
bilging-ibex-wordpress  1        1        1           0          1s
...
```

## <a name="list-helm-releases"></a>Lista Helm versões

Para ver uma lista das versões instaladas no seu cluster, utilize o [lista helm] [ helm-list] comando.

```azurecli-interactive
helm list
```

Saída:

```
NAME            REVISION    UPDATED                     STATUS      CHART           NAMESPACE
bilging-ibex    1           Tue Jun  5 14:31:49 2018    DEPLOYED    wordpress-1.0.9 default
```

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre a gestão Kubernetes gráficos, consulte a documentação de Helm.

> [!div class="nextstepaction"]
> [Documentação de Helm][helm-documentation]

<!-- LINKS - external -->
[helm]: https://github.com/kubernetes/helm/
[helm-documentation]: https://docs.helm.sh/
[helm-init]: https://docs.helm.sh/helm/#helm-init
[helm-install]: https://docs.helm.sh/helm/#helm-install
[helm-install-options]: https://github.com/kubernetes/helm/blob/master/docs/install.md
[helm-list]: https://docs.helm.sh/helm/#helm-list
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[helm-repo-update]: https://docs.helm.sh/helm/#helm-repo-update
[helm-search]: https://docs.helm.sh/helm/#helm-search
[tiller-rbac]: https://docs.helm.sh/using_helm/#tiller-namespaces-and-rbac

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
