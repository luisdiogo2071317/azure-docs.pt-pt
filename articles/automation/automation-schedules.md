---
title: Agendas na automatização do Azure
description: As agendas da automatização são utilizadas para agendar runbooks na automatização do Azure para iniciar automaticamente. Descreve como criar e gerir uma agenda para que possa começar automaticamente um runbook num determinado momento ou numa agenda periódica.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 05/08/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 6c7bd4d4249d304ee7c1df4ae4b8fc0af476b99c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Agendar um runbook na Automatização do Azure

Para agendar um runbook na automatização do Azure para iniciar o um tempo especificado, ligue-o a uma ou mais agendas. Uma agenda pode ser configurada para executar uma vez ou um ocorrer hora a hora ou diária agenda para runbooks no portal do Azure. Também pode agendá-los para semanal, mensal, específicos dias da semana ou dias do mês ou num determinado dia do mês. Um runbook pode ser ligado a várias agendas e uma agenda pode ter vários runbooks ligados ao mesmo.

> [!NOTE]
> As agendas não suportam atualmente configurações de DSC de automatização do Azure.

## <a name="windows-powershell-cmdlets"></a>Cmdlets do Windows PowerShell

Os cmdlets na tabela seguinte são utilizados para criar e gerir agendas com o Windows PowerShell na automatização do Azure. Estes são enviados como parte do [módulo Azure PowerShell](/powershell/azure/overview).

| Cmdlets | Descrição |
|:--- |:--- |
| [Get-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/get-azurermautomationschedule) |Obtém uma agenda. |
| [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) |Cria uma nova agenda. |
| [Remove-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/remove-azurermautomationschedule) |Remove uma agenda. |
| [Set-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) |Define as propriedades de uma agenda existente. |
| [Get-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/set-azurermautomationscheduledrunbook) |Obtém agendada runbooks. |
| [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) |Associa um runbook com uma agenda. |
| [Unregister-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/unregister-azurermautomationscheduledrunbook) |Dissociates um runbook a partir de uma agenda. |

## <a name="creating-a-schedule"></a>Criar uma agenda

Pode criar uma nova agenda para runbooks no portal do Azure ou com o Windows PowerShell.

> [!NOTE]
> A automatização do Azure utiliza os módulos mais recentes na sua conta de automatização quando uma nova tarefa agendada é executada.  Para evitar o impacto dos runbooks e os processos que automatizar, deverá testar primeiro todos os runbooks que tenha ligado agendas com uma conta de automatização dedicada para fins de teste.  Esta ação valida o agendada runbooks continua a funcionar corretamente e se não estiver, ainda pode resolver problemas e aplicar as alterações necessárias antes de migrar a versão do runbook atualizado para produção.
> A conta de automatização não obter automaticamente as novas versões de módulos, a menos que atualizou-los manualmente, selecionando o [os módulos do Azure Update](automation-update-azure-modules.md) opção do **módulos**.

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Para criar uma nova agenda no portal do Azure

1. No portal do Azure, da sua conta de automatização, selecione **agendas** na secção **recursos partilhados** à esquerda.
1. Clique em **adicionar uma agenda** na parte superior da página.
1. No **nova agenda** painel, escreva um **nome** e opcionalmente um **Descrição** para a nova agenda.
1. Selecione se a agenda é executada uma vez, ou com base numa agenda reoccurring selecionando **uma vez** ou **periodicidade**. Se selecionar **uma vez** especificar um **hora de início**e, em seguida, clique em **criar**. Se selecionar **periodicidade**, especifique um **hora de início** e a frequência com que frequência pretende runbook repetir - por **hora**, **dia**, **semana**, ou por **mês**. Se selecionar **semana** ou **mês** na lista pendente, o **opção de periodicidade** aparece no painel e após a seleção, o **opção de periodicidade** painel é apresentado e pode selecionar o dia da semana se tiver selecionado **semana**. Se tiver selecionado **mês**, pode optar por **dias da semana** ou dias específicos do mês de calendário e por fim, que pretende executá-lo no último dia do mês ou não e, em seguida, clique em **OK**.

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Para criar uma nova agenda com o Windows PowerShell

Utilizar o [New-AzureRmAutomationSchedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) cmdlet para criar agendas. Especifique a hora de início da agenda e a frequência que deve ser executada.

