---
title: Implementar contentores com Helm no Kubernetes no Azure
description: Utilize a ferramenta de empacotamento do Helm para implementar contentores num cluster do Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/13/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: dd2deba25615373765dd3492d03c1ba547c8ba8c
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39055139"
---
# <a name="install-applications-with-helm-in-azure-kubernetes-service-aks"></a>Instalar aplicações com Helm no Azure Kubernetes Service (AKS)

[Helm] [ helm] é uma ferramenta de empacotamento de código-fonte aberto que ajuda a instalar e gerenciar o ciclo de vida de aplicações do Kubernetes. Semelhante a gestores de pacotes do Linux, como *APT* e *Yum*, Helm é utilizado para gerir gráficos de Kubernetes, que são pacotes de recursos do Kubernetes pré-configurada.

Este artigo mostra-lhe como configurar e utilizar o Helm num cluster do Kubernetes no AKS.

## <a name="before-you-begin"></a>Antes de começar

Os passos detalhados neste documento partem do princípio de que já criou um cluster do AKS e estabeleceu uma `kubectl` ligação com o cluster. Se precisar destes itens ver, o [início rápido do AKS][aks-quickstart].

## <a name="install-helm-cli"></a>Instale o Helm CLI

O Helm é um cliente que é executado no sistema de desenvolvimento e permite-lhe iniciar, parar e gerir aplicações com Helm.

Se utilizar o Azure Cloud Shell, o Helm CLI já está instalado. Para instalar a CLI do Helm num Mac, utilize `brew`. Para instalação adicionais, consulte as opções [instalar o Helm][helm-install-options].

```console
brew install kubernetes-helm
```

Saída:

```
==> Downloading https://homebrew.bintray.com/bottles/kubernetes-helm-2.9.1.high_sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring kubernetes-helm-2.9.1.high_sierra.bottle.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/kubernetes-helm/2.9.1: 50 files, 66.2MB
```

## <a name="create-a-service-account"></a>Criar uma conta de serviço

Antes de poder implementar Helm num cluster habilitados no RBAC, terá uma conta de serviço e o enlace de função para o serviço de Tiller. Para obter mais informações sobre como proteger o Helm / Tiller num RBAC ativado o cluster, consulte [Tiller, espaços de nomes e RBAC][tiller-rbac]. Se o cluster não for RBAC ativada, ignore este passo.

Crie um ficheiro denominado `helm-rbac.yaml` e copie o YAML seguinte:

```yaml
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

Criar a conta de serviço e a associação de função com o `kubectl create` comando:

```console
kubectl create -f helm-rbac.yaml
```

## <a name="secure-tiller-and-helm"></a>Proteger Tiller e Helm

O cliente do Helm e o serviço de Tiller autenticarem e se comunicar entre si através de TLS/SSL. Este método de autenticação ajuda a proteger o cluster de Kubernetes e que serviços podem ser implementados. Para melhorar a segurança, pode gerar seus próprios certificados assinados. Cada utilizador Helm receberia seu próprio certificado de cliente e Tiller seria possível inicializar no cluster de Kubernetes com certificados aplicados. Para obter mais informações, consulte [utilizando TLS/SSL entre o Helm e Tiller][helm-ssl].

Com um cluster de Kubernetes habilitados no RBAC, pode controlar o nível de acesso que tem de Tiller no cluster. Pode definir o espaço de nomes do Kubernetes que o Tiller está implementado na e restringir os espaços de nomes Tiller, em seguida, pode implementar recursos no. Esta abordagem permite-lhe criar instâncias de Tiller em diferentes namespaces e limites de implementação de limite e definir o âmbito os utilizadores do cliente do Helm para determinados espaços de nomes. Para obter mais informações, consulte [executar Helm controlos de acesso baseado em funções][helm-rbac].

## <a name="configure-helm"></a>Configurar o Helm

Para implementar um Tiller básico para um cluster do AKS, utilize o [helm init] [ helm-init] comando. Se o cluster não for RBAC ativada, remova o `--service-account` argumento e o valor. Se tiver configurado o TLS/SSL para Tiller e Helm, ignore este passo de inicialização básico e em vez disso, forneça o necessário `--tiller-tls-` conforme mostrado no exemplo seguinte.

```console
helm init --service-account tiller
```

Se configurou o TLS/SSL entre o Helm e Tiller fornecem o `--tiller-tls-` parâmetros e os nomes dos seus próprios certificados, conforme mostrado no exemplo a seguir:

```console
helm init \
    --tiller-tls \
    --tiller-tls-cert tiller.cert.pem \
    --tiller-tls-key tiller.key.pem \
    --tiller-tls-verify \
    --tls-ca-cert ca.cert.pem \
    --service-account tiller
```

## <a name="find-helm-charts"></a>Encontrar gráficos Helm

Gráficos Helm são utilizados para implementar aplicações num cluster do Kubernetes. Para pesquisar por gráficos do Helm previamente criados, utilize o [pesquisa helm] [ helm-search] comando:

```console
helm search
```

O resultado de exemplo condensado seguinte mostra alguns dos gráficos Helm disponíveis para utilização:

```
$ helm search

