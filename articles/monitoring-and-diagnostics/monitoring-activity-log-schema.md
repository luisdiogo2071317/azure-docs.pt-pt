---
title: Esquema de eventos de registo de atividade do Azure | Microsoft Docs
description: Compreender o esquema de eventos para dados emitidos para o registo de atividade
author: johnkemnetz
manager: robb
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2017
ms.author: johnkem
ms.openlocfilehash: a5c05466b21184a73d08190856e00ae95ee3727f
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="azure-activity-log-event-schema"></a>Esquema de eventos de registo de atividade do Azure
O **registo de atividade do Azure** é um registo que fornece informações sobre quaisquer eventos ao nível da subscrição ocorridas no Azure. Este artigo descreve o esquema de eventos por categoria de dados.

## <a name="administrative"></a>Administrativa
Esta categoria contém o registo de todos os criar, as operações de atualização, eliminação e a ação executada através do Resource Manager. Os exemplos dos tipos de eventos que poderá ver nesta categoria incluem "criar a máquina virtual" e "Eliminar grupo de segurança" cada ação executada por um utilizador ou aplicação utilizando o Gestor de recursos é modelada como uma operação num tipo de recurso específico. Se o tipo de operação é escrita, Delete ou ação, os registos de início e de êxito ou falha dessa operação é registadas no ficheiro a categoria administrativa. A categoria administrativa também inclui todas as alterações para o controlo de acesso baseado em funções numa subscrição.

### <a name="sample-event"></a>Eventos de exemplo
```json
{
    "authorization": {
        "action": "Microsoft.Network/networkSecurityGroups/write",
        "scope": "/subscriptions/dd042f02-6b3e-4f79-939a-6a381ffed3c0/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG"
    },
    "caller": "rob@contoso.com",
    "channels": "Operation",
    "claims": {
        "aud": "https://management.core.windows.net/",
        "iss": "https://sts.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/",
        "iat": "1234567890",
        "nbf": "1234567890",
        "exp": "1234567890",
        "_claim_names": "{\"groups\":\"src1\"}",
        "_claim_sources": "{\"src1\":{\"endpoint\":\"https://graph.windows.net/1114444b-7467-4144-a616-e3a5d63e147b/users/f409edeb-4d29-44b5-9763-ee9348ad91bb/getMemberObjects\"}}",
        "http://schemas.microsoft.com/claims/authnclassreference": "1",
        "aio": "A3GgTJdwK4vy7Fa7l6DgJC2mI0GX44tML385OpU1Q+z+jaPnFMwB",
        "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
        "appid": "355249ed-15d9-460d-8481-84026b065942",
        "appidacr": "2",
        "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "10845a4d-ffa4-4b61-a3b4-e57b9b31cdb5",
        "e_exp": "262800",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Robertson",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Rob",
        "ipaddr": "111.111.1.111",
        "name": "Rob Robertson",
        "http://schemas.microsoft.com/identity/claims/objectidentifier": "f409edeb-4d29-44b5-9763-ee9348ad91bb",
        "onprem_sid": "S-1-5-21-4837261184-168309720-1886587427-18514304",
        "puid": "18247BBD84827C6D",
        "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "b-24Jf94A3FH2sHWVIFqO3-RSJEiv24Jnif3gj7s",
        "http://schemas.microsoft.com/identity/claims/tenantid": "1114444b-7467-4144-a616-e3a5d63e147b",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "rob@contoso.com",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "rob@contoso.com",
        "uti": "IdP3SUJGtkGlt7dDQVRPAA",
        "ver": "1.0"
    },
    "correlationId": "b5768deb-836b-41cc-803e-3f4de2f9e40b",
    "eventDataId": "d0d36f97-b29c-4cd9-9d3d-ea2b92af3e9d",
    "eventName": {
        "value": "EndRequest",
        "localizedValue": "End request"
    },
    "category": {
        "value": "Administrative",
        "localizedValue": "Administrative"
    },
    "eventTimestamp": "2018-01-29T20:42:31.3810679Z",
    "id": "/subscriptions/dd042f02-6b3e-4f79-939a-6a381ffed3c0/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG/events/d0d36f97-b29c-4cd9-9d3d-ea2b92af3e9d/ticks/636528553513810679",
    "level": "Informational",
    "operationId": "04e575f8-48d0-4c43-a8b3-78c4eb01d287",
    "operationName": {
        "value": "Microsoft.Network/networkSecurityGroups/write",
        "localizedValue": "Microsoft.Network/networkSecurityGroups/write"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Network",
        "localizedValue": "Microsoft.Network"
    },
    "resourceType": {
        "value": "Microsoft.Network/networkSecurityGroups",
        "localizedValue": "Microsoft.Network/networkSecurityGroups"
    },
    "resourceId": "/subscriptions/dd042f02-6b3e-4f79-939a-6a381ffed3c0/resourcegroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG",
    "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
    },
    "subStatus": {
        "value": "",
        "localizedValue": ""
    },
    "submissionTimestamp": "2018-01-29T20:42:50.0724829Z",
    "subscriptionId": "dd042f02-6b3e-4f79-939a-6a381ffed3c0",
    "properties": {
        "statusCode": "Created",
        "serviceRequestId": "a4c11dbd-697e-47c5-9663-12362307157d",
        "responseBody": "",
        "requestbody": ""
    },
    "relatedEvents": []
}

```

