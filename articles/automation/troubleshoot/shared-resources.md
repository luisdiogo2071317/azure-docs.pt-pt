---
title: Resolver problemas de erros com recursos de automatização do Azure partilhados
description: Aprenda a solucionar problemas com os recursos de automatização do Azure partilhados
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 11/05/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 385d2969e65647ab0b5c5e21c07b127104587e7e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263562"
---
# <a name="troubleshoot-errors-with-shared-resources"></a>Resolver problemas de erros com recursos partilhados

Este artigo aborda soluções para resolver problemas que pode encontrar ao utilizar os recursos partilhados na automatização do Azure.

## <a name="modules"></a>Módulos

### <a name="module-stuck-importing"></a>Cenário: Um módulo está preso a importar

#### <a name="issue"></a>Problema

Quando importa ou atualizar os módulos na automatização do Azure, encontrar um módulo que fica preso no **importando** estado.

#### <a name="error"></a>Erro

Importar os módulos do PowerShell é um processo complexo de vários passo. Este processo introduz a possibilidade de um módulo não importar corretamente. Se isto ocorrer, o módulo que está a importar pode estar bloqueado num estado transitório. Para saber mais sobre este processo, veja [importar um módulo do PowerShell]( /powershell/developer/module/importing-a-powershell-module#the-importing-process).

#### <a name="resolution"></a>Resolução

Para resolver este problema, tem de remover o módulo que fica preso no **importando** Estado, utilizando o [Remove-AzureRmAutomationModule](/powershell/module/azurerm.automation/remove-azurermautomationmodule) cmdlet. Pode, em seguida, repita a importação do módulo.

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name ModuleName -ResourceGroupName ExampleResourceGroup -AutomationAccountName ExampleAutomationAccount -Force
```

## <a name="next-steps"></a>Passos Seguintes

Se não vir o seu problema ou não é possível resolver o problema, visite um dos seguintes canais de suporte mais:

* Obtenha respostas de especialistas do Azure através dos [fóruns do Azure](https://azure.microsoft.com/support/forums/)
* Ligue-se a [@AzureSupport](https://twitter.com/azuresupport) – a conta oficial do Microsoft Azure para melhorar a experiência do cliente ao ligar a comunidade do Azure aos recursos certos: respostas, suporte e especialistas.
* Se precisar de mais ajuda, pode enviar um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporte**.