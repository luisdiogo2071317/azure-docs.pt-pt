---
title: Execução de Runbooks na automatização do Azure
description: Descreve os detalhes de como um runbook na automatização do Azure é processado.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 10/17/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 2b1a6e2921fdaf9ede1184cfc02c3f61f63c60ac
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49393768"
---
# <a name="runbook-execution-in-azure-automation"></a>Execução de Runbooks na automatização do Azure

Quando inicia um runbook na automatização do Azure, é criada uma tarefa. Uma tarefa é uma instância de execução individual de um runbook. Um operador de automatização do Azure é atribuído para executar cada tarefa. Embora funções de trabalho são partilhadas por muitas contas do Azure, as tarefas de diferentes contas de automatização são isoladas uns dos outros. Não tem controle sobre qual trabalho processa o pedido para a sua tarefa. Um runbook individual pode ter muitos trabalhos em execução em simultâneo. O ambiente de execução de trabalhos a partir da mesma conta de automatização pode ser reutilizado. Quando exibir a lista de runbooks no portal do Azure, ele indica o estado de todas as tarefas que foram iniciados para cada runbook. Pode ver a lista de tarefas para cada runbook controlar o estado de cada. Para obter uma descrição dos Estados de tarefa diferente [Estados das tarefas](#job-statuses).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

O diagrama seguinte mostra o ciclo de vida de uma tarefa de runbook para [runbooks gráficos](automation-runbook-types.md#graphical-runbooks) e [runbooks de fluxo de trabalho do PowerShell](automation-runbook-types.md#powershell-workflow-runbooks).

![Estados das tarefas - fluxo de trabalho do PowerShell](./media/automation-runbook-execution/job-statuses.png)

O diagrama seguinte mostra o ciclo de vida de uma tarefa de runbook para [runbooks do PowerShell](automation-runbook-types.md#powershell-runbooks).

![Estados das tarefas - Script do PowerShell](./media/automation-runbook-execution/job-statuses-script.png)

As tarefas têm acesso aos recursos do Azure por estabelecer a conexão com a sua subscrição do Azure. Apenas têm acesso a recursos no seu centro de dados se esses recursos estão acessíveis a partir da cloud pública.

## <a name="job-statuses"></a>Estados das tarefas

A tabela seguinte descreve os diferentes Estados possíveis para uma tarefa. PowerShell tem dois tipos de erros, erros de terminação e de não terminação. Erros de terminação de definir o estado de runbook como **falhada** caso estas ocorram. Erros de não terminação permitem que o script continuar, mesmo depois que eles ocorrem. Um exemplo de um erro de não terminação está a utilizar o `Get-ChildItem` cmdlet com um caminho que não existe. PowerShell vê que o caminho não existe, emite um erro e continua para a pasta seguinte. Este erro não definir o estado de runbook como **falhada** e pode ser marcado como **concluído**. Para forçar um runbook para parar num erro de não terminação, pode usar `-ErrorAction Stop` sobre o cmdlet.

| Estado | Descrição |
|:--- |:--- |
| Concluído |A tarefa foi concluída com êxito. |
| Com Falhas |Para [runbooks de gráfico e fluxo de trabalho do PowerShell](automation-runbook-types.md), o runbook não conseguiu compilar. Para [runbooks de Script do PowerShell](automation-runbook-types.md), não foi possível iniciar o runbook ou a tarefa ter uma exceção. |
| Falha ao aguardar que recursos |A tarefa falhou porque atingiu o [justa](#fair-share) limitar três vezes e iniciado a partir do ponto de verificação mesmo ou desde o início do runbook cada vez. |
| Em Fila |A tarefa está a aguardar para recursos num trabalho de automatização fique disponível para que ele pode ser iniciado. |
| A iniciar |A tarefa foi atribuída a uma função de trabalho e o sistema está a iniciar. |
| A retomar |O sistema está a retomar a tarefa depois de ter sido suspenso. |
| A executar |A tarefa está em execução. |
| Em execução, à espera de recursos |A tarefa foi descarregada porque atingiu o [justa](#fair-share) limite. Ele retoma em breve do último ponto de verificação. |
| Parada |A tarefa foi parada pelo utilizador antes de ser concluída. |
| A parar |O sistema está a parar a tarefa. |
| Suspenso |A tarefa foi suspensa pelo utilizador, pelo sistema ou por um comando no runbook. Se um runbook não tiver um ponto de verificação, ele começa desde o início do runbook. Se tiver um ponto de verificação, pode começar novamente e retomar a partir do último ponto de verificação. O runbook só é suspensa pelo sistema quando ocorre uma exceção. Por predefinição, ErrorActionPreference está definido como **continuar**, que significa que a tarefa em execução num erro. Se esta variável de preferência estiver definida como **parar**, em seguida, a tarefa suspende num erro. Aplica-se ao [runbooks de gráfico e fluxo de trabalho do PowerShell](automation-runbook-types.md) apenas. |
| A suspender |O sistema está a tentar suspender a tarefa a pedido do utilizador. O runbook tem de atingir o ponto de verificação seguinte antes de poder ser suspenso. Se já passado o último ponto de verificação, em seguida, ele é concluída antes de poder ser suspenso. Aplica-se ao [runbooks de gráfico e fluxo de trabalho do PowerShell](automation-runbook-types.md) apenas. |

## <a name="viewing-job-status-from-the-azure-portal"></a>Visualizar o estado da tarefa no portal do Azure

Pode ver o estado de todas as tarefas de runbook resumido ou pormenorizar os detalhes de uma tarefa de runbook específico no portal do Azure. Pode também configurar integração com sua área de trabalho do Log Analytics para reencaminhar fluxos de estado e a tarefa de trabalho de runbook. Para obter mais informações sobre a integração com o Log Analytics, consulte [reencaminhar o estado da tarefa e fluxos de trabalho de automatização para o Log Analytics](automation-manage-send-joblogs-log-analytics.md).

### <a name="automation-runbook-jobs-summary"></a>Tarefas de runbook de automatização resumidas

No lado direito da sua conta de automatização selecionada, pode ver um resumo de todas as tarefas de runbook sob **estatísticas de tarefas** mosaico.

![Mosaico de estatísticas de tarefas](./media/automation-runbook-execution/automation-account-job-status-summary.png)

Este mosaico apresenta uma contagem e uma representação gráfica do Estado da tarefa para todas as tarefas executadas.

Ao clicar no mosaico apresenta os **tarefas** página, que inclui uma lista resumida de todas as tarefas executadas. Esta página mostra o estado, as horas de início e tempos de conclusão.

![Página de tarefas de conta de automatização](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)

Pode filtrar a lista de tarefas, selecionando **filtrar tarefas** e filtro de um runbook específico, o estado da tarefa, ou da lista pendente e o intervalo de tempo para procurar dentro de um.

![Estado da tarefa de filtro](./media/automation-runbook-execution/automation-account-jobs-filter.png)

Em alternativa, pode ver os detalhes de resumo de tarefa para um runbook específico ao selecionar esse runbook a partir da **Runbooks** página na conta de automatização e, em seguida, selecione a **tarefas** mosaico. Esta ação apresenta os **tarefas** página, e a partir daí, pode clicar num registo de tarefa para ver os detalhes e a saída.

![Página de tarefas de conta de automatização](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)

### <a name="job-summary"></a>Resumo da Tarefa

Pode ver uma lista de todas as tarefas que foram criadas para um determinado runbook e o respetivo estado mais recente. Pode filtrar esta lista por Estado da tarefa e o intervalo de datas da última alteração efetuada à tarefa. Para ver os informações detalhadas e o resultado, clique no nome de uma tarefa. A vista detalhada da tarefa inclui os valores dos parâmetros do runbook fornecidos para essa tarefa.

Pode utilizar os seguintes passos para ver as tarefas de um runbook.

1. No portal do Azure, selecione **automatização** e, em seguida, selecione o nome de uma conta de automatização.
2. A partir do hub, selecione **Runbooks** e, em seguida, no **Runbooks** página selecione um runbook a partir da lista.
3. Na página para o runbook selecionado, clique nas **tarefas** mosaico.
4. Clique das tarefas na lista e, na página de detalhes da tarefa do runbook pode ver os detalhes e o resultado.

## <a name="retrieving-job-status-using-windows-powershell"></a>A obter o estado da tarefa com o Windows PowerShell

Pode utilizar o [Get-AzureRmAutomationJob](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjob) para obter as tarefas criadas para um runbook e os detalhes de uma tarefa específica. Se iniciar um runbook com o Windows PowerShell através de [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook), em seguida, ele retorna a tarefa resultante. Uso [Get-AzureRmAutomationJobOutput](https://docs.microsoft.com/powershell/module/azurerm.automation/get-azurermautomationjoboutput) para uma tarefa de obter o resultado.

Os seguintes comandos de exemplo recuperar a última tarefa de um runbook de exemplo e exibir o respetivo estado, os valores fornecidos para os parâmetros do runbook e o resultado da tarefa.

```azurepowershell-interactive
$job = (Get-AzureRmAutomationJob –AutomationAccountName "MyAutomationAccount" `
–RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
$job.Status
$job.JobParameters
Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output
```

O exemplo seguinte obtém o resultado de uma tarefa específica e retorna cada registo. No caso que ocorreu uma exceção para um dos registos, a exceção é gravada em vez do valor. Este comportamento é útil como exceções podem fornecer informações adicionais, que não podem ser registadas normalmente durante a saída.

```azurepowershell-interactive
$output = Get-AzureRmAutomationJobOutput -AutomationAccountName <AutomationAccountName> -Id <jobID> -ResourceGroupName <ResourceGroupName> -Stream "Any"
foreach($item in $output)
{
    $fullRecord = Get-AzureRmAutomationJobOutputRecord -AutomationAccountName <AutomationAccountName> -ResourceGroupName <ResourceGroupName> -JobId <jobID> -Id $item.StreamRecordId
    if ($fullRecord.Type -eq "Error")
    {
        $fullRecord.Value.Exception
    }
    else
    {
    $fullRecord.Value
    }
}
```

## <a name="get-details-from-activity-log"></a>Obter os detalhes do registo de atividades

Outros detalhes, como a pessoa ou uma conta que iniciou o runbook podem ser obtidos a partir do registo de atividades para a conta de automatização. O exemplo de PowerShell seguinte fornece o último utilizador a executar o runbook em questão:

```powershell-interactive
$SubID = "00000000-0000-0000-0000-000000000000"
$rg = "ResourceGroup01"
$AutomationAccount = "MyAutomationAccount"
$RunbookName = "Test-Runbook"
$JobResourceID = "/subscriptions/$subid/resourcegroups/$rg/providers/Microsoft.Automation/automationAccounts/$AutomationAccount/jobs"

Get-AzureRmLog -ResourceId $JobResourceID -MaxRecord 1 | Select Caller
```

## <a name="fair-share"></a>Justa

Para poder partilhar recursos entre todos os runbooks na cloud, automatização do Azure será temporariamente descarregar ou parar qualquer tarefa de execução durante mais de três horas. Tarefas de [baseada no PowerShell runbooks](automation-runbook-types.md#powershell-runbooks) e [runbooks de Python](automation-runbook-types.md#python-runbooks) são paradas e não é reiniciado, e o estado da tarefa mostra parado.

Para tarefas de execução prolongadas, é recomendado utilizar uma [Função de Trabalho de Runbook Híbrida](automation-hrw-run-runbooks.md#job-behavior). Os Runbook Workers híbridos não estão limitados pelo justa e não tem uma limitação quanto um runbook pode executar. A outra tarefa [limites](../azure-subscription-service-limits.md#automation-limits) aplicam-se a áreas de segurança do Azure e os Runbook Workers híbridos. Embora os Runbook Workers híbridos não estão limitados pelo limite de cota razoável de duração de 3 horas, runbooks foi executada nos mesmos ainda deve ser desenvolvido para oferecer suporte a comportamentos de reinício de problemas de infraestrutura local inesperado.

Outra opção é otimizar o runbook utilizando runbooks subordinados. Se o seu runbook percorre a mesma função num número de recursos, como uma operação de base de dados em várias bases de dados, pode mover essa função para um [runbook subordinado](automation-child-runbooks.md) e chamá-lo com o [ Start-AzureRMAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook) cmdlet. Cada um destes runbooks subordinados é executado em paralelo, em processos separados, diminuindo a quantidade total de tempo do runbook principal para conclusão. Pode utilizar o [Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/Get-AzureRmAutomationJob) cmdlet no runbook para verificar o estado da tarefa para cada filho, se existem operações que precisam ser executadas após a conclusão do runbook subordinado.

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre os diferentes métodos que podem ser utilizados para iniciar um runbook na automatização do Azure, veja [iniciar um runbook na automatização do Azure](automation-starting-a-runbook.md)
