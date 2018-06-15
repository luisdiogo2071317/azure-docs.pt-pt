---
title: Registar inquilinos para utilização de controlo na pilha do Azure | Microsoft Docs
description: Detalhes sobre operações utilizados para gerir registos de inquilino e como a utilização de inquilino está registada na pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2018
ms.author: mabrigg
ms.reviewer: alfredo
ms.openlocfilehash: ef7ca59647a1f8c15d85c809609060a5945bedde
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32159116"
---
# <a name="manage-tenant-registration-in-azure-stack"></a>Gerir o registo de inquilino na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas*

Este artigo contém detalhes sobre operações que pode utilizar para gerir os registos de inquilino e como a utilização de inquilino está registada. Pode encontrar os detalhes sobre como adicionar, lista, ou remova os mapeamentos de inquilino. Pode utilizar o PowerShell ou os pontos finais da API de faturação para gerir a utilização de controlo.

## <a name="add-tenant-to-registration"></a>Adicionar o inquilino para o registo

Esta operação é utilizada quando se pretende adicionar um novo inquilino para o registo, para que a respetiva utilização é comunicada numa subscrição do Azure ligado ao seu inquilino do Azure Active Directory (Azure AD).

Também pode utilizar esta operação se pretender alterar a subscrição associada um inquilino, pode chamar PUT/New-AzureRMResource novamente. O mapeamento antigo é substituído.

Tenha em atenção que apenas uma subscrição do Azure pode ser associada um inquilino. Se tentar adicionar uma segunda subscrição a um inquilino existente, a subscrição da primeira é escrita excessiva. 


| Parâmetro                  | Descrição |
|---                         | --- |
| registrationSubscriptionID | A subscrição do Azure que foi utilizada para o registo inicial. |
| customerSubscriptionID     | A subscrição do Azure (não pilha do Azure) que pertencem ao cliente a ser registado. Tem de ser criada na oferta do fornecedor de serviços em nuvem (CSP). Na prática, isto significa que através do Centro de parceiros. Se um cliente tiver mais do que um inquilino, esta subscrição tem de ser criada no inquilino que será utilizado para iniciar sessão na pilha do Azure. |
| resourceGroup              | O grupo de recursos no Azure no qual o seu registo é armazenado. |
| registrationName           | O nome do registo da pilha do Azure. É um objeto armazenado no Azure. O nome é normalmente no azurestack ponto do formulário-CloudID, onde CloudID é o ID de nuvem da sua implementação de pilha do Azure. |

> [!Note]  
> Os inquilinos necessitam de ser registado com cada pilha do Azure que utilizam. Se um inquilino utiliza mais do que uma pilha do Azure, terá de atualizar os registos de cada implementação iniciais com a subscrição de inquilino.

### <a name="powershell"></a>PowerShell

Utilize o cmdlet New-AzureRmResource para atualizar o recurso de registo. Inicie sessão no Azure (`Add-AzureRmAccount`) utilizando a conta utilizada para o registo inicial. Eis um exemplo de como adicionar um inquilino:

```powershell
  New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties
```

### <a name="api-call"></a>Chamada de API

**Operação**: colocar  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  /providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Resposta**: 201 criado  
**Corpo da resposta**: vazio  

## <a name="list-all-registered-tenants"></a>Listar todos os registado inquilinos

Obter uma lista de todos os inquilinos que foram adicionados a um registo.

 > [!Note]  
 > Não se tiverem sido registados nenhum inquilinos, não recebeu uma resposta.

### <a name="parameters"></a>Parâmetros

| Parâmetro                  | Descrição          |
|---                         | ---                  |
| registrationSubscriptionId | A subscrição do Azure que foi utilizada para o registo inicial.   |
| resourceGroup              | O grupo de recursos no Azure no qual o seu registo é armazenado.    |
| registrationName           | O nome do registo da pilha do Azure. É um objeto armazenado no Azure. O nome é normalmente sob a forma de **azurestack**-***CloudID***, onde ***CloudID*** é o ID de nuvem da sua implementação de pilha do Azure.   |

### <a name="powershell"></a>PowerShell

Utilize o cmdlet Get-AzureRmResovurce para listar todas as registado inquilinos. Inicie sessão no Azure (`Add-AzureRmAccount`) utilizando a conta utilizada para o registo inicial. Eis um exemplo de como adicionar um inquilino:

```powershell
  Get-AzureRmResovurce -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>Chamada de API

Pode obter uma lista de todos os mapeamentos de inquilino utilizando a operação GET

**Operação**: introdução  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions?  
api-version=2017-06-01 HTTP/1.1`  
**Resposta**: 200  
**Corpo da resposta**: 

```JSON  
{
    "value": [{
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 1}”,
            "name": " cspSubscriptionId 1",
            "type": “Microsoft.AzureStack\customerSubscriptions”,
            "properties": { "tenantId": "tId1" }
        },
        {
            "id": " subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{ cspSubscriptionId 2}”,
            "name": " cspSubscriptionId2 ",
            "type": “Microsoft.AzureStack\customerSubscriptions”,
            "properties": { "tenantId": "tId2" }
        }
    ],
    "nextLink": "{originalRequestUrl}?$skipToken={opaqueString}"
}
```

## <a name="remove-a-tenant-mapping"></a>Remover um mapeamento do inquilino

Pode remover um inquilino que tenha sido adicionado a um registo. Se esse inquilino ainda está a utilizar recursos na pilha do Azure, a respetiva utilização é cobrada para a subscrição utilizada no registo de pilha do Azure inicial.

### <a name="parameters"></a>Parâmetros

| Parâmetro                  | Descrição          |
|---                         | ---                  |
| registrationSubscriptionId | ID de subscrição para o registo.   |
| resourceGroup              | O grupo de recursos para o registo.   |
| registrationName           | O nome do registo.  |
| customerSubscriptionId     | O ID de subscrição de cliente.  |

### <a name="powershell"></a>PowerShell

```powershell
  Remove-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>Chamada de API

Pode remover os mapeamentos de inquilino utilizando a operação de eliminação.

**Operação**: eliminar  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Resposta**: 204 nenhum conteúdo  
**Corpo da resposta**: vazio

## <a name="next-steps"></a>Passos Seguintes

 - Para obter mais informações sobre como obter as informações de utilização de recursos de pilha do Azure, consulte o artigo [utilização e faturação na pilha de Azure](/azure-stack-billing-and-chargeback.md).
