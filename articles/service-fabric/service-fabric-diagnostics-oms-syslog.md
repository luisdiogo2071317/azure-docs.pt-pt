---
title: Monitorizar eventos de cluster do Linux no Azure Service Fabric | Documentos da Microsoft
description: Saiba como monitorizar eventos de cluster do Linux do Syslog
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/23/2018
ms.author: srrengar
ms.openlocfilehash: 5989ec9f95e2f5129a128d19326833718f37b508
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2018
ms.locfileid: "52730355"
---
# <a name="service-fabric-linux-cluster-events-in-syslog"></a>Eventos de cluster do Service Fabric do Linux do Syslog

Service Fabric expõe um conjunto de eventos de plataforma para o informar de atividade importante no seu cluster. A lista completa dos eventos que são expostos está disponível [aqui](service-fabric-diagnostics-event-generation-operational.md). Existem várias formas através das quais estes eventos podem ser consumidos. Neste artigo, vamos falar sobre como configurar o Service Fabric para gravar esses eventos Syslog.

## <a name="introduction"></a>Introdução

Na versão 6.4, foi introduzido o SyslogConsumer para enviar os eventos de plataforma do Service Fabric para Syslog para clusters do Linux. Depois de ativar, irão fluir automaticamente syslog que podem ser recolhidos e enviados pelo agente de análise do registo de eventos.

Cada evento do Syslog tem 4 componentes
* Recurso
* Identidade
* Mensagem
* Gravidade

O SyslogConsumer grava todos os eventos de plataforma usando recurso `Local0`. Pode atualizar para qualquer recurso válido ao alterar a configuração de config. A identidade utilizada é `ServicFabric`. O campo de mensagem contém o evento todo serializado na JSON, para que possa ser consultado e consumido por uma variedade de ferramentas. 

## <a name="enable-syslogconsumer"></a>Ativar SyslogConsumer

Para ativar o SyslogConsumer, terá de efetuar uma atualização do cluster. O `fabricSettings` secção tem de ser atualizado com o código a seguir. Tenha em atenção de que esse código inclui apenas as secções relacionadas com o SyslogConsumer

```json
    "fabricSettings": [
        {
            "name": "Diagnostics",
            "parameters": [
            {
                "name": "ConsumerInstances",
                "value": "AzureWinFabCsv, AzureWinFabCrashDump, AzureTableWinFabEtwQueryable, SyslogConsumer"
            }
            ]
        },
        {
            "name": "SyslogConsumer",
            "parameters": [
            {
                "name": "ProducerInstance",
                "value": "WinFabLttProducer"
            },
            {
            "name": "ConsumerType",
            "value": "SyslogConsumer"
            },
            {
                "name": "IsEnabled",
                "value": "true"
            }
            ]
        },
        {
            "name": "Common",
            "parameters": [
            {
                "name": "LinuxStructuredTracesEnabled",
                "value": "true"
            }
            ]
        }
    ],
```

Seguem-se as alterações para chamar
1. Na secção comum, é um novo parâmetro chamado `LinuxStructuredTracesEnabled`. **Isto é necessário ter eventos de Linux estruturado e serializado quando enviados para o Syslog.**
2. Na secção de diagnóstico, um novo ConsumerInstance: SyslogConsumer foi adicionado. Isso informa a plataforma que existe outra consumidor dos eventos. 
3. A nova secção, tem de ter SyslogConsumer `IsEnabled` como `true`. É configurada para usar o recurso de Local0 automaticamente. É possível substituir isso adicionando outro parâmetro.

```json
    {
        "name": "New LogFacility",
        "value": "<Valid Syslog Facility>"
    }
```

## <a name="log-analytics-integration"></a>Integração de análise de registos
Pode ler esses eventos do Syslog numa ferramenta de monitoramento, como o Log Analytics. Pode criar uma área de trabalho do Log Analytics ao utilizar o Azure Marketplace a utilizar estas [instruções]. (.. / log-analytics/log-analytics-quick-create-workspace.md) também tem de adicionar o agente Log Analytics ao seu cluster para recolher e enviar estes dados para a área de trabalho. Este é o mesmo agente utilizado para recolher contadores de desempenho. 

1. Navegue para o `Advanced Settings` painel

    ![Definições de área de trabalho](media/service-fabric-diagnostics-oms-syslog/workspace-settings.png)

2. Clique `Data`
3. Clique `Syslog`
4. Configure Local0 como o recurso de controle. Pode adicionar outro recurso se alterado no fabricSettings

    ![Configurar o Syslog](media/service-fabric-diagnostics-oms-syslog/syslog-configure.png)
5. Aceda ao Explorador de consultas clicando `Logs` no menu do recurso de área de trabalho para começar a consultar

    ![Registos de área de trabalho](media/service-fabric-diagnostics-oms-syslog/workspace-logs.png)
6. Pode consultar em relação a `Syslog` tabela à procura de `ServiceFabric` como o ProcessName. A consulta abaixo é um exemplo de como analisar o JSON no evento e exibir o conteúdo

```kusto
    Syslog | where ProcessName == "ServiceFabric" | extend $payload = parse_json(SyslogMessage) | project $payload
```

![Consulta de syslog](media/service-fabric-diagnostics-oms-syslog/syslog-query.png)

O exemplo acima é de um evento de NodeDown. Pode ver a lista completa de eventos [aqui](service-fabric-diagnostics-event-generation-operational.md).

## <a name="next-steps"></a>Passos Seguintes
* [Implementar o agente do Log Analytics](service-fabric-diagnostics-oms-agent.md) para os nós para recolher contadores de desempenho e recolher estatísticas de docker e registos para os seus contentores
* Familiarizar-se com o [pesquisas e consultas de registo](../log-analytics/log-analytics-log-searches.md) funcionalidades oferecidos como parte do Log Analytics
* [Utilizar o estruturador de vistas para criar vistas personalizadas no Log Analytics](../log-analytics/log-analytics-view-designer.md)
* Referência para saber como [integração do Log Analytics com o Syslog](../log-analytics/log-analytics-data-sources-syslog.md).
