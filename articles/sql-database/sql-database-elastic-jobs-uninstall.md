---
title: Como desinstalar a ferramenta de tarefas de bases de dados elásticas
description: Saiba como desinstalar os componentes de tarefas de bases de dados elásticas com o portal do Azure do PowerShell.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 16ef5beb066ccf62702cfb3f632dd459d603dd3b
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2019
ms.locfileid: "55560622"
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
1. Inicie uma janela de comando do Microsoft Azure PowerShell e navegue para o subdiretório de ferramentas na pasta Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x: Tipo **ferramentas de cd**.
   
     PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools
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


