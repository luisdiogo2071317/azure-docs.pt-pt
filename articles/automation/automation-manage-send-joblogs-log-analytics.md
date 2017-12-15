---
title: "Reencaminhar dados de tarefa de automatização do Azure para a análise de registos do OMS | Microsoft Docs"
description: "Este artigo demonstra como enviar estado da tarefa e runbook fluxos de trabalho para a gestão e de análise de registos do Microsoft Operations Management Suite para fornecer informações adicionais."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: c12724c6-01a9-4b55-80ae-d8b7b99bd436
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/31/2017
ms.author: magoedte
ms.openlocfilehash: 0319a7b9248dec9d7cdabba9c18a25463d94284b
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2017
---
# <a name="forward-job-status-and-job-streams-from-automation-to-log-analytics-oms"></a>Reencaminhar estado da tarefa e fluxos de trabalho da automatização para análise de registos (OMS)
Automatização pode enviar as runbook fluxos de trabalho e o estado da tarefa para a sua área de trabalho de análise de registos do Microsoft Operations Management Suite (OMS). Os registos de tarefa e fluxos de trabalho são visíveis no portal do Azure, ou com o PowerShell, para tarefas individuais e Isto permite-lhe efetuar as investigações simples. Agora com a análise de registos, pode:

* Obter conhecimentos aprofundados sobre as tarefas de automatização.
* Acionamento um e-mail ou o alerta com base no seu estado de tarefa de runbook (por exemplo, falha ou suspenso).
* Escreva consultas avançadas nos seus fluxos de trabalho.
* Correlacionar tarefas em contas de automatização.
* Visualize o histórico do trabalho ao longo do tempo.

## <a name="prerequisites-and-deployment-considerations"></a>Pré-requisitos e considerações de implementação
Para começar a enviar os registos de automatização à análise de registos, é necessário:

* Versão de Novembro de 2016 ou posterior do [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) (v2.3.0).
* Uma área de trabalho de análise de registos. Para obter mais informações, consulte [introdução à análise de registos](../log-analytics/log-analytics-get-started.md). 
* O ResourceId para a sua conta de automatização do Azure.


Para localizar o ResourceId para a sua conta de automatização do Azure:

```powershell-interactive
# Find the ResourceId for the Automation Account
Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"
```

Encontrar o ResourceId para a sua área de trabalho de análise de registos, execute o PowerShell seguinte:

```powershell-interactive
# Find the ResourceId for the Log Analytics workspace
Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
```

Se tiver mais do que um contas de automatização ou áreas de trabalho, na saída dos comandos anteriores, localizar o *nome* tem de configurar e copie o valor para *ResourceId*.

Se precisar de localizar o *nome* da sua conta de automatização no portal do Azure, selecione a conta de automatização do **conta de automatização** painel e selecione **todas as definições** . A partir do painel **Todas as definições**, em **Definições da Conta** selecione **Propriedades**.  No painel **Propriedades**, pode ter em atenção estes valores.<br> ![Propriedades da conta de automatização](media/automation-manage-send-joblogs-log-analytics/automation-account-properties.png).

## <a name="set-up-integration-with-log-analytics"></a>Configurar a integração com a análise de registos

1. No seu computador, inicie **do Windows PowerShell** do **iniciar** ecrã.
2. Executar o PowerShell seguinte e edite o valor para o `[your resource id]` e `[resource id of the log analytics workspace]` com os valores do passo anterior.

   ```powershell-interactive
   $workspaceId = "[resource id of the log analytics workspace]"
   $automationAccountId = "[resource id of your automation account]"

   Set-AzureRmDiagnosticSetting -ResourceId $automationAccountId -WorkspaceId $workspaceId -Enabled $true
   ```

Depois de executar este script, verá registos de análise de registos dentro de 10 minutos de novos JobLogs ou JobStreams que está a ser escrito.

