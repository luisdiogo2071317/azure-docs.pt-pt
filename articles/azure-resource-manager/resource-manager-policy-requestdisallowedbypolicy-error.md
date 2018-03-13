---
title: "Erro de RequestDisallowedByPolicy com a política de recursos do Azure | Microsoft Docs"
description: Descreve a causa do erro RequestDisallowedByPolicy.
services: azure-resource-manager,azure-portal
documentationcenter: 
author: genlin
manager: cshepard
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 03/09/2018
ms.author: genli
ms.openlocfilehash: 5a9efa6b807e933726104e7af315589ede5d9b74
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/12/2018
---
# <a name="requestdisallowedbypolicy-error-with-azure-resource-policy"></a>Erro de RequestDisallowedByPolicy com a política de recursos do Azure

Este artigo descreve a causa do erro RequestDisallowedByPolicy, também fornece solução para este erro.

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

Para obter detalhes sobre a política que bloqueados a implementação, utilize um dos métodos seguinte:

### <a name="powershell"></a>PowerShell

No PowerShell, fornecer esse identificador de política, como o `Id` parâmetro para obter detalhes sobre a política que a implementação de bloqueado.

```PowerShell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

### <a name="azure-cli"></a>CLI do Azure

No Azure CLI 2.0, forneça o nome da definição de política:

```azurecli
az policy definition show --name regionPolicyAssignment
```

## <a name="solution"></a>Solução

Para segurança ou a conformidade, os administradores da subscrição podem atribuir políticas que limitam a forma como os recursos são implementados. Por exemplo, a subscrição pode ter uma política que impede a criação de rotas do IP público endereços, grupos de segurança de rede, definidas pelo utilizador ou tabelas de rotas. A mensagem de erro no **sintomas** secção mostra o nome da política.
Para resolver este problema, reveja as políticas de recursos e determinar como implementar os recursos que estão em conformidade com as políticas.

Para obter mais informações, veja os artigos seguintes:

- [O que é a política do Azure?](../azure-policy/azure-policy-introduction.md)
- [Criar e gerir políticas para impor a compatibilidade](../azure-policy/create-manage-policy.md)
