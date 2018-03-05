---
title: "Direcionar as atualizações utilizando o SCCM coleções na automatização do Azure - gestão de atualizações | Microsoft Docs"
description: "Este artigo destina-se a ajudar a configurar o System Center Configuration Manager com esta solução para gerir atualizações de computadores geridos pelo SCCM."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: gwallace
ms.openlocfilehash: c1636ce84f6d0d228dcb15e62af4a88b50fd84bd
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="integrate-system-center-configuration-manager-with-update-management"></a>Integrar o System Center Configuration Manager com a gestão de atualizações

Os clientes que investiram no System Center Configuration Manager para gerir PCs, servidores e dispositivos móveis também contam com a sua potência e maturidade na gestão de atualizações de software como parte do respetivo ciclo de gestão de atualizações de software (SUM).

Pode comunicar e atualizar os servidores Windows geridos através da criação e teste previamente implementações de atualização de software no Configuration Manager e obter o estado detalhado das implementações de atualização foi concluída com o [solução de gestão de atualização](automation-update-management.md). Se utilizar o Configuration Manager para relatórios de conformidade de atualização, mas não para gerir implementações de atualização com os servidores do Windows, pode continuar a relatórios para o Configuration Manager, enquanto as atualizações de segurança são geridas com a solução de gestão de atualização.

## <a name="prerequisites"></a>Pré-requisitos

* Tem de ter o [solução de gestão de atualizações](automation-update-management.md) adicionado à sua conta de automatização.
* Os servidores Windows atualmente geridos pelo seu ambiente do System Center Configuration Manager também precisam de reportar para a área de trabalho do Log Analytics que também tem a solução de Gestão de Atualizações ativada.
* Esta funcionalidade está ativada no System Center Configuration Manager versão atual do ramo 1606 e planos superior. Para integrar o seu site de administração central do Configuration Manager ou um site primário autónomo com o Log Analytics e importar coleções, reveja [Ligar o Configuration Manager ao Log Analytics](../log-analytics/log-analytics-sccm.md).  
* Os agentes do Windows têm de ser configurados para comunicar com um servidor de Windows Server Update Services (WSUS) ou ter acesso ao Microsoft Update, caso não recebam atualizações de segurança do Configuration Manager.   

A forma de gerir os clientes alojados no IaaS do Azure com o seu ambiente existente do Configuration Manager depende principalmente da ligação que tem entre os datacenters do Azure e a sua infraestrutura. Esta ligação afeta quaisquer alterações de estrutura que possa ter de fazer na infraestrutura do Configuration Manager e o custo relacionado para suportar essas alterações necessárias. Para compreender quais as considerações de planeamento que precisa de avaliar antes de continuar, reveja [Configuration Manager no Azure - Perguntas mais frequentes](/sccm/core/understand/configuration-manager-on-azure#networking).

## <a name="configuration"></a>Configuração

### <a name="manage-software-updates-from-configuration-manager"></a>Gerir atualizações de software a partir do Configuration Manager 

Se pretender continuar a gerir implementações de atualizações a partir do Configuration Manager, execute os passos seguintes. A automatização do Azure estabelece ligação ao Configuration Manager para aplicar atualizações nos computadores cliente ligados à sua área de trabalho de análise de registos. O conteúdo da atualização está disponível na cache do computador cliente como se a implementação fosse gerida pelo Configuration Manager.

1. Crie uma implementação de atualização de software a partir do site de nível superior na hierarquia do Configuration Manager com o processo descrito em [Implementar o processo de atualização de software](/sccm/sum/deploy-use/deploy-software-updates). A única definição que tem de ser configurada de forma diferente de uma implementação padrão é a opção **Não instalar atualizações de software** para controlar o comportamento de transferência do pacote de implementação. Este comportamento é gerido pela solução de gestão de atualização através da criação de uma implementação de atualizações agendadas no próximo passo.

1. Na automatização do Azure, selecione **gestão de atualizações**. Criar uma nova implementação seguir os passos descritos no [criar uma implementação de atualização](automation-tutorial-update-management.md#schedule-an-update-deployment) e selecione **importados grupos** no **tipo** pendente para selecionar o adequado Coleção do Configuration Manager. Tenha em atenção os seguintes pontos importantes: um. Se uma janela de manutenção está definida na coleção de dispositivos selecionada do Configuration Manager, os membros da coleção honrá-lo em vez do **duração** definição definida na implementação agendada.
    b. Os membros da coleção de destino têm de ter uma ligação à Internet (direta, através de um servidor proxy ou através do Gateway do OMS).

Depois de concluir a implementação da atualização através da automatização do Azure, os computadores de destino que são membros do grupo do computador selecionado irão instalar atualizações na hora agendada da respetiva cache do cliente local. Pode [ver o estado de implementação da atualização](automation-tutorial-update-management.md#view-results-of-an-update-deployment) para monitorizar os resultados da implementação.

### <a name="manage-software-updates-from-azure-automation"></a>Gerir atualizações de software da automatização do Azure

Para gerir atualizações para as VMs do Windows Server que são clientes do Configuration Manager, precisa de configurar a política de cliente para desativar a funcionalidade de Gestão de Atualizações de Software para todos os clientes geridos por esta solução. Por predefinição, as definições de cliente visam todos os dispositivos na hierarquia. Para obter mais informações sobre esta definição de política e sobre como configurá-la, reveja [Como configurar as definições de cliente no System Center Configuration Manager](/sccm/core/clients/deploy/configure-client-settings).

Depois de efetuar esta alteração de configuração, criar uma nova implementação seguir os passos descritos no [criar uma implementação de atualização](automation-tutorial-update-management.md#schedule-an-update-deployment) e selecione **importados grupos** no **tipo** pendente para selecionar a coleção do Configuration Manager adequado.

## <a name="next-steps"></a>Passos Seguintes
