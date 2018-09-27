---
title: Criar uma área de trabalho do Log Analytics no Portal do Azure | Documentos da Microsoft
description: Saiba como criar uma área de trabalho do Log Analytics para ativar a recolha de dados e soluções de gerenciamento de seus ambientes na cloud e no local no portal do Azure.
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
ms.date: 08/23/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: a52654ab620908b9c8c07edf3fcdfbf70068528d
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220795"
---
# <a name="create-a-log-analytics-workspace-in-the-azure-portal"></a>Criar uma área de trabalho do Log Analytics no portal do Azure
No portal do Azure que pode configurar uma área de trabalho do Log Analytics, que é um ambiente do Log Analytics exclusivo com seu próprio repositório de dados, origens de dados e soluções.  Os passos descritos neste artigo são necessários se pretende recolher dados das seguintes origens:

* Recursos do Azure na sua subscrição
* Computadores monitorizados pelo System Center Operations Manager no local
* Coleções de dispositivos do System Center Configuration Manager 
* Dados de diagnóstico ou de registo do armazenamento do Azure

Para outras origens, como as VMs do Azure e o Windows ou VMs do Linux no seu ambiente, consulte os seguintes tópicos:

*  [Recolher dados de máquinas virtuais do Azure](log-analytics-quick-collect-azurevm.md) 
*  [Recolher dados de computador com Linux híbrida](log-analytics-quick-collect-linux-computer.md)
*  [Recolher dados do computador de Windows híbrida](log-analytics-quick-collect-windows-computer.md)

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="sign-in-to-azure-portal"></a>Iniciar sessão no portal do Azure
Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-a-workspace"></a>Criar uma área de trabalho
1. No portal do Azure, clique em **All services** (Todos os serviços). Na lista de recursos, escreva **Log Analytics**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **Log Analytics**.

    ![Portal do Azure](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)
  
2. Clique em **Criar** e, em seguida, selecione as opções para os seguintes itens:

  * Forneça um nome para a nova **área de trabalho do Log Analytics**, tal como *DefaultLAWorkspace*. 
  * Selecione uma **Subscrição** à qual ligar ao escolher na lista pendente se a predefinição selecionada não é adequada.
  * Para **grupo de recursos**, optar por utilizar um recurso existente já grupo configuração ou criar um novo.  
  * Selecione um disponíveis **localização**.  Para obter mais informações, veja em que [regiões do Log Analytics está disponível no](https://azure.microsoft.com/regions/services/).
  * Se estiver a criar uma área de trabalho numa subscrição nova criada depois de 2 de abril de 2018, a mesma utilizará automaticamente o plano de preços *Por GB* e a opção para selecionar um escalão de preços não estará disponível.  Se estiver a criar uma área de trabalho para uma subscrição existente criada antes 2 de Abril de ou para uma subscrição que estava associada a uma inscrição de Enterprise Agreement (EA) já existente, selecione o escalão de preço preferido.  Para obter mais informações sobre os escalões específicos, consulte [detalhes de preços do Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/).

        ![Create Log Analytics resource blade](media/log-analytics-quick-collect-azurevm/create-loganalytics-workspace-02.png)  

3. Depois de fornecer as informações necessárias sobre o **área de trabalho do Log Analytics** painel, clique em **OK**.  

Enquanto as informações são confirmadas e a área de trabalho criada, pode acompanhar o progresso em **Notificações**, no menu. 

## <a name="next-steps"></a>Passos Seguintes
Agora que tem uma área de trabalho disponível, pode configurar a recolha de monitorização de telemetria, executar pesquisas de registos para analisar esses dados e adicionar uma solução de gestão para fornecer dados adicionais e informações de análise. 

* Para ativar a recolha de dados dos recursos do Azure com o diagnóstico do Azure ou o armazenamento do Azure, veja [registos do serviço de recolha do Azure e as métricas de utilização do Log Analytics](log-analytics-azure-storage.md).  
* [Adicionar o System Center Operations Manager como uma origem de dados](log-analytics-om-agents.md) para recolher dados de agentes que reportam o grupo de gestão do Operations Manager e o armazenamos em sua área de trabalho do Log Analytics. 
* Ligue-se [Configuration Manager](log-analytics-sccm.md) para importar computadores que são membros de coleções na hierarquia.  
* Reveja os [soluções de gestão](https://docs.microsoft.com/azure/monitoring/monitoring-solutions-inventory?toc=%2fazure%2flog-analytics%2ftoc.json) disponíveis e como adicionar ou remover uma solução da sua área de trabalho.
