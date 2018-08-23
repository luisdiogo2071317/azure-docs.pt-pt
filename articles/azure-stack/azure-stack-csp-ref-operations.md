---
title: Registe-se de inquilinos para utilização de controlo no Azure Stack | Documentos da Microsoft
description: Detalhes sobre as operações utilizados para gerir registos de inquilino e como a utilização de inquilino está registada no Azure Stack.
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
ms.date: 06/08/2018
ms.author: brenduns
ms.reviewer: alfredo
ms.openlocfilehash: bb46881425398618df54288a9d2e6d65bb03dad4
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "42061512"
---
# <a name="manage-tenant-registration-in-azure-stack"></a>Gerir o registo do inquilino no Azure Stack

*Aplica-se a: sistemas integrados do Azure Stack*

Este artigo contém detalhes sobre as operações que pode utilizar para gerir os registos de inquilino e como a utilização do inquilino é rastreada. Pode encontrar detalhes sobre como adicionar, lista, ou remova os mapeamentos de inquilino. Pode utilizar o PowerShell ou os pontos finais de API de faturação para gerir a utilização de controlo.

## <a name="add-tenant-to-registration"></a>Adicionar o inquilino para registo

Esta operação é utilizada quando pretende adicionar um novo inquilino para o registo, para que seu uso é reportado numa subscrição do Azure ligado ao seu inquilino do Azure Active Directory (Azure AD).

Também pode usar essa operação se pretender alterar a subscrição associada um inquilino, pode chamar PUT/New-AzureRMResource novamente. O mapeamento antigo é substituído.

Tenha em atenção que apenas uma subscrição do Azure pode ser associada a um inquilino. Se tentar adicionar uma segunda assinatura a um inquilino existente, a primeira assinatura é escrita excessiva. 

### <a name="use-api-profiles"></a>Utilizar perfis de API

Os cmdlets neste artigo exigem que especifique um perfil de API quando a executar o PowerShell. Perfis de API representam um conjunto de fornecedores de recursos do Azure e suas versões de API. Eles ajudam a utilizar a versão correta da API ao interagir com múltiplas clouds do Azure, por exemplo ao trabalhar com global do Azure e o Azure Stack. Os perfis são especificados por um nome que corresponde à respetiva data de lançamento. Neste artigo, terá de utilizar o **2017-09-03** perfil.

Para obter mais informações sobre o Azure Stack e perfis de API, consulte [perfis de versão de API de gerir no Azure Stack](user/azure-stack-version-profiles.md). Para obter instruções sobre como colocar em funcionamento com o perfil de API com o PowerShell, consulte [perfis de versão de API de utilização para o PowerShell no Azure Stack](user/azure-stack-version-profiles-powershell.md).

### <a name="parameters"></a>Parâmetros

| Parâmetro                  | Descrição |
|---                         | --- |
| registrationSubscriptionID | A subscrição do Azure que foi utilizada para o registo inicial. |
| customerSubscriptionID     | A subscrição do Azure (não o Azure Stack) pertencentes ao cliente sejam registrados. Tem de ser criada na oferta do fornecedor de serviços Cloud (CSP). Na prática, isso significa através do Centro de parceiros. Se um cliente tiver mais do que um inquilino, esta subscrição tem de ser criada no inquilino que será utilizado para iniciar sessão no Azure Stack. |
| resourceGroup              | O grupo de recursos no Azure em que o registo é armazenado. |
| registrationName           | O nome do registo do seu Azure Stack. É um objeto armazenado no Azure. O nome, normalmente, está a ser azurestack ponto o formulário-CloudID, onde CloudID é o ID de Cloud da sua implementação do Azure Stack. |

> [!Note]  
> Os inquilinos precisam de estar registado em cada Azure Stack que utilizam. Se um inquilino utiliza mais do que um Azure Stack, terá de atualizar os registros de iniciais de cada implementação com a subscrição do inquilino.

### <a name="powershell"></a>PowerShell

Utilize o cmdlet New-AzureRmResource para atualizar o recurso de Registro. Inicie sessão no Azure (`Add-AzureRmAccount`) usando a conta que utilizou para o registo inicial. Eis um exemplo de como adicionar um inquilino:

```powershell
  New-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/{customerSubscriptionId}" -ApiVersion 2017-06-01 -Properties
```

### <a name="api-call"></a>Chamada à API

**Operação**: colocar  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  /providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Resposta**: 201 criado  
**Corpo da resposta**: vazio  

## <a name="list-all-registered-tenants"></a>Listar registados todos os inquilinos

Obter uma lista de todos os inquilinos que foram adicionados a um registo.

 > [!Note]  
 > Se tiveram sido registados não inquilinos, não receberá nenhuma resposta.

### <a name="parameters"></a>Parâmetros

| Parâmetro                  | Descrição          |
|---                         | ---                  |
| registrationSubscriptionId | A subscrição do Azure que foi utilizada para o registo inicial.   |
| resourceGroup              | O grupo de recursos no Azure em que o registo é armazenado.    |
| registrationName           | O nome do registo do seu Azure Stack. É um objeto armazenado no Azure. O nome é normalmente na forma de **azurestack**-***CloudID***, em que ***CloudID*** é o ID de Cloud da sua implementação do Azure Stack.   |

### <a name="powershell"></a>PowerShell

Utilize o cmdlet Get-AzureRmResource para listar registados todos os inquilinos. Inicie sessão no Azure (`Add-AzureRmAccount`) usando a conta que utilizou para o registo inicial. Eis um exemplo de como adicionar um inquilino:

```powershell
  Get-AzureRmResource -ResourceId "subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions" -ApiVersion 2017-06-01
```

### <a name="api-call"></a>Chamada à API

Pode obter uma lista de todos os mapeamentos de inquilino usando a operação GET

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

Pode remover um inquilino que tenha sido adicionado a um registo. Se esse inquilino ainda está usando os recursos no Azure Stack, é cobrada a utilização das mesmas para a subscrição utilizada no registo do Azure Stack inicial.

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

### <a name="api-call"></a>Chamada à API

Pode remover os mapeamentos de inquilino usando a operação de eliminação.

**Operação**: eliminar  
**RequestURI**: `subscriptions/{registrationSubscriptionId}/resourceGroups/{resourceGroup}  
/providers/Microsoft.AzureStack/registrations/{registrationName}/customerSubscriptions/  
{customerSubscriptionId}?api-version=2017-06-01 HTTP/1.1`  
**Resposta**: 204 nenhum conteúdo  
**Corpo da resposta**: vazio

## <a name="next-steps"></a>Passos Seguintes

 - Para saber mais sobre como recuperar informações de utilização de recursos do Azure Stack, veja [utilização e faturação no Azure Stack](/azure-stack-billing-and-chargeback.md).
