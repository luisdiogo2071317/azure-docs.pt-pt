---
title: Como desinstalar a ferramenta de tarefas de bases de dados elásticas
description: Saiba como desinstalar os componentes de tarefas de bases de dados elásticas com o portal do Azure do PowerShell.
services: sql-database
manager: craigg
author: stevestein
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 06/14/2018
ms.author: sstein
ms.openlocfilehash: 395bbf50373d3a6e3848fba9fd3db0d6989023f4
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/13/2018
ms.locfileid: "35649493"
---
# <a name="uninstall-elastic-database-jobs-components"></a>Desinstalar componentes de tarefas de bases de dados elásticas


[!INCLUDE [elastic-database-jobs-deprecation](../../includes/sql-database-elastic-jobs-deprecate.md)]


**Tarefas de base de dados elásticas** componentes podem ser desinstalados com o portal do Azure ou o PowerShell.

## <a name="uninstall-elastic-database-jobs-components-using-the-azure-portal"></a>Desinstalar componentes de tarefas de bases de dados elásticas com o portal do Azure
1. Abra o [Portal do Azure](https://portal.azure.com/).
2. Navegue para a subscrição que contém **tarefas de bases de dados elásticas** componentes, ou seja, a subscrição na qual banco de dados elásticas componentes de tarefas foram instalados.
3. Clique em **navegue** e clique em **grupos de recursos**.
4. Selecione o grupo de recursos com o nome "__ElasticDatabaseJob".
5. Elimine o grupo de recursos.

## <a name="uninstall--elastic-database-jobs-components-using-powershell"></a>Desinstalar componentes de tarefas de bases de dados elásticas com o PowerShell
1. Inicie uma janela de comando do Microsoft Azure PowerShell e navegue para o subdiretório de ferramentas na pasta Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x: tipo **ferramentas de cd**.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x* > Ferramentas de cd
2. Execute o script do PowerShell de.\UninstallElasticDatabaseJobs.ps1.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\UninstallElasticDatabaseJobs.ps1   PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\UninstallElasticDatabaseJobs.ps1

Ou simplesmente, execute o seguinte script, partindo do princípio de predefinição valores em que é utilizada na instalação dos componentes:

        $ResourceGroupName = "__ElasticDatabaseJob"
        Switch-AzureMode AzureResourceManager

        $resourceGroup = Get-AzureResourceGroup -Name $ResourceGroupName
        if(!$resourceGroup)
        {
            Write-Host "The Azure Resource Group: $ResourceGroupName has already been deleted.  Elastic database job components are uninstalled."
            return
        }

        Write-Host "Removing the Azure Resource Group: $ResourceGroupName.  This may take a few minutes.”
        Remove-AzureResourceGroup -Name $ResourceGroupName -Force
        Write-Host "Completed removing the Azure Resource Group: $ResourceGroupName.  Elastic database job compoennts are now uninstalled."

## <a name="next-steps"></a>Passos Seguintes
Para instalar novamente as tarefas de bases de dados elásticas, consulte [instalar o serviço de tarefa de bases de dados elásticas](sql-database-elastic-jobs-service-installation.md)

Para uma descrição geral das tarefas de bases de dados elásticas, consulte [descrição geral das tarefas de bases de dados elásticas](sql-database-elastic-jobs-overview.md).

<!--Image references-->


