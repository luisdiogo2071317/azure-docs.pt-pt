---
title: Criar cópias de segurança de VMs do Azure em escala | Microsoft Docs
description: Este tutorial apresenta detalhes sobre a criação de cópias de segurança de várias máquinas virtuais do Azure para um cofre dos Serviços de Recuperação.
services: backup
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
keywords: virtual machine backup; back up virtual machine; backup and disaster recovery
ms.assetid: ''
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/06/2017
ms.author: trinadhk;jimpark;markgal;
ms.custom: mvc
ms.openlocfilehash: 62cc623dc3130119c5ec803933012c5545d703e5
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="back-up-azure-virtual-machines-in-azure-at-scale"></a>Criar cópias de segurança de máquinas virtuais do Azure em escala

Este tutorial apresenta detalhes sobre a criação de cópias de segurança de máquinas virtuais do Azure para um cofre dos Serviços de Recuperação. Grande parte do trabalho de criação de máquinas virtuais consiste na preparação. Antes de poder criar cópias de segurança (ou proteger) uma máquina virtual, tem de concluir os [pré-requisitos](backup-azure-arm-vms-prepare.md) para preparar o seu ambiente para proteger as VMs. 

> [!IMPORTANT]
> Este tutorial parte do princípio de que já criou um grupo de recursos e uma máquina virtual do Azure.

## <a name="create-a-recovery-services-vault"></a>Criar um cofre dos Serviços de Recuperação 

Os [cofres dos Serviços de Recuperação](backup-azure-recovery-services-vault-overview.md) são contentores que contêm os pontos de recuperação dos itens cuja cópia de segurança está a ser feita. Os cofres dos Serviços de Recuperação são recursos do Azure que podem ser implementados e geridos como parte de um grupo de recursos do Azure. Neste tutorial, vai criar um cofre dos Serviços de Recuperação no mesmo grupo de recursos da máquina virtual que está a ser protegida.


Da primeira vez que utilizar o Azure Backup, tem de registar o fornecedor do Serviço de Recuperação do Azure na sua subscrição. Se já tiver registado o fornecedor na subscrição, avance para o próximo passo.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices
```

Crie um cofre dos Serviços de Recuperação com **New-AzureRmRecoveryServicesVault**. Confirme que especifica o nome e a localização do grupo de recursos que foram utilizados na configuração da máquina virtual cuja cópia de segurança quer fazer. 

```powershell
New-AzureRmRecoveryServicesVault -Name myRSvault -ResourceGroupName "myResourceGroup" -Location "EastUS"
```

Muitos cmdlets do Azure Backup requerem o objeto do cofre dos Serviços de Recuperação como entrada. Por este motivo, é conveniente armazenar o objeto do cofre dos Serviços de Recuperação do Backup numa variável. Em seguida, utilize **Set-AzureRmRecoveryServicesBackupProperties** para definir a opção **-BackupStorageRedundancy** como [Armazenamento Georredundante (GRS)](../storage/common/storage-redundancy-grs.md). 

```powershell
$vault1 = Get-AzureRmRecoveryServicesVault –Name myRSVault
Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
```

## <a name="back-up-azure-virtual-machines"></a>Fazer cópia de segurança de máquinas virtuais do Azure

Antes de poder executar a cópia de segurança inicial, tem de definir o contexto do cofre. O contexto do cofre é o tipo de dados protegidos no cofre. Quando cria um cofre dos Serviços de Recuperação, aquele inclui políticas de proteção e retenção predefinidas. A política de proteção predefinida aciona um trabalho de cópia de segurança todos os dias a uma hora especificada. A política de retenção predefinida retém o ponto de recuperação diária durante 30 dias. Neste tutorial, aceite a política predefinida. 

Utilize **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** para definir o contexto do cofre. Quando o contexto do cofre estiver definido, é aplicado a todos os cmdlets subsequentes. 

```powershell
Get-AzureRmRecoveryServicesVault -Name myRSVault | Set-AzureRmRecoveryServicesVaultContext
```

Utilize **[Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)** para acionar o trabalho de cópia de segurança. O trabalho de cópia de segurança cria um ponto de recuperação. Se esta for a cópia de segurança inicial, o ponto de recuperação será uma cópia de segurança completa. As cópias de segurança subsequentes criam uma cópia incremental.

```powershell
$namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered -FriendlyName "V2VM"
$item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType AzureVM
$job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
```

## <a name="delete-the-recovery-services-vault"></a>Eliminar o cofre dos Serviços de Recuperação

Para eliminar um cofre dos Serviços de Recuperação, tem, primeiro, de eliminar todos os pontos de recuperação no cofre e, depois, anular o registo do mesmo. Os comandos seguintes detalham esses passos. 


```powershell
$Cont = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered
$PI = Get-AzureRmRecoveryServicesBackupItem -Container $Cont[0] -WorkloadType AzureVm
Disable-AzureRmRecoveryServicesBackupProtection -RemoveRecoveryPoints $PI[0]
Unregister-AzureRmRecoveryServicesBackupContainer -Container $namedContainer
Remove-AzureRmRecoveryServicesVault -Vault $vault1
```

## <a name="troubleshooting-errors"></a>Resolução de erros
Se se deparar com problemas ao criar a cópia de segurança da sua máquina virtual, veja o [artigo Back up Azure virtual machines troubleshooting](backup-azure-vms-troubleshoot.md) (Resolução de problemas de criação de cópias de segurança de máquinas virtuais do Azure) para obter ajuda.

## <a name="next-steps"></a>Passos seguintes
Agora que já protegeu as suas máquinas virtuais, veja os artigos abaixo para saber mais sobre as tarefas de gestão e como restaurar as máquinas virtuais a partir de um ponto de recuperação.

* Para modificar a política de cópia de segurança, veja [Use AzureRM.RecoveryServices.Backup cmdlets to back up virtual machines](backup-azure-vms-automation.md#create-a-protection-policy) (Utilizar os cmdlets AzureRM.RecoveryServices.Backup para criar cópias de segurança de máquinas virtuais).
* [Gerir e monitorizar as máquinas virtuais](backup-azure-manage-vms.md)
* [Monitorizar máquinas virtuais](backup-azure-arm-restore-vms.md)
