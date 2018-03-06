---
title: Tutorial do Azure Container Service - monitorizar o Kubernetes
description: Tutorial do Azure Container Service - monitorizar o Kubernetes com o Microsoft Operations Management Suite (OMS)
services: container-service
author: dlepow
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/26/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 965ce4b7e154684fc1d171c90f17498afc828a66
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2018
---
# <a name="monitor-a-kubernetes-cluster-with-operations-management-suite"></a>Monitorizar um cluster do Kubernetes com o Operations Management Suite

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

É fundamental monitorizar o cluster e os contentores do Kubernetes, especialmente ao gerir um cluster de produção em escala, com várias aplicações. 

Pode tirar partido das várias soluções de monitorização do Kubernetes, da Microsoft ou de outros fornecedores. Neste tutorial, vai monitorizar o seu cluster do Kubernetes com a solução Contentores do [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), a solução da Microsoft para gestão de TI com base na cloud. (A solução Contentores do OMS está em pré-visualização.)

Este tutorial, a parte sete de sete, abrange as seguintes tarefas:

> [!div class="checklist"]
> * Obter definições da Área de Trabalho do OMS
> * Configurar os agentes OMS em nós do Kubernetes
> * Aceder a informações de monitorização no portal do OMS ou no portal do Azure

## <a name="before-you-begin"></a>Antes de começar

Nos tutoriais anteriores, foi compactada uma aplicação em imagens de contentores, carregadas estas imagens para o Azure Container Registry e criado um cluster de Kubernetes. 

Se ainda não concluiu estes passos e pretende acompanhar, regresse ao [Tutorial 1 – Criar imagens de contentor](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="get-workspace-settings"></a>Obter definições da Área de Trabalho

Quando puder aceder ao [portal do OMS](https://mms.microsoft.com), aceda a **Definições** > **Origens Ligadas** > **Servidores Linux**. Aqui, pode encontrar o *ID da Área de Trabalho* e uma *Chave da Área de Trabalho* primária ou secundária. Tome nota destes valores, pois precisará deles para configurar os agentes OMS no cluster.

## <a name="set-up-oms-agents"></a>Configurar os agentes OMS

Segue-se um ficheiro YAML para configurar os agentes OMS em nós de cluster do Linux. Cria um [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) do Kubernetes, que executa um único pod idêntico em cada nó de cluster. O recurso DaemonSet é ideal para implementar um agente de monitorização. 

Guarde o seguinte texto num ficheiro denominado `oms-daemonset.yaml` e substitua os valores de marcador de posição de *myWorkspaceID* e *myWorkspaceKey* pelos seus ID da Área de Trabalho e Chave do OMS. (Na produção, pode codificar estes valores como segredos.)

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
    agentVersion: v1.3.4-127
    dockerProviderVersion: 10.0.0-25
  spec:
   containers:
     - name: omsagent 
       image: "microsoft/oms"
       imagePullPolicy: Always
       env:
       - name: WSID
         value: myWorkspaceID
       - name: KEY 
         value: myWorkspaceKey
       - name: DOMAIN
         value: opinsights.azure.com
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
       livenessProbe:
        exec:
         command:
         - /bin/bash
         - -c
         - ps -ef | grep omsagent | grep -v "grep"
        initialDelaySeconds: 60
        periodSeconds: 60
   volumes:
    - name: docker-sock 
      hostPath:
       path: /var/run/docker.sock
    - name: host-log
      hostPath:
       path: /var/log
```

Crie o DaemonSet com o seguinte comando:

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

Para ver se o DaemonSet está criado, execute:

```azurecli-interactive
kubectl get daemonset
```

O resultado é semelhante ao seguinte:

```azurecli-interactive
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

Após os agentes estarem em execução, demora alguns minutos para o OMS ingerir e processar os dados.

## <a name="access-monitoring-data"></a>Aceder aos dados de monitorização

Veja e analise o dados de monitorização do contentor OMS com a [solução Contentor](../../log-analytics/log-analytics-containers.md) no portal do OMS ou no portal do Azure. 

Para instalar a solução Contentor através do [portal do OMS](https://mms.microsoft.com), aceda à **Galeria de Soluções**. Em seguida, adicione **Solução Contentor**. Em alternativa, adicione a solução Contentores do [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).

No portal do OMS, procure um mosaico de resumo **Contentores** no dashboard do OMS. Clique no mosaico para obter detalhes, incluindo: eventos, erros, estado, inventário de imagens e a utilização da CPU e memória dos contentores. Para obter informações mais detalhadas, clique num registo em qualquer mosaico ou efetue uma [pesquisa de registos](../../log-analytics/log-analytics-log-searches.md).

![Dashboard Contentores no portal do OMS](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Da mesma forma, no portal do Azure, aceda ao **Log Analytics** e selecione o nome da sua área de trabalho. Para ver o mosaico de resumo **Contentores**, clique em **Soluções** > **Contentores**. Para ver os detalhes, clique no mosaico.

Veja a [documentação do Azure Log Analytics](../../log-analytics/index.yml) para obter documentação de orientação detalhada sobre a consulta e análise de dados de monitorização.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, monitorizou o cluster de Kubernetes com o OMS. Tarefas abrangidas incluídas:

> [!div class="checklist"]
> * Obter definições da Área de Trabalho do OMS
> * Configurar os agentes OMS em nós do Kubernetes
> * Aceder a informações de monitorização no portal do OMS ou no portal do Azure


Siga esta ligação para ver os exemplos de scripts pré-criados do Container Service.

> [!div class="nextstepaction"]
> [Exemplos de script do Azure Container Service](cli-samples.md)
