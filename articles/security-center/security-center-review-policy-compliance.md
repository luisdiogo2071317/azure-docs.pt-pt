---
title: Rever a compatibilidade de política do Centro de segurança com a API REST do Azure | Microsoft Docs
description: Saiba como utilizar as APIs REST da Azure para rever a compatibilidade atual com as políticas do Centro de segurança.
services: security-center
documentationcenter: na
author: lleonard-msft
manager: MBaldwin
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: alleonar
ms.openlocfilehash: 6c6764eec59633f0bdd0fa396c1581117a0c1e1d
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="review-security-center-policy-compliance-using-rest-apis"></a>Compatibilidade de política do Centro de segurança de revisão com REST APIs

Centro de segurança periodicamente valida os recursos do Azure com as políticas de segurança definidas. Centro de segurança também fornece uma API de REST que lhe permite rever a compatibilidade das suas próprias aplicações; Pode consultar o serviço diretamente ou importar resultados JSON para outras aplicações. 

Aqui, saiba obter o conjunto atual de recomendações de todos os recursos do Azure associados uma subscrição.

Para obter o conjunto atual de recomendações:
``` http
GET https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Security/tasks?api-version={api-version}
Content-Type: application/json   
Authorization: Bearer
```

## <a name="build-the-request"></a>Criar o pedido  

O `{subscription-id}` parâmetro é necessário e deve conter o ID de subscrição para a subscrição do Azure, definir as políticas. Se tiver várias subscrições, consulte [trabalhar com várias subscrições](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions).  

O `api-version` é necessário o parâmetro. Neste momento, estes pontos finais são suportados apenas para `api-version=2015-06-01-preview`. 

Os cabeçalhos seguintes são necessários: 

|Cabeçalho de pedido|Descrição|  
|--------------------|-----------------|  
|*Tipo de conteúdo:*|Necessário. Definido como `application/json`.|  
|*Autorização:*|Necessário. Definido como um `Bearer` [token de acesso](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |  

## <a name="response"></a>Resposta  

Código de estado 200 (OK) é devolvido para uma resposta com êxito, que contém uma lista de tarefas recomendadas para proteger os seus recursos do Azure.

``` json
{  
  "value": [  
    {  
       "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Security/locations/{region}/tasks/{task-id}",
       "name": "{task_id}",
       "type": "Microsoft.Security/locations/{region}/tasks",
       "properties": {
       "state": "Active",
       "subState": "NA",
       "creationTimeUtc": "{create-time}",
       "lastStateChangeTimeUtc": "{last-state-change}",
       "securityTaskParameters": "{security-task-properties}"
    } 
  ]  
}  
```  

Cada item na **valor** representa uma recomendação:

|Propriedade de resposta|Descrição|
|----------------|----------|
|**state** | Indica se a recomendação é `active` ou `resolved`. |
|**creationTimeUtc** | Data e hora, em UTC, que mostra que foi criada a recomendação. |
|**lastStateChangeUtc** | Data e hora, no UTV, da última alteração de estado, se aplicável. |
|**securityTaskParameters** | Fornece detalhes sobre a recomendação; propriedades variam de acordo com a recomendação subjacente. |
||
  
Para obter recomendações de atualmente suportadas, consulte [implementar recomendações de segurança](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

Outros códigos de estado indicam condições de erro. Nestes casos, o objeto de resposta inclui uma descrição explicar o motivo pelo qual o pedido falhou.

``` json
{  
  "value": [  
    {  
      "description": "Error response describing why the operation failed."  
    }  
  ]  
}  
```  

## <a name="example-response"></a>Resposta de exemplo  

``` json
{  
  "value": [  
        {
            "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Security/locations/{region}/tasks/{task-id}",
            "name": "{task_id}",
            "type": "Microsoft.Security/locations/{region}/tasks",
            "properties": {
                "state": "Active",
                "subState": "NA",
                "creationTimeUtc": "{create-time}",
                "lastStateChangeTimeUtc": "{last-state-change}",
                "securityTaskParameters": {
                    "vmId": "/subscriptions/{subscription-id}/resourceGroups/{resource_group}/providers/Microsoft.Compute/virtualMachines/{vm_name}",
                    "vmName": "{vm_name}",
                    "severity": "{severity}",
                    "isOsDiskEncrypted": {is_os_disk_encrypted},
                    "isDataDiskEncrypted": {is_data_disk_encrypted},
                    "name": "EncryptionOnVm",
                    "uniqueKey": "EncryptionOnVmTaskParameters_/subscriptions/{subscription-id}/resourceGroups/{resoource_group}/providers/Microsoft.Compute/virtualMachines/{vm_name}",
                    "resourceId": "/subscriptions/{subscription_id}/resourceGroups/{resource_group}/providers/Microsoft.Compute/virtualMachines/{vm_name}"
                }
            }
        },
        {
            "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Security/locations/{location}/tasks/{task-id}",
            "name": "{task-id}",
            "type": "Microsoft.Security/locations/{region}/tasks",
            "properties": {
                "state": "Active",
                "subState": "NA",
                "creationTimeUtc": "{create-time}",
                "lastStateChangeTimeUtc": "{last-state-change}",
                "securityTaskParameters": {
                    "serverName": "{sql-server-name}",
                    "serverId": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Sql/servers/{server-id}",
                    "name": "Enable auditing for the SQL server",
                    "uniqueKey": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Sql/servers/{server-id}/auditingPolicies/Default",
                    "resourceId": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Sql/servers/{server-id}"
                }
            }
        }  ]  
}  
```  

Esta resposta mostra duas recomendações; cada item na lista corresponde a uma recomendação específica. O primeiro recomenda a encriptação de armazenamento numa máquina virtual com Linux e o segundo sugere que ativar a auditoria para um SQL server.

As recomendações variam de acordo com as políticas que tiver ativada. Para obter mais informações, incluindo as recomendações atualmente disponíveis, consulte [gerir recomendações de segurança no Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-recommendations).


## <a name="see-also"></a>Consulte também  
- [Definir políticas de segurança](https://docs.microsoft.com/azure/security-center/security-center-policies-overview)
- [Fornecedor de recursos de segurança do Azure REST API](https://msdn.microsoft.com/library/azure/mt704034.aspx)   
- [Introdução à API REST do Azure](https://docs.microsoft.com/rest/api/azure/)   
- [Módulo do PowerShell do Centro de segurança do Azure](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.22)
