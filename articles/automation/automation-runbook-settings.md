---
title: Definições de Runbook na automatização do Azure
description: Descreve as definições de configuração para um runbook na automatização do Azure e como alterá-las com o portal do Azure e o Windows PowerShell.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 589df59e02a18629d5f405ff1ce8870333f2228e
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397718"
---
# <a name="runbook-settings"></a>Definições de runbook
Cada runbook na automatização do Azure tem várias definições que os ajudam a ser identificados e alterar seu comportamento de Registro em log. Cada uma destas definições é descrita abaixo seguida pelos procedimentos sobre como modificá-los.

## <a name="settings"></a>Definições
### <a name="name-and-description"></a>Nome e descrição
Não é possível alterar o nome de um runbook depois de este ter sido criado. A descrição é opcional e pode ter até 512 carateres.

### <a name="tags"></a>Etiquetas
As etiquetas permitem atribuir palavras e expressões para ajudar a identificar um runbook diferentes. Por exemplo, quando submeter um runbook para o [galeria do PowerShell](https://www.powershellgallery.com/), especificar etiquetas específicas para identificar as categorias do runbook deve estar listado no. Pode especificar várias etiquetas para um runbook, separando-as com vírgulas.

### <a name="logging"></a>Registo
Por predefinição, os registos verboso e progresso não são escritos no histórico da tarefa. Pode alterar as definições de um determinado runbook para efetuar estes registos. Para obter mais informações sobre estes registos, consulte [Runbook Output and Messages](automation-runbook-output-and-messages.md).

## <a name="changing-runbook-settings"></a>Alterar definições de runbooks

### <a name="changing-runbook-settings-with-the-azure-portal"></a>Alterar definições de runbooks no portal do Azure
Pode alterar as definições de um runbook no portal do Azure a **definições** painel do runbook.

1. No portal do Azure, selecione **automatização** e, em seguida, clique no nome de uma conta de automatização.
2. Selecione o **Runbooks** separador.
3. Clique no nome de um runbook e será direcionado para o painel de definições para o runbook. A partir daqui pode especificar ou modificar etiquetas, a descrição do runbook, configurar o registo e definições de rastreio e aceder às ferramentas de suporte para o ajudar a resolver problemas.     

### <a name="changing-runbook-settings-with-windows-powershell"></a>Alterar definições de runbooks com o Windows PowerShell
Pode utilizar o [Set-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/set-azurermautomationrunbook) cmdlet para alterar as definições de um runbook. Se quiser especificar várias etiquetas, pode fornecer uma matriz ou uma única cadeia de caracteres com valores separado por vírgulas para o parâmetro de etiquetas. Pode obter as etiquetas atuais com o [Get-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/get-azurermautomationrunbook).

Os comandos de exemplo seguintes mostram como definir as propriedades de um runbook. Este exemplo adiciona três etiquetas para as etiquetas existentes e especifica que registos verbosos devem ter sessão iniciados.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="next-steps"></a>Passos Seguintes
* Para saber como criar e recuperar mensagens de erro e de saída a partir de runbooks, consulte o artigo [Runbook Output and Messages](automation-runbook-output-and-messages.md) 
* Para saber como adicionar um runbook que já foi desenvolvido pela Comunidade ou outra origem ou para criar seus próprios runbook, consulte [criar ou importar um Runbook](automation-creating-importing-runbook.md) 

