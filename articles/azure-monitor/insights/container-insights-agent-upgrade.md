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
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: 57bfa47d60ffd8aa7c4240ab77f788773e426bd8
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51715868"
---
# <a name="how-to-upgrade-the-azure-monitor-for-containers-preview-agent"></a>Como atualizar o Azure Monitor para agente de contentores (pré-visualização)
Monitor do Azure para contentores utiliza uma versão em contentores do agente do Log Analytics para Linux. Quando for lançada uma nova versão do agente, o agente não é atualizado automaticamente nos seus clusters do Kubernetes geridos alojados no Azure Kubernetes Service (AKS).

Este artigo descreve o processo para atualizar o agente.

## <a name="upgrading-agent-on-monitored-kubernetes-cluster"></a>Atualizar o agente no cluster de Kubernetes monitorizado
O processo para atualizar o agente consiste em dois passos direta. A primeira etapa é desativar a monitorização com o Azure Monitor para contentores com a CLI do Azure.  Siga os passos descritos no [desativar a monitorização](container-insights-optout.md?toc=%2fazure%2fmonitoring%2ftoc.json#azure-cli) artigo. Com a CLI do Azure permite-nos remover o agente a partir de nós do cluster sem afetar a solução e os dados correspondentes, que são armazenados na área de trabalho. 

>[!NOTE]
>Enquanto estiver a efetuar esta atividade de manutenção, os nós do cluster não estão a reencaminhar os dados recolhidos e vistas de desempenho não apresentará dados entre a hora a remover o agente e instalar a nova versão. 
>

Para instalar a nova versão do agente, siga os passos descritos na [monitorização carregar](container-insights-onboard.md?toc=%2fazure%2fmonitoring%2ftoc.json#enable-monitoring-using-azure-cli) artigo com a CLI do Azure, para concluir este processo.  

Depois da ativar a monitorização, poderá demorar cerca de 15 minutos antes de poder visualizar as métricas de estado de funcionamento atualizado para o cluster. 

## <a name="next-steps"></a>Passos Seguintes
Se ocorrerem problemas durante a atualização do agente, consulte a [guia de resolução de problemas](container-insights-troubleshoot.md) para o suporte.