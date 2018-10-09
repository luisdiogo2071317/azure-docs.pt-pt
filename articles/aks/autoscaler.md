---
title: Kubernetes no Azure - dimensionamento automático de Cluster
description: Saiba como utilizar o dimensionamento automático de cluster com o Azure Kubernetes Service (AKS) para dimensionar automaticamente o seu cluster para satisfazer a procura.
services: container-service
author: sakthivetrivel
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/19/18
ms.author: sakthivetrivel
ms.custom: mvc
ms.openlocfilehash: 3bac6534f43d62e6eb9381b8513025ba9117ed04
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857011"
---
# <a name="cluster-autoscaler-on-azure-kubernetes-service-aks---preview"></a>Dimensionamento automático de cluster no Azure Kubernetes Service (AKS) - pré-visualização

O Azure Kubernetes Service (AKS) fornece uma solução flexível para implementar um cluster de Kubernetes gerido no Azure. Como recurso procura aumenta, o cluster de dimensionamento automático permite que o cluster para crescer para satisfazer essa demanda com base nas restrições que definir. O dimensionamento automático de cluster (AC) faz isso ao dimensionar os nós de agente com base em pendentes pods. Este analisa o cluster periodicamente para verificar a existência de pods ou nós vazios pendentes e aumenta o tamanho se possível. Por predefinição, a AC verifica a existência pendentes pods cada 10 segundos e remove um nó, se for desnecessárias durante mais de 10 minutos. Quando utilizado com o [dimensionamento automático de horizontal de pods](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) (HPA), irá atualizar o HPA de réplicas de pod e recursos de acordo com a pedido. Caso haja suficiente nós ou desnecessários seguindo este pod, dimensionamento, a AC responderá e agendar os pods no novo conjunto de nós.

Este artigo descreve como implementar o dimensionamento automático de cluster em nós de agente. No entanto, uma vez que o dimensionamento automático do cluster é implementado no namespace kube system, o dimensionamento automático não irá reduzir verticalmente o nó em execução desse pod.

