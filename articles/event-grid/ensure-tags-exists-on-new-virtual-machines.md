---
title: "Integração da automatização do Azure com o evento grelha | Microsoft Docs"
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
ms.openlocfilehash: 6798f98755ad1d70d316b074643700f7b3e25ee7
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/02/2017
---
# <a name="integrating-azure-automation-with-event-grid-and-microsoft-teams"></a>Integração da automatização do Azure com a grelha de eventos e equipas da Microsoft

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Importe o runbook de exemplo de grelha de eventos.
> * Crie um webhook de equipas opcional.
> * Crie um webhook para o runbook.
> * Crie uma subscrição de grelha de eventos.
> * Crie a VM que aciona o runbook.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, é necessário o seguinte.
+ [Conta de automatização](../automation/automation-offering-get-started.md) para manter o runbook que é acionado da subscrição da grelha de eventos.

## <a name="import-event-grid-sample-runbook"></a>Importar o runbook de exemplo de grelha de eventos
1.  Abra a conta de automatização e clique na página de Runbooks.
2.  Clique no botão "Procurar Galeria".
![Lista de Runbook a partir da IU](media/ensure-tags-exists-on-new-virtual-machines/event-grid-runbook-list.png)
3.  Pesquise "Grelha de evento" e importar o runbook para a conta de automatização.
![Importar runbook da Galeria](media/ensure-tags-exists-on-new-virtual-machines/gallery-event-grid.png)
4.  Clique em "Editar" para ver a origem de Runbook e clique no botão "Publicar".
![Publicar o runbook a partir da IU](media/ensure-tags-exists-on-new-virtual-machines/publish-runbook.png)

## <a name="create-an-optional-teams-webhook"></a>Criar um webhook de equipas opcional
1.  Na Microsoft Teams, escolha mais opções (…) junto ao nome do canal e escolha os conectores.
![Ligações de equipas](media/ensure-tags-exists-on-new-virtual-machines/teams-webhook.png)
2.  Percorra a lista de conectores para entrada de Webhook e clique em Adicionar.
![Ligação de webhook equipas](media/ensure-tags-exists-on-new-virtual-machines/select-teams-webhook.png)
3.  Introduza AzureAutomationIntegration para o nome e clique em criar.
![Webhook de equipas](media/ensure-tags-exists-on-new-virtual-machines/configure-teams-webhook.png)
4.  Copie o webhook para a área de transferência e guardá-lo. O URL do webhook é utilizado para enviar informações para Teams da Microsoft.
5.  Selecione concluído para guardar o webhook.

## <a name="create-a-webhook-for-the-runbook"></a>Criar um webhook para o runbook
1.  Abra o runbook veja VMWrite.
2.  Clique no Webhooks e no botão de webhook adicionar ![criar o webhook](media/ensure-tags-exists-on-new-virtual-machines/add-webhook.png)
2.  Introduza "WatchVMEventGrid" para o nome e copie o URL para a área de transferência e guardar.
![Configurar o nome de webhook](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-name.png)
3.  Selecione parâmetros e introduza o URL do webhook Teams da Microsoft e deixar o WEBHOOKDATA em branco.
![Configurar parâmetros do webhook](media/ensure-tags-exists-on-new-virtual-machines/configure-webhook-parameters.png)
4.  Selecione OK para criar o webhook de runbook de automatização.

## <a name="create-an-event-grid-subscription"></a>Criar uma subscrição de grelha de eventos
1.  Clique na página de grelha de eventos da descrição de geral a conta de automatização.
![Lista de grelha de eventos](media/ensure-tags-exists-on-new-virtual-machines/event-grid-list.png)
2.  Clique no botão de nova subscrição de evento.
3.  Configure a subscrição com as seguintes informações:
    *   Introduza AzureAutomation para o nome. 
    *   Tipo de tópico, escolha as subscrições do Azure.
    *   Desmarque "Subscrever a todos os tipos de evento"
    *   Em tipos de eventos, escolha recursos escrita com êxito.
    *   No ponto final de subscritor, introduza o URL do Webhook para o runbook veja VMWrite.
    *   Num filtro de prefixo, introduza a subscrição e grupo de recursos que pretende procurar novas VMs criadas. Este deve ter o seguinte aspeto /subscriptions/124aa551-849d-46e4-a6dc-0bc4895422aB/resourcegroups/ContosoResourceGroup/providers/Microsoft.Compute/virtualMachines ![lista da grelha de eventos](media/ensure-tags-exists-on-new-virtual-machines/configure-event-grid-subscription.png)
6.  Clique em "Criar" para guardar a subscrição da grelha de eventos.

## <a name="create-vm-that-triggers-runbook"></a>Criar a VM que aciona o runbook
1.  Crie uma nova máquina virtual no grupo de recursos que especificou no caso de filtro do prefixo da subscrição de grelha.
2.  Deve ser chamado para o runbook VMWrite veja e adicionar uma nova etiqueta para a VM.
![VMTag](media/ensure-tags-exists-on-new-virtual-machines/vm-tag.png)
3.  Uma nova mensagem é enviada para o canal de equipas.

![Agrupamentos de notificação](media/ensure-tags-exists-on-new-virtual-machines/teams-vm-message.png)

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, configurar a integração entre a grelha de eventos e automatização. Aprendeu a:

> [!div class="checklist"]
> * Importe o runbook de exemplo de grelha de eventos.
> * Crie um webhook de equipas opcional.
> * Crie um webhook para o runbook.
> * Crie uma subscrição de grelha de eventos.
> * Crie a VM que aciona o runbook.

> [!div class="nextstepaction"]
> [Criar e eventos personalizados com grelha de eventos de rotas](../event-grid/custom-event-quickstart.md)