### <a name="property-descriptions"></a>Descrições das propriedades
| Nome do elemento | Descrição |
| --- | --- |
| Autorização |Blob de propriedades do RBAC do evento. Normalmente, inclui as propriedades "action", "função" e "âmbito". |
| chamador |Endereço de e-mail do utilizador que efetuou a operação, afirmação UPN ou afirmação SPN com base na disponibilidade. |
| canais |Um dos seguintes valores: "Admin", "Operação" |
| afirmações |O token JWT utilizado pelo Active Directory para autenticar o utilizador ou aplicação para efetuar esta operação no Gestor de recursos. |
| correlationId |Normalmente, um GUID no formato de cadeia. Eventos que partilham um correlationId pertencem à mesma ação de uber. |
| descrição |Descrição de texto estático de um evento. |
| eventDataId |Identificador exclusivo de um evento. |
| httpRequest |BLOB que descrevem os pedidos de Http. Normalmente inclui o "clientRequestId", "clientIpAddress" e "method" (método HTTP. Por exemplo, colocar). |
| nível |Nível do evento. Um dos seguintes valores: "Críticas", "Error", "Aviso", "Informativa" e "Verboso" |
| resourceGroupName |Nome do grupo de recursos para o recurso afetado. |
| resourceProviderName |Nome do fornecedor de recursos para o recurso afectado |
| resourceId |Id de recurso do recurso afetado. |
| operationId |Um GUID partilhado entre os eventos que correspondem a uma única operação. |
| operationName |Nome da operação. |
| propriedades |Conjunto de `<Key, Value>` pares (ou seja, um dicionário) que descrevem os detalhes do evento. |
| status |Cadeia que descrevem o estado da operação. Alguns valores comuns são: foi iniciado em curso, com êxito, falha, Active Directory, a resolvido. |
| subStatus |Normalmente, o código de estado HTTP do resto correspondente chamar, mas também pode incluir outras cadeias que descrevem um subestado, como estes valores comuns: OK (código de estado de HTTP: 200), criado (código de estado HTTP: 201), aceite (código de estado HTTP: 202), não conteúdo (código de estado de HTTP: 204), pedido incorreto (código de estado de HTTP: 400), não foi encontrado (código de estado de HTTP: 404), conflito (código de estado HTTP : 409), erro de servidor interno (código de estado HTTP: 500), serviço indisponível (código de estado HTTP: 503), tempo limite do Gateway (código de estado HTTP: 504). |
| eventTimestamp |Timestamp quando o evento foi gerado pelo processamento do pedido correspondente o evento de serviço do Azure. |
| submissionTimestamp |Timestamp quando o evento ficou disponível para consulta. |
| subscriptionId |ID da subscrição do Azure. |

## <a name="service-health"></a>Estado de funcionamento do serviço
Esta categoria contém o registo de qualquer incidentes de estado de funcionamento do serviço ocorridas no Azure. Um exemplo do tipo de evento que vir esta categoria é de "Azure SQL Server nos EUA Leste estão a ocorrer um período de indisponibilidade." Eventos do serviço do Estado de funcionamento são fornecidos em cinco varieties: ação necessária, recuperação assistido, incidentes, manutenção, informações ou segurança e surgem apenas se tiver um recurso na subscrição que seria afetada pelo evento.