NAME                           CHART VERSION    APP VERSION  DESCRIPTION
stable/acs-engine-autoscaler   2.2.0            2.1.1        Scales worker nodes within agent pools
stable/aerospike               0.1.7            v3.14.1.2    A Helm chart for Aerospike in Kubernetes
stable/anchore-engine          0.1.7            0.1.10       Anchore container analysis and policy evaluatio...
stable/apm-server              0.1.0            6.2.4        The server receives data from the Elastic APM a...
stable/ark                     1.0.1            0.8.2        A Helm chart for ark
stable/artifactory             7.2.1            6.0.0        Universal Repository Manager supporting all maj...
stable/artifactory-ha          0.2.1            6.0.0        Universal Repository Manager supporting all maj...
stable/auditbeat               0.1.0            6.2.4        A lightweight shipper to audit the activities o...
stable/aws-cluster-autoscaler  0.3.3                         Scales worker nodes within autoscaling groups.
stable/bitcoind                0.1.3            0.15.1       Bitcoin is an innovative payment network and a ...
stable/buildkite               0.2.3            3            Agent for Buildkite
stable/burrow                  0.4.4            0.17.1       Burrow is a permissionable smart contract machine
stable/centrifugo              2.0.1            1.7.3        Centrifugo is a real-time messaging server.
stable/cerebro                 0.1.0            0.7.3        A Helm chart for Cerebro - a web admin tool tha...
stable/cert-manager            v0.3.3           v0.3.1       A Helm chart for cert-manager
stable/chaoskube               0.7.0            0.8.0        Chaoskube periodically kills random pods in you...
stable/chartmuseum             1.5.0            0.7.0        Helm Chart Repository with support for Amazon S...
stable/chronograf              0.4.5            1.3          Open-source web application written in Go and R...
stable/cluster-autoscaler      0.6.4            1.2.2        Scales worker nodes within autoscaling groups.
stable/cockroachdb             1.1.1            2.0.0        CockroachDB is a scalable, survivable, strongly...
stable/concourse               1.10.1           3.14.1       Concourse is a simple and scalable CI system.
stable/consul                  3.2.0            1.0.0        Highly available and distributed service discov...
stable/coredns                 0.9.0            1.0.6        CoreDNS is a DNS server that chains plugins and...
stable/coscale                 0.2.1            3.9.1        CoScale Agent
stable/dask                    1.0.4            0.17.4       Distributed computation in Python with task sch...
stable/dask-distributed        2.0.2                         DEPRECATED: Distributed computation in Python
stable/datadog                 0.18.0           6.3.0        DataDog Agent
...
```

Para atualizar a lista de gráficos, utilize o [atualização de repositório helm] [ helm-repo-update] comando. O exemplo seguinte mostra uma atualização do repositório com êxito:

```console
$ helm repo update

Hang tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

## <a name="run-helm-charts"></a>Executar gráficos Helm

Para instalar os gráficos com Helm, utilize o [helm install] [ helm-install] de comando e especifique o nome do gráfico para instalar. Para ver isso em ação, vamos instalar uma implementação básica do Wordpress com um gráfico Helm. Se tiver configurado o TLS/SSL, adicione o `--tls` parâmetro a utilizar o certificado de cliente do Helm.

```console
helm install stable/wordpress
```

O resultado de exemplo condensado seguinte mostra o estado de implementação dos recursos do Kubernetes criados pelo gráfico Helm:

```
$ helm install stable/wordpress

NAME:   wishful-mastiff
LAST DEPLOYED: Thu Jul 12 15:53:56 2018
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1beta1/Deployment
NAME                       DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
wishful-mastiff-wordpress  1        1        1           0          1s

==> v1beta1/StatefulSet
NAME                     DESIRED  CURRENT  AGE
wishful-mastiff-mariadb  1        1        1s

==> v1/Pod(related)
NAME                                        READY  STATUS   RESTARTS  AGE
wishful-mastiff-wordpress-6f96f8fdf9-q84sz  0/1    Pending  0         1s
wishful-mastiff-mariadb-0                   0/1    Pending  0         1s

==> v1/Secret
NAME                       TYPE    DATA  AGE
wishful-mastiff-mariadb    Opaque  2     2s
wishful-mastiff-wordpress  Opaque  2     2s

==> v1/ConfigMap
NAME                           DATA  AGE
wishful-mastiff-mariadb        1     2s
wishful-mastiff-mariadb-tests  1     2s

==> v1/PersistentVolumeClaim
NAME                       STATUS   VOLUME   CAPACITY  ACCESS MODES  STORAGECLASS  AGE
wishful-mastiff-wordpress  Pending  default  2s

==> v1/Service
NAME                       TYPE          CLUSTER-IP   EXTERNAL-IP  PORT(S)                     AGE
wishful-mastiff-mariadb    ClusterIP     10.1.116.54  <none>       3306/TCP                    2s
wishful-mastiff-wordpress  LoadBalancer  10.1.217.64  <pending>    80:31751/TCP,443:31264/TCP  2s
...
```

Demora um minuto ou dois para o *EXTERNAL-IP* endereço do serviço Wordpress para ser preenchido e permitem-lhe aceder ao mesmo com um navegador da web.

## <a name="list-helm-releases"></a>Lista de versões do Helm

Para ver uma lista das versões instaladas no seu cluster, utilize o [o comando helm list] [ helm-list] comando. O exemplo seguinte mostra a versão Wordpress implementada no passo anterior. Se tiver configurado o TLS/SSL, adicione o `--tls` parâmetro a utilizar o certificado de cliente do Helm.

```console
$ helm list

NAME             REVISION    UPDATED                     STATUS      CHART              NAMESPACE
wishful-mastiff  1           Thu Jul 12 15:53:56 2018    DEPLOYED    wordpress-2.1.3  default
```

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre como gerir implementações de aplicações do Kubernetes com Helm, consulte a documentação do Helm.

> [!div class="nextstepaction"]
> [Documentação do Helm][helm-documentation]

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
[helm-ssl]: https://docs.helm.sh/using_helm/#using-ssl-between-helm-and-tiller

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
