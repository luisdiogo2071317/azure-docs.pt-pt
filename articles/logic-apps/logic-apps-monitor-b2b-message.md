---
title: Monitorizar mensagens B2B e configurar o registo - Azure Logic Apps | Documentos da Microsoft
description: Monitorizar AS2, X12 e mensagens EDIFACT. Configure o registo de diagnóstico para a sua conta de integração no Azure Logic Apps.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.date: 07/21/2017
ms.openlocfilehash: decc0271ae0e7c359f72648d7c4d0076892285da
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43125180"
---
# <a name="monitor-b2b-messages-and-set-up-logging-for-integration-accounts-in-azure-logic-apps"></a>Monitorizar mensagens B2B e configurar o registo para contas de integração no Azure Logic Apps

Após configurar a comunicação de B2B entre dois processos de negócios ou aplicativos através da sua conta de integração, a executar essas entidades podem trocar mensagens entre si. Para confirmar esta comunicação funciona conforme esperado, pode configurar a monitorização para AS2, X12, e mensagens EDIFACT, juntamente com o registo de diagnósticos para a sua conta de integração através da [do Azure Log Analytics](../log-analytics/log-analytics-overview.md) serviço. Este serviço monitoriza a sua cloud e ambientes, ajudando-o a manter a disponibilidade e desempenho, no local e também recolhe detalhes de tempo de execução e os eventos de depuração mais avançada. Também pode [utilizar os seus dados de diagnóstico com outros serviços](#extend-diagnostic-data), como o armazenamento do Azure e os Hubs de eventos do Azure.

## <a name="requirements"></a>Requisitos

* Uma aplicação de lógica que está configurada com o registo de diagnósticos. Saiba mais [como configurar o registo para essa aplicação lógica](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

  > [!NOTE]
  > Depois de cumprir este requisito, deve ter uma área de trabalho do Log Analytics. Deve usar a mesma área de trabalho do Log Analytics ao configurar o registo para a sua conta de integração. Se não tiver uma área de trabalho do Log Analytics, saiba [como criar uma área de trabalho do Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md).

* Uma conta de integração que está ligada à sua aplicação lógica. Saiba mais [como criar uma conta de integração com uma ligação à sua aplicação lógica](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).

## <a name="turn-on-diagnostics-logging-for-your-integration-account"></a>Ativar diagnósticos de registo para a sua conta de integração

Pode ativar o registo diretamente a partir de sua conta de integração ou [através do serviço do Azure Monitor](#azure-monitor-service). O Azure Monitor proporciona monitorização básica com dados de nível de infra-estrutura. Saiba mais sobre [do Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md).

### <a name="turn-on-diagnostics-logging-directly-from-your-integration-account"></a>Ativar diagnósticos de registo diretamente a partir da sua conta de integração

1. Na [portal do Azure](https://portal.azure.com), localize e selecione a sua conta de integração. Sob **monitorização**, escolha **os registos de diagnóstico** conforme mostrado aqui:

   ![Localize e selecione a sua conta de integração, escolha "Registos de diagnóstico"](media/logic-apps-monitor-b2b-message/integration-account-diagnostics.png)

2. Depois de selecionar a sua conta de integração, os seguintes valores são automaticamente selecionados. Se estes valores estão corretos, escolha **ativar os diagnósticos**. Caso contrário, selecione os valores que pretende:

   1. Sob **subscrição**, selecione a subscrição do Azure que utiliza com a sua conta de integração.
   2. Sob **grupo de recursos**, selecione o grupo de recursos que utilizar com a sua conta de integração.
   3. Sob **tipo de recurso**, selecione **contas de integração**. 
   4. Sob **recursos**, selecione a sua conta de integração. 
   5. Escolher **ativar os diagnósticos**.

   ![Configurar diagnósticos para a sua conta de integração](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. Sob **as definições de diagnóstico**e, em seguida **estado**, escolha **no**.

   ![Ativar os diagnósticos do Azure](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. Agora, selecione a área de trabalho do Log Analytics e os dados a utilizar para o registo, conforme mostrado:

   1. Selecione **enviar para o Log Analytics**. 
   2. Sob **do Log Analytics**, escolha **configurar**. 
   3. Sob **áreas de trabalho do OMS**, selecione a área de trabalho do Log Analytics a utilizar para o registo.
   4. Sob **Log**, selecione a **IntegrationAccountTrackingEvents** categoria.
   5. Escolha **Guardar**.

   ![Configurar o Log Analytics, para que possa enviar dados de diagnóstico para um registo](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. Agora [configurar o controlo para as mensagens B2B no Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

<a name="azure-monitor-service"></a>

### <a name="turn-on-diagnostics-logging-through-azure-monitor"></a>Ativar o registo de diagnóstico através do Azure Monitor

1. Na [portal do Azure](https://portal.azure.com), no menu principal do Azure, escolha **Monitor**, **registos de diagnóstico**. Em seguida, selecione a sua conta de integração conforme mostrado aqui:

   ![Escolha "Monitorizar", "Registos de diagnóstico", selecione a sua conta de integração](media/logic-apps-monitor-b2b-message/monitor-service-diagnostics-logs.png)

2. Depois de selecionar a sua conta de integração, os seguintes valores são automaticamente selecionados. Se estes valores estão corretos, escolha **ativar os diagnósticos**. Caso contrário, selecione os valores que pretende:

   1. Sob **subscrição**, selecione a subscrição do Azure que utiliza com a sua conta de integração.
   2. Sob **grupo de recursos**, selecione o grupo de recursos que utilizar com a sua conta de integração.
   3. Sob **tipo de recurso**, selecione **contas de integração**.
   4. Sob **recursos**, selecione a sua conta de integração.
   5. Escolher **ativar os diagnósticos**.

   ![Configurar diagnósticos para a sua conta de integração](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. Sob **as definições de diagnóstico**, escolha **no**.

   ![Ativar os diagnósticos do Azure](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. Agora, selecione a categoria de eventos e de área de trabalho do Log Analytics para o registo conforme mostrado:

   1. Selecione **enviar para o Log Analytics**. 
   2. Sob **do Log Analytics**, escolha **configurar**. 
   3. Sob **áreas de trabalho do OMS**, selecione a área de trabalho do Log Analytics a utilizar para o registo.
   4. Sob **Log**, selecione a **IntegrationAccountTrackingEvents** categoria.
   5. Quando tiver terminado, escolha **Save** (Guardar).

   ![Configurar o Log Analytics, para que possa enviar dados de diagnóstico para um registo](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. Agora [configurar o controlo para as mensagens B2B no Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a>Expandir como e onde utiliza dados de diagnóstico com outros serviços

Juntamente com o Azure Log Analytics, pode expandir como utilizar dados de diagnóstico da sua aplicação lógica com outros serviços do Azure, por exemplo: 

* [Arquivar registos do diagnóstico do Azure no armazenamento do Azure](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [Registos de diagnóstico do Azure Stream para os Hubs de eventos do Azure](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

Pode, em seguida, a monitorização utilizando a telemetria e de análise a partir de outros serviços, como get em tempo real [do Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) e [Power BI](../log-analytics/log-analytics-powerbi.md). Por exemplo:

* [Dados de Stream dos Hubs de eventos para o Stream Analytics](../stream-analytics/stream-analytics-define-inputs.md)
* [Analisar dados de transmissão em fluxo com o Stream Analytics e criar um dashboard de análise em tempo real no Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Com base nas opções que pretende configurar, certifique-se de que primeiro [criar uma conta de armazenamento do Azure](../storage/common/storage-create-storage-account.md) ou [criar um hub de eventos do Azure](../event-hubs/event-hubs-create.md). Em seguida, selecione as opções para onde pretende enviar dados de diagnóstico:

![Enviar dados para o hub de evento ou a conta de armazenamento do Azure](./media/logic-apps-monitor-b2b-message/storage-account-event-hubs.png)

> [!NOTE]
> Os períodos de retenção aplicam-se apenas quando optar por utilizar uma conta de armazenamento.

## <a name="supported-tracking-schemas"></a>Suporte a esquemas de controlo

O Azure suporta estes tipos de esquema, os quais corrigiram esquemas, exceto o tipo personalizado de controlo.

* [Esquemas de controlo de AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Esquemas de controlo de X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Personalizar esquema de controlo](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Passos Seguintes

* [Monitorizar mensagens B2B no Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md "mensagens controlar B2B no OMS")
* [Saiba mais sobre o Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Saiba mais sobre o Enterprise Integration Pack")