### <a name="sample-event"></a>Eventos de exemplo
```json
{
  "channels": "Admin",
  "correlationId": "c550176b-8f52-4380-bdc5-36c1b59d3a44",
  "description": "Active: Network Infrastructure - UK South",
  "eventDataId": "c5bc4514-6642-2be3-453e-c6a67841b073",
  "eventName": {
      "value": null
  },
  "category": {
      "value": "ServiceHealth",
      "localizedValue": "Service Health"
  },
  "eventTimestamp": "2017-07-20T23:30:14.8022297Z",
  "id": "/subscriptions/mySubscriptionID/events/c5bc4514-6642-2be3-453e-c6a67841b073/ticks/636361902148022297",
  "level": "Warning",
  "operationName": {
      "value": "Microsoft.ServiceHealth/incident/action",
      "localizedValue": "Microsoft.ServiceHealth/incident/action"
  },
  "resourceProviderName": {
      "value": null
  },
  "resourceType": {
      "value": null,
      "localizedValue": ""
  },
  "resourceId": "/subscriptions/mySubscriptionID",
  "status": {
      "value": "Active",
      "localizedValue": "Active"
  },
  "subStatus": {
      "value": null
  },
  "submissionTimestamp": "2017-07-20T23:30:34.7431946Z",
  "subscriptionId": "mySubscriptionID",
  "properties": {
    "title": "Network Infrastructure - UK South",
    "service": "Service Fabric",
    "region": "UK South",
    "communication": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "incidentType": "Incident",
    "trackingId": "NA0F-BJG",
    "impactStartTime": "2017-07-20T21:41:00.0000000Z",
    "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"UK South\"}],\"ServiceName\":\"Service Fabric\"}]",
    "defaultLanguageTitle": "Network Infrastructure - UK South",
    "defaultLanguageContent": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "stage": "Active",
    "communicationId": "636361902146035247",
    "version": "0.1.1"
  }
}
```

### <a name="property-descriptions"></a>Descrições das propriedades
Nome do elemento | Descrição
-------- | -----------
canais | Um dos seguintes valores: "Admin", "Operação"
correlationId | Normalmente, é um GUID no formato de cadeia. Eventos com que pertencem à mesma ação de uber normalmente partilhar o mesmo correlationId.
descrição | Descrição do evento.
eventDataId | O identificador exclusivo de um evento.
eventName | O título do evento.
nível | Nível do evento. Um dos seguintes valores: "Críticas", "Error", "Aviso", "Informativa" e "Verboso"
resourceProviderName | Nome do fornecedor de recursos para o recurso afetado. Se não conhecido, este será nulo.
resourceType| O tipo de recurso do recurso afetado. Se não conhecido, este será nulo.
subStatus | Normalmente, é nulo para eventos de estado de funcionamento do serviço.
eventTimestamp | Timestamp quando o registo de eventos foi gerado e submetido para o registo de atividade.
submissionTimestamp |   Timestamp quando o evento ficou disponível no registo de atividade.
subscriptionId | A subscrição do Azure em que este evento foi registado.
status | Cadeia que descrevem o estado da operação. Alguns valores comuns são: Active Directory, resolvido.
operationName | Nome da operação. Normalmente, Microsoft.ServiceHealth/incident/action.
categoria | "ServiceHealth"
resourceId | Id de recurso do recurso afetado, se conhecida. ID de subscrição é fornecido em contrário.
Properties.title | O título localizado para esta comunicação. Inglês é o idioma predefinido.
Properties.Communication | Os detalhes localizados da comunicação com o markup HTML. Inglês é o predefinido.
Properties.incidentType | Os valores possíveis: AssistedRecovery ActionRequired, informações, incidentes, manutenção, a segurança
Properties.trackingId | Identifica o incidente que está associado este evento. Utilize esta opção para correlacionar os eventos relacionados com um incidente.
Properties.impactedServices | Um blob JSON com caráter de escape que descreve os serviços e regiões que são afetadas pelo incidente. Uma lista de serviços, cada um deles tem uma ServiceName e uma lista de ImpactedRegions, cada um deles tem uma RegionName.
Properties.defaultLanguageTitle | A comunicação em inglês
Properties.defaultLanguageContent | A comunicação em inglês como markup de html ou texto simples
Properties.Stage | Os valores possíveis para AssistedRecovery, ActionRequired, informações, incidentes, de segurança: estão ativos, resolvido. Para manutenção são: Active Directory, planeada, em curso, cancelado, Rescheduled, resolvido, concluída
Properties.communicationId | A comunicação este evento está associado.

