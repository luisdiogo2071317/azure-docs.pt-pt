---
title: Testar um runbook na automatização do Azure
description: Antes de publicar um runbook na automatização do Azure, pode testar de modo a garantir que funciona conforme esperado.  Este artigo descreve como testar um runbook e vê o resultado.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 95e3f4426fab8ed3ff28877607dee8694962e79f
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54422478"
---
# <a name="testing-a-runbook-in-azure-automation"></a>Testar um runbook na automatização do Azure
Quando testa um runbook, o [versão de rascunho](automation-creating-importing-runbook.md#publishing-a-runbook) é executado e quaisquer ações que executar são concluídas. Nenhum histórico da tarefa é criado, mas a [saída](automation-runbook-output-and-messages.md#output-stream) e [aviso e erro](automation-runbook-output-and-messages.md#message-streams) fluxos são apresentados no teste de painel de saída. Mensagens para o [Stream verboso](automation-runbook-output-and-messages.md#message-streams) são apresentados no painel de resultados apenas se for o [$VerbosePreference variável](automation-runbook-output-and-messages.md#preference-variables) está definido para continuar.

Mesmo que a versão de rascunho está em execução, o runbook executa o fluxo de trabalho normalmente ainda e efetua quaisquer ações sobre recursos no ambiente. Por esse motivo, deve testar apenas runbooks em recursos de não produção.

O procedimento para testar cada [tipo de runbook](automation-runbook-types.md) é o mesmo e há não são nenhuma diferença no teste entre o editor de texto e o editor gráfico no portal do Azure.  

## <a name="to-test-a-runbook-in-the-azure-portal"></a>Para testar um runbook no portal do Azure
Pode trabalhar com qualquer [tipo de runbook](automation-runbook-types.md) no portal do Azure.

1. Abra a versão de rascunho do runbook em ambos os [editor de texto](automation-edit-textual-runbook.md) ou [editor gráfico](automation-graphical-authoring-intro.md).
2. Clique nas **teste** botão para abrir o painel de teste.
3. Se o runbook tiver parâmetros, estes serão apresentados no painel esquerdo, onde pode fornecer valores para ser utilizado para o teste.
4. Se quiser executar o teste num [Runbook Worker híbrido](automation-hybrid-runbook-worker.md), em seguida, altere **configurações de execução** para **função de trabalho híbrida** e selecione o nome do grupo de destino.  Caso contrário, mantenha a predefinição **Azure** para executar o teste na cloud.
5. Clique nas **iniciar** botão para iniciar o teste.
6. Se o runbook estiver [fluxo de trabalho do PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) ou [gráfico](automation-runbook-types.md#graphical-runbooks), em seguida, pode parar ou suspendê-lo enquanto ele está sendo testado com os botões por baixo do painel de resultados. Quando suspende o runbook, conclui a atividade atual antes de ser suspenso. Depois do runbook está suspenso, pode pará-lo ou reiniciá-lo.
7. Verificar a saída do runbook no painel de saída.

## <a name="next-steps"></a>Próximos Passos
* Para saber como criar ou importar um runbook, consulte [criar ou importar um runbook na automatização do Azure](automation-creating-importing-runbook.md)
* Para obter mais informações sobre a Criação de Gráficos, consulte [Criação de gráficos na Automatização do Azure](automation-graphical-authoring-intro.md)
* Para começar com runbooks do fluxo de trabalho do PowerShell, consulte o artigo [O meu primeiro runbook do fluxo de trabalho do PowerShell](automation-first-runbook-textual.md)
* Para saber mais sobre como configurar a devolver erros e mensagens de estado de runbooks, incluindo recomendado práticas, consulte [Runbook resultados e mensagens na automatização do Azure](automation-runbook-output-and-messages.md)


