---
title: Compreender a forma como o trabalho de alertas de métrica no Azure Monitor.
description: Obtenha uma visão geral do que pode fazer com alertas de métricas e como elas funcionam no Azure Monitor.
author: snehithm
ms.author: snmuvva
ms.date: 9/18/2018
ms.topic: conceptual
ms.service: azure-monitor
ms.component: alerts
ms.openlocfilehash: 0ac19ec18c443f677260ba22b6bce1c06237fb2e
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816435"
---
# <a name="understand-how-metric-alerts-work-in-azure-monitor"></a>Compreender a forma como o trabalho de alertas de métrica no Azure Monitor

Alertas de métricas no Azure Monitor trabalham no topo de métricas multidimensionais. Estas métricas podem ser métricas de plataforma, [métricas personalizadas](metrics-custom-overview.md), [populares registos do Log Analytics convertido em métricas](monitoring-metric-alerts-logs.md), métricas padrão do Application Insights. Alertas de métricas avaliar em intervalos regulares para verificar se a condições de métrica de um ou mais séries de tempo são verdadeiros e notificá-lo quando são cumpridas das avaliações. Alertas de métricas com monitoração de estado, ou seja, eles apenas enviam notificações quando o estado é alterado.

## <a name="how-do-metric-alerts-work"></a>Como funcionam os alertas de métricas

Pode definir uma regra de alerta de métrica, especificando um recurso de destino a ser monitorizado, métrica nome e a condição (um operador e um limite) e um grupo de ação para ser acionado quando a regra de alerta é acionado.
Digamos que tenha criado uma regra de alerta de métrica simple da seguinte forma:

- (O recurso do Azure que pretende monitorizar) do recurso de destino: myVM
- Métrica: Percentagem da CPU
- Agregação de tempo (estatística que é executada sobre os valores de métrica não processados. Suporte de tempo as agregações são Min, Max, Avg, Total): médio
- Período (a vista de olhos back-janela ao longo do que métrica valores são verificados): durante os últimos 5 minutos
- Frequência (a frequência com que o alerta de métrica verifica se as condições são cumpridas): 1 min
- Operador: Maior do que
- Limiar: 70

Desde o momento em que a regra de alerta é criada, o monitor é executado a cada 1 minuto e procura valores de métrica para os últimos 5 minutos e verifica se a média desses valores excede 70. Se a condição for cumprida, ou seja, 70 de excede a percentagem de CPU média nos últimos 5 minutos, a regra de alerta é acionado uma notificação ativada. Se tiver configurado uma mensagem de e-mail ou uma ação de hook de web no grupo de ação associado a regra de alerta, receberá uma notificação ativada em ambos.

Esta instância específica do disparo de regra de alerta também pode ser visualizada no portal do Azure, no painel todos os alertas.

Por exemplo, a utilização no "myVM" continua a estar acima do limiar em verificações subsequentes, a regra de alerta não será acionado novamente até que a condição for resolvida.

Depois de algum tempo, se a utilização no "myVM" resume volta ao normal vai ou seja, inferior ao limiar especificado. A regra de alerta monitoriza a condição para mais duas vezes, para enviar uma notificação resolvido. A regra de alerta envia uma mensagem resolvido/desativada quando a condição do alerta não for cumprida para três pontos finais consecutivos reduzir o ruído em caso de em oscilação condições.

Como a notificação resolvida é enviada através de webhooks ou e-mail, o estado da instância do alerta (chamado de estado do Monitor) no portal do Azure é também definido como resolvido.

## <a name="monitoring-at-scale-using-metric-alerts-in-azure-monitor"></a>Monitorização em escala com alertas de métricas no Azure Monitor

### <a name="using-dimensions"></a>Usando dimensões

Alertas de métricas no Azure Monitor também suportam várias combinações de valor de dimensão com uma regra de monitorização. Vamos entender por que pode usar várias combinações de dimensão com a ajuda de um exemplo.

Suponhamos que tem um plano do serviço de aplicações para o seu site. Pretende monitorizar a utilização da CPU em várias instâncias a executar a sua aplicação/site da web. Pode fazer isso da seguinte forma a utilizar uma regra de alerta de métrica

- Recurso de destino: myAppServicePlan
- Métrica: Percentagem da CPU
- Dimensões
  - Instância = InstanceName1, InstanceName2
- Agregação de tempo: médio
- Período: durante os últimos 5 minutos
- Frequência: 1 min
- Operador: GreaterThan
- Limiar: 70

Como antes, esta regra monitoriza se a utilização média da CPU nos últimos 5 minutos exceder 70%. No entanto, com a mesma regra, pode monitorizar duas instâncias a executar o seu Web site. Cada instância irá obter monitorizada individualmente e recebe notificações individualmente.

Por exemplo, tem uma aplicação web que está a ver a pedido em massa e terá de adicionar mais instâncias. A regra acima ainda monitoriza apenas duas instâncias. No entanto, pode criar uma regra da seguinte forma.