## <a name="alert"></a>Alerta
Esta categoria contém o registo de todas as ativações de alertas do Azure. Um exemplo do tipo de evento que vir esta categoria é "% de CPU no myVM foi superior a 80 para os últimos 5 minutos." Um conceito de alerta de ter uma variedade de sistemas do Azure – pode definir uma regra de algumas ordenação e receber uma notificação quando as condições corresponderem dessa regra. Sempre que um tipo de alerta do Azure suportado 'ativa,' ou as condições são cumpridas para gerar uma notificação, um registo da ativação também é enviado para esta categoria do registo de atividade.

### <a name="sample-event"></a>Eventos de exemplo

```json
{
  "caller": "Microsoft.Insights/alertRules",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/alertRules"
  },
  "correlationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "description": "'Disk read LessThan 100000 ([Count]) in the last 5 minutes' has been resolved for CloudService: myResourceGroup/Production/Event.BackgroundJobsWorker.razzle (myResourceGroup)",
  "eventDataId": "149d4baf-53dc-4cf4-9e29-17de37405cd9",
  "eventName": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "category": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle/events/149d4baf-53dc-4cf4-9e29-17de37405cd9/ticks/636362258535221920",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "Microsoft.ClassicCompute",
    "localizedValue": "Microsoft.ClassicCompute"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle",
  "resourceType": {
    "value": "Microsoft.ClassicCompute/domainNames/slots/roles",
    "localizedValue": "Microsoft.ClassicCompute/domainNames/slots/roles"
  },
  "operationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "operationName": {
    "value": "Microsoft.Insights/AlertRules/Resolved/Action",
    "localizedValue": "Microsoft.Insights/AlertRules/Resolved/Action"
  },
  "properties": {
    "RuleUri": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert",
    "RuleName": "myalert",
    "RuleDescription": "",
    "Threshold": "100000",
    "WindowSizeInMinutes": "5",
    "Aggregation": "Average",
    "Operator": "LessThan",
    "MetricName": "Disk read",
    "MetricUnit": "Count"
  },
  "status": {
    "value": "Resolved",
    "localizedValue": "Resolved"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T09:24:13.522192Z",
  "submissionTimestamp": "2017-07-21T09:24:15.6578651Z",
  "subscriptionId": "mySubscriptionID"
}
```

### <a name="property-descriptions"></a>Descrições das propriedades
| Nome do elemento | Descrição |
| --- | --- |
| chamador | Sempre Microsoft.Insights/alertRules |
| canais | Sempre "Admin, operação" |
| afirmações | Blob JSON com o tipo de recurso ou SPN (nome principal do serviço), do motor de alerta. |
| correlationId | Um GUID no formato de cadeia. |
| descrição |Descrição de texto estático do evento de alerta. |
| eventDataId |Identificador exclusivo do evento de alerta. |
| nível |Nível do evento. Um dos seguintes valores: "Críticas", "Error", "Aviso", "Informativa" e "Verboso" |
| resourceGroupName |Nome do grupo de recursos para o recurso afetado se se tratar de um alerta de métrico. Para outros tipos de alerta, este é o nome do grupo de recursos que contém o alerta em si. |
| resourceProviderName |Nome do fornecedor de recursos para o recurso afetado se se tratar de um alerta de métrico. Para outros tipos de alerta, este é o nome do fornecedor de recursos para o alerta em si. |
| resourceId | Nome do ID do recurso para o recurso afetado se se tratar de um alerta de métrico. Para outros tipos de alerta, este é o ID de recurso do recurso alerta em si. |
| operationId |Um GUID partilhado entre os eventos que correspondem a uma única operação. |
| operationName |Nome da operação. |
| propriedades |Conjunto de `<Key, Value>` pares (ou seja, um dicionário) que descrevem os detalhes do evento. |
| status |Cadeia que descrevem o estado da operação. Alguns valores comuns são: foi iniciado em curso, com êxito, falha, Active Directory, a resolvido. |
| subStatus | Normalmente, é nulo para alertas. |
| eventTimestamp |Timestamp quando o evento foi gerado pelo processamento do pedido correspondente o evento de serviço do Azure. |
| submissionTimestamp |Timestamp quando o evento ficou disponível para consulta. |
| subscriptionId |ID da subscrição do Azure. |

