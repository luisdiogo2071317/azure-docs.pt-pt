---
title: "Tutorial do Kubernetes no Azure – Monitorizar Kubernetes"
description: Tutorial do AKS - Monitorize o Kubernetes com o Microsoft Operations Management Suite (OMS)
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2fedd615733e3bf51469d3b69d5fe51e3e99087e
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/24/2018
---
# <a name="monitor-azure-container-service-aks"></a>Monitorizar o Azure Container Service (AKS)

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

Crie um novo espaço de trabalho ou selecione um existente. O formulário da Área de Trabalho OMS orienta-o ao longo deste processo.

Ao criar a área de trabalho, selecione **Afixar ao dashboard** para fácil obtenção.

![Área de trabalho do OMS](./media/container-service-tutorial-kubernetes-monitor/oms-workspace.png)

Quando terminar, selecione **OK**. Depois de concluída a validação, selecione **criar** para criar o solução de monitorização do contentor.

Quando a área de trabalho estiver criada, é-lhe apresentada no portal do Azure.

## <a name="get-workspace-settings"></a>Obter definições da Área de Trabalho

O ID e a Chave da Área de Trabalho do Log analytics são necessários para configurar o agente de solução nos nós do Kubernetes.

Para obter estes valores, Selecione **Área de Trabalho OMS** no menu de soluções do contentor da esquerda. Selecione **Definições avançadas** e tome nota do **ID DA ÁREA DE TRABALHO** e **CHAVE PRIMÁRIA**.

## <a name="configure-monitoring-agents"></a>Configurar os agentes de monitorização

O ficheiro de manifesto do Kubernetes seguinte pode ser utilizado para configurar agentes de monitorização do contentor num cluster de Kubernetes. Cria um [DaemonSet][kubernetes-daemonset] do Kubernetes, que executa um único pod em cada nó de cluster.

Guarde o seguinte texto num ficheiro denominado `oms-daemonset.yaml` e substitua os valores de marcador de posição para `WSID` e `KEY` com o ID e a Chave da Área de Trabalho do Log Analytics.

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
    agentVersion: 1.4.0-12
    dockerProviderVersion: 10.0.0-25
  spec:
   containers:
     - name: omsagent
       image: "microsoft/oms"
       imagePullPolicy: Always
       env:
       - name: WSID
         value: <WSID>
       - name: KEY
         value: <KEY>
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
        - mountPath: /var/opt/microsoft/omsagent/state/containerhostname
          name: container-hostname
        - mountPath: /var/log
          name: host-log
        - mountPath: /var/lib/docker/containers/
          name: container-log
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
    - name: container-hostname
      hostPath:
       path: /etc/hostname
    - name: host-log
      hostPath:
       path: /var/log
    - name: container-log
      hostPath:
       path: /var/lib/docker/containers/
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

Após os agentes estarem em execução, demora alguns minutos para o OMS ingerir e processar os dados.

## <a name="access-monitoring-data"></a>Aceder aos dados de monitorização

No portal do Azure, selecione a área de trabalho do Log Analytics que foi afixada no dashboard do portal. Clique no mosaico **Solução de Monitorização do Contentor**. Aqui pode encontrar informações sobre o cluster e contentores do AKS do cluster.

![Dashboard](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Veja a [documentação do Log Analytics do Azure][log-analytics-docs] para obter documentação de orientação detalhada sobre a consulta e análise de dados de monitorização.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, monitorizou o cluster de Kubernetes com o OMS. Tarefas abrangidas incluídas:

> [!div class="checklist"]
> * Configurar a solução de monitorização do contentor
> * Configurar os agentes de monitorização
> * Aceder a informações de monitorização no portal do Azure

Avance para o próximo tutorial para saber mais sobre a atualização do Kubernetes para uma nova versão.

> [!div class="nextstepaction"]
> [Atualizar Kubernetes][aks-tutorial-upgrade]

<!-- LINKS - external -->
[kubernetes-daemonset]: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-upgrade]: ./tutorial-kubernetes-upgrade-cluster.md
[log-analytics-containers]: ../log-analytics/log-analytics-containers.md
[log-analytics-docs]: ../log-analytics/index.yml
