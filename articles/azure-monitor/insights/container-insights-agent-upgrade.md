---
title: Como atualizar o Azure Monitor para agente de contentores (pré-visualização) | Documentos da Microsoft
description: Este artigo descreve como atualizar o agente de Log Analytics utilizado pelo Azure Monitor para contentores.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2018
ms.author: magoedte
ms.openlocfilehash: 026bdd6a59dc84220e7e52707cee3c1971fba838
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53104324"
---
# <a name="how-to-upgrade-the-azure-monitor-for-containers-preview-agent"></a>Como atualizar o Azure Monitor para agente de contentores (pré-visualização)
Monitor do Azure para contentores utiliza uma versão em contentores do agente do Log Analytics para Linux. Quando for lançada uma nova versão do agente, o agente é automaticamente atualizado nos seus clusters do Kubernetes geridos alojados no Azure Kubernetes Service (AKS).  

Se a atualização de agente falhar, este artigo descreve o processo de atualizar manualmente o agente. Para seguir as versões lançadas, consulte [anúncios de lançamentos de agente](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).   

## <a name="upgrading-agent-on-monitored-kubernetes-cluster"></a>Atualizar o agente no cluster de Kubernetes monitorizado
O processo para atualizar o agente consiste em dois passos direta. A primeira etapa é desativar a monitorização com o Azure Monitor para contentores com a CLI do Azure.  Siga os passos descritos no [desativar a monitorização](container-insights-optout.md?toc=%2fazure%2fmonitoring%2ftoc.json#azure-cli) artigo. Com a CLI do Azure permite-nos remover o agente a partir de nós do cluster sem afetar a solução e os dados correspondentes, que são armazenados na área de trabalho. 

>[!NOTE]
>Enquanto estiver a efetuar esta atividade de manutenção, os nós do cluster não estão a reencaminhar os dados recolhidos e vistas de desempenho não apresentará dados entre a hora a remover o agente e instalar a nova versão. 
>

Para instalar a nova versão do agente, siga os passos descritos na [monitorização carregar](container-insights-onboard.md?toc=%2fazure%2fmonitoring%2ftoc.json#enable-monitoring-using-azure-cli) artigo com a CLI do Azure, para concluir este processo.  

Depois da ativar a monitorização, poderá demorar cerca de 15 minutos antes de poder visualizar as métricas de estado de funcionamento atualizado para o cluster. Para verificar se o agente atualizado com êxito, execute o comando: `kubectl logs omsagent-484hw --namespace=kube-system`

O estado deve assemelhar-se o exemplo seguinte, onde o valor para *omi* e *omsagent* deve corresponder à versão mais recente especificada no [histórico de versões de agente](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).  

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
    omi 1.4.2.5
    omsagent 1.6.0-163
    docker-cimprov 1.0.0.31

## <a name="next-steps"></a>Passos Seguintes
Se ocorrerem problemas durante a atualização do agente, consulte a [guia de resolução de problemas](container-insights-troubleshoot.md) para o suporte.