---
title: Criar e gerir grupos de ação no portal do Azure
description: Saiba como criar e gerir grupos de ação no portal do Azure.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 1b66f40594d0dd578e2e5680fb0d5bb0f82f1241
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/26/2018
ms.locfileid: "52311811"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Criar e gerir grupos de ação no portal do Azure
## <a name="overview"></a>Descrição geral ##
Um grupo de ação é uma coleção de preferências de notificações definido pelo utilizador. Alertas de monitorização e estado de funcionamento do serviço do Azure são configuradas para utilizar um grupo de ação específica, quando o alerta é acionado. Vários alertas podem utilizar o mesmo grupo de ação ou a grupos de ação diferentes consoante os requisitos do utilizador.

Este artigo mostra-lhe como criar e gerir grupos de ação no portal do Azure.

Cada ação é constituída pelas seguintes propriedades:

* **Nome**: um identificador exclusivo dentro do grupo de ação.  
* **Tipo de ação**: A ação a efetuar. Os exemplos incluem enviar um e-mail de chamada, SMS, voz; ou acionar vários tipos de ações automatizadas. Ver tipos neste artigo. 
* **Detalhes**: os detalhes de correspondentes que variam de acordo com *tipo de ação*. 

Para obter informações sobre como utilizar os modelos Azure Resource Manager para configurar grupos de ação, veja [modelos do Resource Manager de grupo de ação](monitoring-create-action-group-with-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Criar um grupo de ação com o portal do Azure ##
1. Na [portal](https://portal.azure.com), selecione **Monitor**. O **Monitor** painel consolida todas as suas monitorização definições e dados numa vista.

    ![O serviço de "Monitor"](./media/monitoring-action-groups/home-monitor.png)
1. Selecione **alertas** , em seguida, selecione **gerir grupos de ação**.

    ![Gerir o botão de grupos de ação](./media/monitoring-action-groups/manage-action-groups.png)
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

## <a name="manage-your-action-groups"></a>Gerir os seus grupos de ação ##
Depois de criar um grupo de ação, é visível na **grupos de ação** secção a **Monitor** painel. Selecione o grupo de ação que pretende gerir para:

* Adicionar, editar ou remover ações.
* Elimine o grupo de ação.

## <a name="action-specific-information"></a>Informações específicas da ação
**Aplicação do Azure Push** -pode ter até 10 ações de aplicação do Azure num grupo de ação. Neste momento, a ação de aplicações do Azure só suporta ServiceHealth alertas. Qualquer outra altura alerta será ignorada. Ver [configurar alertas sempre que uma notificação de estado de funcionamento do serviço é lançada](monitoring-activity-log-alerts-on-service-notifications.md).

**E-mail** -serão enviadas mensagens de correio eletrónico dos seguintes endereços de e-mail. Certifique-se de que a filtragem de e-mail está configurado corretamente
   - azure-noreply@microsoft.com
   - azureemail-noreply@microsoft.com
   - alerts-noreply@mail.windowsazure.com

Pode ter até 1000 ações de e-mail num grupo de ação. Consulte a [informações de limitação de taxas](./monitoring-alerts-rate-limiting.md) artigo

**ITSM** -pode ter até 10 ações do ITSM numa ação de ITSM do grupo de ação requer uma ligação de ITSM. Saiba como criar uma [ligação de ITSM](../log-analytics/log-analytics-itsmc-overview.md).

**Aplicação lógica** -pode ter até 10 ações de aplicação lógica num grupo de ação

**Aplicação de função** -as teclas de função para aplicações de função configuradas como ações são lidas por meio da API de funções, o que necessita atualmente de aplicações de funções de v2 para configurar a definição "AzureWebJobsSecretStorageType" para "ficheiros" de aplicação, consulte [ Mudar para a gestão de chaves no V2 funções]( https://aka.ms/funcsecrets) para obter mais informações.

**Runbook** -pode ter até 10 ações do Runbook numa veja de grupo de ação para o [limites do serviço de subscrição do Azure](../azure-subscription-service-limits.md) para limites em cargas de Runbook

**SMS** -pode ter até 10 ações de SMS num grupo de ação que consulte o [informações de limitação de taxas](./monitoring-alerts-rate-limiting.md) consulte o artigo a [comportamento de alertas do SMS](monitoring-sms-alert-behavior.md) artigo

**Voz** -pode ter até 10 ações de voz num grupo de ação</dd>
Consulte a [informações de limitação de taxas](./monitoring-alerts-rate-limiting.md) artigo</dd>

**Webhook** -pode ter até 10 ações de Webhook num grupo de ação. Lógica de repetição - o período de tempo limite para uma resposta é 10 segundos. A chamada de webhook será repetida um máximo de 2 horas quando os seguintes códigos de estado HTTP são devolvidos: 408, 429, 503, 504 ou o ponto final HTTP não responde. Da primeira repetição acontece após 10 segundos. A segunda e última repetição acontece após 100 segundos.

Intervalos de endereços IP de origem
    - 13.106.57.181
    - 13.106.54.3
    - 13.106.54.19
    - 13.106.38.142
    - 13.106.38.148
    - 13.106.57.196

Para receber atualizações sobre as alterações a estes endereços IP, recomendamos que configure uma [alerta de estado de funcionamento do serviço](./monitoring-service-notifications.md) que monitoriza para notificações informativas sobre o serviço de grupos de ação.


## <a name="next-steps"></a>Passos Seguintes ##
* Saiba mais sobre [comportamento de alertas do SMS](monitoring-sms-alert-behavior.md).  
* Obter um [compreensão sobre o esquema de webhook de alerta de registo de atividades](monitoring-activity-log-alerts-webhook.md).  
* Saiba mais sobre [conector ITSM](../log-analytics/log-analytics-itsmc-overview.md)
* Saiba mais sobre [limitação de velocidade](monitoring-alerts-rate-limiting.md) em alertas.
* Obter um [descrição geral dos alertas de registo de atividade](monitoring-overview-alerts.md)e saiba como receber alertas.  
* Saiba como [configurar alertas sempre que uma notificação de estado de funcionamento do serviço é lançada](monitoring-activity-log-alerts-on-service-notifications.md).
