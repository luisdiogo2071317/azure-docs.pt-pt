---
title: "Transmitir dados para os Hubs de eventos de monitorização do Azure | Microsoft Docs"
description: "Saiba como fluxo todos os seus dados de monitorização do Azure para um hub de eventos para obter os dados para um parceiro SIEM ou a ferramenta de análise."
author: johnkemnetz
manager: robb
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/11/2018
ms.author: johnkem
ms.openlocfilehash: b2813035b4665a36b475e791965d395b84ddb3f1
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="stream-azure-monitoring-data-to-an-event-hub-for-consumption-by-an-external-tool"></a>Azure de fluxo monitorização dados para um hub de eventos para consumo por uma ferramenta externa

Monitor do Azure fornece um único pipeline para obter acesso a todos os dados de monitorização do seu ambiente do Azure, permitindo-lhe configurar facilmente parceiro SIEM e ferramentas para consumir dados de monitorização. Este artigo explica como configurar diferentes camadas de dados do seu ambiente do Azure para serem enviados para um Event Hubs espaço de nomes ou evento de concentrador único, onde podem ser recolhido por uma ferramenta externa.

## <a name="what-data-can-i-send-into-an-event-hub"></a>Os dados que pode enviar para um hub de eventos 

No seu ambiente do Azure, existem vários 'escalões' dos dados de monitorização e o método de aceder aos dados a partir de cada camada varia ligeiramente. Normalmente, estas camadas podem ser descritas como:

- **Dados de monitorização da aplicação:** dados sobre o desempenho e a funcionalidade do código de escrever e em execução no Azure. Exemplos de dados de monitorização de aplicações incluem rastreios de desempenho, os registos de aplicações e telemetria de utilizador. Dados de monitorização de aplicações, normalmente, são recolhidas de uma das seguintes formas:
  - Por instrumentação, tais como o seu código com um SDK a [Application Insights SDK](../application-insights/app-insights-overview.md).
  - Através da execução de um agente de monitorização que escuta para a nova aplicação de registo na máquina de execução da sua aplicação, tais como o [o agente de diagnóstico do Windows Azure](./azure-diagnostics.md) ou [agente de diagnóstico do Azure de Linux](../virtual-machines/linux/diagnostic-extension.md).
- **SO convidado dados de monitorização:** dados sobre o sistema operativo no qual a aplicação está em execução. Exemplos de dados de monitorização de SO convidado seria Linux syslog ou de eventos do sistema Windows. Para recolher este tipo de dados, terá de instalar um agente, tais como o [o agente de diagnóstico do Windows Azure](./azure-diagnostics.md) ou [agente de diagnóstico do Azure de Linux](../virtual-machines/linux/diagnostic-extension.md).
- **Dados de monitorização de recursos do Azure:** dados sobre o funcionamento de um recurso do Azure. Para alguns tipos de recursos do Azure, como as máquinas virtuais, há um SO convidado e a aplicação (ões) para monitorizar dentro de que o serviço do Azure. Para outros recursos do Azure, tais como grupos de segurança de rede, o recurso de dados de monitorização é a camada de dados disponíveis mais alta (porque não existe nenhum SO convidado ou aplicação em execução nesses recursos). Estes dados podem ser recolhidos utilizando [definições de diagnóstico de recurso](./monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings).
- **Dados de monitorização de plataforma do Azure:** dados sobre a operação e a gestão de uma subscrição do Azure ou o inquilino, bem como os dados sobre o estado de funcionamento e a operação do Azure em si. O [registo de atividade](./monitoring-overview-activity-logs.md), incluindo dados de estado de funcionamento do serviço e o Active Directory, as auditorias são exemplos de plataforma de dados de monitorização. Estes dados podem ser recolhidos através de definições de diagnóstico, bem como.

Dados a partir de qualquer camada podem ser enviados para um hub de eventos, onde podem ser solicitado para uma ferramenta de parceiro. As secções seguintes descrevem como configurar dados de cada escalão de transmissão em fluxo para um hub de eventos. Os passos partem do princípio de que já tem recursos na camada para ser monitorizada.