- Recurso de destino: myAppServicePlan
- Métrica: Percentagem da CPU
- Dimensões
  - Instância = *
- Agregação de tempo: médio
- Período: durante os últimos 5 minutos
- Frequência: 1 min
- Operador: GreaterThan
- Limiar: 70

Esta regra irá monitorizar automaticamente todos os valores para a instância como Pode monitorizar instâncias de conforme eles forem aparecendo sem a necessidade de modificar a regra de alerta de métrica novamente.

### <a name="monitoring-multiple-resources-using-metric-alerts"></a>Monitoramento de vários recursos com alertas de métricas

Como já foi mencionado na secção anterior, é possível ter uma única regra de alerta métrica que monitoriza cada combinação de dimensão individuais (ou seja uma série de tempo de métrica). Anteriormente, mas continua a ser limitada para fazê-lo um recurso de cada vez. Monitor do Azure também suporta a monitorização de vários recursos com uma regra de alerta de métrica. Esta funcionalidade é atualmente suportadas apenas em máquinas de virtuais e de pré-visualização. Além disso, um alerta de métrica único pode monitorizar os recursos numa região do Azure.

Pode especificar o âmbito da monitorização, um alerta de métrica único de uma das três formas:

- como uma lista de máquinas virtuais numa única região do Azure dentro de uma subscrição
- todas as máquinas virtuais (numa região do Azure) num ou mais grupos de recursos numa subscrição
- todas as máquinas virtuais (numa região do Azure) numa subscrição

Criação de regras de alerta de métrica que monitorizam os vários recursos não é atualmente suportada através do portal do Azure. Pode criar estas regras através de [modelos Azure Resource Manager](monitoring-create-metric-alerts-with-templates.md#resource-manager-template-for-metric-alert-that-monitors-multiple-resources). Receberá notificações individuais para cada máquina virtual. 

## <a name="typical-latency"></a>Latência típica

Para alertas de métricas, normalmente, será notificado em menos de 5 minutos se definir a frequência de regra de alerta para ser 1 min. Em casos de sobrecarga para sistemas de notificação, poderá ver uma latência de mais tempo.

## <a name="supported-resource-types-for-metric-alerts"></a>Tipos de recurso suportados para alertas de métricas

Pode encontrar a lista completa dos tipos de recurso suportados desta [artigo](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported).

Se estiver a utilizam atualmente os alertas de métricas clássicas e estiver à procura para ver se os alertas de métricas suportam os todos os tipos de recurso que está a utilizar, a tabela seguinte mostra os recursos, tipos suportados através de alertas de métrica clássicas e se são suportados por alertas de métricas hoje ou não.

|Tipo de recurso suportado através de alertas de métrica clássicas | Suportado por alertas de métricas |
|-------------------------------------------------|----------------------------|
| Microsoft.ApiManagement/service | Sim |
| Microsoft.Batch/batchAccounts| Sim|
|Microsoft.Cache/redis| Sim
|Microsoft.ClassicCompute/virtualMachines | Não |
|Microsoft.ClassicCompute/domainNames/slots/roles | Não|
|Microsoft.CognitiveServices/accounts | Não |
|Microsoft.Compute/virtualMachines | Sim|
|Microsoft.Compute/virtualMachineScaleSets| Sim|
|Microsoft.ClassicStorage/storageAccounts| Não |
|Microsoft.DataFactory/datafactories | Sim|
|Microsoft.DBforMySQL/servers| Sim|
|Microsoft.DBforPostgreSQL/servers| Sim|
|Microsoft.Devices/IotHubs | Não|
|Microsoft.DocumentDB/databaseAccounts| Não|
|Microsoft.EventHub/namespaces | Sim|
|Microsoft.Logic/workflows | Sim|
|Microsoft.Network/loadBalancers |Sim|
|Microsoft.Network/publicIPAddresses| Sim|
|Microsoft.Network/applicationGateways| Sim|
|Microsoft.Network/expressRouteCircuits| Sim|
|Microsoft.Network/trafficManagerProfiles | Sim|
|Microsoft.Search/searchServices | Não|
|Microsoft.ServiceBus/namespaces| Não|
|Microsoft.Storage/storageAccounts | Sim|
|Microsoft.StreamAnalytics/streamingjobs| Sim|
|Microsoft.TimeSeriesInsights/environments | Sim|
|Microsoft. Web/serverfarms | Sim |
|Microsoft. / Sites da Web (excluindo as funções) | Sim|
|Microsoft. HostingEnvironments/Web/multiRolePools | Não|
|Microsoft. HostingEnvironments/Web/workerPools| Não
|Microsoft.SQL/Servers | Não|

## <a name="next-steps"></a>Passos Seguintes

- [Saiba como criar, ver e gerir alertas de métricas no Azure](alert-metric.md)
- [Saiba como implementar utilizando modelos Azure Resource Manager de alertas de métricas](monitoring-create-metric-alerts-with-templates.md)
- [Saiba mais sobre os grupos de ação](monitoring-action-groups.md)
