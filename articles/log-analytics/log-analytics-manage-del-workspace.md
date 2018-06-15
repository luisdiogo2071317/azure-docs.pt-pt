---
title: Eliminar uma área de trabalho do Log Analytics do Azure | Microsoft Docs
description: Saiba como eliminar a sua área de trabalho de análise de registos, se tiver criado uma numa subscrição pessoal ou reestruturar o seu modelo da área de trabalho.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2018
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 084b8fa0addbe2ddeee699b58c9b0de1a6f27d99
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33938697"
---
# <a name="delete-an-azure-log-analytics-workspace-with-the-azure-portal"></a>Eliminar uma área de trabalho do Log Analytics do Azure com o portal do Azure
Este artigo mostra como utilizar o portal do Azure para eliminar uma área de trabalho de análise de registos que poderá deixar de necessitar. 

## <a name="to-delete-a-workspace"></a>Para eliminar uma área de trabalho 
Quando elimina uma área de trabalho de análise de registos, todos os dados relacionados com a sua área de trabalho é eliminado do serviço no prazo de 30 dias.  Quer tenha cuidado quando elimina uma área de trabalho porque poderá haver dados importantes e de configuração que pode afetar negativamente as operações de serviço. Considere a outros serviços do Azure e as origens que armazenam os dados na análise de registos, tais como:

* Application Insights
* Centro de Segurança do Azure
* Automatização do Azure
* Agentes em execução em máquinas virtuais Windows e Linux
* Agentes de computadores em execução no Windows e Linux no seu ambiente
* System Center Operations Manager
* Soluções de gestão 

Quaisquer agentes e grupos de gestão do System Center Operations Manager configurados para o relatório para a área de trabalho continuam a num Estado órfão.  Os agentes, soluções, de inventário e outros serviços do Azure estão integrados com a área de trabalho antes de continuar.   
 
Se for um administrador e existirem vários utilizadores associados à área de trabalho, a associação entre esses utilizadores e a área de trabalho é interrompida. Se os utilizadores estão associados a outras áreas de trabalho, em seguida, podem continuar a utilizar a análise de registos com as outras áreas de trabalho. No entanto, se não estão associados com outras áreas de trabalho, em seguida, precisam de criar uma área de trabalho para utilizar a análise de registos. 

1. Inicie sessão no [Portal do Azure](http://portal.azure.com). 
2. No portal do Azure, clique em **Mais serviços**, que se encontra no canto inferior esquerdo. Na lista de recursos, escreva **Log Analytics**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **Log Analytics**.
3. No painel de subscrições de análise de registos, selecione uma área de trabalho e, em seguida, clique em **eliminar** da parte superior do painel do meio.<br><br> ![Eliminar a opção do painel de propriedades da área de trabalho](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace.png)<br>  
4. Quando for apresentada a janela da mensagem de confirmação pedir-lhe para confirmar a eliminação da área de trabalho, clique em **Sim**.<br><br> ![Confirmar eliminação da área de trabalho](media/log-analytics-manage-del-workspace/log-analytics-delete-workspace-confirm.png)

