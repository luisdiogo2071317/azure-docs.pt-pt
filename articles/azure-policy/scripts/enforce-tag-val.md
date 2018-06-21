---
title: Exemplo de json do Azure Policy – Impor a etiqueta e o respetivo valor | Microsoft Docs
description: Esta política de exemplo de json requer um nome de etiqueta e um valor especificado.
services: azure-policy
documentationcenter: ''
author: DCtheGeek
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-policy
ms.devlang: ''
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: ''
ms.date: 10/30/2017
ms.author: dacoulte
ms.custom: mvc
ms.openlocfilehash: 08702d9886a0d6282089bb8d73715be83e1a9a6a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34601127"
---
# <a name="enforce-tag-and-its-value"></a>Impor etiqueta e o respetivo valor

Esta política requer um nome de etiqueta e um valor especificado. Especifica o nome de etiqueta e o valor a impor.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Modelo de exemplo

[!code-json[main](../../../policy-templates/samples/built-in-policy/enforce-tag-value/azurepolicy.json "Enforce tag and its value")]

Pode implementar este modelo através do [portal do Azure](#deploy-with-the-portal), do [PowerShell](#deploy-with-powershell) ou da [CLI do Azure](#deploy-with-azure-cli).

## <a name="deploy-with-the-portal"></a>Implementar com o portal

[![Implementar no Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2Fbuilt-in-policy%2Fenforce-tag-value%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>Implementar com o PowerShell

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = New-AzureRmPolicyDefinition -Name "enforce-tag-value" -DisplayName "Enforce tag and its value" -description "Enforces a required tag and its value." -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/enforce-tag-value/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/enforce-tag-value/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope>  -tagName <tagName> -tagValue <tagValue> -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>Limpar a implementação do PowerShell

Execute o seguinte comando para remover o grupo de recursos, a VM e todos os recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>Implementar com a CLI do Azure

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy definition create --name 'enforce-tag-value' --display-name 'Enforce tag and its value' --description 'Enforces a required tag and its value.' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/enforce-tag-value/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/enforce-tag-value/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "enforce-tag-value"
```

### <a name="clean-up-azure-cli-deployment"></a>Limpar a implementação da CLI do Azure

Execute o seguinte comando para remover o grupo de recursos, a VM e todos os recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Passos seguintes

- Veja mais exemplos nas [amostras do Azure Policy](../json-samples.md).