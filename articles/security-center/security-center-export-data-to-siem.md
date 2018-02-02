---
title: "Exportar dados de segurança do Azure para o SIEM - configuração do Pipeline [pré-visualização] | Microsoft Docs"
description: "Este artigo documentos produzir de obter os registos do System center para um SIEM de segurança do Azure"
services: security-center
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2018
ms.author: barclayn
ms.openlocfilehash: aef623f047bd7e14cb5bd17fb2a2c18e3c5d42b9
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="azure-security-data-export-to-siem--pipeline-configuration-preview"></a>Exportação de dados de segurança do Azure para o SIEM - configuração do Pipeline [pré-visualização]

Este documento fornece detalhes sobre o procedimento para exportar dados de segurança do Centro de segurança do Azure para um SIEM.

Eventos processados produzidos pelo centro de segurança do Azure são publicados para o Azure [registo de atividade](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), um registo de tipos disponíveis por meio do Monitor do Azure. Monitor do Azure oferece um pipeline consolidado para o encaminhamento qualquer um dos seus dados de monitorização para uma ferramenta SIEM. Isto é feito por transmissão em fluxo de dados para um Hub de eventos em que, em seguida, ser solicitado para uma ferramenta de parceiro.

Este pipe utiliza o [monitorização do Azure único pipeline](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) para obter acesso aos dados de monitorização do seu ambiente do Azure. Isto permite-lhe configurar facilmente SIEMs e ferramentas de monitorização para consumir dados.

As secções seguintes descrevem como configurar dados de transmissão em fluxo para um hub de eventos. Os passos partem do princípio de que já tenha configurado na sua subscrição do Azure do Centro de segurança do Azure.

Descrição geral de alto nível

![Descrição geral de alto nível](media/security-center-export-data-to-siem/overview.png)

## <a name="what-is-the-azure-security-data-exposed-to-siem"></a>O que é expostos à SIEM os dados de segurança do Azure?

Nesta versão de pré-visualização expomos o [alertas de segurança.](../security-center/security-center-managing-and-responding-alerts.md) Em versões futuras, iremos irá enriqueça o conjunto de dados com recomendações de segurança.

## <a name="how-to-setup-the-pipeline"></a>Como configurar o pipeline? 

### <a name="create-an-event-hub"></a>Criar um Hub de Eventos 

Antes de começar, terá de [criar um espaço de nomes de Event Hubs](../event-hubs/event-hubs-create.md). Este espaço de nomes e o Hub de eventos é o destino para todos os dados de monitorização.

### <a name="stream-the-azure-activity-log-to-event-hubs"></a>Fluxo de registo de atividade do Azure para os Event Hubs

Consulte o artigo seguinte [registo de atividade de fluxo para os Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md)

### <a name="install-a-partner-siem-connector"></a>Instalar um conector do parceiro SIEM 

Encaminhamento os dados de monitorização para um Hub de eventos com a monitorização do Azure permite-lhe integrar facilmente com parceiros SIEM e ferramentas de monitorização.

Consulte a seguinte ligação para ver a lista de [suportado SIEMs](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub)

## <a name="example-for-querying-data"></a>Exemplo para consultar os dados 

Eis algumas consultas de Splunk que pode utilizar para solicitar dados de alerta:

| **Descrição da consulta**                                | **Consulta**                                                                                                                              |
|---------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Todos os Alertas                                              | o índice = Microsoft.Security/locations/alerts principal                                                                                         |
| Resumir contagem das operações pelo respetivo nome             | **Alertas** índice = sourcetype principal = "amal: segurança" \| tabela operationName \| estatísticas contagem por operationName                                |
| Obter as informações de alertas: tempo, o nome, o estado, o ID e o subscrição | o índice = Microsoft.Security/locations/alerts principal \| tabela \_tempo, properties.eventName, estado, properties.operationId, am_subscriptionId |


## <a name="next-steps"></a>Passos Seguintes

- [SIEMs suportados](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub)
- [Transmitir em fluxo o registo de atividades para o Hubs de Eventos](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md)
- [Alertas de segurança.](../security-center/security-center-managing-and-responding-alerts.md)