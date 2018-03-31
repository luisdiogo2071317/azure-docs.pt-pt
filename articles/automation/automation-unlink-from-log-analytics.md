---
title: Desassociar a conta de automatização do Azure do Log Analytics | Microsoft Docs
description: Este artigo fornece uma descrição geral de como desassociar a sua conta de automatização do Azure a partir de uma área de trabalho de análise de registos.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: b8b00f8a82dd63df13ccd0bc7e10429323c15ab3
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2018
---
# <a name="how-to-unlink-your-automation-account-from-a-log-analytics-workspace"></a>Como desativar a sua conta de automatização a partir de uma área de trabalho de análise de registos a

A automatização do Azure integra-se a análise de registos para apenas não suporta a monitorização de tarefas de runbook em todas as contas de automatização, mas também é necessária quando importar as seguintes soluções que são dependentes de análise de registos:

* [Gestão de Atualizações](../operations-management-suite/oms-solution-update-management.md)
* [Monitorização de Alterações](../log-analytics/log-analytics-change-tracking.md)
* [VMs de início/paragem durante off-hours](automation-solution-vm-management.md)
 
Se decidir que já não pretende integrar a sua conta de automatização com a análise de registos, pode desassociar a sua conta diretamente a partir do portal do Azure.  Antes de continuar, terá primeiro de remover as soluções mencionadas anteriormente, caso contrário, este processo será impedido de continuar.  Reveja o tópico da solução específica que importou para compreender os passos necessários para removê-lo.  

Depois de remover estas soluções podem efetuar os seguintes passos para desassociar a sua conta de automatização.

## <a name="unlink-workspace"></a>Desassociar a área de trabalho

1. A partir do portal do Azure, abra a conta de automatização e na automatização da página Selecionar conta **desassociar a área de trabalho** na secção **recursos relacionados** à esquerda.<br><br> ![Desassociar a opção de área de trabalho](media/automation-unlink-from-log-analytics/automation-unlink-workspace-option.png)<br><br>  
2. Na página de área de trabalho desassociar, clique em **desassociar a área de trabalho**.<br><br> ![Desassociar a página da área de trabalho](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).<br><br>  Irá receber um pedido de confirmação de que pretende continuar.<br><br>
3. Enquanto a automatização do Azure tenta desassociar a conta da área de trabalho de análise de registos, pode acompanhar o progresso em **notificações** no menu.

Se utilizou a solução de gestão de atualizações, opcionalmente, poderá remover os seguintes itens que já não são necessárias depois de remover a solução.

* Agendas de atualização.  Cada terão nomes que correspondem ao que criou as implementações de atualização)

* Grupos de trabalho híbrida criados para a solução.  Cada um irá ser com o nome da mesma forma para machine1.contoso.com_9ceb8108 - 26 c 9-4051 b6b3-227600d715c8).

Se utilizou as VMs de início/paragem durante a solução de off-hours, opcionalmente, poderá remover os seguintes itens que já não são necessárias depois de remover a solução.

* Iniciar e parar agendas de runbook VM 
* Iniciar e parar runbooks VM
* Variáveis   

## <a name="next-steps"></a>Passos Seguintes

Para reconfigurar a sua conta de automatização para integrar com a análise de registos, consulte o artigo [reencaminhar o estado da tarefa e fluxos de trabalho da automatização para análise de registos](automation-manage-send-joblogs-log-analytics.md). 