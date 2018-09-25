---
title: Como resolver problemas relacionados com o Azure Monitor para contentores | Documentos da Microsoft
description: Este artigo descreve como pode resolver problemas e resolver problemas com o Azure Monitor para contentores.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2018
ms.author: magoedte
ms.openlocfilehash: de7ae5788224b83105e4dc9a24aea35c8b841c88
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46986732"
---
# <a name="troubleshooting-azure-monitor-for-containers"></a>Resolução de problemas do Azure Monitor para contentores

Quando configurar a monitorização do seu cluster do Azure Kubernetes Service (AKS) com o Azure Monitor para contentores, poderá ocorrer um problema que impede a recolha de dados ou comunicar o estado. Este artigo fornece detalhes sobre alguns problemas comuns e passos de resolução de problemas.

## <a name="azure-monitor-for-containers-is-enabled-but-not-reporting-any-information"></a>Monitor do Azure para contentores está ativado mas não comunicam quaisquer informações
Se o Monitor do Azure para contentores com êxito é ativado e configurado, mas não é possível ver informações de estado ou não são devolvidos resultados de uma consulta de registo do Log Analytics, diagnosticar o problema ao seguir estes passos: 

1. Verifique o estado do agente ao executar o comando: 

    `kubectl get ds omsagent --namespace=kube-system`

    A saída deve assemelhar-se o seguinte, que indica que foi implementado devidamente:

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```  
2. Verifique o estado de implementação com a versão do agente *06072018* ou posteriormente utilizando o comando:

    `kubectl get deployment omsagent-rs -n=kube-system`

    A saída deve assemelhar-se ao seguinte exemplo, o que indica que foi implementado devidamente:

    ```
    User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
    NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
    omsagent   1         1         1            1            3h
    ```

3. Verifique o estado de pod para verificar se ele está em execução com o comando: `kubectl get pods --namespace=kube-system`

    A saída deve assemelhar-se o exemplo a seguir com o estado *em execução* para o omsagent:

    ```
    User@aksuser:~$ kubectl get pods --namespace=kube-system 
    NAME                                READY     STATUS    RESTARTS   AGE 
    aks-ssh-139866255-5n7k5             1/1       Running   0          8d 
    azure-vote-back-4149398501-7skz0    1/1       Running   0          22d 
    azure-vote-front-3826909965-30n62   1/1       Running   0          22d 
    omsagent-484hw                      1/1       Running   0          1d 
    omsagent-fkq7g                      1/1       Running   0          1d 
    ```

4. Verifique os registos do agente. Quando o agente em contentores é implementado, ele executa uma verificação rápida ao executar comandos OMI e apresenta a versão do agente e fornecedor. 

5. Para verificar se o agente foram carregadas com êxito, execute o comando: `kubectl logs omsagent-484hw --namespace=kube-system`

    O estado deve assemelhar-se o exemplo seguinte:

    ```
    User@aksuser:~$ kubectl logs omsagent-484hw --namespace=kube-system
    :
    :
    instance of Container_HostInventory
    {
        [Key] InstanceID=3a4407a5-d840-4c59-b2f0-8d42e07298c2
        Computer=aks-nodepool1-39773055-0
        DockerVersion=1.13.1
        OperatingSystem=Ubuntu 16.04.3 LTS
        Volume=local
        Network=bridge host macvlan null overlay
        NodeRole=Not Orchestrated
        OrchestratorType=Kubernetes
    }
    Primary Workspace: b438b4f6-912a-46d5-9cb1-b44069212abc    Status: Onboarded(OMSAgent Running)
    omi 1.4.2.2
    omsagent 1.6.0.23
    docker-cimprov 1.0.0.31
    ```

## <a name="next-steps"></a>Passos Seguintes
Com a monitorização ativada para capturar métricas de estado de funcionamento para os nós de cluster do AKS e os pods, estas métricas de estado de funcionamento estão disponíveis no portal do Azure. Para saber como utilizar o Azure Monitor para contentores, veja [estado de funcionamento do serviço de Kubernetes do Azure de modo de exibição](monitoring-container-insights-analyze.md).