---
title: Automatizar a remoção de grupos de recursos com a automatização do Azure
description: Versão do Fluxo de Trabalho do PowerShell de um cenário da Automatização do Azure, incluindo runbooks, para remover todos os grupos de recursos da sua subscrição.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: c8fcb7abbc28aef06c43b6ece89eb7bd6916694e
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
---
# <a name="azure-automation-scenario---automate-removal-of-resource-groups"></a>Cenário da Automatização do Azure – remoção automática de grupos de recursos
Muitos clientes criam mais do que um grupo de recursos. Alguns podem ser utilizados para gerir aplicações de produção, ao passo que outros podem ser utilizados como ambientes de desenvolvimento e de teste. Automatizar a implementação destes recursos é uma coisa, mas conseguir desativar um grupo de recursos com o clique de um botão é outra. Pode simplificar esta tarefa de gestão comum através da Automatização do Azure. Isto é útil se estiver a trabalhar com uma subscrição do Azure que tenha um limite de gastos através de uma oferta de membro, como o MSDN ou o programa Microsoft Partner Network Cloud Essentials.

Este cenário baseia-se num runbook do PowerShell e foi concebido para remover um ou mais grupos de recursos que especificar da sua subscrição. A predefinição do runbook é testar antes de continuar. Isto garante que não eliminará o grupo de recursos acidentalmente antes de estar pronto para concluir este procedimento.   

## <a name="getting-the-scenario"></a>Obter o cenário
Este cenário é constituído por um runbook do PowerShell que pode transferir a partir da [Galeria do PowerShell](https://www.powershellgallery.com/packages/Remove-ResourceGroup/1.0/DisplayScript). Também pode importá-lo diretamente a partir da [Galeria de Runbooks](automation-runbook-gallery.md) no portal do Azure.<br><br>

| Runbook | Descrição |
| --- | --- |
| Remove-ResourceGroup |Remove um ou mais grupos de recursos do Azure e os recursos associados da subscrição. |

<br>
São definidos para este runbook os seguintes parâmetros de entrada:

| Parâmetro | Descrição |
| --- | --- |
| NameFilter (obrigatório) |Especifica um filtro de nomes para limitar os grupos de recursos que planeia eliminar. Pode utilizar uma lista separada por vírgulas para transmitir vários valores.<br>O filtro não é sensível e corresponde a qualquer grupo de recursos que contenha a cadeia. |
| PreviewMode (opcional) |Executa o runbook para ver que grupos de recursos seriam eliminados, mas sem realizar ações.<br>A predefinição é **verdadeiro**, para ajudar a evitar a eliminação acidental de um ou mais grupos de recursos transmitidos para o runbook. |

## <a name="install-and-configure-this-scenario"></a>Instalar e configurar este cenário
### <a name="prerequisites"></a>Pré-requisitos
Este runbook utiliza a [conta Run As do Azure](automation-sec-configure-azure-runas-account.md) para se autenticar.    

### <a name="install-and-publish-the-runbooks"></a>Instalar e publicar os runbooks
Depois de transferir o runbook, pode importá-lo ao seguir o procedimento em [Procedimentos para importar runbooks](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation). Publique o runbook após ter sido importado com êxito para a sua conta da Automatização.

## <a name="using-the-runbook"></a>Utilizar o runbook
Os seguintes passos guiá-lo a execução deste runbook e ajuda a que se familiarize com como funciona. Está a testar o runbook neste exemplo, não realmente eliminar o grupo de recursos.  

1. No portal do Azure, abra a sua conta de Automatização e clique em **Runbooks**.
2. Selecione o runbook **Remove-ResourceGroup** e clique em **Iniciar**.
3. Quando iniciar o runbook, o **iniciar Runbook** página abre e pode configurar os parâmetros. Introduza os nomes dos grupos de recursos na sua subscrição, que pode utilizar para fins de teste e faz com que não existem perigo se acidentalmente eliminado.

   > [!NOTE]
   > Certifique-se de que a opção **Previewmode** está definida como **verdadeiro** para evitar eliminar os grupos de recursos selecionados. Este runbook não remover o grupo de recursos que contém a conta de automatização que esteja a executar este runbook.  
   >
   >
1. Depois de configurar todos os valores do parâmetro, clique em **OK** e o runbook será adicionado à fila para ser executado.  

Para ver os detalhes do **remover ResourceGroup** tarefa de runbook no portal do Azure, em **recursos**, selecione **tarefas** no runbook. Selecione, a tarefa que pretende visualizar. O resumo da tarefa apresenta os parâmetros de entrada e o fluxo de saída, para além de informações gerais sobre a tarefa e eventuais exceções que possam ter ocorrido.<br> ![Estado da tarefa do runbook Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-status.png).

O **Resumo da Tarefa** inclui mensagens dos fluxos de saída, aviso e erro. Selecione **Saída** para ver resultados detalhados da execução do runbook.<br> ![Resultados da saída do runbook Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-output.png)

## <a name="next-steps"></a>Passos Seguintes
* Para começar a criar o seu próprio runbook, veja [Criar ou importar um runbook na Automatização do Azure](automation-creating-importing-runbook.md).
* Para começar a utilizar runbooks do Fluxo de Trabalho do PowerShell, veja [O meu primeiro runbook do Fluxo de Trabalho do PowerShell](automation-first-runbook-textual.md).
