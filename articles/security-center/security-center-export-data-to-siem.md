---
title: Dados de segurança do Azure exportar para SIEM – Pipeline de configuração | Documentos da Microsoft
description: Este artigo documenta a produzir de obter registos de center para um SIEM de segurança do Azure
services: security-center
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/26/2018
ms.author: barclayn
ms.openlocfilehash: 4cc9478197b39198a94ace1cbefab81de3cdb32b
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/26/2018
ms.locfileid: "52306731"
---
# <a name="azure-security-data-export-to-siem-pipeline-configuration"></a>Exportação de dados de segurança do Azure à configuração do Pipeline de SIEM

Este documento detalha o procedimento para exportar dados de segurança do Centro de segurança do Azure para um SIEM.

Os eventos processados produzidos pelo centro de segurança do Azure são publicados para o Azure [registo de atividades](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), um registo de tipos disponíveis através do Azure Monitor. Monitor do Azure oferece um pipeline consolidado para o encaminhamento qualquer um dos seus dados de monitorização para uma ferramenta SIEM. Isso é feito por transmissão em fluxo de dados para um Hub de eventos em que ele, em seguida, pode ser extraído para uma ferramenta de parceiro.

Este pipe utiliza a [monitorização do Azure único pipeline](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) para obter acesso aos dados de monitorização do seu ambiente do Azure. Isto permite-lhe configurar facilmente SIEMs e ferramentas de monitorização para consumir os dados.

As secções seguintes descrevem como configurar dados sejam transmitidos para um hub de eventos. Os passos partem do princípio de que já tenha configurado na sua subscrição do Azure no Centro de segurança do Azure.

Descrição geral de alto nível

![Descrição geral de alto nível](media/security-center-export-data-to-siem/overview.png)

## <a name="what-is-the-azure-security-data-exposed-to-siem"></a>O que é expostos para o SIEM os dados de segurança do Azure?

Nesta versão expomos a [alertas de segurança.](../security-center/security-center-managing-and-responding-alerts.md) Em versões futuras, podemos irá melhorar o conjunto de dados com recomendações de segurança.

## <a name="how-to-setup-the-pipeline"></a>Como configurar o pipeline

### <a name="create-an-event-hub"></a>Criar um Hub de Eventos

Antes de começar, precisa [criar um espaço de nomes de Hubs de eventos](../event-hubs/event-hubs-create.md). Este espaço de nomes e o Hub de eventos é o destino para todos os seus dados de monitorização.

### <a name="stream-the-azure-activity-log-to-event-hubs"></a>O registo de atividades do Azure para os Hubs de eventos do Stream

Veja o artigo seguinte [registo de atividades de fluxo para os Hubs de eventos](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md)

### <a name="install-a-partner-siem-connector"></a>Instalar um conector SIEM parceiro 

Encaminhamento seus dados de monitorização para um Hub de eventos com o Azure Monitor permite-lhe integrar facilmente com parceiros SIEM e ferramentas de monitorização.

Veja a seguinte ligação para ver a lista de [suportado SIEMs](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub)

## <a name="example-for-querying-data"></a>Exemplo para consultar dados 

Eis algumas consultas de Splunk que pode utilizar para extrair dados de alertas:

| **Descrição da consulta**                                | **Consulta**                                                                                                                              |
|---------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Todos os Alertas                                              | índice = Microsoft.Security/locations/alerts principal                                                                                         |
| Resumir contagem das operações pelo respetivo nome             | índice = sourcetype principal = "amal: segurança" \| operationName tabela \| estatísticas contagem por operationName                                |
| Obter as informações de alertas: tempo, o nome, o estado, o ID e o subscrição | índice = principal Microsoft.Security/locations/alerts \| tabela \_tempo, properties.eventName, estado, properties.operationId, am_subscriptionId |


## <a name="next-steps"></a>Passos Seguintes

- [SIEMs suportados](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md#what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub)
- [Transmitir em fluxo o registo de atividades para o Hubs de Eventos](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md)
- [Alertas de segurança.](../security-center/security-center-managing-and-responding-alerts.md)