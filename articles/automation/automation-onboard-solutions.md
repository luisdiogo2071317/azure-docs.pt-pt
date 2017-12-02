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
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2017
ms.author: eamono
ms.openlocfilehash: 9ae5e9ebcbcf3af61318eac98defc6b249417f41
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/02/2017
---
# <a name="onboard-update-and-change-tracking-solutions-to-azure-automation"></a>Integrar atualizações e soluções de alterações para a automatização do Azure

Neste tutorial, saiba como soluções de atualização, controlo de alterações e inventário carregar automaticamente para as VMs da automatização do Azure:

> [!div class="checklist"]
> * Carregar uma máquina virtual do Azure manualmente.
> * Instalar e atualizar os módulos do Azure necessários.
> * Importe um runbook que onboards VMs do Azure.
> * Inicie o runbook que onboards VMs do Azure automaticamente.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, é necessário o seguinte.
+ Subscrição do Azure. Se ainda não tiver uma, pode [ativar as vantagens de subscritor do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ou [inscrever-se numa conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
+ [Conta de automatização](automation-offering-get-started.md) para gerir máquinas.

## <a name="onboard-an-azure-virtual-machine-manually"></a>Carregar uma máquina virtual do Azure manualmente

1.  Abra a conta de automatização.
2.  Clique na página de inventário.
![Integrar soluções de inventário](media/automation-onboard-solutions/inventory-onboard.png)
3.  Selecione uma área de trabalho de análise de registos existente ou criar um novo. Clique no botão de ativação.
4.  Quando o controlo e de inventário de solução integração notificação de alteração estiver concluída, clique na página Gestão de atualizações.
![Integrar soluções de atualização](media/automation-onboard-solutions/update-onboard.png)
4.  Clique em Ativar botão que onboards a solução de atualização.
5.  Quando a notificação de integração da solução de atualização estiver concluída, clique na página registo de alterações.
![Carregar o registo de alterações](media/automation-onboard-solutions/change-tracking-onboard-vm.png)
6.  Clique no botão Adicionar a VM do Azure.
![Selecione a VM de registo de alterações](media/automation-onboard-solutions/enable-change-tracking.png)
7.  Selecione uma VM do Azure e clique no botão Ativar para carregar para a solução de inventário e o controlo de alterações.
8.  Quando tiver concluído a notificação de integração VM, clique na página Gestão de atualizações.
![Carregar VM para a gestão de atualizações](media/automation-onboard-solutions/update-onboard-vm.png)
9.  Clique no botão Adicionar a VM do Azure.
![Selecione a VM para a gestão de atualizações](media/automation-onboard-solutions/enable-update.png)
10.  Selecione uma VM do Azure e clique no botão Ativar para integrar a solução de gestão de atualização.

## <a name="install-and-update-required-azure-modules"></a>Instalar e atualizar os módulos do Azure necessários

É necessário para atualizar para os módulos do Azure mais recentes e importar AzureRM.OperationalInsights para automatizar a solução de integração com êxito.
1.  Clique na página de módulos.
![Módulos de atualização](media/automation-onboard-solutions/update-modules.png)
2.  Clique no botão de módulos do Azure de atualização que atualiza para a versão mais recente. Aguarde que as atualizações concluir.
3.  Clique no botão Procurar de galeria para abrir a galeria do módulo.
![Importe o módulo de OperationalInsights](media/automation-onboard-solutions/import-operational-insights-module.png)
4.  Procurar AzureRM.OperationalInsights e importar este módulo para a conta de automatização.

## <a name="import-a-runbook-that-onboards-solutions-to-azure-vms"></a>Importar um runbook que soluções onboards para VMs do Azure

1.  Clique na página de Runbooks sob a categoria "AUTOMATIZAÇÃO de processos".
2.  Clique no botão "Procurar Galeria".
3.  Pesquise "atualizar e registo de alterações" e importar o runbook para a conta de automatização.
![Importar o runbook de integração](media/automation-onboard-solutions/import-from-gallery.png)
4.  Clique em "Editar" para ver a origem de Runbook e clique no botão "Publicar".
![Importar o runbook de integração](media/automation-onboard-solutions/publish-runbook.png)

## <a name="start-the-runbook-that-onboards-azure-vms-automatically"></a>Iniciar o runbook que onboards VMs do Azure automaticamente

Tem de ter integrado o registo de alterações ou soluções de atualização para uma VM do Azure para iniciar este runbook. Requer uma máquina virtual existente e o grupo de recursos com o solução integrado de parâmetros.
1.  Abra o runbook MultipleSolution ativar.
![Vários runbooks de solução](media/automation-onboard-solutions/runbook-overview.png)
2.  Clique no botão de início e introduza os seguintes valores de parâmetros.
    *   VMNAME. O nome de VM existente para carregar a atualização ou a solução de registo de alterações. Deixar em branco e todas as VMs no grupo de recursos são integrado.
    *   VMRESOURCEGROUP. O nome do grupo de recursos que a VM seja um membro.
    *   ID DE SUBSCRIÇÃO. O ID de subscrição se encontra a VM nova para carregar. Deixe em branco e a subscrição da área de trabalho é utilizada. Quando recebe um ID de subscrição diferente, a conta RunAs para esta conta de automatização deve ser adicionada também como um contribuinte para esta subscrição.
    *   ALREADYONBOARDEDVM. O nome da VM que foi manualmente integrado para atualizações ou ChangeTracking solução.
    *   ALREADYONBOARDEDVMRESOURCEGROUP. O nome do grupo de recursos que a VM seja um membro.
    *   SOLUTIONTYPE. Introduza "Atualizações" ou "ChangeTracking"
    
    ![Vários parâmetros do runbook de solução](media/automation-onboard-solutions/runbook-parameters.png)
3.  Clique em OK para iniciar a tarefa de runbook.
4.  Monitorize o progresso e os erros na página de tarefa de runbook.

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações, consulte [agendamento runbooks](automation-schedules.md).