Antes de começar, terá de [criar um hub de espaço de nomes e eventos de Event Hubs](../event-hubs/event-hubs-create.md). Este hub de espaço de nomes e o evento é o destino para todos os seus dados de monitorização.

## <a name="how-do-i-set-up-azure-platform-monitoring-data-to-be-streamed-to-an-event-hub"></a>Como posso configurar dados de monitorização de plataforma do Azure para transmissão em fluxo para um hub de eventos?

Dados de monitorização de plataforma do Azure é proveniente de duas origens principais:
1. O [registo de atividade do Azure](./monitoring-overview-activity-logs.md), que contém a criar, atualizar e eliminar operações de Gestor de recursos, as alterações no [estado de funcionamento do serviço do Azure](../service-health/service-health-overview.md) que possam afetar os recursos na sua subscrição, o [estado de funcionamento do recurso](../service-health/resource-health-overview.md) transições de estado e vários outros tipos de eventos ao nível da subscrição. [Este artigo fornece detalhes sobre todas as categorias de eventos que são apresentados no registo de atividade do Azure](./monitoring-activity-log-schema.md).
2. [Relatórios do Azure Active Directory](../active-directory/active-directory-reporting-azure-portal.md), que contém o histórico de início de sessão auditoria e atividade de registo das alterações efetuadas dentro de um determinado inquilino. -Não é possível aos dados do Azure Active Directory de fluxo para um hub de eventos.

### <a name="stream-azure-activity-log-data-into-an-event-hub"></a>Dados de registo de atividade do Azure de fluxo para um hub de eventos

Para enviar dados a partir do registo de atividade do Azure para um espaço de nomes de Event Hubs, iremos configurar um perfil de registo na sua subscrição. [Siga este guia](./monitoring-stream-activity-logs-event-hubs.md) para configurar um perfil de registo na sua subscrição. Opte por fazê-lo Depois por subscrição que pretende monitorizar.

> [!TIP]
> Um perfil de registo atualmente só permite-lhe selecionar um espaço de nomes de Event Hubs, no qual um hub de eventos é criado com os nome 'insights--registos operacionais.' -Não é possível especificar o seu próprio nome de hub de eventos num perfil de registo.

## <a name="how-do-i-set-up-azure-resource-monitoring-data-to-be-streamed-to-an-event-hub"></a>Como posso configurar dados de monitorização de recursos do Azure para transmissão em fluxo para um hub de eventos?

Recursos do Azure emitir dois tipos de dados de monitorização:
1. [Registos de diagnóstico de recursos](./monitoring-overview-of-diagnostic-logs.md)
2. [Métricas](monitoring-overview-metrics.md)

Ambos os tipos de dados são enviados para um hub de eventos a utilizar uma definição de diagnóstico do recurso. [Siga este guia](./monitoring-stream-diagnostic-logs-to-event-hubs.md) para configurar uma definição de diagnóstico de recursos num recurso específico. Configure uma definição de diagnóstico de recurso em cada recurso a partir do qual pretende recolher registos.

