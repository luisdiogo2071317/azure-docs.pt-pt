---
title: "Criar uma tarefa de observador na conta de automatização do Azure | Microsoft Docs"
description: "Saiba como criar uma tarefa de observador na conta de automatização do Azure para ver para novos ficheiros criados numa pasta."
services: automation
documentationcenter: 
author: eamonoreilly
manager: 
editor: 
ms.assetid: 0dd95270-761f-448e-af48-c8b1e82cd821
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/11/2017
ms.author: eamono
ms.openlocfilehash: 294faa48f9840919b087594835706bad8048d45b
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/22/2017
---
# <a name="create-an-azure-automation-watcher-tasks-to-track-file-changes-on-a-local-machine"></a>Criar um observador da automatização do Azure tarefas para controlar as alterações ao ficheiro num computador local

A automatização do Azure utiliza tarefas do observador para ver eventos e a acionar ações. Este tutorial explica como criar uma tarefa de observador para monitorizar quando é adicionado um novo ficheiro para um diretório.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Importar um runbook de observador
> * Criar uma variável de automatização
> * Criar um runbook de ação
> * Criar uma tarefa de observador
> * Acionar um observador do
> * Verifique os resultados

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, é necessário o seguinte:

* Subscrição do Azure. Se ainda não tiver uma, pode [ativar as vantagens de subscritor do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ou [inscrever-se numa conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Conta de automatização](automation-offering-get-started.md) para reter os runbooks de observador e a ação e a tarefa de observador.
* A [runbook worker híbrido](automation-hybrid-runbook-worker.md) onde é executada a tarefa de observador.

## <a name="import-a-watcher-runbook"></a>Importar um runbook de observador

Este tutorial utiliza um runbook de observador denominado **veja NewFile** para procurar novos ficheiros num diretório. O runbook de observador obtém a última escrita conhecidos para os ficheiros numa pasta e analisa todos os ficheiros mais recentes que esse marca de água. Neste passo, é possível importar este runbook na sua conta de automatização.

1. Abra a sua conta de automatização e clique em de **Runbooks** página.
1. Clique em de **procurar galeria** botão.
1. Procure "Observador runbook", selecione **runbook de observador que procura novos ficheiros num diretório** e selecione **importação**.
  ![Importar runbook da automatização a partir da IU](media/automation-watchers-tutorial/importsourcewatcher.png)
1. Atribua ao runbook, um nome e descrição e selecione **OK** para importar o runbook para a sua conta de automatização.
1. Selecione **editar** e, em seguida, clique em **publicar**. Quando lhe for pedido selecione **Sim** para publicar o runbook.

## <a name="create-an-automation-variable"></a>Criar uma variável de automatização

Um [variável de automatização](automation-variables.md) é utilizado para armazenar os carimbos que o runbook anterior lê e armazena de cada ficheiro. 

1. Selecione **variáveis** em **recursos PARTILHADOS** e selecione **+ adicionar uma variável**.
1. Introduza "NewFileTimestamp veja" para o nome
1. Selecione o DateTime de tipo.
1. Clique em de **criar** botão. Esta ação cria a variável de automatização.

## <a name="create-an-action-runbook"></a>Criar um runbook de ação

Um runbook de ação é utilizado numa tarefa de observador para agir sobre dados transmitidos ao mesmo a partir de um runbook de observador. Neste passo, atualizar importar um runbook de ação predefinido denominado "NewFile de processo".

1. Navegue até à sua conta de automatização e selecione **Runbooks** sob o **AUTOMATIZAÇÃO de processos** categoria.
1. Clique em de **procurar galeria** botão.
1. Procure "Action observador" e selecione **ação de observador que processa os eventos acionados por um runbook de observador** e selecione **importação**.
  ![Importar runbook da ação de IU](media/automation-watchers-tutorial/importsourceaction.png)
1. Atribua ao runbook, um nome e descrição e selecione **OK** para importar o runbook para a sua conta de automatização.
1. Selecione **editar** e, em seguida, clique em **publicar**. Quando lhe for pedido selecione **Sim** para publicar o runbook.

## <a name="create-a-watcher-task"></a>Criar uma tarefa de observador

A tarefa de observador contém duas partes. O observador e a ação. O observador é executada num intervalo definido na tarefa de observador. Dados do runbook observador são transmitidos para o runbook de ação. Neste passo, configure a tarefa de observador referenciar os runbooks de observador e a ação definidos nos passos anteriores.

1. Navegue até à sua conta de automatização e selecione **tarefas do observador do** sob o **AUTOMATIZAÇÃO de processos** categoria.
1. Selecione a página de tarefas do observador e clique em **+ adicionar uma tarefa de observador** botão.
1. Introduza "WatchMyFolder" como o nome.

1. Selecione **configurar observador** e selecione o **veja NewFile** runbook.

1. Introduza os seguintes valores para os parâmetros:

   * **FOLDERPATH** -uma pasta na função de trabalho híbrida onde obterem criados novos ficheiros. d:\examplefiles
   * **EXTENSÃO** -deixe em branco para processar todas as extensões de ficheiro.
   * **RECURSE** -deixe este valor como predefinição.
   * **DEFINIÇÕES de execução** -escolha o worker híbrido.

1. Clique em OK e, em seguida, selecione para regressar à página de observador.
1. Selecione **configurar ação** e selecione o runbook "NewFile de processo".
1. Introduza os seguintes valores para parâmetros:

   *    **EVENTDATA** -deixar em branco. Dados seja transmitidos a partir do runbook de observador.  
   *    **Definições de execução** -deixe como a Azure como este runbook é executado no serviço de automatização.

1. Clique em **OK**e, em seguida, selecione para regressar à página de observador.
1. Clique em **OK** para criar a tarefa de observador.

![Configure a ação de observador a partir da IU](media/automation-watchers-tutorial/watchertaskcreation.png)

## <a name="trigger-a-watcher"></a>Acionar um observador do

Para testar o observador está a funcionar conforme esperado, tem de criar um ficheiro de teste.

Remoto para o worker híbrido. Abra **PowerShell** e criar um ficheiro de teste na pasta.
  
   ```PowerShell-interactive
   New-Item -Name ExampleFile1.txt
   ```

O exemplo seguinte mostra o resultado esperado.

```
    Directory: D:\examplefiles


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       12/11/2017   9:05 PM              0 ExampleFile1.txt
```

## <a name="inspect-the-output"></a>Verifique os resultados

1. Navegue até à sua conta de automatização e selecione **tarefas do observador do** sob o **AUTOMATIZAÇÃO de processos** categoria.
1. Selecione a tarefa de observador "WatchMyFolder".
1. Clique em **ver fluxos observador** em **fluxos** que o observador encontrado o novo ficheiro e iniciar o runbook de ação.
1. Para ver as tarefas de runbook de ação, clique no **ver tarefas de ação de observador**. Cada tarefa pode ser selecionado a vista de detalhes da tarefa.

   ![Tarefas de ação de observador da IU](media/automation-watchers-tutorial/WatcherActionJobs.png)

O resultado esperado quando encontra-se o novo ficheiro pode ser visto no exemplo seguinte:

```
Message is Process new file...



Passed in data is @{FileName=D:\examplefiles\ExampleFile1.txt; Length=0}
```

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Importar um runbook de observador
> * Criar uma variável de automatização
> * Criar um runbook de ação
> * Criar uma tarefa de observador
> * Acionar um observador do
> * Verifique os resultados

Siga esta ligação para obter mais informações sobre a criação do seu próprio runbook.

> [!div class="nextstepaction"]
> [O meu primeiro runbook do PowerShell](automation-first-runbook-textual-powershell.md).
