---
title: Relatórios de acesso - RBAC do Azure | Microsoft Docs
description: Gere um relatório que apresenta uma lista de todas as alterações no acesso às suas subscrições do Azure com controlo de acesso baseado em funções nos últimos 90 dias.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/17/2017
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: deef89e62dcec3141be46549cc0fb34b33a6335a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="create-an-access-report-for-role-based-access-control"></a>Criar um relatório de acesso para controlo de acesso baseado em funções
Sempre que alguém concede ou revoga acesso na suas subscrições, as alterações são registadas nos eventos do Azure. Pode criar relatórios de histórico de alterações de acesso para ver todas as alterações nos últimos 90 dias.

## <a name="create-a-report-with-azure-powershell"></a>Criar um relatório com o Azure PowerShell
Para criar um relatório de histórico de alterações de acesso no PowerShell, utilize o [Get-AzureRMAuthorizationChangeLog](/powershell/module/azurerm.resources/get-azurermauthorizationchangelog) comando.

Quando chamar este comando, pode especificar que a propriedade das atribuições de que pretende listada, incluindo o seguinte:

| Propriedade | Descrição |
| --- | --- |
| **Ação** |Indica se o acesso foi concedido ou revogado |
| **autor da chamada** |O proprietário responsável para que a alteração de acesso |
| **PrincipalId** | O identificador exclusivo do utilizador, grupo ou aplicação que foi atribuída a função |
| **PrincipalName** |O nome do utilizador, grupo ou aplicação |
| **PrincipalType** |Indica se a atribuição foi de um utilizador, grupo ou aplicação |
| **RoleDefinitionId** |O GUID da função que foi concedido ou revogado |
| **RoleName** |A função que foi concedida ou revogada |
| **Âmbito** | O identificador exclusivo da subscrição, grupo de recursos ou recursos aos quais se aplica a atribuição | 
| **ScopeName** |O nome do recurso, grupo de recursos ou subscrição |
| **ScopeType** |Indica se a atribuição foi no âmbito de recurso, grupo de recursos ou subscrição |
| **Timestamp** |A data e hora em que o acesso foi alterado |

Este comando de exemplo apresenta uma lista de todas as alterações de acesso na subscrição nos últimos sete dias:

```
Get-AzureRMAuthorizationChangeLog -StartTime ([DateTime]::Now - [TimeSpan]::FromDays(7)) | FT Caller,Action,RoleName,PrincipalType,PrincipalName,ScopeType,ScopeName
```

![Get-AzureRMAuthorizationChangeLog - captura de ecrã do PowerShell](./media/change-history-report/access-change-history.png)

## <a name="create-a-report-with-azure-cli"></a>Criar um relatório com a CLI do Azure
Para criar um relatório de histórico de alterações de acesso na interface de linha de comandos do Azure (CLI), utilize o `azure role assignment changelog list` comando.

## <a name="export-to-a-spreadsheet"></a>Exportar para uma folha de cálculo
Para guardar o relatório ou manipular os dados, exporte as alterações de acesso para um ficheiro. csv. Em seguida, pode ver o relatório numa folha de cálculo para revisão.

![Changelog visualizada como folha de cálculo - captura de ecrã](./media/change-history-report/change-history-spreadsheet.png)

## <a name="next-steps"></a>Passos Seguintes
* Trabalhar com [funções personalizadas no Azure RBAC](custom-roles.md)
* Saiba como gerir [RBAC do Azure com o powershell](role-assignments-powershell.md)

