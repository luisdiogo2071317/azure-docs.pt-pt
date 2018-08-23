---
title: Runbooks subordinados na automatização do Azure
description: Descreve os diferentes métodos para iniciar um runbook na automatização do Azure a partir de outro runbook e partilhar informações entre eles.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 08/14/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 2060239b27ef05c34ea6f5b388b4c4086a44a826
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/15/2018
ms.locfileid: "42059566"
---
# <a name="child-runbooks-in-azure-automation"></a>Runbooks subordinados na automatização do Azure

É uma prática recomendada na automatização do Azure para escrever runbooks modulares, reutilizáveis, com uma função discreta que pode ser utilizada por outros runbooks. Um runbook principal chamará, frequentemente, um ou mais runbooks subordinados para executar a funcionalidade necessária. Existem duas formas de chamar um runbook subordinado e cada uma delas apresenta diferenças distintas que deverá compreender para que pode determinar qual é melhor para os seus cenários diferentes.

## <a name="invoking-a-child-runbook-using-inline-execution"></a>Invocar um runbook subordinado com a execução inline

Para invocar um runbook inline a partir de outro runbook, utilize o nome do runbook e forneça valores para seus parâmetros, exatamente da mesma forma que usaria uma atividade ou um cmdlet.  Todos os runbooks na mesma conta de automatização estão disponíveis para todos os outros a ser utilizada desta forma. O runbook principal aguarda o runbook subordinado seja concluído antes de passar para a próxima linha e qualquer resultado é devolvido diretamente ao principal.

Quando invoca um runbook inline, ele é executado na mesma tarefa que o runbook principal. Não haverá nenhuma indicação no histórico da tarefa do runbook subordinado que foi executado. Todas as exceções e qualquer fluxo de saída do runbook subordinado serão associadas ao principal. Isso resulta em menos tarefas e os torna mais fácil de controlar e resolver problemas desde eventuais exceções geradas pelo runbook subordinado e qualquer uma das respetivas saídas de fluxo são associadas a tarefa principal.

Quando um runbook é publicado, todos os runbooks subordinados que aquele invoque já tem de ser publicados. Isto acontece porque a automatização do Azure cria uma associação com todos os runbooks subordinados quando um runbook é compilado. Se não forem, o runbook principal aparecerá ter sido publicado corretamente, mas gerará uma exceção quando é iniciada. Se isto acontecer, pode voltar a publicar o runbook principal para referenciar adequadamente os runbooks subordinados. Não é necessário voltar a publicar o runbook principal se qualquer um dos runbooks subordinados forem alterados, porque a associação já terá sido criada.

