---
title: Agendas de manutenção do Azure (pré-visualização) | Documentos da Microsoft
description: Agendamento de manutenção permite aos clientes planejar os eventos de manutenção agendada necessário que o serviço do Azure SQL Data warehouse utiliza para implementar novas funcionalidades, atualizações e patches.
services: sql-data-warehouse
author: antvgski
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 10/07/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: a6eedc0bac7aab69a9138f4f63d0d9d802e74dfc
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228454"
---
# <a name="change-a-maintenance-schedule"></a>Alterar uma agenda de manutenção 

## <a name="portal"></a>Portal
Um agendamento de manutenção pode ser atualizado ou alterado em qualquer altura. No entanto, se a instância selecionada está atualmente passar pelo ciclo de manutenção do Active Directory, as definições será guardada e só se tornam Active Directory durante o período de manutenção identificado seguinte. [Saiba mais](https://docs.microsoft.com/azure/service-health/resource-health-overview) sobre como monitorizar o seu armazém de dados durante um evento de manutenção do Active Directory. 

Em pré-visualização, podemos irá ser onde poderá selecionar duas janelas de manutenção durante um período de 7 dias. Cada janela de manutenção pode ter entre 3 e 8 horas cada, com 3 horas atualmente a ser a opção disponível mais curta. Manutenção pode ocorrer em qualquer altura dentro de uma janela de manutenção identificados, mas não irá ocorrer fora desses identificado janelas de tempo com notificação anterior, também irá experiência ser breve perda de conectividade, como o service implementa o novo código aos seus dados armazém. 

## <a name="identifying-the-primary-and-secondary-windows"></a>Identificando a primária e secundária do windows

Os windows principais e secundários devem ser identificados nos intervalos de dias separados (ou seja, a janela principal (Terça-feira, Quinta-feira), a janela secundária (Sábado, Domingo)

Conclua os seguintes passos para alterar a agenda de manutenção que foi aplicada ao seu armazém de dados no portal.
1.  Inicie sessão no [portal do Azure](https://portal.azure.com/).
2.  Selecione o armazém de dados que pretende atualizar. No painel de descrição geral, será aberta a página. 
3.  Página de definições da agenda de manutenção pode ser acessada, clicando na manutenção agenda (pré-visualização) resumida ligação no painel de descrição geral ou por meio da opção de agenda de manutenção no Menu da esquerda de recursos.  

    ![Opções do painel de descrição geral](media/sql-data-warehouse-maintenance-scheduling/maintenance-change-option.png)

4. É possível identificar o intervalo de dias preferencial para a janela de manutenção principal com os botões na parte superior da página. Esta seleção determina se a janela principal irá ocorrer num dia da semana ou no fim de semana. A seleção irá atualizar os valores de lista pendente abaixo da mesma forma. Durante a pré-visualização algumas regiões podem ainda não suporta o conjunto completo de opções disponíveis do dia. Estes valores serão atualizados nos próximos meses.

   ![Painel de definições de manutenção](media/sql-data-warehouse-maintenance-scheduling/maintenance-settings-page.png)

5. Escolha sua preferenciais primária e secundária janelas de manutenção com o dia, hora de início e dropdowns de janela de tempo abaixo. A agenda de resumo na parte inferior do painel será atualizado com base nos valores de lista pendente selecionados.

#### <a name="dropdown-options"></a>Opções de menu pendente
- Dia: Dia preferencial para realizar a manutenção durante o período selecionado.
- Hora de início: preferencial a hora de início da janela de manutenção.
- Janela de tempo: preferencial a duração da janela do tempo.

  Depois de selecionar as janelas de manutenção preferencial, clique em Guardar. Será apresentada uma mensagem de confirmação confirmar a que sua nova agenda não está ativa. Se estiver a guardar uma agenda numa região que ainda não suporta o agendamento de manutenção, em seguida, será apresentada a seguinte mensagem. As definições serão guardadas e fique ativas quando o recurso se torna disponível na sua região selecionada.    

    ![Não está ativa no alerta de região](media/sql-data-warehouse-maintenance-scheduling/maintenance-notactive-toast.png)

## <a name="next-steps"></a>Passos Seguintes
- [Saiba mais](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) sobre as ações de Webhook para regras de alerta de registo.
- [Saiba mais](https://docs.microsoft.com/azure/service-health/service-health-overview) sobre o Azure Service Health


