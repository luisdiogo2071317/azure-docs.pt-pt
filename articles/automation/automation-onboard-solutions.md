---
title: "Integrar atualizações e soluções de alterações para a automatização do Azure | Microsoft Docs"
description: "Saiba como integrar atualizações e soluções de alterações para a automatização do Azure."
services: automation
documentationcenter: 
author: eamonoreilly
manager: 
editor: 
ms.assetid: edae1156-2dc7-4dab-9e5c-bf253d3971d0
ms.service: automation
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/11/2017
ms.author: eamono
ms.custom: mvc
ms.openlocfilehash: e277aa44dfe625780d72a78010f0638c73a6b182
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="onboard-update-and-change-tracking-solutions-to-azure-automation"></a>Integrar atualizações e soluções de alterações para a automatização do Azure

Neste tutorial, saiba como soluções de atualização, controlo de alterações e inventário carregar automaticamente para as VMs da automatização do Azure:

> [!div class="checklist"]
> * Carregar uma VM do Azure
> * Permitir soluções
> * Instalar e atualizar módulos
> * Importar o runbook de integração
> * Iniciar o runbook

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, é necessário o seguinte:

* Subscrição do Azure. Se ainda não tiver uma, pode [ativar as vantagens de subscritor do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ou [inscrever-se numa conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Conta de automatização](automation-offering-get-started.md) para gerir máquinas.
* Uma [máquina virtual](../virtual-machines/windows/quick-create-portal.md) para carregar.

## <a name="onboard-an-azure-vm"></a>Carregar uma VM do Azure

Para carregar máquinas virtuais do Azure automaticamente, uma VM existente tem de ser integrado com o registo de alterações ou uma solução de gestão de atualizações. Neste passo, pode carregar uma máquina virtual com a gestão de atualizações e controlo de alterações.

### <a name="enable-change-tracking-and-inventory"></a>Ativar o registo de alterações e inventário

A solução de controlo de alterações e de inventário fornece a capacidade de [controlar as alterações](automation-vm-change-tracking.md) e [inventário](automation-vm-inventory.md) nas suas máquinas virtuais. Neste passo, ative a solução numa máquina virtual.

1. No menu à esquerda, selecione **as contas de automatização**e, em seguida, selecione a sua conta de automatização na lista.
1. Selecione **inventário** em **gestão de configuração**.
1. Selecione uma área de trabalho de análise de registos existente ou criar um novo. Clique em de **ativar** botão.

![Integrar soluções de atualização](media/automation-onboard-solutions/inventory-onboard.png)

Quando o controlo e de inventário de solução integração notificação de alteração estiver concluída, clique em **gestão de atualizações** em **gestão de configuração**.

### <a name="enable-update-management"></a>Ativar a gestão de atualização

A solução de gestão de atualizações permite-lhe gerir as atualizações e correções de erros para as suas VMs do Windows Azure. Pode avaliar o estado das atualizações disponíveis, agendar a instalação de atualizações necessárias, e reveja os resultados de implementação para verificar atualizações foram aplicados com êxito para a VM. Neste passo, ative a solução para a VM.

1. A partir da sua conta de automatização, selecione **gestão de atualizações** em **gestão de ATUALIZAÇÕES**.
1. A área de trabalho de análise do registo selecionada é o mesmo espaço de trabalho utilizado no passo anterior. Clique em **ativar** integrar a solução de gestão de atualização.

![Integrar soluções de atualização](media/automation-onboard-solutions/update-onboard.png)

Enquanto está a ser instalada a solução de gestão de atualização, é apresentada uma faixa azul. Quando a solução é ativada selecione **alterações** em **gestão de configuração** para ir para o passo seguinte.

### <a name="select-azure-vm-to-be-managed"></a>Selecione a VM do Azure para serem geridos

Agora que as soluções estão ativadas, pode adicionar uma VM do Azure para carregar a essas soluções.

1. Da sua conta de automatização no **alterações** página, selecione **+ adicionar a VM do Azure** para adicionar a máquina virtual.

1. Selecione a VM a partir da lista e selecione **ativar**. Esta ação permite a alteração tacking e a solução de inventário da máquina virtual.

   ![Ativar a solução de atualização para vm](media/automation-onboard-solutions/enable-change-tracking.png)

1. Quando a notificação de integração VM for concluído, no, selecione a conta de automatização **gestão de atualizações** em **gestão de ATUALIZAÇÕES**.

1. Selecione **+ adicionar a VM do Azure** para adicionar a máquina virtual.

1. Selecione a VM a partir da lista e selecione **ativar**. Esta ação permite a solução de gestão de atualização para a máquina virtual.

   ![Ativar a solução de atualização para vm](media/automation-onboard-solutions/enable-update.png)

> [!NOTE]
> Se não aguardar a outra solução concluir, quando ativar a seguinte solução recebe uma mensagem a indicar: *instalação de outra solução está em curso nisto ou outra máquina virtual. Quando concluir essa instalação no botão Ativar está ativado e podem pedir a instalação da solução nesta máquina virtual.*

## <a name="install-and-update-modules"></a>Instalar e atualizar módulos

É necessário para atualizar para os módulos do Azure mais recentes e importar `AzureRM.OperationalInsights` para automatizar a solução de integração com êxito.

## <a name="update-azure-modules"></a>Atualizar os módulos do Azure

A partir da sua conta de automatização, selecione **módulos** em **recursos PARTILHADOS**. Selecione **atualizar módulos do Azure** para atualizar os módulos do Azure para a versão mais recente. Selecione **Sim** na linha de comandos para atualizar todos os módulos do Azure existentes para a versão mais recente.

![Módulos de atualização](media/automation-onboard-solutions/update-modules.png)

### <a name="install-azurermoperationalinsights-module"></a>Instalar o módulo de AzureRM.OperationalInsights

Do **módulos** página, selecione **procurar galeria** para abrir a galeria do módulo. Procurar AzureRM.OperationalInsights e importar este módulo para a conta de automatização.

![Importe o módulo de OperationalInsights](media/automation-onboard-solutions/import-operational-insights-module.png)

## <a name="import-the-onboarding-runbook"></a>Importar o runbook de integração

1. A partir da sua conta de automatização, selecione em **Runbooks** sob o **AUTOMATIZAÇÃO de processos**.
1. Selecione **procurar galeria**.
1. Procurar **atualizar e registo de alterações**, clique o runbook e selecione **importação** no **Ver origem** página. Selecione **OK** para importar o runbook para a conta de automatização.

  ![Importar o runbook de integração](media/automation-onboard-solutions/import-from-gallery.png)

1. No **Runbook** página, selecione **editar**, em seguida, selecione **publicar**. No **publicar o Runbook** caixa de diálogo, selecione **Sim** para publicar o runbook.

## <a name="start-the-runbook"></a>Iniciar o runbook

Tem de ter integrado o registo de alterações ou soluções de atualização para uma VM do Azure para iniciar este runbook. Requer uma máquina virtual existente e o grupo de recursos com o solução integrado de parâmetros.

1. Abra o runbook MultipleSolution ativar.

   ![Vários runbooks de solução](media/automation-onboard-solutions/runbook-overview.png)

1. Clique no botão de início e introduza os seguintes valores de parâmetros.

   * **VMNAME** -deixar em branco. O nome de VM existente para carregar a atualização ou a solução de registo de alterações. Por abandonar o fileparser este valor em branco, todas as VMs no grupo de recursos são integrado.
   * **VMRESOURCEGROUP** -o nome do grupo de recursos para as VMs ser integrado.
   * **SUBSCRIPTIONID** -deixar em branco. O ID de subscrição da nova VM para ser integrado. Se deixado em branco, é utilizada a subscrição da área de trabalho. Quando recebe um ID de subscrição diferente, a conta RunAs para esta conta de automatização deve ser adicionada também como um contribuinte para esta subscrição.
   * **ALREADYONBOARDEDVM** -o nome da VM que foi manualmente integrado para atualizações ou ChangeTracking solução.
   * **ALREADYONBOARDEDVMRESOURCEGROUP** -o nome do grupo de recursos que a VM seja um membro.
   * **SOLUTIONTYPE** -introduza **atualizações** ou **ChangeTracking**

   ![Ativar MultipleSolution parâmetros do runbook](media/automation-onboard-solutions/runbook-parameters.png)

1. Selecione **OK** para iniciar a tarefa de runbook.
1. Monitorize o progresso e os erros na página de tarefa de runbook.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Carregar uma máquina virtual do Azure manualmente.
> * Instalar e atualizar os módulos do Azure necessários.
> * Importe um runbook que onboards VMs do Azure.
> * Inicie o runbook que onboards VMs do Azure automaticamente.

Siga esta ligação para obter mais informações sobre o agendamento de runbooks.

> [!div class="nextstepaction"]
> [Agendamento runbooks](automation-schedules.md).
