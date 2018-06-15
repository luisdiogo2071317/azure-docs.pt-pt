---
title: Configurar notificações de estado de funcionamento para sistemas de gestão problema existente, utilizando um webhook | Microsoft Docs
description: Obter personalizadas notificações sobre eventos de estado de funcionamento do serviço para o seu sistema de gestão do problema existente.
author: shawntabrizi
manager: scotthit
editor: ''
services: service-health
documentationcenter: service-health
ms.assetid: ''
ms.service: service-health
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/27/2018
ms.author: shtabriz
ms.openlocfilehash: 8535caf482b10912e6f7bc6df445756094d7603f
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2018
ms.locfileid: "30261417"
---
# <a name="configure-health-notifications-for-existing-problem-management-systems-using-a-webhook"></a>Configurar notificações de estado de funcionamento para sistemas de gestão problema existente, utilizando um webhook

Este artigo mostra como configurar os alertas de estado de funcionamento do serviço para enviar dados através de Webhooks para o sistema de notificação existente.

Atualmente, pode configurar alertas de estado de funcionamento do serviço para que quando um incidente de serviço do Azure ocorrer, seja notificado através de mensagem de texto ou mensagem de correio eletrónico.
No entanto, poderão já ter o sistema externo de notificação existente no local que pretende utilizar.
Este documento mostra as partes mais importantes do payload do webhook e como pode criar alertas personalizados para ser notificado quando resolver problemas do serviço afetá-lo.

Se pretender utilizar uma integração pré-configuradas, consulte como:
* [Configurar alertas com ServiceNow](service-health-alert-webhook-servicenow.md)
* [Configurar alertas com PagerDuty](service-health-alert-webhook-pagerduty.md)
* [Configurar alertas com OpsGenie](service-health-alert-webhook-opsgenie.md)

## <a name="configuring-a-custom-notification-using-the-service-health-webhook-payload"></a>Configurar uma notificação utilizando o payload de webhook de estado de funcionamento do serviço
Se pretender configurar a sua própria integração de webhook personalizado, tem de analisar o payload JSON que é enviado durante a notificações de estado de funcionamento do serviço.

Procure [aqui para ver um exemplo](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md) que o `ServiceHealth` payload de webhook aspeto.

Pode detetar este é um alerta de estado de funcionamento do serviço ao observar `context.eventSource == "ServiceHealth"`. A partir daí, as propriedades que são mais relevantes para a ingestão de são:
 * `data.context.activityLog.status`
 * `data.context.activityLog.level`
 * `data.context.activityLog.subscriptionId`
 * `data.context.activityLog.properties.title`
 * `data.context.activityLog.properties.impactStartTime`
 * `data.context.activityLog.properties.communication`
 * `data.context.activityLog.properties.impactedServices`
 * `data.context.activityLog.properties.trackingId`

## <a name="creating-a-direct-link-to-the-service-health-dashboard-for-an-incident"></a>Criar uma ligação direta para o dashboard de estado de funcionamento do serviço para um incidente
Pode criar uma ligação direta ao seu dashboard do Estado de funcionamento do serviço no ambiente de trabalho ou de dispositivo móvel através da geração de um URL especializado. Utilize o `trackingId`, bem como os primeiro e últimos três dígitos do seu `subscriptionId`, para formar:
```
https://app.azure.com/h/<trackingId>/<first and last three digits of subscriptionId>
```

Por exemplo, se sua `subscriptionId` é `bba14129-e895-429b-8809-278e836ecdb3` e os seus `trackingId` é `0DET-URB`, em seguida, o URL do serviço de estado de funcionamento é:

```
https://app.azure.com/h/0DET-URB/bbadb3
```

## <a name="using-the-level-to-detect-the-severity-of-the-issue"></a>Utilizar o nível para detetar a gravidade do problema
De gravidade mais baixa para gravidade mais elevada, o `level` propriedade no payload pode ser qualquer uma das `Informational`, `Warning`, `Error`, e `Critical`.

## <a name="parsing-the-impacted-services-to-understand-the-full-scope-of-the-incident"></a>Analisar os serviços afectados para compreender o âmbito completo do incidente
Alertas de estado de funcionamento do serviço podem informar sobre problemas em várias regiões e serviços. Para obter os detalhes completos, tem de analisar o valor de `impactedServices`.
O conteúdo dentro é um [JSON escape](http://json.org/) cadeia quando unescaped, contém outro objeto JSON que possa ser analisado regularmente.

```json
{"data.context.activityLog.properties.impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"Australia East\"},{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"Alerts & Metrics\"},{\"ImpactedRegions\":[{\"RegionName\":\"Australia Southeast\"}],\"ServiceName\":\"App Service\"}]"}
```

Torna-se:

```json
[
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia East"
         },
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"Alerts & Metrics"
   },
   {
      "ImpactedRegions":[
         {
            "RegionName":"Australia Southeast"
         }
      ],
      "ServiceName":"App Service"
   }
]
```

Isto mostra que existem problemas com "Alertas e métricas" no Leste da Austrália e Sudeste, bem como problemas com o "Serviço de aplicações" Sudeste da Austrália.


## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Testar a sua integração de webhook através de um pedido POST de HTTP
1. Crie o payload de estado de funcionamento de serviço que pretende enviar. Pode encontrar um payload de webhook do Estado de funcionamento do serviço do exemplo em [Webhooks para a atividade do Azure registar alertas](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md).

2. Crie um pedido POST de HTTP da seguinte forma:

    ```
    POST        https://your.webhook.endpoint

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
3. Deverá receber um `2XX - Successful` resposta.

4. Aceda a [PagerDuty](https://www.pagerduty.com/) para confirmar a sua integração foi configurada com êxito.

## <a name="next-steps"></a>Passos Seguintes
- Reveja o [esquema de webhook alerta de registo de atividade](../monitoring-and-diagnostics/monitoring-activity-log-alerts-webhook.md). 
- Saiba mais sobre [notificações de estado de funcionamento do serviço](../monitoring-and-diagnostics/monitoring-service-notifications.md).
- Saiba mais sobre [grupos ação](../monitoring-and-diagnostics/monitoring-action-groups.md).