Os parâmetros de um runbook subordinado invocado inline podem ser qualquer tipo de dados, incluindo objetos complexos e não existe nenhuma [serialização JSON](automation-starting-a-runbook.md#runbook-parameters) porque não existe quando iniciar o runbook no portal do Azure ou com o Cmdlet Start-AzureRmAutomationRunbook.

### <a name="runbook-types"></a>Tipos de runbooks

Quais são os tipos podem chamar entre si:

* R [runbook do PowerShell](automation-runbook-types.md#powershell-runbooks) e [runbooks gráficos](automation-runbook-types.md#graphical-runbooks) pode chamar cada outros embutidos (ambos são PowerShell com base).
* R [runbook de fluxo de trabalho do PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) e runbooks de fluxo de trabalho de PowerShell gráfico pode chamar cada outros embutidos (ambos são o fluxo de trabalho do PowerShell com base)
* Os tipos de PowerShell e os tipos de fluxo de trabalho do PowerShell não é possível chamar entre si inline e tem de utilizar Start-AzureRmAutomationRunbook.

Quando publicar a questão de ordem:

* A ordem de publicação de runbooks só é importante para os runbooks de fluxo de trabalho do PowerShell e o fluxo de trabalho de PowerShell gráfico.

Quando chamar um runbook subordinado gráfico ou fluxo de trabalho do PowerShell com a execução inline, use apenas o nome do runbook.  Quando chama um runbook subordinado do PowerShell, tem de começar o seu nome com *.\\*  para especificar que o script está localizado no diretório local.

### <a name="example"></a>Exemplo

O exemplo seguinte invoca um runbook subordinado de teste que aceita três parâmetros, um objeto complexo, um número inteiro e um booleano. A saída do runbook subordinado é atribuída a uma variável.  Neste caso, o runbook subordinado é um runbook de fluxo de trabalho do PowerShell.

```azurepowershell-interactive
$vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true
```

Segue-se o mesmo exemplo com um runbook do PowerShell como o filho.

```azurepowershell-interactive
$vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = .\PS-ChildRunbook.ps1 –VM $vm –RepeatCount 2 –Restart $true
```

## <a name="starting-a-child-runbook-using-cmdlet"></a>A partir de um runbook subordinado com o cmdlet

Pode utilizar o [Start-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) cmdlet para iniciar um runbook, conforme descrito na [para iniciar um runbook com o Windows PowerShell](automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell). Existem dois modos de utilização para este cmdlet.  Num modo, o cmdlet devolve o id da tarefa, assim que a tarefa subordinada é criada para o runbook subordinado.  No outro modo, que permitem ao especificar que o **-aguardar** parâmetro, o cmdlet aguarda até que o filho da tarefa estiver concluída e irá devolver o resultado do runbook subordinado.

Será executada a tarefa de um runbook subordinado iniciado com um cmdlet parte da tarefa do runbook principal. Isso resulta em mais tarefas do que invocar o runbook inline e torna mais difícil de controlar. O principal pode iniciar vários runbooks subordinados forma assíncrona, sem aguardar a conclusão de cada. Para esse mesmo tipo de execução paralela de invocar os runbooks subordinados inline, o runbook principal seria necessário usar a [palavra-chave paralela](automation-powershell-workflow.md#parallel-processing).

A saída dos runbooks subordinados não são devolvidos para o runbook de principal de forma fiável devido a tempo. Também o determinadas variáveis, como a $VerbosePreference, $WarningPreference, e outras pessoas não podem ser propagadas para os runbooks subordinados. Para evitar estes problemas, pode invocar runbooks subordinados como separadas das tarefas de automatização com o `Start-AzureRmAutomationRunbook` cmdlet com o `-Wait` mudar. Isto impede o runbook principal até que o runbook subordinado seja concluído.

Se não pretender que o runbook principal até ser bloqueado em espera, pode invocar o runbook subordinado com `Start-AzureRmAutomationRunbook` cmdlet sem o `-Wait` mudar. Em seguida, seria necessário usar `Get-AzureRmAutomationJob` a aguardar pela conclusão da tarefa, e `Get-AzureRmAutomationJobOutput` e `Get-AzureRmAutomationJobOutputRecord` para recuperar os resultados.

Parâmetros para um runbook subordinado iniciado com um cmdlet são fornecidos como uma tabela de hash, conforme descrito em [parâmetros do Runbook](automation-starting-a-runbook.md#runbook-parameters). Apenas os tipos de dados simples podem ser utilizados. Se o runbook tem um parâmetro com um tipo de dados complexos, em seguida, tem de ser chamado inline.

Se a trabalhar com várias subscrições o contexto da subscrição poderão perder-se ao invocar runbooks subordinados. Para garantir que o contexto da subscrição é transferido para os runbooks subordinados, adicione o `DefaultProfile` parâmetro para o cmdlet e passe o contexto para o mesmo.

### <a name="example"></a>Exemplo

O exemplo seguinte inicia um runbook subordinado com parâmetros e, em seguida, aguarda que este seja concluído com o Start-AzureRmAutomationRunbook-parâmetro de espera. Depois de concluído, o resultado é recolhido do runbook subordinado. Para utilizar `Start-AzureRmAutomationRunbook`, tem de autenticar a sua subscrição do Azure.

```azurepowershell-interactive
# Connect to Azure with RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzureRmAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -DefaultProfile $AzureContext `
    –Parameters $params –wait
```

## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Comparação de métodos para chamar um runbook subordinado

A tabela seguinte resume as diferenças entre os dois métodos para chamar um runbook a partir de outro runbook.

|  | Inline | Cmdlet |
|:--- |:--- |:--- |
| Tarefa |Runbook subordinado é executado na mesma tarefa como pai. |Uma tarefa separada é criada para o runbook subordinado. |
| Execução |O runbook principal aguarda que o runbook subordinado seja concluído antes de continuar. |O runbook principal continua imediatamente após o início do runbook subordinado *ou* runbook principal aguarda o conclusão do trabalho filho. |
| Saída |O runbook principal pode obter resultados diretamente do runbook subordinado. |O runbook principal tem de obter o resultado da tarefa do runbook subordinado *ou* runbook principal pode obter resultados diretamente do runbook subordinado. |
| Parâmetros |Valores para parâmetros do runbook subordinado são especificados em separado e podem utilizar qualquer tipo de dados. |Valores para parâmetros do runbook subordinado tem de ser combinados numa única tabela de hash e só podem incluir simples, matriz, e essa serialização do JSON de tirar partido de tipos de dados de objetos. |
| Conta de Automatização |O runbook principal só pode utilizar o runbook subordinado na mesma conta de automatização. |O runbook principal pode utilizar o runbook subordinado a partir de qualquer conta de automatização da mesma subscrição do Azure e até mesmo uma subscrição diferente se tiver uma ligação à mesma. |
| Publicação |Runbook subordinado tem de ser publicado antes de o runbook principal. |Runbook subordinado tem de ser publicado em qualquer altura antes do runbook principal ser iniciado. |

## <a name="next-steps"></a>Passos Seguintes

* [Iniciar um runbook na automatização do Azure](automation-starting-a-runbook.md)
* [Resultado do Runbook e mensagens na automatização do Azure](automation-runbook-output-and-messages.md)
