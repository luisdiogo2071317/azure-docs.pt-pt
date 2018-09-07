---
title: Cópia de segurança do Azure Stack | Documentos da Microsoft
description: Efetue uma cópia de segurança a pedido no Azure Stack com cópia de segurança no local.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.openlocfilehash: a11de2a4580515f6a358438a706e5be3f5543e28
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/06/2018
ms.locfileid: "44025315"
---
# <a name="back-up-azure-stack"></a>Criar cópias de segurança do Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Efetue uma cópia de segurança a pedido no Azure Stack com cópia de segurança no local. Para obter instruções sobre como configurar o ambiente do PowerShell, consulte [instalar o PowerShell para o Azure Stack ](azure-stack-powershell-install.md). Para iniciar sessão no Azure Stack, veja [com o portal de administrador no Azure Stack](azure-stack-manage-portals.md).

## <a name="start-azure-stack-backup"></a>Iniciar cópia de segurança do Azure Stack

### <a name="start-a-new-backup-without-job-progress-tracking"></a>Iniciar uma nova cópia de segurança sem o progresso da tarefa de controlo
Utilize o início AzSBackup para iniciar uma nova cópia de segurança imediatamente com nenhum progresso da tarefa de controlo.

```powershell
   Start-AzsBackup -Force
```

### <a name="start-azure-stack-backup-with-job-progress-tracking"></a>Iniciar a cópia de segurança do Azure Stack com o progresso da tarefa de controlo
Utilize AzSBackup iniciar para iniciar uma nova cópia de segurança com a variável de - AsJob para controlar o progresso da tarefa de cópia de segurança.

```powershell
    $backupjob = Start-AzsBackup -Force -AsJob 
    "Start time: " + $backupjob.PSBeginTime;While($backupjob.State -eq "Running"){("Job is currently: " `
    + $backupjob.State+" ;Duration: " + (New-TimeSpan -Start ($backupjob.PSBeginTime) `
    -End (Get-Date)).Minutes);Start-Sleep -Seconds 30};$backupjob.Output

    if($backupjob.State -eq "Completed"){Get-AzsBackup | where {$_.BackupId -eq $backupjob.Output.BackupId}}
```

## <a name="confirm-backup-has-completed"></a>Confirmar cópia de segurança foi concluída

### <a name="confirm-backup-has-completed-using-powershell"></a>Confirmar cópia de segurança foi concluída com o PowerShell
Utilize os seguintes comandos do PowerShell para se certificar que cópia de segurança foi concluída com êxito:

```powershell
   Get-AzsBackup
```

O resultado deverá ser semelhante a seguinte saída:

```powershell
    BackupDataVersion : 1.0.1
    BackupId          : <backup ID>
    RoleStatus        : {NRP, SRP, CRP, KeyVaultInternalControlPlane...}
    Status            : Succeeded
    CreatedDateTime   : 7/6/2018 6:46:24 AM
    TimeTakenToCreate : PT20M32.364138S
    DeploymentID      : <deployment ID>
    StampVersion      : 1.1807.0.41
    OemVersion        : 
    Id                : /subscriptions/<subscription ID>/resourceGroups/System.local/providers/Microsoft.Backup.Admin/backupLocations/local/backups/<backup ID>
    Name              : local/<local name>
    Type              : Microsoft.Backup.Admin/backupLocations/backups
    Location          : local
    Tags              : {}
```

### <a name="confirm-backup-has-completed-in-the-administration-portal"></a>Confirmar cópia de segurança foi concluída no portal de administração
Utilize o portal de administração do Azure Stack para verificar que essa cópia de segurança foi concluída com êxito ao seguir estes passos:

1. Abra o [portal de administração do Azure Stack](azure-stack-manage-portals.md).
2. Selecione **todos os serviços**e, no **administração** categoria selecione > **cópia de segurança da infraestrutura**. Escolher **Configuration** no **cópia de segurança da infraestrutura** painel.
3. Encontrar o **Name** e **data concluída** da cópia de segurança no **cópias de segurança disponíveis** lista.
4. Verifique se o **estado** é **Succeeded**.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o fluxo de trabalho para recuperar a partir de um evento de perda de dados. Ver [recuperar da perda catastrófica de dados](azure-stack-backup-recover-data.md).
