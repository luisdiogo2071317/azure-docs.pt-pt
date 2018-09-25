---
title: Gerenciar Estados de alerta e inteligentes de grupo
description: Gerir os Estados das instâncias do grupo de alerta e inteligente
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: f1c4e34f9c14a9ca273cd115c653f8075286ee2a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46994552"
---
# <a name="manage-alert-and-smart-group-states"></a>Gerenciar Estados de alerta e inteligentes de grupo
Alertas no Azure Monitor agora tem um [estado e uma condição de monitor de alerta](https://aka.ms/azure-alerts-overview) e, da mesma forma, os grupos de inteligente têm um [inteligente o estado do grupo](https://aka.ms/smart-groups). Alterações ao estado agora são capturadas no histórico associado com o respetivo grupo de alerta ou inteligente. Este artigo orienta-o pelo processo de alteração do Estado, para um alerta e um grupo inteligente.

## <a name="change-the-state-of-an-alert"></a>Alterar o estado de um alerta
1. Pode alterar o estado de um alerta das seguintes formas diferentes: 
    * Na página de todos os alertas, clique na caixa de verificação junto aos alertas que pretende alterar o estado da e clique em Alterar Estado.   
    ![Monitorização](./media/monitoring-alerts-managing-alert-states/state-all-alerts.jpg)
    * Na página de detalhes do alerta para uma determinada instância de alerta, pode clicar em estado de alteração   
    ![Monitorização](./media/monitoring-alerts-managing-alert-states/state-alert-details.jpg)
    * Na página de detalhes do alerta para uma instância de alerta específica, no painel do grupo de Smart pode clicar na caixa de verificação junto aos alertas que pretende    
    ![Monitorização](./media/monitoring-alerts-managing-alert-states/state-alert-details-sg.jpg)

    * Na página de detalhes do grupo inteligente, na lista de alertas de membro pode clicar a caixa de verificação junto aos alertas que pretende alterar o estado da e clique em alterar Stateto alterar o estado do e clique em Alterar Estado.   
    ![Monitorização](./media/monitoring-alerts-managing-alert-states/state-sg-details-alerts.jpg)
1. Ao clicar em estado de alteração, abre um pop-up permitindo que selecione o estado (New/confirmado/fechado) e introduza um comentário, se necessário.   
![Monitorização](./media/monitoring-alerts-managing-alert-states/state-alert-change.jpg)
1. Depois de o fazer, a alteração de estado é registrada no histórico do alerta correspondente. Isso pode ser exibido ao abrir a respectiva página de detalhes e a verificar a secção de histórico.    
![Monitorização](./media/monitoring-alerts-managing-alert-states/state-alert-history.jpg)

## <a name="change-the-state-of-a-smart-group"></a>Alterar o estado de um grupo inteligente
1. Pode alterar o estado de um grupo de smart das seguintes formas diferentes:
    1. Na página de lista do grupo de Smart, pode clicar na caixa de verificação junto aos grupos inteligentes que pretende alterar o estado da e clique em Alterar Estado  
    ![Monitorização](./media/monitoring-alerts-managing-alert-states/state-sg-list.jpg)
    1. Na página de detalhes do grupo inteligente, pode clicar em estado de alteração        
    ![Monitorização](./media/monitoring-alerts-managing-alert-states/state-sg-details.jpg)
1. Ao clicar em estado de alteração, abre um pop-up permitindo que selecione o estado (New/confirmado/fechado) e introduza um comentário, se necessário. 
![Monitorização](./media/monitoring-alerts-managing-alert-states/state-sg-change.jpg)
   > [!NOTE]
   >  Alterar o estado de um grupo inteligente não altera o estado dos alertas membro individual.

1. Depois de o fazer, a alteração de estado é registrada na história do respetivo grupo inteligente. Isso pode ser exibido ao abrir a respectiva página de detalhes e a verificar a secção de histórico.     
![Monitorização](./media/monitoring-alerts-managing-alert-states/state-sg-history.jpg)