Para ver os registos, execute a consulta seguinte na pesquisa de registos de análise de registos:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION""`

### <a name="verify-configuration"></a>Verifique a configuração
Para confirmar a sua conta de automatização está a enviar os registos para a sua área de trabalho de análise de registos, verifique se o diagnóstico está corretamente configurado na conta de automatização utilizando o PowerShell seguinte:

```powershell-interactive
Get-AzureRmDiagnosticSetting -ResourceId $automationAccountId
```

Na saída Certifique-se de que:
+ Em *registos*, o valor para *ativado* é *verdadeiro*.
+ O valor de *WorkspaceId* está definido como ResourceId da sua área de trabalho de análise de registos.

## <a name="log-analytics-records"></a>Registos do Log Analytics

Cria dois tipos de registos de análise de registos de diagnóstico da automatização do Azure e é etiquetado como **AzureDiagnostics**. As seguintes consultas de utilizam o idioma de consulta atualizado à análise de registos. Para obter informações sobre consultas comuns entre a linguagem de consulta legado e o novo idioma de consulta de análise de registos do Azure, visite [legada para a nova folha de referência de linguagem de consulta de análise de registos do Azure](https://docs.loganalytics.io/docs/Learn/References/Legacy-to-new-to-Azure-Log-Analytics-Language)

### <a name="job-logs"></a>Registos da tarefa
| Propriedade | Descrição |
| --- | --- |
| TimeGenerated |Data e hora da execução do trabalho do runbook. |
| RunbookName_s |O nome do runbook. |
| Caller_s |Quem iniciou a operação. Os valores possíveis são um endereço de e-mail ou o sistema para trabalhos agendados. |
| Tenant_g | GUID que identifica o inquilino para o chamador. |
| JobId_g |GUID que é o Id do trabalho do runbook. |
| ResultType |O estado do trabalho do runbook. Os valores possíveis são:<br>-Novo<br>- Iniciado<br>- Parado<br>- Suspenso<br>- Falhado<br>-Concluída |
| Categoria | Classificação do tipo de dados. Para a Automatização, o valor é JobLogs. |
| OperationName | Especifica o tipo de operação efetuada no Azure. Para automatização, o valor é a tarefa. |
| Recurso | Nome da conta de automatização |
| SourceSystem | Como a análise de registos recolhidos os dados. Sempre *Azure* para diagnóstico do Azure. |
| ResultDescription |Descreve o estado do resultado do trabalho do runbook. Os valores possíveis são:<br>- Trabalho iniciado<br>- Trabalho falhado<br>- Trabalho Concluído |
| CorrelationId |GUID que é o Id de Correlação do trabalho do runbook. |
| ResourceId |Especifica o id de recurso de conta de automatização do Azure do runbook. |
| SubscriptionId | A subscrição do Azure Id (GUID) para a conta de automatização. |
| ResourceGroup | Nome do grupo de recursos para a conta de automatização. |
| ResourceProvider | MICROSOFT. AUTOMATIZAÇÃO |
| ResourceType | AUTOMATIONACCOUNTS |


### <a name="job-streams"></a>Fluxos de trabalho
| Propriedade | Descrição |
| --- | --- |
| TimeGenerated |Data e hora da execução do trabalho do runbook. |
| RunbookName_s |O nome do runbook. |
| Caller_s |Quem iniciou a operação. Os valores possíveis são um endereço de e-mail ou o sistema para trabalhos agendados. |
| StreamType_s |O tipo de fluxo de trabalho. Os valores possíveis são:<br>- Em Curso<br>- Saída<br>- Aviso<br>- Erro<br>- Depuração<br>- Verboso |
| Tenant_g | GUID que identifica o inquilino para o chamador. |
| JobId_g |GUID que é o Id do trabalho do runbook. |
| ResultType |O estado do trabalho do runbook. Os valores possíveis são:<br>-Em curso |
| Categoria | Classificação do tipo de dados. Para a Automatização, o valor é JobStreams. |
| OperationName | Especifica o tipo de operação efetuada no Azure. Para automatização, o valor é a tarefa. |
| Recurso | Nome da conta de automatização |
| SourceSystem | Como a análise de registos recolhidos os dados. Sempre *Azure* para diagnóstico do Azure. |
| ResultDescription |Inclui o fluxo de saída do runbook. |
| CorrelationId |GUID que é o Id de Correlação do trabalho do runbook. |
| ResourceId |Especifica o id de recurso de conta de automatização do Azure do runbook. |
| SubscriptionId | A subscrição do Azure Id (GUID) para a conta de automatização. |
| ResourceGroup | Nome do grupo de recursos para a conta de automatização. |
| ResourceProvider | MICROSOFT. AUTOMATIZAÇÃO |
| ResourceType | AUTOMATIONACCOUNTS |

## <a name="viewing-automation-logs-in-log-analytics"></a>Visualizar automatização registos de análise de registos
Agora que tiver iniciado a enviar os registos da tarefa de automatização para análise de registos, vamos ver o que pode fazer com estes registos no interior de análise de registos.

Para ver os registos, execute a seguinte consulta:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION"`

### <a name="send-an-email-when-a-runbook-job-fails-or-suspends"></a>Envie um e-mail quando uma tarefa de runbook falha ou suspende
Uma das principal cliente pede-lhe destina-se a capacidade de enviar um e-mail ou um texto quando algo não bate com uma tarefa de runbook.   

