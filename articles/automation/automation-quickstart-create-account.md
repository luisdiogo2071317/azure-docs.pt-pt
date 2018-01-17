---
title: "Início Rápido do Azure - Criar uma conta de Automatização do Azure | Microsoft Docs"
description: "Saiba como criar uma conta de Automatização do Azure e executar um runbook"
services: automation
author: csand-msft
ms.author: csand
ms.date: 12/13/2017
ms.topic: quickstart
ms.service: automation
ms.custom: mvc
ms.openlocfilehash: 5522c56749f9c26f376661636dd62f88017f805e
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="create-an-azure-automation-account"></a>Crie uma conta de Automatização do Azure

As contas de Automatização do Azure podem ser criadas através do Azure. Este método fornece uma interface de utilizador baseada no browser para criar e configurar contas de Automatização e os recursos relacionados. Este início rápido analisa a criação de uma conta de Automatização e a execução um runbook na conta.

Se não tiver uma subscrição do Azure, crie uma [conta do Azure gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Azure em https://portal.azure.com

## <a name="create-automation-account"></a>Criar conta de Automatização

1. Clique no botão **Novo** localizado no canto superior esquerdo do Azure.

1. Selecione **Monitorização + Gestão**e, em seguida, selecione **Automatização**.

1. Introduza as informações da conta. Para **Criar uma conta Run As do Azure**, escolha **Sim** para que os artefactos para simplificar a autenticação no Azure sejam ativados automaticamente. Quando terminar, clique em **Criar**, para iniciar a implementação da conta de Automatização.

    ![Introduza as informações sobre a conta de Automatização na página](./media/automation-quickstart-create-account/create-automation-account-portal-blade.png)  

1. A conta de Automatização é afixada ao dashboard do Azure. Quando concluída a implementação, a descrição geral de conta de Automatização é aberta automaticamente.

    ![Descrição geral da conta de Automatização](./media/automation-quickstart-create-account/automation-account-overview.png)

## <a name="run-a-runbook"></a>Executar um runbook

Execute um dos runbooks do tutorial.

1. Clique em **Runbooks** em **AUTOMATIZAÇÃO DE PROCESSOS**. A lista de runbooks é apresentada. Vários runbooks do tutorial são ativados por predefinição na conta.

    ![Lista de runbooks da conta de Automatização](./media/automation-quickstart-create-account/automation-runbooks-overview.png)

1. Selecione o runbook **AzureAutomationTutorialScript**. Esta ação abre a página de descrição geral do runbook.

    ![Descrição geral do runbook](./media/automation-quickstart-create-account/automation-tutorial-script-runbook-overview.png)

1. Clique em **Iniciar**e na página **Iniciar Runbook**, clique em **OK** para iniciar o runbook.

    ![Página de tarefa do runbook](./media/automation-quickstart-create-account/automation-tutorial-script-job.png)

1. Depois do **Estado da tarefa** passar a **Em execução**, clique em **Resultado** ou **Todos os Registos** para ver o resultado da tarefa de runbook. Para este runbook do tutorial, o resultado é uma lista dos seus recursos Azure.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o grupo de recursos, a conta de Automatização e todos os recursos relacionados. Para fazê-lo, selecione o grupo de recursos para a conta de Automatização e clique em **Eliminar**.

## <a name="next-steps"></a>Passos seguintes

Neste guia de introdução, implementou uma conta de Automatização, iniciou uma tarefa de runbook e visualizou os resultados das tarefas. Para saber mais sobre a Automatização do Azure, avance para o início rápido para criar o seu primeiro runbook.

> [!div class="nextstepaction"]
> [Guia de Introdução à Automatização - Criar Runbook](./automation-quickstart-create-runbook.md)