> [!TIP]
> Pode utilizar a política do Azure para se certificar de que cada recurso dentro de um determinado âmbito sempre está configurado com uma definição de diagnóstico [utilizando o efeito de DeployIfNotExists na regra de política](../azure-policy/policy-definition.md#policy-rule). Hoje em dia DeployIfNotExists só é suportada em políticas incorporadas.

## <a name="how-do-i-set-up-guest-os-monitoring-data-to-be-streamed-to-an-event-hub"></a>Como posso configurar dados de monitorização de SO convidado para transmissão em fluxo para um hub de eventos?

Terá de instalar um agente para enviar dados de monitorização de SO de convidado para um hub de eventos. Para Windows ou Linux, especifique os dados que pretende que sejam enviados para o hub de eventos, bem como o hub de eventos para o qual os dados devem ser enviados num ficheiro de configuração e passaram se o ficheiro de configuração para o agente em execução na VM.

### <a name="stream-linux-data-to-an-event-hub"></a>Transmitir dados de Linux para um hub de eventos

O [agente Linux do Azure diagnóstico](../virtual-machines/linux/diagnostic-extension.md) pode ser utilizado para enviar dados a partir de uma máquina de Linux para um hub de eventos de monitorização. Fazê-lo ao adicionar o hub de eventos como um sink no seu LAD definições de ficheiros protegidos de configuração JSON. [Consulte este artigo para obter mais informações sobre como adicionar o sink de hub de eventos para o agente Linux do Azure diagnóstico](../virtual-machines/linux/diagnostic-extension.md#protected-settings).

> [!NOTE]
> Não é possível configurar a transmissão em fluxo de dados de monitorização de SO de convidado para um hub de eventos no portal. Em vez disso, tem de editar manualmente o ficheiro de configuração.

### <a name="stream-windows-data-to-an-event-hub"></a>Dados de fluxo do Windows para um hub de eventos

O [agente de diagnóstico do Windows Azure](./azure-diagnostics.md) pode ser utilizado para enviar dados a partir de uma máquina Windows para um hub de eventos de monitorização. Fazê-lo ao adicionar o hub de eventos como um sink a secção de privateConfig o WAD do ficheiro de configuração. [Consulte este artigo para obter mais informações sobre como adicionar o sink de hub de eventos para o agente de diagnóstico do Windows Azure](./azure-diagnostics-streaming-event-hubs.md).

> [!NOTE]
> Não é possível configurar a transmissão em fluxo de dados de monitorização de SO de convidado para um hub de eventos no portal. Em vez disso, tem de editar manualmente o ficheiro de configuração.

## <a name="how-do-i-set-up-application-monitoring-data-to-be-streamed-to-event-hub"></a>Como configurar aplicação dados de monitorização para transmissão em fluxo para o hub de eventos

Dados de monitorização de aplicações requer que o código é equipado com um SDK, pelo que não é uma solução para fins gerais para dados para um hub de eventos no Azure de monitorização de aplicações de encaminhamento. No entanto, [Azure Application Insights](../application-insights/app-insights-overview.md) é um serviço que pode ser utilizado para recolher dados de nível de aplicação do Azure. Se estiver a utilizar o Application Insights, pode transmitir dados de monitorização para um hub de eventos da seguinte forma:

1. [Configurar a exportação contínua](../application-insights/app-insights-export-telemetry.md) dos dados do Application Insights para uma conta de armazenamento.

2. Configurar uma aplicação de lógica de acionada por temporizador que [obtém dados a partir do blob storage](../connectors/connectors-create-api-azureblobstorage.md#use-an-action) e [pushes-la como uma mensagem para o hub de eventos](../connectors/connectors-create-api-azure-event-hubs.md#send-events-to-your-event-hub-from-your-logic-app).

## <a name="what-can-i-do-with-the-monitoring-data-being-sent-to-my-event-hub"></a>O que posso fazer com os dados de monitorização a ser enviados para os meus hub de eventos?

Encaminhamento os dados de monitorização para um hub de eventos com a monitorização do Azure permite-lhe integrar facilmente com parceiros SIEM e ferramentas de monitorização. A maioria das ferramentas requerem a cadeia de ligação do hub de eventos e determinadas permissões à sua subscrição do Azure para ler dados do hub de eventos. Eis uma lista não exaustiva das ferramentas com a integração do Monitor do Azure:

* **IBM QRadar** -o DSM de Azure da Microsoft e o protocolo de Hub de eventos do Microsoft Azure estão disponíveis para transferência a partir do [do site de suporte do IBM](http://www.ibm.com/support). Pode [saber mais sobre a integração com o Azure aqui](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0).
* **Splunk** - [o suplemento de Monitor do Azure para Splunk](https://splunkbase.splunk.com/app/3534/) está disponível no Splunkbase e um projeto de código aberto. [Documentação está aqui](https://github.com/Microsoft/AzureMonitorAddonForSplunk/wiki/Azure-Monitor-Addon-For-Splunk).
* **SumoLogic** -as instruções para configurar SumoLogic consumir dados a partir de um hub de eventos são [disponível aqui](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub)

## <a name="next-steps"></a>Próximos Passos
* [O registo de atividade para uma conta de armazenamento de arquivo](monitoring-archive-activity-log.md)
* [Leia a descrição geral do registo de atividade do Azure](monitoring-overview-activity-logs.md)
* [Configurar um alerta com base em eventos de um registo de atividade](insights-auditlog-to-webhook-email.md)