Para criar uma regra de alerta, pode começa por criar uma pesquisa de registo para os registos da tarefa de runbook que deve invocar o alerta. Clique em de **alerta** botão para criar e configurar a regra de alerta.

1. Na página de descrição geral da análise do registo, clique em **pesquisa registo**.
2. Criar uma consulta de pesquisa de registo para o alerta, escrevendo a pesquisa seguinte para o campo de consulta: `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended")` também pode agrupar pelo RunbookName utilizando:`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and (ResultType == "Failed" or ResultType == "Suspended") | summarize AggregatedValue = count() by RunbookName_s`

   Se configurar os registos de mais do que uma conta de automatização ou a subscrição para a sua área de trabalho, pode agrupar os alertas por subscrição e conta de automatização. Nome da conta de automatização pode ser encontrado no campo de recurso na pesquisa de JobLogs.
1. Para abrir o **Adicionar regra de alerta** ecrã, clique em **alerta** na parte superior da página. Para obter mais informações sobre as opções para configurar o alerta, consulte [alertas na análise de registos](../log-analytics/log-analytics-alerts.md#alert-rules).

### <a name="find-all-jobs-that-have-completed-with-errors"></a>Localizar todas as tarefas que foram concluídas com erros
Para além dos alertas sobre falhas, pode encontrar quando uma tarefa de runbook tem um erro de não interrupção. Nestes casos PowerShell produz uma sequência de erro, mas os erros de não interrupção não fazer com que a tarefa suspender ou falhar.    

1. Na sua área de trabalho de análise de registos, clique em **pesquisa registo**.
2. No campo de consulta, escreva `AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and StreamType_s == "Error" | summarize AggregatedValue = count() by JobId_g` e, em seguida, clique em de **pesquisa** botão.

### <a name="view-job-streams-for-a-job"></a>Fluxos de trabalho de vista de uma tarefa
Quando estiver a depuração uma tarefa, também poderá procurar nos fluxos de trabalho. A consulta seguinte mostra todos os fluxos de uma única tarefa com o GUID 2ebd22ea-e05e-4eb9 - 9d 76-d73cbd4356e0:   

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobStreams" and JobId_g == "2ebd22ea-e05e-4eb9-9d76-d73cbd4356e0" | sort by TimeGenerated asc | project ResultDescription`

### <a name="view-historical-job-status"></a>Ver o estado do histórico de tarefas
Por fim, poderá visualizar o histórico do trabalho ao longo do tempo. Pode utilizar esta consulta para procurar o estado das suas tarefas ao longo do tempo.

`AzureDiagnostics | where ResourceProvider == "MICROSOFT.AUTOMATION" and Category == "JobLogs" and ResultType != "started" | summarize AggregatedValue = count() by ResultType, bin(TimeGenerated, 1h)`  
<br> ![Gráfico de estado de tarefa históricos do OMS](media/automation-manage-send-joblogs-log-analytics/historical-job-status-chart.png)<br>

## <a name="summary"></a>Resumo
Ao enviar os dados de estado e de fluxo da tarefa de automatização à análise de registos, pode obter uma melhor aprofundadas sobre o estado das suas tarefas de automatização por:
+ Configurar alertas para notificá-lo quando existe um problema.
+ Utilizar vistas personalizadas e consultas de pesquisa para visualizar os resultados de runbook, estado de tarefa de runbook e outras relacionados com indicadores chaves ou métricas.  

Análise de registos dá-lhe maior visibilidade operacional para as tarefas de automatização e pode ajudar a incidentes de endereço mais rápidas.  

## <a name="next-steps"></a>Passos seguintes
* Para obter mais informações sobre como construir consultas de pesquisa diferentes e reveja os registos da tarefa de automatização com a análise de registos, consulte o artigo [pesquisas de registo na análise de registos](../log-analytics/log-analytics-log-searches.md).
* Para compreender como criar e obter mensagens de erro e de saída a partir de runbooks, consulte [Runbook de saída e mensagens](automation-runbook-output-and-messages.md).
* Para saber mais sobre a execução de runbooks, como monitorizar tarefas de runbooks e outros detalhes técnicos, veja [Acompanhar uma tarefa de runbook](automation-runbook-execution.md).
* Para saber mais sobre a análise de registos do OMS e origens de recolha de dados, consulte [dados de armazenamento do Azure recolha na descrição geral da análise de registos](../log-analytics/log-analytics-azure-storage.md).
