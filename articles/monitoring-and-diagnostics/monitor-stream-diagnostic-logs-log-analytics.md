---
title: Transmitir os registos de diagnóstico do Azure ao Log Analytics
description: Saiba como transmitir os registos de diagnóstico do Azure para uma área de trabalho de análise de registos.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/04/2018
ms.author: johnkem
ms.component: logs
ms.openlocfilehash: 634cecb247686afd8c5c749d6e28b301d7e07c4f
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263512"
---
# <a name="stream-azure-diagnostic-logs-to-log-analytics"></a>Transmitir os registos de diagnóstico do Azure ao Log Analytics

**[Os registos de diagnóstico do Azure](monitoring-overview-of-diagnostic-logs.md)**  pode transmissão em fluxo em tempo real para utilizar o portal, os cmdlets do PowerShell ou do Azure CLI 2.0 de Log Analytics do Azure.

## <a name="what-you-can-do-with-diagnostics-logs-in-log-analytics"></a>O que pode fazer com o diagnóstico de registos de análise de registos

Análise de registos do Azure é uma ferramenta de pesquisa e a análise de registo flexível que permite-lhe obter conhecimentos aprofundados sobre os dados em bruto de registo gerados a partir de recursos do Azure. Algumas funcionalidades incluem:

* **Pesquisa de registo** -escrita avançado consultas através de dados de registo, correlacionar os registos de várias origens e até mesmo geram gráficos podem afixar ao dashboard do Azure.
* **Alertas** -detetar quando um ou mais eventos correspondem a uma consulta específica e ficarem notificados com uma chamada de e-mail ou o webhook.
* **Soluções** -utilizar pré-criadas vistas e dashboards dão-lhe informações imediatas sobre os seus dados de registo.
* **Análise avançada** - aplicar aprendizagem e padrão algoritmos correspondentes para identificar possíveis problemas revelados pelos seus registos.

## <a name="enable-streaming-of-diagnostic-logs-to-log-analytics"></a>Ative a transmissão em fluxo de registos de diagnóstico ao Log Analytics

Pode ativar a transmissão em fluxo de registos de diagnóstico programaticamente, através do portal, ou utilizando o [as APIs REST da Azure Monitor](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings). Qualquer forma, pode cria uma definição de diagnóstico no que especificou uma área de trabalho de análise de registos e as categorias de registo e métricas que pretende enviar para essa área de trabalho. Um diagnóstico **categoria de registo** é um tipo de registo que pode fornecer um recurso.

A área de trabalho de análise de registos não tem de estar na mesma subscrição, como o recurso emitir os registos, desde que o utilizador que configura a definição possui acesso RBAC adequado para ambas as subscrições.

> [!NOTE]
> Atualmente, o envio de métricas multidimensionais através das definições de diagnóstico não é suportado. As métricas com dimensões são exportadas como métricas dimensionais simples e agregadas em valores de dimensões.
>
> *Por exemplo*: a métrica “Mensagens Recebidas” num hub do Hub de Eventos pode ser explorada e representada ao nível da linha. No entanto, se for exportada através das definições de diagnóstico, a métrica será representada como todas as mensagens recebidas em todas as filas do hub do Hub de Eventos.
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>Registos de diagnóstico de fluxo através do portal
1. No portal, navegue para o Monitor do Azure e clique em **definições de diagnóstico**

    ![Secção de monitorização do Monitor do Azure](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-blade.png)

2. Opcionalmente filtrar a lista por tipo de recurso ou grupo de recursos, em seguida, clique no recurso para o qual pretende configurar uma definição de diagnóstico.

3. Se não existem definições de existem no recurso que selecionou, são-lhe pedido para criar uma definição. Clique em "Ativar o diagnóstico".

   ![Adicionar definição de diagnóstico - sem definições existentes](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-none.png)

   Se existirem definições existentes no recurso, irá ver uma lista das definições já configurada neste recurso. Clique em "Adicionar definição de diagnóstico".

   ![Adicionar definição de diagnóstico - existente definições](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-multiple.png)

3. Dê a definição de um nome e a caixa de verificação **enviar ao Log Analytics**, em seguida, selecione uma área de trabalho de análise de registos.

   ![Adicionar definição de diagnóstico - existente definições](media/monitoring-stream-diagnostic-logs-to-log-analytics/diagnostic-settings-configure.png)

4. Clique em **Guardar**.

Após alguns instantes, a nova definição é apresentada na sua lista de definições para este recurso, e os registos de diagnóstico são transmissão em fluxo para essa área de trabalho, assim como novos dados de evento são gerados. Tenha em atenção que podem ser quinze minutos entre quando é emitido um evento e quando for apresentada na análise de registos.

### <a name="via-powershell-cmdlets"></a>Através de Cmdlets do PowerShell
Para ativar a transmissão em fluxo através de [Cmdlets do PowerShell do Azure](insights-powershell-samples.md), pode utilizar o `Set-AzureRmDiagnosticSetting` cmdlet com os seguintes parâmetros:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource ID] -WorkspaceID [resource ID of the Log Analytics workspace] -Categories [list of log categories] -Enabled $true
```

Tenha em atenção que a propriedade workspaceID demora o ID de recurso do Azure completo da área de trabalho, não a área de trabalho ID/chave apresentada no portal da análise de registos.

### <a name="via-azure-cli-20"></a>Através da CLI do Azure 2.0

Para ativar a transmissão em fluxo através de [Azure CLI 2.0](insights-cli-samples.md), pode utilizar o [criar definições de diagnóstico do monitor az](/cli/azure/monitor/diagnostic-settings#az-monitor-diagnostic-settings-create) comando.

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --workspace <log analytics name or object ID> \
    --resource <target resource object ID> \
    --resource-group <log analytics workspace resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

Pode adicionar categorias adicionais no registo de diagnóstico adicionando dicionários para a matriz JSON transmitida como o `--logs` parâmetro.

O `--resource-group` argumento só é necessário se `--workspace` não é um ID de objeto.

## <a name="how-do-i-query-the-data-in-log-analytics"></a>Como posso consultar os dados na análise de registos?

No painel pesquisa de registo no portal ou a experiência de análise avançadas como parte da análise de registos, pode consultar os registos de diagnóstico como parte da solução de gestão do registo em tabela AzureDiagnostics. Também existem [várias soluções para os recursos do Azure](../log-analytics/log-analytics-add-solutions.md) pode instalar para obter informações imediatas sobre os dados de registo está a enviar para análise de registos.

## <a name="next-steps"></a>Passos Seguintes

* [Leia mais sobre os registos de diagnóstico do Azure](monitoring-overview-of-diagnostic-logs.md)
