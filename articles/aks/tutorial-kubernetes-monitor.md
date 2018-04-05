---
title: Tutorial do Kubernetes no Azure – Monitorizar Kubernetes
description: Tutorial do AKS - Monitorizar o Kubernetes com o Azure Log Analytics
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 86ae0c5ab302c49fa58df887d9dffef6cec31708
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="tutorial-monitor-azure-container-service-aks"></a>Tutorial: Monitorizar o Azure Container Service (AKS)

É fundamental monitorizar o cluster e contentores do Kubernetes, especialmente ao executar um cluster de produção em escala, com várias aplicações.

Neste tutorial, configura a monitorização do seu cluster do AKS com a [Solução de contentores para o Log Analytics][log-analytics-containers].

Este tutorial, parte sete de oito, abrange as seguintes tarefas:

> [!div class="checklist"]
> * Configurar a solução de monitorização do contentor
> * Configurar os agentes de monitorização
> * Aceder a informações de monitorização no portal do Azure

## <a name="before-you-begin"></a>Antes de começar

Nos tutoriais anteriores, foi compactada uma aplicação em imagens de contentores, carregadas estas imagens para o Azure Container Registry e criado um cluster de Kubernetes.

Se ainda não concluiu estes passos e pretende acompanhar, regresse ao [Tutorial 1 – Criar imagens de contentor][aks-tutorial-prepare-app].

## <a name="configure-the-monitoring-solution"></a>Configurar a solução de monitorização

No portal do Azure, selecione **Criar um recurso** e procure `Container Monitoring Solution`. Depois de localizar, selecione **Criar**.

![Adicionar solução](./media/container-service-tutorial-kubernetes-monitor/add-solution.png)

Crie uma nova área de trabalho do Log Analytics ou selecione uma existente. A Área de Trabalho do Log Analytics orienta-o ao longo deste processo.

Ao criar a área de trabalho, selecione **Afixar ao dashboard** para fácil obtenção.

![Área de trabalho do Log Analytics](./media/container-service-tutorial-kubernetes-monitor/oms-workspace.png)

Quando terminar, selecione **OK**. Depois de concluída a validação, selecione **criar** para criar o solução de monitorização do contentor.

Quando a área de trabalho estiver criada, é-lhe apresentada no portal do Azure.

## <a name="get-workspace-settings"></a>Obter definições da Área de Trabalho

O ID e a Chave da Área de Trabalho do Log analytics são necessários para configurar o agente de solução nos nós do Kubernetes.

Para obter estes valores, Selecione **Área de Trabalho OMS** no menu de soluções do contentor da esquerda. Selecione **Definições avançadas** e tome nota do **ID DA ÁREA DE TRABALHO** e **CHAVE PRIMÁRIA**.

## <a name="create-kubernetes-secret"></a>Criar segredo do Kubernetes

Armazene as definições da área de trabalho do Log Analytics no segredo do Kubernetes denominado `omsagent-secret` com o comando [kubectl create secret][kubectl-create-secret]. Atualize `WORKSPACE_ID` com o seu ID da área de trabalho do Log Analytics e `WORKSPACE_KEY` com a chave da área de trabalho.

```console
kubectl create secret generic omsagent-secret --from-literal=WSID=WORKSPACE_ID --from-literal=KEY=WORKSPACE_KEY
```

## <a name="configure-monitoring-agents"></a>Configurar os agentes de monitorização

O ficheiro de manifesto do Kubernetes seguinte pode ser utilizado para configurar agentes de monitorização do contentor num cluster de Kubernetes. Cria um [DaemonSet][kubernetes-daemonset] do Kubernetes, que executa um único pod em cada nó de cluster.

Guarde o seguinte texto num ficheiro denominado `oms-daemonset.yaml`.

```YAML
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
 name: omsagent
spec:
 template:
  metadata:
   labels:
    app: omsagent
    agentVersion: 1.4.3-174
    dockerProviderVersion: 1.0.0-30
  spec:
   containers:
     - name: omsagent 
       image: "microsoft/oms"
       imagePullPolicy: Always
       securityContext:
         privileged: true
       ports:
       - containerPort: 25225
         protocol: TCP 
       - containerPort: 25224
         protocol: UDP
       volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-sock
        - mountPath: /var/log 
          name: host-log
        - mountPath: /etc/omsagent-secret
          name: omsagent-secret
          readOnly: true
        - mountPath: /var/lib/docker/containers 
          name: containerlog-path  
       livenessProbe:
        exec:
         command:
         - /bin/bash
         - -c
         - ps -ef | grep omsagent | grep -v "grep"
        initialDelaySeconds: 60
        periodSeconds: 60
   nodeSelector:
    beta.kubernetes.io/os: linux    
   # Tolerate a NoSchedule taint on master that ACS Engine sets.
   tolerations:
    - key: "node-role.kubernetes.io/master"
      operator: "Equal"
      value: "true"
      effect: "NoSchedule"     
   volumes:
    - name: docker-sock 
      hostPath:
       path: /var/run/docker.sock
    - name: host-log
      hostPath:
       path: /var/log 
    - name: omsagent-secret
      secret:
       secretName: omsagent-secret
    - name: containerlog-path
      hostPath:
       path: /var/lib/docker/containers    
```

Crie o DaemonSet com o seguinte comando:

```azurecli
kubectl create -f oms-daemonset.yaml
```

Para ver se o DaemonSet está criado, execute:

```azurecli
kubectl get daemonset
```

O resultado é semelhante ao seguinte:

```
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR                 AGE
omsagent   3         3         3         3            3           beta.kubernetes.io/os=linux   8m
```

Após os agentes estarem em execução, demora alguns minutos para o Log Analytics ingerir e processar os dados.

## <a name="access-monitoring-data"></a>Aceder aos dados de monitorização

No portal do Azure, selecione a área de trabalho do Log Analytics que foi afixada no dashboard do portal. Clique no mosaico **Solução de Monitorização do Contentor**. Aqui pode encontrar informações sobre o cluster e contentores do AKS do cluster.

![Dashboard](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Veja a [documentação do Log Analytics do Azure][log-analytics-docs] para obter documentação de orientação detalhada sobre a consulta e análise de dados de monitorização.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, monitorizou o cluster de Kubernetes com o Log Analytics. Tarefas abrangidas incluídas:

> [!div class="checklist"]
> * Configurar a solução de monitorização do contentor
> * Configurar os agentes de monitorização
> * Aceder a informações de monitorização no portal do Azure

Avance para o próximo tutorial para saber mais sobre a atualização do Kubernetes para uma nova versão.

> [!div class="nextstepaction"]
> [Atualizar Kubernetes][aks-tutorial-upgrade]

<!-- LINKS - external -->
[kubectl-create-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-daemonset]: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-upgrade]: ./tutorial-kubernetes-upgrade-cluster.md
[log-analytics-containers]: ../log-analytics/log-analytics-containers.md
[log-analytics-docs]: ../log-analytics/index.yml
