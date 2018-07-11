---
title: Execução de Runbooks na automatização do Azure
description: Descreve os detalhes de como um runbook na automatização do Azure é processado.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 05/08/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 4b9bfc0df01dd8fc8a6a1b7aed5ade466164a82f
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37930057"
---
# <a name="runbook-execution-in-azure-automation"></a>Execução de Runbooks na automatização do Azure

Quando inicia um runbook na automatização do Azure, é criada uma tarefa. Uma tarefa é uma instância de execução individual de um runbook. Um operador de automatização do Azure é atribuído para executar cada tarefa. Embora funções de trabalho são partilhadas por várias contas do Azure, as tarefas de diferentes contas de automatização são isoladas uns dos outros. Não precisa controlar sobre qual trabalho processa o pedido para a sua tarefa. Um runbook individual pode ter várias tarefas em execução em simultâneo. O ambiente de execução de trabalhos a partir da mesma conta de automatização pode ser reutilizado. Quando exibir a lista de runbooks no portal do Azure, ele indica o estado de todas as tarefas que foram iniciados para cada runbook. Pode ver a lista de tarefas para cada runbook para poder controlar o estado de cada. Para obter uma descrição dos Estados de tarefa diferente [Estados das tarefas](#job-statuses).

[!INCLUDE [gdpr-dsr-and-stp-note.md](../../includes/gdpr-dsr-and-stp-note.md)]

O diagrama seguinte mostra o ciclo de vida de uma tarefa de runbook para [runbooks gráficos](automation-runbook-types.md#graphical-runbooks) e [runbooks de fluxo de trabalho do PowerShell](automation-runbook-types.md#powershell-workflow-runbooks).

![Estados das tarefas - fluxo de trabalho do PowerShell](./media/automation-runbook-execution/job-statuses.png)

O diagrama seguinte mostra o ciclo de vida de uma tarefa de runbook para [runbooks do PowerShell](automation-runbook-types.md#powershell-runbooks).

![Estados das tarefas - Script do PowerShell](./media/automation-runbook-execution/job-statuses-script.png)

As tarefas têm acesso aos recursos do Azure por estabelecer a conexão com a sua subscrição do Azure. Apenas têm acesso a recursos no seu centro de dados se esses recursos estão acessíveis a partir da cloud pública.

## <a name="job-statuses"></a>Estados das tarefas

A tabela seguinte descreve os diferentes Estados possíveis para uma tarefa.

| Estado | Descrição |
|:--- |:--- |
| Concluída |A tarefa foi concluída com êxito. |
| Com Falhas |Para [runbooks de gráfico e fluxo de trabalho do PowerShell](automation-runbook-types.md), o runbook não conseguiu compilar. Para [runbooks de Script do PowerShell](automation-runbook-types.md), não foi possível iniciar o runbook ou a tarefa encontrou uma exceção. |
| Falha ao aguardar que recursos |A tarefa falhou porque atingiu o [justa](#fair-share) limitar três vezes e iniciado a partir do ponto de verificação mesmo ou desde o início do runbook cada vez. |
| Em Fila |A tarefa está a aguardar para recursos num trabalho de automatização fique disponível para que ele pode ser iniciado. |
| A iniciar |A tarefa foi atribuída a uma função de trabalho e o sistema está a iniciá-la. |
| A retomar |O sistema está a retomar a tarefa depois de ter sido suspenso. |
| A executar |A tarefa está em execução. |
| Em execução, à espera de recursos |A tarefa foi descarregada porque atingiu o [justa](#fair-share) limite. Ele retoma em breve do último ponto de verificação. |
| Parada |A tarefa foi parada pelo utilizador antes de ser concluída. |
| A parar |O sistema está a parar a tarefa. |
| Suspenso |A tarefa foi suspensa pelo utilizador, pelo sistema ou por um comando no runbook. Uma tarefa suspensa pode ser iniciada novamente e retomar a partir do último ponto de verificação ou desde o início do runbook, caso não tenha pontos de verificação. O runbook só é suspensa pelo sistema quando ocorre uma exceção. Por predefinição, ErrorActionPreference está definido como **continuar**, que significa que a tarefa em execução num erro. Se esta variável de preferência estiver definida como **parar**, em seguida, a tarefa suspende num erro. Aplica-se ao [runbooks de gráfico e fluxo de trabalho do PowerShell](automation-runbook-types.md) apenas. |
| A suspender |O sistema está a tentar suspender a tarefa a pedido do utilizador. O runbook tem de atingir o ponto de verificação seguinte antes de poder ser suspenso. Se já passado o último ponto de verificação, em seguida, ele é concluída antes de poder ser suspenso. Aplica-se ao [runbooks de gráfico e fluxo de trabalho do PowerShell](automation-runbook-types.md) apenas. |

## <a name="viewing-job-status-from-the-azure-portal"></a>Visualizar o estado da tarefa no portal do Azure

Pode ver o estado de todas as tarefas de runbook resumido ou pormenorizar os detalhes de uma tarefa de runbook específico no portal do Azure ou ao configurar a integração com sua área de trabalho do Log Analytics para reencaminhar fluxos de estado e a tarefa de trabalho de runbook. Para obter mais informações sobre a integração com o Log Analytics, consulte [reencaminhar o estado da tarefa e fluxos de trabalho de automatização para o Log Analytics](automation-manage-send-joblogs-log-analytics.md).

### <a name="automation-runbook-jobs-summary"></a>Tarefas de runbook de automatização resumidas

No lado direito da sua conta de automatização selecionada, pode ver um resumo de todas as tarefas de runbook para uma conta de automatização sob **estatísticas de tarefas** mosaico.

![Mosaico de estatísticas de tarefas](./media/automation-runbook-execution/automation-account-job-status-summary.png).

Este mosaico apresenta uma contagem e uma representação gráfica do Estado da tarefa para todas as tarefas executadas.

Ao clicar no mosaico apresenta os **tarefas** painel, que inclui uma lista resumida de todas as tarefas executadas, com o estado, a execução da tarefa e horários de início e conclusão.

![Painel de tarefas de conta de automatização](./media/automation-runbook-execution/automation-account-jobs-status-blade.png)

Pode filtrar a lista de tarefas, selecionando **filtrar tarefas** e filtro de um runbook específico, o estado da tarefa, ou na lista pendente, o intervalo de data/hora para procurar dentro de um.

![Estado da tarefa de filtro](./media/automation-runbook-execution/automation-account-jobs-filter.png)

Em alternativa, pode ver os detalhes de resumo de tarefa para um runbook específico ao selecionar esse runbook a partir da **Runbooks** painel na sua conta de automatização e, em seguida, selecione a **tarefas** mosaico. Isto apresenta os **tarefas** painel, e a partir daí, pode clicar num registo de tarefa para ver os detalhes e a saída.

![Painel de tarefas de conta de automatização](./media/automation-runbook-execution/automation-runbook-job-summary-blade.png)

### <a name="job-summary"></a>Resumo da Tarefa

Pode ver uma lista de todas as tarefas que foram criadas para um determinado runbook e o respetivo estado mais recente. Pode filtrar esta lista por Estado da tarefa e o intervalo de datas da última alteração efetuada à tarefa. Para ver os informações detalhadas e o resultado, clique no nome de uma tarefa. A vista detalhada da tarefa inclui os valores dos parâmetros do runbook fornecidos para essa tarefa.

Pode utilizar os seguintes passos para ver as tarefas de um runbook.

1. No portal do Azure, selecione **automatização** e, em seguida, selecione o nome de uma conta de automatização.
2. A partir do hub, selecione **Runbooks** e, em seguida, no **Runbooks** painel selecione um runbook a partir da lista.
3. No painel do runbook selecionado, clique nas **tarefas** mosaico.
4. Clique das tarefas na lista e no painel de detalhes de tarefa do runbook pode ver os detalhes e o resultado.

## <a name="retrieving-job-status-using-windows-powershell"></a>A obter o estado da tarefa com o Windows PowerShell

Pode utilizar o [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx) para obter as tarefas criadas para um runbook e os detalhes de uma tarefa específica. Se iniciar um runbook com o Windows PowerShell através de [Start-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx), em seguida, ele retorna a tarefa resultante. Uso [Get-AzureRmAutomationJob](https://msdn.microsoft.com/library/mt619440.aspx)o resultado de saída para obter uma tarefa.

Os seguintes comandos de exemplo recuperar a última tarefa de um runbook de exemplo e exibir o respetivo estado, os valores fornecidos para os parâmetros do runbook e o resultado da tarefa.

```azurepowershell-interactive
$job = (Get-AzureRmAutomationJob –AutomationAccountName "MyAutomationAccount" `
–RunbookName "Test-Runbook" -ResourceGroupName "ResourceGroup01" | sort LastModifiedDate –desc)[0]
$job.Status
$job.JobParameters
Get-AzureRmAutomationJobOutput -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAcct" -Id $job.JobId –Stream Output
```

O exemplo seguinte obtém o resultado de uma tarefa específica e retorna cada registo. No caso que ocorreu uma exceção para um dos registos, a exceção é gravada em vez do valor. Isto é útil como exceções podem fornecer informações adicionais que não podem ser registadas normalmente durante a saída.

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

Para poder partilhar recursos entre todos os runbooks na cloud, automatização do Azure será temporariamente descarregado, todas as tarefas após a execução de três horas. Durante este período, tarefas de [runbooks baseados em PowerShell](automation-runbook-types.md#powershell-runbooks) são parados e não forem reiniciados. O estado da tarefa mostra **parado**. Este tipo de runbook é sempre reiniciado desde o início, uma vez que não suportam os pontos de verificação.

[Runbooks de fluxo de trabalho-baseada no PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) são retomado a partir de seus últimos [ponto de verificação](https://docs.microsoft.com/system-center/sma/overview-powershell-workflows#bk_Checkpoints). Depois de executar a três horas, a tarefa de runbook está suspenso pelo serviço e o estado mostra **em execução, aguardar por recursos**. Quando uma área de segurança se torna disponível, o runbook é automaticamente reiniciado pelo serviço de automatização e currículos do último ponto de verificação. Este é o comportamento normal de fluxo de trabalho de PowerShell para suspender/reinício. Se o runbook novamente exceder a três horas de tempo de execução, o processo se repete até três vezes. Após o reinício de terceiro, se o runbook ainda não concluiu em três horas, em seguida, a tarefa de runbook é falhou e mostra o estado da tarefa **falha, aguardar por recursos**. Neste caso, receberá a seguinte exceção com a falha.

*A tarefa não é possível continuar a ser executada uma vez que ele foi expulso repetidamente do mesmo ponto de verificação. Certifique-se de que o Runbook não realiza operações demoradas sem manter seu estado.*

Isso serve para proteger o serviço de runbooks em execução indefinidamente sem concluir, dado que não fazem torná-lo para o ponto de verificação seguinte sem a ser descarregado novamente.

Se o runbook tiver sem pontos de verificação ou a tarefa não tinha alcançado o primeiro ponto de verificação antes de a ser descarregado, em seguida, reinicia desde o início.

Para execução prolongada tarefas, é recomendado utilizar um [Runbook Worker híbrido](automation-hrw-run-runbooks.md#job-behavior). Os Runbook Workers híbridos não estão limitados pelo justa e não ter tem uma limitação quanto um runbook pode executar.

Se estiver a utilizar um runbook de fluxo de trabalho do PowerShell no Azure, ao criar um runbook, deve garantir que o tempo para executar todas as atividades entre dois pontos de verificação não exceda três horas. Poderá ter de adicionar pontos de verificação ao runbook para se certificar de que não atingir este limite de três horas ou divida de longa execução de operações. Por exemplo, o runbook pode executar um reindex num banco de dados SQL. Se esta operação única não for concluída dentro do limite de cota razoável, a tarefa é descarregada e reiniciada desde o início. Neste caso, deve dividir a operação de reindex em várias etapas, como a reindexação uma tabela por vez e, em seguida, inserir um ponto de verificação após cada operação para que a tarefa foi retomada após a última operação seja concluída.

## <a name="next-steps"></a>Passos Seguintes

* Para saber mais sobre os diferentes métodos que podem ser utilizados para iniciar um runbook na automatização do Azure, veja [iniciar um runbook na automatização do Azure](automation-starting-a-runbook.md)