### <a name="properties-field-per-alert-type"></a>Campo de propriedades por tipo de alerta
O campo de propriedades irá conter valores diferentes consoante a origem do evento de alerta. Dois fornecedores de evento de alerta comuns são alertas de registo de atividade e métricas alertas.

#### <a name="properties-for-activity-log-alerts"></a>Propriedades de alertas de registo de atividade
| Nome do elemento | Descrição |
| --- | --- |
| properties.subscriptionId | O ID de subscrição provenientes do evento do registo de atividade que provocou a esta regra de alerta de registo de atividade ser ativado. |
| properties.eventDataId | O ID de dados de eventos provenientes do evento do registo de atividade que provocou a esta regra de alerta de registo de atividade ser ativado. |
| properties.resourceGroup | O grupo de recursos provenientes do evento do registo de atividade que provocou a esta regra de alerta de registo de atividade ser ativado. |
| properties.resourceId | O ID de recurso provenientes do evento do registo de atividade que provocou a esta regra de alerta de registo de atividade ser ativado. |
| properties.eventTimestamp | O carimbo de eventos do evento de registo de atividade que provocou a esta regra de alerta de registo de atividade ser ativado. |
| properties.operationName | O nome de operação do atividade do registo de eventos que provocou a esta regra de alerta de registo de atividade ser ativado. |
| properties.status | O estado provenientes do evento do registo de atividade que provocou a esta regra de alerta de registo de atividade ser ativado.|

#### <a name="properties-for-metric-alerts"></a>Propriedades de alertas métricas
| Nome do elemento | Descrição |
| --- | --- |
| properties.RuleUri | ID de recurso a métrica de regra de alerta em si. |
| properties.RuleName | O nome da métrica a regra de alerta. |
| properties.RuleDescription | A descrição da regra de alerta métrica (como definido na regra de alerta). |
| properties.Threshold | O valor de limiar utilizado na avaliação de métrica a regra de alerta. |
| properties.WindowSizeInMinutes | O tamanho da janela utilizado na avaliação de métrica de regra de alerta. |
| properties.Aggregation | O tipo de agregação definido na regra de alerta métrica. |
| properties.Operator | O operador condicional utilizado na avaliação de métrica de regra de alerta. |
| properties.MetricName | O nome da métrica da métrica utilizado na avaliação de métrica de regra de alerta. |
| properties.MetricUnit | A unidade métrica para a métrica utilizada na avaliação de métrica de regra de alerta. |

## <a name="autoscale"></a>Dimensionamento Automático
Esta categoria contém o registo de quaisquer eventos relacionados com a operação do motor de dimensionamento automático com base em quaisquer definições de dimensionamento automático que definiu na sua subscrição. Um exemplo do tipo de evento que vir esta categoria é "Escala de dimensionamento automático ação falhou". Utilizar o dimensionamento automático, pode automaticamente aumentar ou reduzir horizontalmente em número de instâncias de um tipo de recurso suportados com base na hora do dia e/ou carga dados (métricos) através de uma definição de dimensionamento automático. Quando as condições são cumpridas à escala, ou reduza verticalmente, o início e foi concluída com êxito ou falha eventos serão registados nesta categoria.

