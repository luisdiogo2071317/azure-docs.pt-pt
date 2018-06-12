---
title: Criar e gerir grupos de ação no portal do Azure
description: Saiba como criar e gerir grupos de ação no portal do Azure.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/1/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 63216d56fb3acbb954086fbf026441e69073621e
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263070"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Criar e gerir grupos de ação no portal do Azure
## <a name="overview"></a>Descrição geral ##
Um grupo de ação é uma coleção das preferências de notificações definidas pelo utilizador. Alertas de monitorização e estado de funcionamento do serviço do Azure estão configuradas para utilizar um grupo de ação específica, quando o alerta é acionado. Vários alertas podem utilizar o mesmo grupo de ação ou grupos de ação diferente dependendo dos requisitos do utilizador.

Este artigo mostra como criar e gerir grupos de ação no portal do Azure.

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
4. Introduza um nome no **nome do grupo de ação** caixa e introduza um nome no **nome abreviado** caixa. O nome abreviado é utilizado em vez de um nome de grupo de ação completo quando as notificações são enviadas através deste grupo.

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
<dd>Mensagens de correio eletrónico serão enviadas partir os seguintes endereços de e-mail. Certifique-se de que a filtragem de correio eletrónico está corretamente configurada

    - azure-noreply@microsoft.com
    - azureemail-noreply@microsoft.com
    - alerts-noreply@mail.windowsazure.com
    
</dd>
<dd>Pode ter mais de 1000 ações de correio eletrónico num grupo de ação</dd>
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
