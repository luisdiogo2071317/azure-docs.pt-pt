---
title: Erro RequestDisallowedByPolicy com a política de recursos do Azure | Documentos da Microsoft
description: Descreve a causa do erro RequestDisallowedByPolicy.
services: azure-resource-manager
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 7330c8369fa8232c90fe6931745e298107ed6ad1
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50418063"
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Erro RequestDisallowedByPolicy com a política de recursos do Azure

Este artigo descreve a causa do erro RequestDisallowedByPolicy, também fornece soluções para este erro.

## <a name="symptom"></a>Sintoma

Durante a implementação, poderá receber um **RequestDisallowedByPolicy** erro impede a criação dos recursos. O exemplo seguinte mostra o erro:

```json
{
  "statusCode": "Forbidden",
  "serviceRequestId": null,
  "statusMessage": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}",
  "responseBody": "{\"error\":{\"code\":\"RequestDisallowedByPolicy\",\"message\":\"The resource action 'Microsoft.Network/publicIpAddresses/write' is disallowed by one or more policies. Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'.\"}}"
}
```

## <a name="troubleshooting"></a>Resolução de problemas

Para obter detalhes sobre a política que bloqueado a implementação, utilize o seguinte dos métodos:

### <a name="powershell"></a>PowerShell

No PowerShell, fornecer esse identificador de política como a `Id` parâmetro para obter detalhes sobre a política que bloqueado a sua implementação.

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="azure-cli"></a>CLI do Azure

Na CLI do Azure, forneça o nome da definição de política:

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Solução

Para segurança ou conformidade, os administradores de subscrição poderão atribuir políticas que limitam a forma como os recursos são implementados. Por exemplo, a sua subscrição pode ter uma política que impede a criação de rotas do IP público endereços, grupos de segurança de rede, definidas pelo utilizador ou tabelas de rotas. A mensagem de erro no **sintomas** secção mostra o nome da política.
Para resolver este problema, reveja as políticas de recursos e determinar como implementar recursos que estão em conformidade com essas políticas.

Para obter mais informações, veja os artigos seguintes:

- [O que é o Azure Policy?](../azure-policy/azure-policy-introduction.md)
- [Criar e gerir políticas para impor a conformidade](../azure-policy/create-manage-policy.md)
