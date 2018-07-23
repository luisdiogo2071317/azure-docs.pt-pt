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
ms.openlocfilehash: 4f8df8e7004ca3cee832b6230dc153b21e2a6c18
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186718"
---
# <a name="cluster-autoscaler-on-azure-kubernetes-service-aks---preview"></a>Dimensionamento automático de cluster no Azure Kubernetes Service (AKS) - pré-visualização

O Azure Kubernetes Service (AKS) fornece uma solução flexível para implementar um cluster de Kubernetes gerido no Azure. Como recurso procura aumenta, o cluster de dimensionamento automático permite que o cluster para crescer para satisfazer essa demanda com base nas restrições que definir. O dimensionamento automático de cluster (AC) faz isso ao dimensionar os nós de agente com base em pendentes pods. Este analisa o cluster periodicamente para verificar a existência de pods ou nós vazios pendentes e aumenta o tamanho se possível. Por predefinição, a AC verifica a existência pendentes pods cada 10 segundos e remove um nó, se for desnecessárias durante mais de 10 minutos. Quando utilizado com o dimensionamento horizontal de pods automático (HPA), atualizará o HPA de réplicas de pod e recursos de acordo com a pedido. Se não existirem suficiente nós ou desnecessários seguindo este pod, dimensionamento, a AC responderá e agendar os pods no novo conjunto de nós.

> [!IMPORTANT]
> Integração de dimensionamento automático de cluster do Azure Kubernetes Service (AKS) está atualmente em **pré-visualização**. As pré-visualizações são tornadas disponíveis para si na condição de concordar com os [termos suplementares de utilização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Alguns aspetos desta funcionalidade podem alterar-se após a disponibilidade geral (GA).
>

## <a name="prerequisites"></a>Pré-requisitos

Este documento parte do princípio de que tem um cluster do AKS habilitados no RBAC. Se precisar de um cluster do AKS, consulte a [início rápido do Azure Kubernetes Service (AKS)][aks-quick-start].

 Para utilizar o dimensionamento automático de cluster, o cluster tem de ser a utilizar o Kubernetes v1.10.X ou superior e têm de ter ativado o RBAC. Para atualizar o cluster, consulte o artigo sobre [atualizar um cluster do AKS][aks-upgrade].

## <a name="gather-information"></a>Recolher informações

A lista seguinte mostra todas as informações que tem de fornecer na definição do dimensionamento automático.

- *ID de subscrição*: ID correspondente à subscrição utilizada para este cluster
- *Nome do grupo de recursos* : nome do grupo de recursos do cluster pertence a 
- *Nome do cluster*: nome do cluster
- *ID de cliente*: ID da aplicação concedidas pela permissão gerar passo
- *Segredo do cliente*: segredo da aplicação concedido pela permissão gerar passo
- *ID do inquilino*: ID do inquilino (proprietário da conta)
- *Grupo de recursos de nó*: nome do grupo de recursos que contém os nós de agente no cluster
- *Nome do conjunto de nó*: nome do nó do agrupamento que gostaria de dimensionamento
- *Número mínimo de nós*: número mínimo de nós de existir no cluster
- *Número máximo de nós*: número máximo de nós de existir no cluster
- *Tipo de VM*: serviço utilizado para gerar o cluster de Kubernetes

Obtenha o seu ID de subscrição com: 

``` azurecli
az account show --query id
```

Gere um conjunto de credenciais do Azure ao executar o seguinte comando:

```console
$ az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscription-id>" --output json

"appId": <app-id>,
"displayName": <display-name>,
"name": <name>,
"password": <app-password>,
"tenant": <tenant-id>
```

O ID da aplicação, a palavra-passe e o ID de inquilino será o ID de cliente, clientSecret e tenantID nas etapas a seguir.

Obtenha o nome do seu conjunto de nós ao executar o seguinte comando. 

```console
$ kubectl get nodes --show-labels
```

Saída:

```console
NAME                       STATUS    ROLES     AGE       VERSION   LABELS
aks-nodepool1-37756013-0   Ready     agent     1h        v1.10.3   agentpool=nodepool1,beta.kubernetes.io/arch=amd64,beta.kubernetes.io/instance-type=Standard_DS1_v2,beta.kubernetes.io/os=linux,failure-domain.beta.kubernetes.io/region=eastus,failure-domain.beta.kubernetes.io/zone=0,kubernetes.azure.com/cluster=MC_[resource-group]\_[cluster-name]_[location],kubernetes.io/hostname=aks-nodepool1-37756013-0,kubernetes.io/role=agent,storageprofile=managed,storagetier=Premium_LRS
 ```

Em seguida, extraia o valor da etiqueta **agentpool**. O nome predefinido para o conjunto de nós de um cluster é "nodepool1".

Para obter o nome do seu grupo de recursos de nó, extrair o valor da etiqueta **kubernetes.azure.com<span></span>/cluster**. O nome de grupo de recursos de nó é, geralmente, o formato MC_ [grupo de recursos]\__ de [nome do cluster] [local].

O parâmetro vmType refere-se para o serviço a ser utilizado, eis o AKS.

Agora, deve ter as seguintes informações:

- subscriptionID
- ResourceGroup
- ClusterName
- ID de cliente
- ClientSecret
- TenantID
- NodeResourceGroup
- VMType

Em seguida, codifique a totalidade desses valores com base64. Por exemplo, para codificar o valor de VMType com base64:

```console
$ echo AKS | base64
QUtTCg==
```

## <a name="create-secret"></a>Criar segredo
Utilizando estes dados, crie um segredo para a implementação utilizando os valores encontrados nos passos anteriores no seguinte formato:

```yaml
---
apiVersion: v1
kind: Secret
metadata:
  name: cluster-autoscaler-azure
  namespace: kube-system
data:
  ClientID: <base64-encoded-client-id>
  ClientSecret: <base64-encoded-client-secret>
  ResourceGroup: <base64-encoded-resource-group>
  SubscriptionID: <base64-encode-subscription-id>
  TenantID: <base64-encoded-tenant-id>
  VMType: QUtTCg==
  ClusterName: <base64-encoded-clustername>
  NodeResourceGroup: <base64-encoded-node-resource-group>
---
```

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
      - image: k8s.gcr.io/cluster-autoscaler:{{ ca_version }}
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
kubectl create -f cluster-autoscaler-containerservice.yaml
```

Para verificar se o dimensionamento automático de cluster está em execução, utilize o seguinte comando e verifique a lista de pods. Se houver um pod prefixado com "cluster-dimensionamento automático" em execução, o dimensionamento automático de cluster foi implementado.

```console
kubectl -n kube-system get pods
```

Para ver o estado do dimensionamento automático cluster, execute

```console
kubectl -n kube-system describe configmap cluster-autoscaler-status
```

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
