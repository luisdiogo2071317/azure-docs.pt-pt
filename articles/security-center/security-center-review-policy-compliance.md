---
title: Rever a conformidade de política do Centro de segurança com API REST do Azure | Documentos da Microsoft
description: Saiba como utilizar APIs REST do Azure para rever a conformidade atual com as políticas do Centro de segurança.
services: security-center
documentationcenter: na
author: lleonard-msft
manager: MBaldwin
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: alleonar
ms.openlocfilehash: 1443486590859aac5591aff2ab0551bed9228d7b
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44301754"
---
# <a name="review-security-center-policy-compliance-using-rest-apis"></a>Conformidade de política do Centro de segurança de revisão através de REST APIs

Centro de segurança periodicamente valida os recursos do Azure com as políticas de segurança definidas. Centro de segurança também fornece uma API REST que permite-lhe rever a conformidade de seus próprios aplicativos; Pode consultar o serviço diretamente ou importar resultados JSON em outros aplicativos. 

Aqui, vai aprender a obter o conjunto atual de recomendações de todos os recursos do Azure associados a uma subscrição.

Para obter o conjunto atual de recomendações:
``` http
GET https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Security/tasks?api-version={api-version}
Content-Type: application/json   
Authorization: Bearer
```

## <a name="build-the-request"></a>Criar o pedido  

O `{subscription-id}` parâmetro é obrigatório e deve conter o ID de subscrição para a subscrição do Azure, definir as políticas. Se tiver várias subscrições, veja [trabalhar com várias subscrições](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions).  

O `api-version` é necessário o parâmetro. Neste momento, estes pontos finais são suportados apenas para `api-version=2015-06-01-preview`. 

Os seguintes cabeçalhos são necessários: 

|Cabeçalho do pedido|Descrição|  
|--------------------|-----------------|  
|*Tipo de conteúdo:*|Necessário. Definido como `application/json`.|  
|*Autorização:*|Necessário. Definido como válido `Bearer` [token de acesso](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |  

## <a name="response"></a>Resposta  

Código de estado 200 (OK) é devolvido para uma resposta com êxito, que contém uma lista de tarefas recomendadas para proteger os recursos do Azure.

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
|**state** | Indica se a recomendação está `active` ou `resolved`. |
|**CreationTimeUtc** | Data e hora, em UTC, que mostra que a recomendação foi criada. |
|**lastStateChangeUtc** | Data e hora, em UTV, da última alteração de estado, se aplicável. |
|**securityTaskParameters** | Fornece detalhes sobre a recomendação; propriedades variam de acordo com a recomendação subjacente. |
||
  
Para obter recomendações atualmente suportadas, veja [implementar recomendações de segurança](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

Outros códigos de estado indicam condições de erro. Nestes casos, o objeto de resposta inclui uma descrição que explica por que o pedido falhou.

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

Esta resposta mostra dois recomendações; cada item na lista corresponde a uma recomendação específica. A primeira recomenda a encriptação de armazenamento na máquina virtual do Linux e o segundo sugere que ativar a auditoria para um SQL server.

As recomendações variam de acordo com as políticas que ativou. Para obter mais informações, incluindo as recomendações atualmente disponíveis, consulte [recomendações de segurança no Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-recommendations).


## <a name="see-also"></a>Consulte também  
- [Definir políticas de segurança](https://docs.microsoft.com/azure/security-center/security-center-policies-overview)
- [API de REST de fornecedor de recursos de segurança do Azure](https://msdn.microsoft.com/library/azure/mt704034.aspx)   
- [Introdução à API REST do Azure](https://docs.microsoft.com/rest/api/azure/)   
- [Módulo de PowerShell do Centro de segurança do Azure](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.22)
