---
title: Cópia de segurança do Azure Stack | Documentos da Microsoft
description: Efetue uma cópia de segurança a pedido no Azure Stack com cópia de segurança no local.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/08/2017
ms.author: mabrigg
ms.reviewer: hectorl
ms.openlocfilehash: c9e7ffae1b988d0940d10acdb1b387a25e0466ec
ms.sourcegitcommit: d76d9e9d7749849f098b17712f5e327a76f8b95c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242891"
---
# <a name="back-up-azure-stack"></a>Criar cópias de segurança do Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Efetue uma cópia de segurança a pedido no Azure Stack com cópia de segurança no local. Para obter instruções sobre como configurar o ambiente do PowerShell, consulte [instalar o PowerShell para o Azure Stack ](azure-stack-powershell-install.md). Para iniciar sessão no Azure Stack, veja [configurar o ambiente de operador e inicie sessão no Azure Stack](azure-stack-powershell-configure-admin.md).

## <a name="start-azure-stack-backup"></a>Iniciar cópia de segurança do Azure Stack

Utilize AzSBackup iniciar para iniciar uma nova cópia de segurança com a variável de - AsJob para controlar o progresso. 

```powershell
    $backupjob = Start-AzsBackup -Force -AsJob
    "Start time: " + $backupjob.PSBeginTime;While($backupjob.State -eq "Running"){("Job is currently: " + $backupjob.State+" ;Duration: " + (New-TimeSpan -Start ($backupjob.PSBeginTime) -End (Get-Date)).Minutes);Start-Sleep -Seconds 30};$backupjob.Output
```

## <a name="confirm-backup-completed-via-powershell"></a>Confirmar cópia de segurança concluída através do PowerShell

```powershell
    if($backupjob.State -eq "Completed"){Get-AzsBackup | where {$_.BackupId -eq $backupjob.Output.BackupId}}
```

- O resultado deverá ser semelhante a seguinte saída:

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

## <a name="confirm-backup-completed-in-the-administration-portal"></a>Confirmar cópia de segurança concluída no portal de administração

1. Abra o portal de administração do Azure Stack em [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).
2. Selecione **mais serviços** > **cópia de segurança da infraestrutura**. Escolher **Configuration** no **cópia de segurança da infraestrutura** painel.
3. Encontrar o **Name** e **data concluída** da cópia de segurança no **cópias de segurança disponíveis** lista.
4. Verifique se o **estado** é **Succeeded**.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre o fluxo de trabalho para recuperar a partir de um evento de perda de dados. Ver [recuperar da perda catastrófica de dados](azure-stack-backup-recover-data.md).