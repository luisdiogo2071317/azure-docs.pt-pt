---
title: Utilizador iniciadas por ação de Runbook da automatização do Azure na análise de registos | Microsoft Docs
description: Este artigo descreve como executar um runbook de automatização a partir de uma análise de registos pesquisa resultado a pedido.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/04/2017
ms.author: magoedte
ms.openlocfilehash: c59a32e1b2d460e04c4c6f5d1be2dd655abbef27
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
ms.locfileid: "29394620"
---
# <a name="take-action-with-an-automation-runbook-from-a-log-analytics-log-search-result"></a>Ação com um Runbook de automatização a partir de um resultado de pesquisa de registos de análise de registos

A partir de um resultado de pesquisa de registo no Log Analytics do Azure, agora, pode selecionar **ação** para executar um runbook de automatização.  O runbook pode ser utilizado para corrigir o problema ou alguns outros ação, tais como recolher informações de resolução de problemas, enviem um e-mail ou criar um pedido de serviço. 

## <a name="components-and-features-used"></a>Componentes e funcionalidades utilizados
* [Conta de automatização do Azure](../automation/automation-offering-get-started.md)
* [Área de trabalho de análise de registo](../log-analytics/log-analytics-overview.md)

## <a name="to-initiate-runbook-from-log-search"></a>Para iniciar o runbook a partir de pesquisa de registo

Para executar ações num evento e iniciar um runbook a partir dos resultados da pesquisa de registo, comece por criar uma pesquisa de registo e na lista de resultados pode invocar uma runbook a pedido.  Isto pode ser conseguido da funcionalidade de pesquisa de registo no [portal do Azure](../log-analytics/log-analytics-log-search-new.md).  Neste exemplo, vamos efetuar uma pesquisa de registo do portal do Azure com uma demonstração básico desta funcionalidade.

1. No portal do Azure, clique em **todos os serviços** e selecione **Log Analytics**.  
2. Selecione a sua área de trabalho de análise de registos.
3. Na área de trabalho, selecione **pesquisa registo**.  
4. Na página de pesquisa de registo, execute uma pesquisa de registo.  
5. Nos resultados de pesquisa de registo, clique em elipse à esquerda de um dos campos e para o pop-up, selecione **executar ações em**.<br><br> ![Selecione a ação demorar do resultado de pesquisa](./media/log-analytics-log-search-takeaction/log-search-takeaction-menuoption.png) 
6. Selecione **executar um runbook** e selecione um runbook seja executado.  Pode selecionar qualquer runbook na conta de automatização que esteja ligada à área de trabalho de análise de registos.  Tenha em atenção o seguinte:

    * Os Runbooks estão organizados por etiquetas
    * Os valores de parâmetro de entrada do Runbook podem ser selecionados diretamente a partir de campos do resultado da pesquisa.  Na lista pendente aparecerá a apresentar todos os campos disponíveis do resultado de selecção.  
    * Pode optar por executar o runbook [runbook worker híbrido](../automation/automation-hybrid-runbook-worker.md) que tem instalado no computador que tenha o problema se tiver um grupo de trabalho de Runbook híbrida correspondente que contém apenas esta máquina como membro.  Se o nome do grupo de trabalho híbrida corresponde ao nome do computador que está a ser o resultado da pesquisa de registo, o grupo é selecionado automaticamente.    

6. Depois de clicar em **executar**, abre a página de tarefa de runbook para permite-lhe rever o estado da tarefa.   

Se selecionar um runbook que foi configurado para ser [chamado a partir de um alerta de análise de registos](../automation/automation-invoke-runbook-from-omsla-alert.md), tem um parâmetro de entrada chamado **WebhookData** que **objeto** tipo.  Se o parâmetro de entrada é obrigatório, tem de passar os resultados da pesquisa para o runbook, pelo que pode converter a cadeia formatada em JSON para um tipo de objeto, permitindo-lhe filtrar itens específicos que irá referenciar em atividades do runbook.  Fazê-lo ao selecionar **procurar resultado (objeto)** na lista pendente.<br><br> ![Selecione o objeto de dados de Webhook para o parâmetro de runbook](media/log-analytics-log-search-takeaction/select-runbook-and-properties.png)   
    
## <a name="next-steps"></a>Passos Seguintes

* Reveja o [referência de pesquisa de registo de análise de registos](log-analytics-search-reference.md) para ver todos os campos de pesquisa e facetas disponíveis na análise de registos.
* Para saber como invocar um runbook da automatização automaticamente, reveja [chamar um runbook de automatização do Azure a partir de um alerta de análise de registos](../automation/automation-invoke-runbook-from-omsla-alert.md).  