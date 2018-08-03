---
title: Desassociar a conta de Automatização do Azure a partir do Log Analytics
description: Este artigo fornece uma descrição geral de como desassociar a conta de automatização do Azure de uma área de trabalho do Log Analytics.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 08/01/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1328ce8c306188c32bce5385f58f118a63c08deb
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426538"
---
# <a name="how-to-unlink-your-automation-account-from-a-log-analytics-workspace"></a>Como desassociar a conta de automatização de uma área de trabalho do Log Analytics

A automatização do Azure integra-se com o Log Analytics para não apenas suportar a monitorização das suas tarefas de runbook em todas as suas contas de automatização, mas também é necessária ao importar as seguintes soluções que dependem do Log Analytics:

* [Gestão de Atualizações](../operations-management-suite/oms-solution-update-management.md)
* [Monitorização de Alterações](../log-analytics/log-analytics-change-tracking.md)
* [Iniciar/parar VMs fora do horário comercial](automation-solution-vm-management.md)

Se decidir que já não pretende integrar a sua conta de automatização com o Log Analytics, pode desassociar a sua conta diretamente a partir do portal do Azure.  Antes de continuar, tem primeiro de remover as soluções mencionadas anteriormente, caso contrário, este processo será impedido de prosseguir. Reveja o artigo para a solução específica que importou para compreender os passos necessários para removê-lo.

Depois de remover estas soluções, pode executar os seguintes passos para desassociar a conta de automatização.

> [!NOTE]
> Algumas soluções, incluindo versões anteriores da solução de monitorização de SQL do Azure podem ter criado os recursos de automatização e também poderão ter de ser removida antes de desassociar a área de trabalho.

## <a name="unlink-workspace"></a>Desassociar área de trabalho

1. A partir do portal do Azure, abra sua conta de automatização e na Automação de conta de página select **ligado área de trabalho** na secção **recursos relacionados** à esquerda.

1. Na página de área de trabalho de desassociar, clique em **desassociar área de trabalho**.

   ![Desassociar a página de área de trabalho](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).

   Irá receber um pedido de confirmação de que pretende continuar.

1. Enquanto a automatização do Azure tenta desassociar a conta de sua área de trabalho do Log Analytics, pode acompanhar o progresso em **notificações** no menu.

Se utilizou a solução de gestão de atualizações, opcionalmente, pode querer remover os seguintes itens que já não são necessários depois de remover a solução.

* Agenda de atualização - cada terão nomes que coincidem com as implementações de atualização que criou)

* Grupos de trabalho híbrida criados para a solução - cada um terá o nome da mesma forma para 9-4051-b6b3-227600d715c8 de machine1.contoso.com_9ceb8108 - 26 c).

Se utilizou a iniciar/parar VMs durante a solução de horário comercial, opcionalmente, pode querer remover os seguintes itens que já não são necessários depois de remover a solução.

* Iniciar e parar agendas de runbook VM
* Iniciar e parar runbooks VM
* Variáveis

## <a name="next-steps"></a>Passos Seguintes

Para reconfigurar a sua conta de automatização para integrar com o Log Analytics, consulte [reencaminhar o estado da tarefa e fluxos de trabalho de automatização para o Log Analytics](automation-manage-send-joblogs-log-analytics.md).