---
title: Interpretar o esquema de registo de auditoria do Azure Active Directory no Azure Monitor (pré-visualização) | Documentos da Microsoft
description: Descrever o esquema de registo de auditoria do Azure AD para utilização no Azure Monitor (pré-visualização)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 6d22758d84d7b5acb3c5f7f1b3f26a9d8ef78b12
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45732181"
---
# <a name="interpret-the-azure-ad-audit-logs-schema-in-azure-monitor-preview"></a>Interpretar o esquema de registos de auditoria do Azure AD no Azure Monitor (pré-visualização)

Este artigo descreve o esquema de registo de auditoria do Azure Active Directory (Azure AD) no Azure Monitor. Cada entrada de log individuais é armazenada como texto e formatada como um blob JSON, conforme mostrado nos exemplos a seguir dois: 

```json
{ 
    "records": [ 
    { 
        "time": "2018-03-17T00:14:31.2585575Z", 
        "operationName": "Change password (self-service)",
        "operationVersion": "1.0",
        "category": "Audit", 
        "tenantId": "bf85dc9d-cb43-44a4-80c4-469e8c58249e", 
        "resultType": "Success", 
        "resultSignature": "-1", 
        "resultDescription": "None", 
        "durationMs": "-1", 
        "correlationId": "60d5e89a-b890-413f-9e25-a047734afe9f", 
        "identity": "sreens@wingtiptoysonline.com", 
        "Level": "Informational", 
        "location": "WUS", 
        "properties": { 
            "identityType": "UPN", 
            "operationType": "Update", 
            "additionalDetails": "None", 
            "additionalTargets": "", 
            "targetUpdatedProperties": "", 
            "targetResourceType": "UPN__TenantContextID__PUID__ObjectID__ObjectClass", 
            "targetResourceName": "sreens@wingtiptoysonline.com__bf85dc9d-cb43-44a4-80c4-469e8c58249e__1003BFFD9FEB17DB__7a408bdd-7d97-4574-8511-dd747b56465d__User", 
            "auditEventCategory": "UserManagement" 
        } 
    } 
    ] 
} 
```

```json
{ 
    "records": [ 
    { 
        "time": "2018-03-18T19:47:43.0368859Z", 
        "operationName": "Update service principal.", 
        "operationVersion": "1.0", 
        "category": "Audit", 
        "tenantId": "bf85dc9d-cb43-44a4-80c4-469e8c58249e", 
        "resultType": "Success", 
        "resultSignature": "-1", 
        "durationMs": "-1", 
        "callerIpAddress": "<null>", 
        "correlationId": "14916c7a-5a7d-44e8-9b06-74b49efb08ee", 
        "identity": "NA", 
        "Level": "Informational", 
        "properties": { 
            "identityType": "NA", 
            "operationType": "Update", 
            "additionalDetails": {}, 
            "additionalTargets": "", 
            "targetUpdatedProperties": [ 
            { 
                "Name": "Included Updated Properties", 
                "OldValue": null, 
                "NewValue": "" 
            }, 
            { 
                "Name": "TargetId.ServicePrincipalNames", 
                "OldValue": null, 
                "NewValue": "http://adapplicationregistry.onmicrosoft.com/salesforce.com/primary;cd3ed3de-93ee-400b-8b19-b61ef44a0f29" 
            } 
            ], 
        "targetResourceType": "Other__ObjectID__ObjectClass__Name__AppId__SPN", 
        "targetResourceName": "ServicePrincipal_ea70a262-4da3-440a-b396-9734ddfd9df2__ea70a262-4da3-440a-b396-9734ddfd9df2__ServicePrincipal__Salesforce__cd3ed3de-93ee-400b-8b19-b61ef44a0f29__http://adapplicationregistry.onmicrosoft.com/salesforce.com/primary;cd3ed3de-93ee-400b-8b19-b61ef44a0f29", 
        "auditEventCategory": "ApplicationManagement" 
      } 
    } 
    ] 
} 
```

## <a name="field-and-property-descriptions"></a>Descrições de campo e de propriedade

| Nome do campo | Descrição |
|------------|-------------|
| hora       | A data e hora (UTC). |
| operationName | O nome da operação. |
| operationVersion | A versão de REST API é solicitada pelo cliente. |
| categoria | Atualmente, *auditoria* é o único valor suportado. |
| TenantId | O GUID associada os registos de inquilino. |
| resultType | O resultado da operação. O resultado pode ser *sucesso* ou *falha*. |
| resultSignature |  Este campo é não mapeado, e pode ignorá-lo com segurança. | 
| resultDescription | Uma descrição adicional do resultado, quando disponível. | 
| durationMs |  Este campo é não mapeado, e pode ignorá-lo com segurança. |
| callerIpAddress | O endereço IP do cliente que efetuou o pedido. | 
| correlationId | Um GUID opcional que é passado pelo cliente. Ele pode ajudar a correlacionar operações do lado do cliente com as operações do lado do servidor e é útil quando está a controlar os registos que abrangem serviços. |
| identidade | A identidade do token que foi apresentado quando efetuou o pedido. A identidade pode ser uma conta de utilizador, a conta de sistema ou o principal de serviço. |
| nível | O tipo de mensagem. Para os registos de auditoria, o nível é sempre *informativo*. |
| localização | A localização do datacenter. |
| propriedades | Lista de propriedades suportadas que estão relacionados com um registo de auditoria. Para obter mais informações, consulte a tabela seguinte. | 

<br>

| Nome da propriedade | Descrição |
|---------------|-------------|
| AuditEventCategory | O tipo de evento de auditoria. Pode ser *gestão de utilizadores*, *gestão de aplicações*, ou outro tipo.|
| Tipo de identidade | O tipo pode ser *aplicativo* ou *utilizador*. |
| Tipo de Operação | O tipo pode ser *Add*, *Update*, *eliminar*. ou *outros*. |
| Tipo de Recurso de Destino | Especifica o tipo de recurso de destino que a operação foi efetuada. O tipo pode ser *aplicativo*, *utilizador*, *função*, *política* | 
| Nome do recurso de destino | O nome do recurso de destino. Pode ser um nome de aplicação, um nome de função, um nome principal de utilizador ou um nome principal de serviço. |
| additionalTargets | Lista as propriedades adicionais para operações específicas. Por exemplo, para uma operação de atualização, os valores antigos e novos valores estão listados na *targetUpdatedProperties*. | 

## <a name="next-steps"></a>Passos Seguintes

* [Interpret sign-in logs schema in Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md) (Interpretar o esquema dos registos de início de sessão no Azure Monitor)
* [Leia mais sobre os registos de diagnóstico do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Perguntas mais frequentes e problemas conhecidos](concept-activity-logs-in-azure-monitor.md#frequently-asked-questions)
