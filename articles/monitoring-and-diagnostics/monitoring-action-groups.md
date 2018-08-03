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
ms.openlocfilehash: 091a097fc9fafd5bdc6a2521f4fa2a1b6b77ba4c
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39422558"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Criar e gerir grupos de ação no portal do Azure
## <a name="overview"></a>Descrição geral ##
Um grupo de ação é uma coleção de preferências de notificações definido pelo utilizador. Alertas de monitorização e estado de funcionamento do serviço do Azure são configuradas para utilizar um grupo de ação específica, quando o alerta é acionado. Vários alertas podem utilizar o mesmo grupo de ação ou a grupos de ação diferentes consoante os requisitos do utilizador.

Este artigo mostra-lhe como criar e gerir grupos de ação no portal do Azure.

Cada ação é constituída pelas seguintes propriedades:

* **Nome**: um identificador exclusivo dentro do grupo de ação.  
* **Tipo de ação**: enviar uma chamada de voz ou SMS, enviar um e-mail, chamar um webhook, enviar dados para uma ferramenta de ITSM, chamar uma aplicação lógica, enviar uma notificação push para a aplicação do Azure ou executar um runbook da automatização.
* **Detalhes**: correspondentes de telefone número, endereço de e-mail, webhook URI ou detalhes de ligação de ITSM.

Para obter informações sobre como utilizar os modelos Azure Resource Manager para configurar grupos de ação, veja [modelos do Resource Manager de grupo de ação](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Criar um grupo de ação com o portal do Azure ##
1. Na [portal](https://portal.azure.com), selecione **Monitor**. O **Monitor** painel consolida todas as suas monitorização definições e dados numa vista.

    ![O serviço de "Monitor"](./media/monitoring-action-groups/home-monitor.png)
1. Na **configurações** secção, selecione **grupos de ação**.

    ![O separador "Grupos de ação"](./media/monitoring-action-groups/action-groups-blade.png)
1. Selecione **grupo de ação de adicionar**e preencha os campos.

    ![O comando "Adicionar grupo de ação"](./media/monitoring-action-groups/add-action-group.png)
1. Introduza um nome na **nome do grupo de ação** caixa e introduza um nome na **nome abreviado** caixa. O nome abreviado é utilizado em vez de um nome de grupo de ação completo quando as notificações são enviadas através deste grupo.

      ![Caixa de diálogo do grupo de ação de adicionar"](./media/monitoring-action-groups/action-group-define.png)

1. O **subscrição** caixa autofills com a sua subscrição atual. Esta subscrição é aquela na qual o grupo de ação é guardado.

1. Selecione o **grupo de recursos** no qual o grupo de ação é guardado.

1. Defina uma lista de ações ao fornecer a cada ação:

    a. **Nome**: introduza um identificador exclusivo para esta ação.

    b. **Tipo de ação**: selecionar E-Mail/SMS/Push/voz, aplicação lógica, Webhook, ITSM ou Runbook de automatização.

    c. **Detalhes**: com base no tipo de ação, introduza um número de telefone, endereço de e-mail, webhook URI, aplicações do Azure, a ligação ITSM ou runbook de automatização. Para a ação de ITSM, adicionalmente especificar **Item de trabalho** e requer a sua ferramenta ITSM outros campos.

1. Selecione **OK** para criar o grupo de ação.

## <a name="action-specific-information"></a>Informações específicas da ação
<dl>
<dt>Push da aplicação do Azure</dt>
<dd>Pode ter até 10 ações de aplicação do Azure num grupo de ação.</dd>
<dd>Neste momento, a ação de aplicações do Azure só suporta ServiceHealth alertas. Qualquer outra altura alerta será ignorada. Ver [configurar alertas sempre que uma notificação de estado de funcionamento do serviço é lançada](monitoring-activity-log-alerts-on-service-notifications.md).</dd>

<dt>e-mail</dt>
<dd>Mensagens de correio eletrónico serão enviadas dos seguintes endereços de e-mail. Certifique-se de que a filtragem de e-mail está configurado corretamente

    - azure-noreply@microsoft.com
    - azureemail-noreply@microsoft.com
    - alerts-noreply@mail.windowsazure.com
    
</dd>
<dd>Talvez tenha até 1000 ações de e-mail num grupo de ação</dd>
<dd>Consulte a [informações de limitação de taxas](./monitoring-alerts-rate-limiting.md) artigo</dd>

<dt>ITSM</dt>
<dd>Pode ter até 10 ações do ITSM um grupo de ação</dd>
<dd>Ação de ITSM requer uma ligação de ITSM. Saiba como criar uma [ligação de ITSM](../log-analytics/log-analytics-itsmc-overview.md).</dd>

<dt>Aplicação lógica</dt>
<dd>Pode ter até 10 ações de aplicação lógica num grupo de ação</dd>

<dt>Runbook</dt>
<dd>Pode ter até 10 ações de Runbook num grupo de ação</dd>

<dt>SMS</dt>
<dd>Pode ter até 10 ações de SMS num grupo de ação</dd>
<dd>Consulte a [informações de limitação de taxas](./monitoring-alerts-rate-limiting.md) artigo</dd>
<dd>Consulte a [comportamento de alertas do SMS](monitoring-sms-alert-behavior.md) artigo</dd>

<dt>Voz</dt>
<dd>Pode ter até 10 ações de voz num grupo de ação</dd>
<dd>Consulte a [informações de limitação de taxas](./monitoring-alerts-rate-limiting.md) artigo</dd>

<dt>Webhook</dt>
<dd>Pode ter até 10 ações de Webhook num grupo de ação
<dd>Lógica de repetição - o período de tempo limite para uma resposta é 10 segundos. A chamada de webhook será repetida um máximo de 2 horas quando os seguintes códigos de estado HTTP são devolvidos: 408, 429, 503, 504 ou o ponto final HTTP não responde. Da primeira repetição acontece após 10 segundos. A segunda e última repetição acontece após 100 segundos.</dd>
</dl>

## <a name="manage-your-action-groups"></a>Gerir os seus grupos de ação ##
Depois de criar um grupo de ação, é visível na **grupos de ação** secção a **Monitor** painel. Selecione o grupo de ação que pretende gerir para:

* Adicionar, editar ou remover ações.
* Elimine o grupo de ação.

## <a name="next-steps"></a>Passos Seguintes ##
* Saiba mais sobre [comportamento de alertas do SMS](monitoring-sms-alert-behavior.md).  
* Obter um [compreensão sobre o esquema de webhook de alerta de registo de atividades](monitoring-activity-log-alerts-webhook.md).  
* Saiba mais sobre [conector ITSM](../log-analytics/log-analytics-itsmc-overview.md)
* Saiba mais sobre [limitação de velocidade](monitoring-alerts-rate-limiting.md) em alertas.
* Obter um [descrição geral dos alertas de registo de atividade](monitoring-overview-alerts.md)e saiba como receber alertas.  
* Saiba como [configurar alertas sempre que uma notificação de estado de funcionamento do serviço é lançada](monitoring-activity-log-alerts-on-service-notifications.md).