### <a name="sample-event"></a>Eventos de exemplo
```json
{
  "caller": "Microsoft.Insights/autoscaleSettings",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/autoscaleSettings"
  },
  "correlationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "description": "The autoscale engine attempting to scale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
  "eventDataId": "a5b92075-1de9-42f1-b52e-6f3e4945a7c7",
  "eventName": {
    "value": "AutoscaleAction",
    "localizedValue": "AutoscaleAction"
  },
  "category": {
    "value": "Autoscale",
    "localizedValue": "Autoscale"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup/events/a5b92075-1de9-42f1-b52e-6f3e4945a7c7/ticks/636361956518681572",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "microsoft.insights",
    "localizedValue": "microsoft.insights"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup",
  "resourceType": {
    "value": "microsoft.insights/autoscalesettings",
    "localizedValue": "microsoft.insights/autoscalesettings"
  },
  "operationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "operationName": {
    "value": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action",
    "localizedValue": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action"
  },
  "properties": {
    "Description": "The autoscale engine attempting to scale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
    "ResourceName": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource",
    "OldInstancesCount": "3",
    "NewInstancesCount": "2",
    "LastScaleActionTime": "Fri, 21 Jul 2017 01:00:51 GMT"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T01:00:51.8681572Z",
  "submissionTimestamp": "2017-07-21T01:00:52.3008754Z",
  "subscriptionId": "mySubscriptionID"
}

```

### <a name="property-descriptions"></a>Descrições das propriedades
| Nome do elemento | Descrição |
| --- | --- |
| chamador | Always Microsoft.Insights/autoscaleSettings |
| canais | Sempre "Admin, operação" |
| afirmações | Blob JSON com o tipo de recurso ou SPN (nome principal do serviço), do motor de dimensionamento automático. |
| correlationId | Um GUID no formato de cadeia. |
| descrição |Descrição de texto estático do evento de dimensionamento automático. |
| eventDataId |Identificador exclusivo do evento de dimensionamento automático. |
| nível |Nível do evento. Um dos seguintes valores: "Críticas", "Error", "Aviso", "Informativa" e "Verboso" |
| resourceGroupName |Nome do grupo de recursos para a definição de dimensionamento automático. |
| resourceProviderName |Nome do fornecedor de recursos para a definição de dimensionamento automático. |
| resourceId |Id de recurso de definição de dimensionamento automático. |
| operationId |Um GUID partilhado entre os eventos que correspondem a uma única operação. |
| operationName |Nome da operação. |
| propriedades |Conjunto de `<Key, Value>` pares (ou seja, um dicionário) que descrevem os detalhes do evento. |
| properties.Description | Descrição detalhada do que o motor de dimensionamento automático estava a fazer. |
| properties.ResourceName | ID de recurso do recurso afetado (o recursos em que estava a ser efetuada a ação de dimensionamento) |
| properties.OldInstancesCount | O número de instâncias antes da ação de dimensionamento automático demorou em vigor. |
| properties.NewInstancesCount | O número de instâncias depois da ação de dimensionamento automático demorou em vigor. |
| properties.LastScaleActionTime | Timestamp quando ocorreu a ação de dimensionamento automático. |
| status |Cadeia que descrevem o estado da operação. Alguns valores comuns são: foi iniciado em curso, com êxito, falha, Active Directory, a resolvido. |
| subStatus | Normalmente, é nulo para o dimensionamento automático. |
| eventTimestamp |Timestamp quando o evento foi gerado pelo processamento do pedido correspondente o evento de serviço do Azure. |
| submissionTimestamp |Timestamp quando o evento ficou disponível para consulta. |
| subscriptionId |ID da subscrição do Azure. |

## <a name="security"></a>Segurança
Esta categoria contém o registo de todos os alertas gerados pelo centro de segurança do Azure. Um exemplo do tipo de evento que vir esta categoria é "ficheiro de extensão duplo suspeita executado".

