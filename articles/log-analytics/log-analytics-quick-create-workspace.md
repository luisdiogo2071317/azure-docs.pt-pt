---
title: Criar uma área de trabalho no Log Analytics do Azure | Microsoft Docs
description: Saiba como criar uma área de trabalho de análise de registos para ativar a recolha de dados e soluções de gestão dos seus ambientes de nuvem e no local.
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
ms.topic: conceptal
ms.date: 04/02/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: eff79e8ccf0b878b4c083753f245aae993be72de
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127572"
---
# <a name="create-a-log-analytics-workspace-in-the-azure-portal"></a>Criar uma área de trabalho de análise de registos no portal do Azure
No Portal do Azure que pode configurar uma área de trabalho de análise de registos, que é um ambiente de análise de registos exclusivo com as suas próprias repositório de dados, as origens de dados e soluções.  Os passos descritos neste artigo são necessários se de que pretende recolher dados das seguintes origens:

* Recursos do Azure na sua subscrição
* Computadores monitorizados pelo System Center Operations Manager no local
* Coleções de dispositivos do System Center Configuration Manager 
* Dados de diagnóstico ou de registo do armazenamento do Azure

Para outras origens, como VMs do Azure e os computadores Windows ou Linux no seu ambiente, consulte os tópicos seguintes:

*  [Recolher dados sobre máquinas virtuais do Azure](log-analytics-quick-collect-azurevm.md) 
*  [Recolher dados sobre computadores com Linux](log-analytics-quick-collect-linux-computer.md)
*  [Recolher dados sobre computadores Windows](log-analytics-quick-collect-windows-computer.md)

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="log-in-to-azure-portal"></a>Iniciar sessão no portal do Azure
Inicie sessão no portal do Azure em [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Criar uma área de trabalho
1. No portal do Azure, clique em **All services** (Todos os serviços). Na lista de recursos, escreva **Log Analytics**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **Log Analytics**.<br><br> ![Portal do Azure](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
2. Clique em **Criar** e, em seguida, selecione as opções para os seguintes itens:

  * Indique um nome para a nova **Área de Trabalho do OMS**, como *DefaultLAWorkspace*. 
  * Selecione uma **Subscrição** à qual ligar ao escolher na lista pendente se a predefinição selecionada não é adequada.
  * Para **grupo de recursos**, optar por utilizar um recurso existente grupo já configuração ou crie um novo.  
  * Selecione um disponível **localização**.  Para obter mais informações, veja em que [regiões está disponível o Log Analytics](https://azure.microsoft.com/regions/services/).
  * Se estiver a criar uma área de trabalho numa subscrição nova criada depois de 2 de abril de 2018, a mesma utilizará automaticamente o plano de preços *Por GB* e a opção para selecionar um escalão de preços não estará disponível.  Se estiver a criar uma área de trabalho para uma subscrição já existente criada antes de 2 de abril ou para uma subscrição que estava associada a uma inscrição EA já existente, selecione o seu escalão de preço preferido.  Para obter informações adicionais sobre os escalões específicos, veja [Detalhes de Preços do Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-02.png)<br>  

3. Depois de fornecer as informações necessárias no painel **Área de Trabalho do OMS**, clique em **OK**.  

Enquanto as informações são confirmadas e a área de trabalho criada, pode acompanhar o progresso em **Notificações**, no menu. 

## <a name="next-steps"></a>Passos Seguintes
Agora que tem uma área de trabalho disponível, pode configurar a recolha de monitorização de telemetria, executar pesquisas de registo para analisar os dados e adicionar uma solução de gestão para fornecer dados adicionais e informações de análise. 

* Para ativar a recolha de dados a partir dos recursos do Azure com o diagnóstico do Azure ou o armazenamento do Azure, consulte [métricas para utilização na análise de registos e registos do serviço Azure recolher](log-analytics-azure-storage.md).  
* [Adicionar o System Center Operations Manager como uma origem de dados](log-analytics-om-agents.md) para recolher dados de agentes de relatórios do seu grupo de gestão do Operations Manager e guarde-o no seu repositório de área de trabalho de análise de registos. 
* Ligar [do Configuration Manager](log-analytics-sccm.md) para importar a computadores que são membros de coleções na hierarquia.  
* Reveja o [soluções de gestão](/log-analytics-add-solutions.md) disponíveis e como adicionar ou remover uma solução da sua área de trabalho.

