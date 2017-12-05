---
title: "Integração da automatização do Azure com grelha de eventos | Microsoft Docs"
description: "Saiba como adicionar automaticamente uma etiqueta quando é criada uma nova VM e enviar uma notificação para Teams da Microsoft."
keywords: "automatização runbook, as equipas, grelha de eventos, a máquina virtual, VM"
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
ms.date: 11/28/2017
ms.author: eamono
ms.openlocfilehash: 8b698659ed91782b80dbefbfea02aa036c09210d
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2017
---
# <a name="integrate-azure-automation-with-event-grid-and-microsoft-teams"></a>Integração da automatização do Azure com grelha de eventos e equipas da Microsoft

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Importe um runbook de exemplo da grelha de eventos.
> * Crie um webhook de Microsoft Teams opcional.
> * Crie um webhook para o runbook.
> * Crie uma subscrição de evento grelha.
> * Crie uma VM que aciona o runbook.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, um [conta de automatização do Azure](../automation/automation-offering-get-started.md) é necessário para manter o runbook que é acionado da subscrição do Azure eventos grelha.

## <a name="import-an-event-grid-sample-runbook"></a>Importar um runbook de exemplo da grelha de eventos
1. Selecione **as contas de automatização**e selecione o **Runbooks** página.

2. Selecione o **procurar galeria** botão.

    ![Lista de Runbook a partir da IU](media/ensure-tags-exists-on-new-virtual-machines/event-grid-runbook-list.png)

3. Procurar **eventos grelha**e importe-o runbook para a conta de automatização.

    ![Importar runbook da Galeria](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)

4. Selecione **editar** para ver a origem de runbook e selecionar o **publicar** botão.

    ![Publicar o runbook a partir da IU](media/ensure-tags-exists-on-new-virtual-machines/publish-runbook.png)

## <a name="create-an-optional-microsoft-teams-webhook"></a>Criar um webhook de Microsoft Teams opcional
1. Na Microsoft Teams, selecione **mais opções** seguinte para o nome do canal e, em seguida, selecione **conectores**.

    ![Ligações de Teams da Microsoft](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)

2. Desloque-se através da lista de conectores para **Webhook entrada**e selecione **adicionar**.

    ![Ligação de webhook Teams da Microsoft](media/ensure-tags-exists-on-new-virtual-machines/select-teams-webhook.png)

3. Introduza **AzureAutomationIntegration** para o nome e selecione **criar**.

    ![Webhook Teams da Microsoft](media/ensure-tags-exists-on-new-virtual-machines/configure-teams-webhook.png)

4. Copie o webhook para a área de transferência e guardá-lo. O URL do webhook é utilizado para enviar informações para Teams da Microsoft.

5. Selecione **feito** para guardar o webhook.

## <a name="create-a-webhook-for-the-runbook"></a>Criar um webhook para o runbook
1. Abra o runbook veja VMWrite.

2. Selecione **Webhooks**e selecione o **adicionar Webhook** botão.

    ![Criar webhook](media/ensure-tags-exists-on-new-virtual-machines/add-webhook.png)

3. Introduza **WatchVMEventGrid** para o nome. Copie o URL para a área de transferência e guardá-lo.

    ![Configurar o nome de webhook](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-name.png)

4. Selecione **parâmetros e definições de execução**e introduza o URL do webhook Teams da Microsoft. Deixe **WEBHOOKDATA** em branco.

    ![Configurar parâmetros do webhook](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)

5. Selecione **OK** para criar o webhook de runbook de automatização.


## <a name="create-an-event-grid-subscription"></a>Criar uma subscrição de evento grelha
1. No **conta de automatização** página Descrição geral, selecione **grelha de evento**.

    ![Lista de grelha de eventos](media/ensure-tags-exists-on-new-virtual-machines/event-grid-list.png)

2. Selecione o **de subscrição de evento** botão.

3. Configure a subscrição com as seguintes informações:

    *   Introduza **AzureAutomation** para o nome. 
    *   No **tipo de tópico**, selecione **subscrições do Azure**.
    *   Limpar o **subscrever todos os tipos de evento** caixa de verificação.
    *   No **tipos de evento**, selecione **recursos escrever êxito**.
    *   No **URL de ponto final completo**, introduza o URL do webhook para o runbook veja VMWrite.
    *   No **prefixo filtro**, introduza a subscrição e criado de grupo de recursos em que pretende procurar novas VMs. Este deve ter o seguinte aspeto /subscriptions/124aa551-849d-46e4-a6dc-0bc4895422aB/resourcegroups/ContosoResourceGroup/providers/Microsoft.Compute/virtualMachines

    ![Lista de grelha de eventos](media/ensure-tags-exists-on-new-virtual-machines/configure-event-grid-subscription.png)

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