Os comandos de exemplo seguintes mostram como criar um agendamento para a partir do dia 15 e 30th de cada mês utilizando um cmdlet do Azure Resource Manager.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
$scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
-DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"
```

## <a name="linking-a-schedule-to-a-runbook"></a>Ligar uma agenda a um runbook

Um runbook pode ser ligado a várias agendas e uma agenda pode ter vários runbooks ligados ao mesmo. Se um runbook tiver parâmetros, pode fornecer valores para os mesmos. Tem de fornecer valores para todos os parâmetros obrigatórios e pode fornecer valores para os parâmetros opcionais. Estes valores são utilizados sempre que o runbook for iniciado por este agendamento. Pode anexar o mesmo runbook ao agendamento outro e especifique os valores de parâmetro diferentes.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Para ligar uma agenda a um runbook com o portal do Azure

1. No portal do Azure, da sua conta de automatização, selecione **Runbooks** na secção **automatização de processos** à esquerda.
1. Clique no nome do runbook a agendar.
1. Se o runbook não está atualmente associado a uma agenda, em seguida, é-lhe dada a opção para criar uma nova agenda ou a ligação para uma agenda existente.
1. Se o runbook tiver parâmetros, pode selecionar a opção **modificar definições de execução (predefinição: Azure)** e **parâmetros** é apresentado o painel onde pode introduzir as informações em conformidade.

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Para ligar uma agenda a um runbook com o Windows PowerShell

Pode utilizar o [Register-AzureRmAutomationScheduledRunbook](/powershell/module/azurerm.automation/register-azurermautomationscheduledrunbook) cmdlet para associar um agendamento. Pode especificar valores para os parâmetros do runbook com o parâmetro de parâmetros. Para obter mais informações sobre a especificação de valores de parâmetros, consulte [iniciar um Runbook na automatização do Azure](automation-starting-a-runbook.md).
Os comandos de exemplo seguintes mostram como ligar uma agenda a um runbook com um cmdlet do Azure Resource Manager com parâmetros.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$runbookName = "Test-Runbook"
$scheduleName = "Sample-DailySchedule"
$params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
–Name $runbookName –ScheduleName $scheduleName –Parameters $params `
-ResourceGroupName "ResourceGroup01"
```

## <a name="scheduling-runbooks-more-frequently"></a>Agendamento de runbooks com mais frequência

O intervalo mais frequente que uma agenda na automatização do Azure pode ser configurada para é uma hora. Se necessitar de agendas para execução mais frequentemente do que o que, existem duas opções:

* Criar um [webhook](automation-webhooks.md) para o runbook e utilize [agendador do Azure](../scheduler/scheduler-get-started-portal.md) para chamar o webhook. O agendador do Azure fornece mais refinada granularidade quando definir uma agenda.

* Crie quatro agendamentos iniciar todos os em 15 minutos de si executar uma vez a cada hora. Este cenário permite que o runbook seja executado a cada 15 minutos com as agendas diferentes.

## <a name="disabling-a-schedule"></a>Desativar uma agenda

Ao desativar uma agenda, qualquer runbook ligada ao mesmo já não é executado nessa agenda. Manualmente pode desactivar uma agenda ou definir um prazo de expiração para agendas com uma frequência quando criá-los. Quando for atingida a hora de expiração, a agenda está desativada.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Para desativar uma agenda do portal do Azure

1. No portal do Azure, da sua conta de automatização, selecione **agendas** na secção **recursos partilhados** à esquerda.
1. Clique no nome de uma agenda para abrir o painel de detalhes.
1. Alteração **ativada** para **não**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>Para desativar uma agenda com o Windows PowerShell

Pode utilizar o [conjunto AzureRmAutomationSchedule](/powershell/module/azurerm.automation/set-azurermautomationschedule) cmdlet para alterar as propriedades de uma agenda existente. Para desativar o agendamento, especifique **falso** para o **IsEnabled** parâmetro.

Os comandos de exemplo seguintes mostram como desactivar uma agenda de um runbook com um cmdlet do Azure Resource Manager.

```azurepowershell-interactive
$automationAccountName = "MyAutomationAccount"
$scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
–Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"
```

## <a name="next-steps"></a>Passos Seguintes

* Para começar com runbooks na automatização do Azure, consulte [iniciar um Runbook na automatização do Azure](automation-starting-a-runbook.md)