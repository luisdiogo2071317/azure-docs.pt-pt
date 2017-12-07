---
title: "Integração da automatização do Azure com grelha de eventos | Microsoft Docs"
description: "Saiba como adicionar automaticamente uma etiqueta quando é criada uma nova VM e enviar uma notificação para Teams da Microsoft."
keywords: "automatização runbook, as equipas, grelha de eventos, a máquina virtual, VM"
services: automation
documentationcenter: 
author: eamonoreilly
manager: 
editor: 
ms.service: automation
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/06/2017
ms.author: eamono
ms.openlocfilehash: 9a4d6ecf19fc96a9c7b92cf246effbf3948fb478
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2017
---
# <a name="integrate-azure-automation-with-event-grid-and-microsoft-teams"></a>Integração da automatização do Azure com grelha de eventos e equipas da Microsoft

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Importe um runbook de exemplo da grelha de eventos.
> * Crie um webhook de Microsoft Teams opcional.
> * Crie um webhook para o runbook.
> * Crie uma subscrição de evento grelha.
> * Crie uma VM que aciona o runbook.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, um [conta de automatização do Azure](../automation/automation-offering-get-started.md) é necessário para manter o runbook que é acionado da subscrição do Azure eventos grelha.

## <a name="import-an-event-grid-sample-runbook"></a>Importar um runbook de exemplo da grelha de eventos
1. Selecione a sua conta de automatização e selecione o **Runbooks** página.

   ![Selecione os runbooks](./media/ensure-tags-exists-on-new-virtual-machines/select-runbooks.png)

2. Selecione o **procurar galeria** botão.

3. Procurar **eventos grelha**e selecione **integração da automatização do Azure com a grelha de evento**. 

    ![Importar runbook da Galeria](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)

4. Selecione **importação** e dê-lhe nome **veja VMWrite**.

5. Depois de ter importado, selecione **editar** para ver a origem de runbook. Selecione o botão **Publicar**.

## <a name="create-an-optional-microsoft-teams-webhook"></a>Criar um webhook de Microsoft Teams opcional
1. Na Microsoft Teams, selecione **mais opções** seguinte para o nome do canal e, em seguida, selecione **conectores**.

    ![Ligações de Teams da Microsoft](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)

2. Desloque-se através da lista de conectores para **Webhook entrada**e selecione **adicionar**.

3. Introduza **AzureAutomationIntegration** para o nome e selecione **criar**.

4. Copie o webhook para a área de transferência e guardá-lo. O URL do webhook é utilizado para enviar informações para Teams da Microsoft.

5. Selecione **feito** para guardar o webhook.

## <a name="create-a-webhook-for-the-runbook"></a>Criar um webhook para o runbook
1. Abra o runbook veja VMWrite.

2. Selecione **Webhooks**e selecione o **adicionar Webhook** botão.

3. Introduza **WatchVMEventGrid** para o nome. Copie o URL para a área de transferência e guardá-lo.

    ![Configurar o nome de webhook](media/ensure-tags-exists-on-new-virtual-machines/copy-url.png)

4. Selecione **configurar parâmetros e definições de execução**e introduza o URL do webhook Teams da Microsoft para **CHANNELURL**. Deixe **WEBHOOKDATA** em branco.

    ![Configurar parâmetros do webhook](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)

5. Selecione **OK** para criar o webhook de runbook de automatização.


## <a name="create-an-event-grid-subscription"></a>Criar uma subscrição de evento grelha
1. No **conta de automatização** página Descrição geral, selecione **grelha de evento**.

    ![Selecione a grelha de eventos](media/ensure-tags-exists-on-new-virtual-machines/select-event-grid.png)

2. Selecione o **+ de subscrição de evento** botão.

3. Configure a subscrição com as seguintes informações:

    *   Introduza **AzureAutomation** para o nome.
    *   No **tipo de tópico**, selecione **subscrições do Azure**.
    *   Limpar o **subscrever todos os tipos de evento** caixa de verificação.
    *   No **tipos de evento**, selecione **recursos escrever êxito**.
    *   No **ponto final de subscritor**, introduza o URL do webhook para o runbook veja VMWrite.
    *   No **prefixo filtro**, introduza a subscrição e criado de grupo de recursos em que pretende procurar novas VMs. Este deve ter o seguinte aspeto:`/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines`

4. Selecione **criar** ao guardar a subscrição de evento grelha.

## <a name="create-a-vm-that-triggers-the-runbook"></a>Criar uma VM que aciona o runbook
1. Crie uma nova VM no grupo de recursos que especificou no caso de filtro do prefixo da subscrição de grelha.

2. Deve ser chamado para o runbook VMWrite veja e adicionar uma nova etiqueta para a VM.

    ![Etiqueta VM](media/ensure-tags-exists-on-new-virtual-machines/vm-tag.png)

3. Uma nova mensagem é enviada para o canal Teams da Microsoft.

    ![Notificação de Teams da Microsoft](media/ensure-tags-exists-on-new-virtual-machines/teams-vm-message.png)

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, configurar a integração entre a grelha de eventos e automatização. Aprendeu a:

> [!div class="checklist"]
> * Importe um runbook de exemplo da grelha de eventos.
> * Crie um webhook de Microsoft Teams opcional.
> * Crie um webhook para o runbook.
> * Crie uma subscrição de evento grelha.
> * Crie uma VM que aciona o runbook.

> [!div class="nextstepaction"]
> [Criar e eventos personalizados com grelha de eventos de rotas](../event-grid/custom-event-quickstart.md)
