---
title: Exemplo de json do Azure Policy – Auditar as definições de auditoria do SQL Server | Microsoft Docs
description: Esta política de exemplo de json audita as definições de auditoria do SQL Server.
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
ms.date: 04/27/2018
ms.author: dacoulte
ms.custom: mvc
ms.openlocfilehash: c56db0f6972b6a4a829f98e671c44ed891ae0a6e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34602555"
---
# <a name="audit-sql-server-audit-settings"></a>Auditar as definições de auditoria do SQL Server

Esta política incorporada fará uma auditoria ao SQL Server para verificar se as definições de auditoria estão ativadas.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>Modelo de exemplo

```json
{
  "if": {
    "field": "type",
    "equals": "Microsoft.SQL/servers"
  },
  "then": {
    "effect": "auditIfNotExists",
    "details": {
      "type": "Microsoft.SQL/servers/auditingSettings",
      "name": "default",
      "existenceCondition": {
        "allOf": [
          {
            "field": "Microsoft.Sql/auditingSettings.state",
            "equals": "[parameters('setting')]"
          }
        ]
      }
    }
  }
}
```

Pode implementar este modelo através do [portal do Azure](#deploy-with-the-portal), do [PowerShell](#deploy-with-powershell) ou da [CLI do Azure](#deploy-with-azure-cli). Para obter a política incorporada, utilize o ID `a6fb4358-5bf4-4ad7-ba82-2cd2f41ce5e9`.

## <a name="parameters"></a>Parâmetros

Para transmitir o valor do parâmetro, utilize o seguinte formato:

```json
{"setting": {"value":"enabled"}}
```

## <a name="deploy-with-the-portal"></a>Implementar com o portal

Ao atribuir uma política, selecione **Auditar as Definições de Auditoria ao Nível do SQL Server** nas definições incorporadas disponíveis.

## <a name="deploy-with-powershell"></a>Implementar com o PowerShell

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = Get-AzureRmPolicyDefinition -Id /providers/Microsoft.Authorization/policyDefinitions/a6fb4358-5bf4-4ad7-ba82-2cd2f41ce5e9

New-AzureRmPolicyAssignment -name "SQL Audit audit" -PolicyDefinition $definition -PolicyParameter '{"setting": {"value":"enabled"}}' -Scope <scope>
```

### <a name="clean-up-powershell-deployment"></a>Limpar a implementação do PowerShell

Execute o seguinte comando para remover a atribuição de política.

```powershell
Remove-AzureRmPolicyAssignment -Name "SQL Audit audit" -Scope <scope>
```

## <a name="deploy-with-azure-cli"></a>Implementar com a CLI do Azure

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy assignment create --scope <scope> --name "SQL Audit audit" --policy a6fb4358-5bf4-4ad7-ba82-2cd2f41ce5e9 --params '{"setting": {"value":"enabled"}}'
```

### <a name="clean-up-azure-cli-deployment"></a>Limpar a implementação da CLI do Azure

Execute o seguinte comando para remover a atribuição de política.

```azurecli-interactive
az policy assignment delete --name "SQL Audit audit" --resource-group myResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

- Veja mais exemplos nas [amostras do Azure Policy](../json-samples.md).