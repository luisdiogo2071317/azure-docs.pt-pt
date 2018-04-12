---
title: Grupo único de ativação pós-falha de georreplicação de exemplo do PowerShell - Base de Dados SQL do Azure | Microsoft Docs
description: Script de exemplo do Azure PowerShell para configurar o grupo de ativação pós-falha de georreplicação ativa para uma base de dados SQL do Azure e realizar a ativação pós-falha.
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: ''
ms.service: sql-database
ms.custom: business continuity, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 04/01/2018
ms.author: janeng
ms.openlocfilehash: 5217063dc99868be6777b09042e5aeb1fe9640a2
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="use-powershell-to-configure-an-active-geo-replication-failover-group-for-a-single-azure-sql-database"></a>Utilizar o PowerShell para configurar um grupo de ativação pós-falha de georreplicação ativa para uma base de dados SQL do Azure

Este exemplo de script do PowerShell configura um grupo de ativação pós-falha de georreplicação ativa para uma base de dados SQL do Azure e realiza a ativação pós-falha para uma réplica secundária da base de dados SQL do Azure.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-scripts"></a>Scripts de exemplo

[!code-powershell[main](../../../powershell_scripts/sql-database/setup-geodr-and-failover-database/setup-geodr-and-failover-database-failover-group.ps1?highlight=19-22 "Set up failover group for single database")]

## <a name="clean-up-deployment"></a>Limpar a implementação

Depois de executar o script de exemplo, pode ser utilizado o seguinte comando para remover o grupo de recursos e todos os recursos associados ao mesmo.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $primaryresourcegroupname
Remove-AzureRmResourceGroup -ResourceGroupName $secondaryresourcegroupname
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Cria um servidor lógico que aloja uma base de dados ou conjunto elástico. |
| [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | Cria um conjunto elástico dentro de um servidor lógico. |
| [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) | Atualiza as propriedades de base de dados ou move uma base de dados para, de ou entre conjuntos elásticos. |
| [New-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/new-azurermsqldatabasesecondary)| Cria uma base de dados secundária para uma base de dados existente e começa a replicação de dados. |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)| Obtém uma ou mais bases de dados. |
| [Set-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/set-azurermsqldatabasesecondary)| Muda uma base de dados secundária para primária, para iniciar a ativação pós-falha.|
| [Get-AzureRmSqlDatabaseReplicationLink](/powershell/module/azurerm.sql/get-azurermsqldatabasereplicationlink) | Obtém as ligações de georreplicação entre uma Base de Dados SQL do Azure e um grupo de recursos ou o SQL Server. |
| [Remove-AzureRmSqlDatabaseSecondary](/powershell/module/azurerm.sql/remove-azurermsqldatabasesecondary) | Termina a replicação de dados entre uma Base de Dados SQL e a base de dados secundária especificada. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |
| [New-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/new-azurermsqldatabasefailovergroup) | Cria um novo Grupo de Ativação Pós-falha da Base de Dados SQL do Azure para os servidores especificados. |
| [Switch-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/switch-azurermsqldatabasefailovergroup) | Troca as funções dos servidores no Grupo de Ativação Pós-falha e muda todas as bases de dados secundárias para a função primária. |
| [Get-AzureRMSqlDatabaseFailoverGroup](/powershell/module/azurerm.sql/get-azurermsqldatabasefailovergroup) | Obtém um Grupo de Ativação Pós-falha da Base de Dados SQL do Azure específico ou lista os Grupos de Ativação Pós-falha num servidor. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Pode ver exemplos do script do PowerShell da Base de Dados SQL adicionais nos [scripts do PowerShell da Base de Dados SQL do Azure](../sql-database-powershell-samples.md).
