---
title: Alertas e métricas DNS do Azure | Microsoft Docs
description: Saiba mais sobre as métricas de DNS do Azure e os alertas.
services: dns
documentationcenter: na
author: KumudD
manager: jennoc
editor: ''
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2018
ms.author: kumud
ms.openlocfilehash: 54c4df446ee5c1bf8d29dd6c33b304f39ce8f1b8
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="azure-dns-metrics-and-alerts"></a>Alertas e métricas DNS do Azure
O DNS do Azure é um serviço de alojamento para domínios DNS que fornece a resolução do nome através da infraestrutura do Microsoft Azure. Este artigo descreve as métricas e alertas para o serviço DNS do Azure.

## <a name="azure-dns-metrics"></a>Métricas DNS do Azure

O DNS do Azure fornece métricas para os clientes para ativá-los monitorizar aspetos específicos do respetivo DNS aqueles alojado no serviço. Além disso, com a métrica de DNS do Azure, pode configurar e receber alertas com base nas condições de interesse. As métricas são fornecidas através de [serviço Azure Monitor](../monitoring-and-diagnostics/index.yml). O DNS do Azure fornece as métricas seguintes através do Monitor do Azure para as zonas DNS:

-   QueryVolume
-   RecordSetCount
-   RecordSetCapacityUtilization

Também pode ver o [definição destas métricas](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftnetworkdnszones) na página de documentação do Monitor do Azure.
>[!NOTE]
> Neste momento, estas métricas só estão disponíveis para as zonas DNS pública alojadas no DNS do Azure. Se tiver de zonas de privadas alojadas no DNS do Azure, estas métricas não irão fornecer dados para essas zonas. Além disso, as métricas e a funcionalidade alerta só é suportada na nuvem pública do Azure. Suporte para nuvens sovereign seguirá numa altura posterior. 

O nível granular de dimensão para estas métricas é a zona DNS.

### <a name="query-volume"></a>Volume de consulta

O *consulta Volume* métrica no DNS do Azure mostra o volume de consultas DNS (tráfego de consulta) que é recebido pelo DNS do Azure para a sua zona DNS. A unidade de medida é Contagem e a agregação é o total de todas as consultas recebidos através de um período de tempo. Para ver esta métrica, selecione a experiência do Explorador de métricas (pré-visualização) no separador Monitor no portal do Azure. Selecione a zona DNS na lista pendente de recursos, selecione a métrica de Volume de consulta e selecionar soma como a agregação. Abaixo captura de ecrã mostra um exemplo.  Para obter mais informações sobre o Explorador de métricas experiência e charting, consulte [Explorador de métricas de Monitor de Azure](../monitoring-and-diagnostics/monitoring-metric-charts.md).

![Volume de consulta](./media/dns-alerts-metrics/dns-metrics-query-volume.png)

*Figura: Métricas de Volume de consulta de DNS do Azure*

### <a name="record-set-count"></a>Contagem de conjunto de registos
O *registo definir contagem* métrica mostra o número de Recordsets no DNS do Azure para a sua zona DNS. Todas as Recordsets definido na sua zona são contadas. A unidade de medida é Contagem e a agregação é o número máximo de todos os Recordsets. Para ver esta métrica, selecione **métricas (pré-visualização)** experiência do **Monitor** separador no portal do Azure. Selecione a sua zona DNS do **recursos** pendente, selecione o **a contagem de definir o registo** métrica e, em seguida, selecione **máximo** como o **agregação** . Para obter mais informações sobre o Explorador de métricas experiência e charting, consulte [Explorador de métricas de Monitor de Azure](../monitoring-and-diagnostics/monitoring-metric-charts.md). 

![Contagem de conjunto de registos](./media/dns-alerts-metrics/dns-metrics-record-set-count.png)

*Figura: Métricas de contagem de conjunto de registos de DNS do Azure*


### <a name="record-set-capacity-utilization"></a>Utilização de capacidade do conjunto de registos
O *utilização da capacidade de definir o registo* métrica no DNS do Azure mostra a percentagem de utilização da sua capacidade de Recordset para uma zona DNS. Cada zona DNS no DNS do Azure está sujeita a um limite de Recordset que define o número máximo de Recordsets que são permitidos para a zona (consulte [limites DNS](dns-zones-records.md#limits)). Por conseguinte, esta métrica mostra-lhe como fechar que está a atingir o limite de Recordset. Por exemplo, se tiver 500 Recordsets configurados para a sua zona DNS e a zona tem o limite de Recordset predefinido de 5000, a métrica RecordSetCapacityUtilization irá mostrar o valor de 10% (o que é obtido por 500 divisória por 5000). A unidade de medida é **percentagem** e **agregação** é do tipo **máximo**. Para ver esta métrica, selecione a experiência do Explorador de métricas (pré-visualização) no separador Monitor no portal do Azure. Selecione a zona DNS na lista pendente de recursos, selecione a métrica de utilização de capacidade de definir o registo e selecione Máx. como a agregação. Abaixo captura de ecrã mostra um exemplo. Para obter mais informações sobre o Explorador de métricas experiência e charting, consulte [Explorador de métricas de Monitor de Azure](../monitoring-and-diagnostics/monitoring-metric-charts.md). 

![Contagem de conjunto de registos](./media/dns-alerts-metrics/dns-metrics-record-set-capacity-uitlization.png)

*Figura: Métricas de Azure DNS registo definir a utilização da capacidade*

## <a name="alerts-in-azure-dns"></a>Alertas no DNS do Azure
Monitor do Azure fornece a capacidade de alerta com valores de métricas disponíveis. As métricas DNS estão disponíveis na nova experiência de configuração de alerta. Conforme descrito em detalhe no [documentação de alertas do Monitor de Azure](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md), pode selecionar a zona DNS como o recurso, escolha o tipo de sinal métrica e configurar a lógica de alerta e outros parâmetros, tais como **período**e **frequência**. Pode definir mais um [ação grupo](../monitoring-and-diagnostics/monitoring-action-groups.md) para quando a condição de alerta for cumprida, whereby o alerta será entregue através de ações que escolheu. Para obter mais informações sobre como configurar alertas de métricas de Monitor do Azure, consulte [criar, ver e gerir alertas utilizando o Monitor de Azure](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md). 

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [DNS do Azure](dns-overview.md).