### <a name="sample-event"></a>Eventos de exemplo
```json
{
    "channels": "Operation",
    "correlationId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "description": "Suspicious double extension file executed. Machine logs indicate an execution of a process with a suspicious double extension.\r\nThis extension may trick users into thinking files are safe to be opened and might indicate the presence of malware on the system.",
    "eventDataId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "eventName": {
        "value": "Suspicious double extension file executed",
        "localizedValue": "Suspicious double extension file executed"
    },
    "category": {
        "value": "Security",
        "localizedValue": "Security"
    },
    "eventTimestamp": "2017-10-18T06:02:18.6179339Z",
    "id": "/subscriptions/d4742bb8-c279-4903-9653-9858b17d0c2e/providers/Microsoft.Security/locations/centralus/alerts/965d6c6a-a790-4a7e-8e9a-41771b3fbc38/events/965d6c6a-a790-4a7e-8e9a-41771b3fbc38/ticks/636439033386179339",
    "level": "Informational",
    "operationId": "965d6c6a-a790-4a7e-8e9a-41771b3fbc38",
    "operationName": {
        "value": "Microsoft.Security/locations/alerts/activate/action",
        "localizedValue": "Microsoft.Security/locations/alerts/activate/action"
    },
    "resourceGroupName": "myResourceGroup",
    "resourceProviderName": {
        "value": "Microsoft.Security",
        "localizedValue": "Microsoft.Security"
    },
    "resourceType": {
        "value": "Microsoft.Security/locations/alerts",
        "localizedValue": "Microsoft.Security/locations/alerts"
    },
    "resourceId": "/subscriptions/d4742bb8-c279-4903-9653-9858b17d0c2e/providers/Microsoft.Security/locations/centralus/alerts/2518939942613820660_a48f8653-3fc6-4166-9f19-914f030a13d3",
    "status": {
        "value": "Active",
        "localizedValue": "Active"
    },
    "subStatus": {
        "value": null
    },
    "submissionTimestamp": "2017-10-18T06:02:52.2176969Z",
    "subscriptionId": "d4742bb8-c279-4903-9653-9858b17d0c2e",
    "properties": {
        "accountLogonId": "0x2r4",
        "commandLine": "c:\\mydirectory\\doubleetension.pdf.exe",
        "domainName": "hpc",
        "parentProcess": "unknown",
        "parentProcess id": "0",
        "processId": "6988",
        "processName": "c:\\mydirectory\\doubleetension.pdf.exe",
        "userName": "myUser",
        "UserSID": "S-3-2-12",
        "ActionTaken": "Detected",
        "Severity": "High"
    },
    "relatedEvents": []
}

```

### <a name="property-descriptions"></a>Descrições das propriedades
| Nome do elemento | Descrição |
| --- | --- |
| canais | Sempre "operação" |
| correlationId | Um GUID no formato de cadeia. |
| descrição |Descrição de texto estático do evento de segurança. |
| eventDataId |Identificador exclusivo do evento de segurança. |
| eventName |Nome amigável do evento de segurança. |
| ID |Identificador de recurso exclusivo do evento de segurança. |
| nível |Nível do evento. Um dos seguintes valores: "Críticas", "Error", "Aviso", "Informativa" ou "Verboso" |
| resourceGroupName |Nome do grupo de recursos para o recurso. |
| resourceProviderName |Nome do fornecedor de recursos de centro de segurança do Azure. Sempre "Microsoft.Security". |
| resourceType |O tipo de recurso que gerou o evento de segurança, tais como "Microsoft.Security/locations/alerts" |
| resourceId |Id de recurso do alerta de segurança. |
| operationId |Um GUID partilhado entre os eventos que correspondem a uma única operação. |
| operationName |Nome da operação. |
| propriedades |Conjunto de `<Key, Value>` pares (ou seja, um dicionário) que descrevem os detalhes do evento. Estas propriedades irão variar consoante o tipo de alerta de segurança. Consulte [nesta página](../security-center/security-center-alerts-type.md) para obter uma descrição dos tipos de alertas do Centro de segurança. |
| Propriedades. Gravidade |O nível de gravidade. Os valores possíveis são "Alta", "Médio" ou "Baixa." |
| status |Cadeia que descrevem o estado da operação. Alguns valores comuns são: foi iniciado em curso, com êxito, falha, Active Directory, a resolvido. |
| subStatus | Normalmente, é nulo para eventos de segurança. |
| eventTimestamp |Timestamp quando o evento foi gerado pelo processamento do pedido correspondente o evento de serviço do Azure. |
| submissionTimestamp |Timestamp quando o evento ficou disponível para consulta. |
| subscriptionId |ID da subscrição do Azure. |

## <a name="next-steps"></a>Passos Seguintes
* [Saiba mais sobre o registo de atividade (anteriormente os registos de auditoria)](monitoring-overview-activity-logs.md)
* [Fluxo de registo de atividade do Azure para os Event Hubs](monitoring-stream-activity-logs-event-hubs.md)