> [!IMPORTANT]
> Integração de dimensionamento automático de cluster do Azure Kubernetes Service (AKS) está atualmente em **pré-visualização**. As pré-visualizações são tornadas disponíveis para si na condição de concordar com os [termos suplementares de utilização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Alguns aspetos desta funcionalidade podem alterar-se após a disponibilidade geral (GA).
>

## <a name="prerequisites"></a>Pré-requisitos

Este documento parte do princípio de que tem um cluster do AKS habilitados no RBAC. Se precisar de um cluster do AKS, consulte a [início rápido do Azure Kubernetes Service (AKS)][aks-quick-start].

 Para utilizar o dimensionamento automático de cluster, o cluster tem de ser a utilizar o Kubernetes v1.10.X ou superior e têm de ter ativado o RBAC. Para atualizar o cluster, consulte o artigo sobre [atualizar um cluster do AKS][aks-upgrade].

## <a name="gather-information"></a>Recolher informações

Para gerar as permissões para o dimensionamento automático de cluster executar no seu cluster, execute este script de bash:

```sh
#! /bin/bash
ID=`az account show --query id -o json`
SUBSCRIPTION_ID=`echo $ID | tr -d '"' `

TENANT=`az account show --query tenantId -o json`
TENANT_ID=`echo $TENANT | tr -d '"' | base64`

read -p "What's your cluster name? " cluster_name
read -p "Resource group name? " resource_group

CLUSTER_NAME=`echo $cluster_name | base64`
RESOURCE_GROUP=`echo $resource_group | base64`

PERMISSIONS=`az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/$SUBSCRIPTION_ID" -o json`
CLIENT_ID=`echo $PERMISSIONS | sed -e 's/^.*"appId"[ ]*:[ ]*"//' -e 's/".*//' | base64`
CLIENT_SECRET=`echo $PERMISSIONS | sed -e 's/^.*"password"[ ]*:[ ]*"//' -e 's/".*//' | base64`

SUBSCRIPTION_ID=`echo $ID | tr -d '"' | base64 `

NODE_RESOURCE_GROUP=`az aks show --name $cluster_name  --resource-group $resource_group -o tsv --query 'nodeResourceGroup' | base64`

echo "---
apiVersion: v1
kind: Secret
metadata:
    name: cluster-autoscaler-azure
    namespace: kube-system
data:
    ClientID: $CLIENT_ID
    ClientSecret: $CLIENT_SECRET
    ResourceGroup: $RESOURCE_GROUP
    SubscriptionID: $SUBSCRIPTION_ID
    TenantID: $TENANT_ID
    VMType: QUtTCg==
    ClusterName: $CLUSTER_NAME
    NodeResourceGroup: $NODE_RESOURCE_GROUP
---"
```

Depois de seguir os passos no script, o script irá transmitir os seus detalhes na forma de um segredo, da seguinte forma:

```yaml
---
apiVersion: v1
kind: Secret
metadata:
  name: cluster-autoscaler-azure
  namespace: kube-system
data:
  ClientID: <base64-encoded-client-id>
  ClientSecret: <base64-encoded-client-secret>$
  ResourceGroup: <base64-encoded-resource-group>  SubscriptionID: <base64-encode-subscription-id>
  TenantID: <base64-encoded-tenant-id>
  VMType: QUtTCg==
  ClusterName: <base64-encoded-clustername>
  NodeResourceGroup: <base64-encoded-node-resource-group>
---
```

Em seguida, obtém o nome do seu conjunto de nós, executando o seguinte comando. 

```console
$ kubectl get nodes --show-labels
```

Saída:

```console
NAME                       STATUS    ROLES     AGE       VERSION   LABELS
aks-nodepool1-37756013-0   Ready     agent     1h        v1.10.3   agentpool=nodepool1,beta.kubernetes.io/arch=amd64,beta.kubernetes.io/instance-type=Standard_DS1_v2,beta.kubernetes.io/os=linux,failure-domain.beta.kubernetes.io/region=eastus,failure-domain.beta.kubernetes.io/zone=0,kubernetes.azure.com/cluster=MC_[resource-group]\_[cluster-name]_[location],kubernetes.io/hostname=aks-nodepool1-37756013-0,kubernetes.io/role=agent,storageprofile=managed,storagetier=Premium_LRS
 ```

Em seguida, extraia o valor da etiqueta **agentpool**. O nome predefinido para o conjunto de nós de um cluster é "nodepool1".

Agora, usando o conjunto de nós e o segredo, é possível criar um gráfico de implementação.

## <a name="create-a-deployment-chart"></a>Criar um gráfico de implementação

Crie um ficheiro denominado `aks-cluster-autoscaler.yaml`e copie no código YAML seguinte.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
  name: cluster-autoscaler
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRole
metadata:
  name: cluster-autoscaler
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
rules:
- apiGroups: [""]
  resources: ["events","endpoints"]
  verbs: ["create", "patch"]
- apiGroups: [""]
  resources: ["pods/eviction"]
  verbs: ["create"]
- apiGroups: [""]
  resources: ["pods/status"]
  verbs: ["update"]
- apiGroups: [""]
  resources: ["endpoints"]
  resourceNames: ["cluster-autoscaler"]
  verbs: ["get","update"]
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["watch","list","get","update"]
- apiGroups: [""]
  resources: ["pods","services","replicationcontrollers","persistentvolumeclaims","persistentvolumes"]
  verbs: ["watch","list","get"]
- apiGroups: ["extensions"]
  resources: ["replicasets","daemonsets"]
  verbs: ["watch","list","get"]
- apiGroups: ["policy"]
  resources: ["poddisruptionbudgets"]
  verbs: ["watch","list"]
- apiGroups: ["apps"]
  resources: ["statefulsets"]
  verbs: ["watch","list","get"]
- apiGroups: ["storage.k8s.io"]
  resources: ["storageclasses"]
  verbs: ["get", "list", "watch"]

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: Role
metadata:
  name: cluster-autoscaler
  namespace: kube-system
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
rules:
- apiGroups: [""]
  resources: ["configmaps"]
  verbs: ["create"]
- apiGroups: [""]
  resources: ["configmaps"]
  resourceNames: ["cluster-autoscaler-status"]
  verbs: ["delete","get","update"]

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: cluster-autoscaler
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-autoscaler
subjects:
  - kind: ServiceAccount
    name: cluster-autoscaler
    namespace: kube-system

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: RoleBinding
metadata:
  name: cluster-autoscaler
  namespace: kube-system
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: cluster-autoscaler
subjects:
  - kind: ServiceAccount
    name: cluster-autoscaler
    namespace: kube-system

---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    app: cluster-autoscaler
  name: cluster-autoscaler
  namespace: kube-system
spec:
  replicas: 1
  selector:
    matchLabels:
      app: cluster-autoscaler
  template:
    metadata:
      labels:
        app: cluster-autoscaler
    spec:
      serviceAccountName: cluster-autoscaler
      containers:
      - image: gcr.io/google-containers/cluster-autoscaler:v1.2.2
        imagePullPolicy: Always
        name: cluster-autoscaler
        resources:
          limits:
            cpu: 100m
            memory: 300Mi
          requests:
            cpu: 100m
            memory: 300Mi
        command:
        - ./cluster-autoscaler
        - --v=3
        - --logtostderr=true
        - --cloud-provider=azure
        - --skip-nodes-with-local-storage=false
        - --nodes=1:10:nodepool1
        env:
        - name: ARM_SUBSCRIPTION_ID
          valueFrom:
            secretKeyRef:
              key: SubscriptionID
              name: cluster-autoscaler-azure
        - name: ARM_RESOURCE_GROUP
          valueFrom:
            secretKeyRef:
              key: ResourceGroup
              name: cluster-autoscaler-azure
        - name: ARM_TENANT_ID
          valueFrom:
            secretKeyRef:
              key: TenantID
              name: cluster-autoscaler-azure
        - name: ARM_CLIENT_ID
          valueFrom:
            secretKeyRef:
              key: ClientID
              name: cluster-autoscaler-azure
        - name: ARM_CLIENT_SECRET
          valueFrom:
            secretKeyRef:
              key: ClientSecret
              name: cluster-autoscaler-azure
        - name: ARM_VM_TYPE
          valueFrom:
            secretKeyRef:
              key: VMType
              name: cluster-autoscaler-azure
        - name: AZURE_CLUSTER_NAME
          valueFrom:
            secretKeyRef:
              key: ClusterName
              name: cluster-autoscaler-azure
        - name: AZURE_NODE_RESOURCE_GROUP
          valueFrom:
            secretKeyRef:
              key: NodeResourceGroup
              name: cluster-autoscaler-azure
      restartPolicy: Always
```

Copie e cole o segredo criado no passo anterior e inseri-la no início do ficheiro.

Em seguida, para definir o intervalo de nós, preencha o argumento `--nodes` em `command` sob a forma MIN:MAX:NODE_POOL_NAME. Por exemplo: `--nodes=3:10:nodepool1` define o número mínimo de nós para 3, o número máximo de nós para 10 e o nome do conjunto de nós para nodepool1.

Em seguida, preencha o campo de imagem sob **contentores** com a versão do dimensionamento automático de cluster que pretende utilizar. AKS requer v1.2.2 ou mais recente. Este exemplo usa v1.2.2.

## <a name="deployment"></a>Implementação

Implementar o dimensionamento automático do cluster através da execução

```console
kubectl create -f aks-cluster-autoscaler.yaml
```

Para verificar se o dimensionamento automático de cluster está em execução, utilize o seguinte comando e verifique a lista de pods. Deve haver um pod prefixado com "cluster-dimensionamento automático" em execução. Se vir isto, o dimensionamento automático de cluster foi implementado.

```console
kubectl -n kube-system get pods
```

Para ver o estado do dimensionamento automático cluster, execute

```console
kubectl -n kube-system describe configmap cluster-autoscaler-status
```

## <a name="interpreting-the-cluster-autoscaler-status"></a>Interpretando o estado do dimensionamento automático de cluster

```console
$ kubectl -n kube-system describe configmap cluster-autoscaler-status
Name:         cluster-autoscaler-status
Namespace:    kube-system
Labels:       <none>
Annotations:  cluster-autoscaler.kubernetes.io/last-updated=2018-07-25 22:59:22.661669494 +0000 UTC

Data
====
status:
----
Cluster-autoscaler status at 2018-07-25 22:59:22.661669494 +0000 UTC:
Cluster-wide:
  Health:      Healthy (ready=1 unready=0 notStarted=0 longNotStarted=0 registered=1 longUnregistered=0)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleUp:     NoActivity (ready=1 registered=1)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleDown:   NoCandidates (candidates=0)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC

NodeGroups:
  Name:        nodepool1
  Health:      Healthy (ready=1 unready=0 notStarted=0 longNotStarted=0 registered=1 longUnregistered=0 cloudProviderTarget=1 (minSize=1, maxSize=5))
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleUp:     NoActivity (ready=1 cloudProviderTarget=1)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC
  ScaleDown:   NoCandidates (candidates=0)
               LastProbeTime:      2018-07-25 22:59:22.067828801 +0000 UTC
               LastTransitionTime: 2018-07-25 00:38:36.41372897 +0000 UTC


Events:  <none>
```

O estado do dimensionamento automático de cluster permite-lhe ver o estado do dimensionamento automático cluster em dois níveis diferentes: em todo o cluster e em cada grupo de nó. Uma vez que atualmente o AKS suporta apenas um conjunto de nós, estas métricas são os mesmos.

* Estado de funcionamento indica o estado de funcionamento geral de nós. Se o dimensionamento automático de cluster precisa lutar criar ou remove nós no cluster, este estado será alterado para "Mau estado de funcionamento". Também é uma análise detalhada do Estado de diferentes nós:
    * "Pronto" significa que um nó é um preparado para ter pods agendadas no mesmo.
    * "Unready" significa que um nó que dividiu depois de iniciada.
    * "NotStarted" significa que um nó não está completamente iniciado.
    * "LongNotStarted" significa que um nó não conseguiu iniciar dentro de um limite razoável.
    * "Registado significa que é registado um nó do grupo
    * "Anular o registo" significa que um nó estiver presente no lado do fornecedor de cluster, mas não conseguiu registar no Kubernetes.
  
* ScaleUp permite-lhe verificar quando o cluster determina que um aumento vertical deve ocorrer no seu cluster.
    * Uma transição é quando altera o número de nós no cluster ou o estado das alterações de um nó.
    * O número de nós prontos é o número de nós disponíveis e prontos no cluster. 
    * O cloudProviderTarget é o número de nós, que o dimensionamento automático de cluster determinou que o cluster precisa lidar com a sua carga de trabalho.

* ScaleDown permite-lhe verificar se existem candidatos para a escala para baixo. 
    * Um candidato para reduzir verticalmente é um nó que determinou o dimensionamento automático do cluster pode ser removido sem afetar a capacidade do cluster para processar a carga de trabalho. 
    * Os tempos de fornecido mostram a última vez o cluster foi marcado para reduzir verticalmente candidatos e sua última hora de transição.

Por fim, em eventos, pode ver a qualquer escala ou reduzir verticalmente a eventos, com falhas ou com êxito e os tempos, de que o dimensionamento automático do cluster tem levadas a cabo.

## <a name="next-steps"></a>Passos Seguintes

Para utilizar o dimensionamento automático de cluster com o dimensionamento automático horizontal de pods, confira [Dimensionar aplicação Kubernetes e a infraestrutura][aks-tutorial-scale].

Saiba mais sobre como implementar e gerir AKS com os tutoriais de AKS.

> [!div class="nextstepaction"]
> [Tutorial do AKS][aks-tutorial-prepare-app]

<!-- LINKS - internal -->
[aks-quick-start]: ./kubernetes-walkthrough.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-scale]: ./tutorial-kubernetes-scale.md
[aks-upgrade]: ./upgrade-cluster.md

<!-- LINKS - external -->
[cluster-autoscale]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md
