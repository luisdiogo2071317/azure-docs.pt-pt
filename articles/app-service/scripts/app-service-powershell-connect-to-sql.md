---
title: Exemplo do Script do Azure PowerShell - Ligar uma aplicação Web a uma base de dados SQL | Microsoft Docs
description: Exemplo do Script do Azure PowerShell - Ligar uma aplicação Web a uma base de dados SQL
services: app-service\web
documentationcenter: ''
author: syntaxc4
manager: erikre
editor: ''
tags: azure-service-management
ms.assetid: 055440a9-fff1-49b2-b964-9c95b364e533
ms.service: app-service
ms.devlang: multiple
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: ffd907d14265a43fd769aaa751fb3678c3c30dea
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
ms.locfileid: "31586692"
---
# <a name="connect-a-web-app-to-a-sql-database"></a>Ligar uma aplicação Web a uma base de dados SQL

Neste cenário, irá aprender a criar uma base de dados SQL do Azure e uma aplicação Web do Azure. Em seguida, ligará a base de dados SQL à aplicação Web com as definições da aplicação.

Se for preciso, instale o Azure PowerShell com a instrução que se encontra no [Guia do Azure PowerShell](/powershell/azure/overview) e, em seguida, execute `Connect-AzureRmAccount` para criar uma ligação ao Azure.

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/app-service/connect-to-sql/connect-to-sql.ps1?highlight=13 "Connect a web app to a SQL database")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Depois de executar o script de exemplo, o seguinte comando pode ser utilizado para remover o grupo de recursos, a aplicação Web e todos os recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [New-AzureRmAppServicePlan](/powershell/module/azurerm.websites/new-azurermappserviceplan) | Cria um plano do Serviço de Aplicações. |
| [New-AzureRmWebApp](/powershell/module/azurerm.websites/new-azurermwebapp) | Cria uma aplicação Web. |
| [New-AzureRMSQLServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Cria um servidor da Base de Dados SQL. |
| [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) | Cria uma regra de firewall para um servidor de Base de Dados SQL. |
| [New-AzureRMSQLDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Cria uma base de dados ou uma base de dados elástica. |
| [Set-AzureRmWebApp](/powershell/module/azurerm.websites/set-azurermwebapp) | Modifica a configuração de uma aplicação web. |

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre o módulo do Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Pode ver os exemplos do Azure Powershell adicionais para as Aplicações Web do Serviço de Aplicações do Azure em [Exemplos do Azure PowerShell](../app-service-powershell-samples.md).
