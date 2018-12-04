---
title: Resolver problemas de erros com recursos de automatização do Azure partilhados
description: Aprenda a solucionar problemas com os recursos de automatização do Azure partilhados
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 12/3/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: ce78c86cdae9a06100fd17d00e0229805e42983b
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52848464"
---
# <a name="troubleshoot-errors-with-shared-resources"></a>Resolver problemas de erros com recursos partilhados

Este artigo aborda soluções para resolver problemas que pode encontrar ao utilizar os recursos partilhados na automatização do Azure.

## <a name="modules"></a>Módulos

### <a name="module-stuck-importing"></a>Cenário: Um módulo está preso a importar

#### <a name="issue"></a>Problema

Um módulo fica preso no **importando** estado quando importa ou atualizar seus módulos na automatização do Azure.

#### <a name="cause"></a>Causa

Importar os módulos do PowerShell é um processo complexo de vários passo. Este processo introduz a possibilidade de um módulo não importar corretamente. Se este problema ocorrer, o módulo que está a importar pode estar bloqueado num estado transitório. Para saber mais sobre este processo, veja [importar um módulo do PowerShell]( /powershell/developer/module/importing-a-powershell-module#the-importing-process).

#### <a name="resolution"></a>Resolução

Para resolver este problema, tem de remover o módulo que fica preso no **importando** Estado, utilizando o [Remove-AzureRmAutomationModule](/powershell/module/azurerm.automation/remove-azurermautomationmodule) cmdlet. Pode, em seguida, repita a importação do módulo.

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name ModuleName -ResourceGroupName ExampleResourceGroup -AutomationAccountName ExampleAutomationAccount -Force
```

## <a name="run-as-accounts"></a>Contas Run as

### <a name="unable-create-update"></a>Cenário: Não for possível criar ou atualizar uma conta Run As

#### <a name="issue"></a>Problema

Ao tentar criar ou atualizar uma conta Run As, receberá um erro semelhante a seguinte mensagem de erro:

```error
You do not have permissions to create…
```

#### <a name="cause"></a>Causa

Não tem as permissões que necessita criar ou atualizar a conta Run As ou o recurso está bloqueado num nível de grupo de recursos.

#### <a name="resolution"></a>Resolução

Para criar ou atualizar uma conta Run As, tem de ter permissões adequadas para os vários recursos utilizados pela conta Run As. Para saber mais sobre as permissões necessárias para criar ou atualizar uma conta Run As, consulte [permissões de conta Run As](../manage-runas-account.md#permissions).

Se o problema é devido a um bloqueio, certifique-se de que o bloqueio está ok para remover e navegue para o recurso que está bloqueado, o bloqueio com o botão direito e escolher **eliminar** ao remover o bloqueio.

## <a name="next-steps"></a>Passos Seguintes

Se não vir o seu problema ou não é possível resolver o problema, visite um dos seguintes canais de suporte mais:

* Obtenha respostas de especialistas do Azure através dos [fóruns do Azure](https://azure.microsoft.com/support/forums/)
* Ligue-se a [@AzureSupport](https://twitter.com/azuresupport) – a conta oficial do Microsoft Azure para melhorar a experiência do cliente ao ligar a comunidade do Azure aos recursos certos: respostas, suporte e especialistas.
* Se precisar de mais ajuda, pode enviar um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione **obter suporte**.