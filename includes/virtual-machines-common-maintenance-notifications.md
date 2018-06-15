---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines
author: zivraf
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: zivr
ms.custom: include file
ms.openlocfilehash: 9666a8fde808981dd798ff712b96a7c620c9003a
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
ms.locfileid: "29958899"
---
## <a name="view-vms-scheduled-for-maintenance-in-the-portal"></a>VMs de vista agendadas para manutenção no portal

Assim que estiver agendada uma onda de manutenção planeada e as notificações são enviadas, pode observar a lista de máquinas virtuais que são afetadas por wave a manutenção futura. 

Pode utilizar o portal do Azure e procure VMs agendadas para manutenção.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. No painel de navegação esquerdo, clique em **máquinas virtuais**.

3. No painel de máquinas virtuais, clique o **colunas** botão para abrir a lista de colunas disponíveis.

4. Selecione e adicione as seguintes colunas:

   **Manutenção** -mostra o estado de manutenção para a VM. Seguem-se os valores possíveis:
      
      | Valor | Descrição |
      |-------|-------------|
      | Começar agora | A VM está na janela de manutenção self-service que lhe permite iniciar a manutenção por si. Consulte abaixo sobre como iniciar manutenção a VM | 
      | Agendadas | A VM está agendada para manutenção sem qualquer opção para iniciar a manutenção. Pode saber da janela de manutenção, selecionando a janela de Auto-agendada nesta vista ou clicando na VM | 
      | Concluída | Iniciou com êxito e concluída a manutenção no VM. | 
      | Ignorada| Selecionou para iniciar a manutenção sem sucesso. Não será possível utilizar a opção manutenção do self-service. A VM tem de ser reiniciado pelo Azure durante a fase de manutenção agendada. | 

   **Manutenção profissional para Active Directory** -mostra a janela de tempo quando Self-pode iniciar manutenção nas suas VMs.
   
   **Manutenção agendada** -mostra a janela de tempo, quando o Azure irá reiniciar a VM para concluir a manutenção. 




## <a name="notification-and-alerts-in-the-portal"></a>Notificações e alertas no portal do

Azure comunica um agendamento para a manutenção planeada, enviando um e-mail para o grupo de proprietário e os coproprietários de subscrição. Pode adicionar destinatários adicionais e canais para esta comunicação através da criação de alertas de registo de atividade do Azure. Para obter mais informações, consulte [monitorizar a atividade subscrição com o registo de atividade do Azure] (.. / articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md)

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu à esquerda, selecione **Monitor**. 
3. No **Monitor - registo de atividade** painel, selecione **alertas**.
4. No **Monitor - alertas** painel, clique em **+ Adicionar alerta de registo de atividade**.
5. Preencha as informações no **Adicionar alerta de registo de atividade** e certifique-se de que define o seguinte página **critérios**: **tipo**: manutenção **estado**: Todos os (não definido Estado ativo ou resolvido) **nível**: todos os
    
Para obter mais informações sobre como configurar alertas de registo de atividade, consulte [criam alertas de registo de atividade](../articles/monitoring-and-diagnostics/monitoring-activity-log-alerts.md)
    
    
## <a name="start-maintenance-on-your-vm-from-the-portal"></a>Iniciar manutenção na VM a partir do portal

Ao observar os detalhes VM, poderá ver mais detalhes relacionados com a manutenção.  
Na parte superior da vista de detalhes de VM, será adicionado um novo friso de notificação se a VM está incluída numa onda de manutenção planeada. Além disso, é adicionada uma nova opção para iniciar manutenção sempre que possível. 


Clique na notificação de manutenção para ver a página de manutenção com mais detalhes sobre a manutenção planeada. A partir daí, irá conseguir **iniciar manutenção** na VM.

Assim que começar a manutenção, a máquina virtual será reiniciada e o estado de manutenção será atualizado para refletir o resultado dentro de alguns minutos.

Se em falta a janela de onde pode começar a manutenção, ainda será capaz de ver a janela quando a VM será reiniciada pelo Azure. 
