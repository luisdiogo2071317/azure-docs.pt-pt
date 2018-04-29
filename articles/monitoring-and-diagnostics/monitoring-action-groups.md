---
title: Criar e gerir grupos de ação no portal do Azure | Microsoft Docs
description: Saiba como criar e gerir grupos de ação no portal do Azure.
author: dkamstra
manager: chrad
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2018
ms.author: dukek
ms.openlocfilehash: 07e3c1a95aa223121117f3deba0269fb6cc280c2
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Criar e gerir grupos de ação no portal do Azure
## <a name="overview"></a>Descrição geral ##
Este artigo mostra como criar e gerir grupos de ação no portal do Azure.

Pode configurar uma lista de ações com grupos de ação. Estes grupos, em seguida, podem ser utilizados por cada alerta que definir, garantindo que as mesmas ações efetuadas sempre que um alerta é acionado.

Cada ação é constituída pelas seguintes propriedades:

* **Nome**: um identificador exclusivo dentro do grupo de ação.  
* **Tipo de ação**: enviar uma chamada de voz ou um SMS, envie um e-mail, chamar um webhook, enviar dados para uma ferramenta ITSM, chamar uma aplicação lógica, enviar uma notificação push para a aplicação do Azure, ou executar um runbook de automatização.
* **Detalhes**: correspondente phone número, endereço de correio eletrónico, webhook URI ou ITSM detalhes de ligação.

Para obter informações sobre como utilizar os modelos Azure Resource Manager para configurar grupos de ação, consulte [modelos de Gestor de recursos do grupo de ação](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Criar um grupo de ação, utilizando o portal do Azure ##
1. No [portal](https://portal.azure.com), selecione **Monitor**. O **Monitor** painel consolida todas as suas monitorização definições e dados de uma vista.

    ![O serviço de "Monitor de"](./media/monitoring-action-groups/home-monitor.png)
2. No **definições** secção, selecione **grupos ação**.

    ![No separador "Grupos de ação"](./media/monitoring-action-groups/action-groups-blade.png)
3. Selecione **adicionar grupo de ação**e preencha os campos.

    ![O comando "Adicionar grupo de ação"](./media/monitoring-action-groups/add-action-group.png)
4. Introduza um nome no **nome do grupo de ação** caixa e introduza um nome no **nome abreviado** caixa. O nome abreviado é utilizado em vez de um nome de grupo ação completa, quando as notificações são enviadas através deste grupo.

      ![Caixa de diálogo de grupo de ação adicionar"](./media/monitoring-action-groups/action-group-define.png)

5. O **subscrição** caixa autofills com a sua subscrição atual. Esta subscrição é o que o grupo de ação é guardado.

6. Selecione o **grupo de recursos** no qual o grupo de ação é guardado.

7. Defina uma lista de ações, fornecendo cada ação:

    a. **Nome**: introduza um identificador exclusivo para esta ação.

    b. **Tipo de ação**: selecione E-Mail/SMS/Push/voz, o aplicação lógica, o Webhook, o ITSM ou o Runbook de automatização.

    c. **Detalhes**: com base no tipo de ação, introduza um número de telefone, endereço de correio eletrónico, webhook URI, aplicações do Azure, ligação ITSM ou runbook de automatização. Além disso especificar para a ação de ITSM, **Item de trabalho** e requer a sua ferramenta ITSM outros campos.

8. Selecione **OK** ao criar o grupo de ação.

## <a name="action-specific-information"></a>Informações específicas de ação
<dl>
<dt>Push de aplicações do Azure</dt>
<dd>Pode ter até 10 ações de aplicação do Azure num grupo de ação.</dd>
<dd>Neste momento, a ação de aplicações do Azure suporta apenas ServiceHealth alertas. Outro alerta sempre que será ignorado. Consulte [configurar alertas sempre que uma notificação de estado de funcionamento do serviço é publicada](monitoring-activity-log-alerts-on-service-notifications.md).</dd>

<dt>Correio eletrónico</dt>
<dd>Pode ter até 50 ações de correio eletrónico num grupo de ação</dd>
<dd>Consulte o [taxa de limitação de informações](./monitoring-alerts-rate-limiting.md) artigo</dd>

<dt>ITSM</dt>
<dd>Pode ter até 10 ações de ITSM num grupo de ação</dd>
<dd>Ação de ITSM requer uma ligação de ITSM. Saiba como criar um [ITSM ligação](../log-analytics/log-analytics-itsmc-overview.md).</dd>

<dt>Aplicação lógica</dt>
<dd>Pode ter até 10 ações de aplicação lógica num grupo de ação</dd>

<dt>runbook</dt>
<dd>Pode ter até 10 Runbook ações num grupo de ação</dd>

<dt>SMS</dt>
<dd>Pode ter até 10 ações de SMS num grupo de ação</dd>
<dd>Consulte o [taxa de limitação de informações](./monitoring-alerts-rate-limiting.md) artigo</dd>
<dd>Consulte o [SMS alerta comportamento](monitoring-sms-alert-behavior.md) artigo</dd>

<dt>voz</dt>
<dd>Pode ter até 10 ações de voz num grupo de ação</dd>
<dd>Consulte o [taxa de limitação de informações](./monitoring-alerts-rate-limiting.md) artigo</dd>

<dt>Webhook</dt>
<dd>Pode ter até 10 ações de Webhook num grupo de ação
<dd>Repetir lógica - o período de tempo limite para uma resposta é 10 segundos. A chamada de webhook será tentada novamente um máximo de 2 horas quando os seguintes códigos de estado HTTP são devolvidos: 408, 429, 503, 504 ou o ponto final de HTTP não responde. A primeira tentativa ocorre após 10 segundos. O segundo e o último novamente ocorre após 100 segundos.</dd>
</dl>

## <a name="manage-your-action-groups"></a>Gerir os grupos de ação ##
Depois de criar um grupo de ação, fica visível no **grupos ação** secção o **Monitor** painel. Selecione o grupo de ação que pretende gerir:

* Adicionar, editar ou remover as ações.
* Elimine o grupo de ação.

## <a name="next-steps"></a>Passos Seguintes ##
* Saiba mais sobre [SMS alerta comportamento](monitoring-sms-alert-behavior.md).  
* Obter um [compreensão sobre o esquema de webhook alerta de registo de atividade](monitoring-activity-log-alerts-webhook.md).  
* Saiba mais sobre [ITSM conector](../log-analytics/log-analytics-itsmc-overview.md)
* Saiba mais sobre [limitação de taxa](monitoring-alerts-rate-limiting.md) em alertas.
* Obter um [descrição geral dos alertas de registo de atividade](monitoring-overview-alerts.md)e saber como receber alertas.  
* Saiba como [configurar alertas sempre que uma notificação de estado de funcionamento do serviço é publicada](monitoring-activity-log-alerts-on-service-notifications.md).
