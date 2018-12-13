---
title: Implementar e gerir cópias de segurança para partilhas de ficheiros do Azure com o PowerShell
description: Utilizar o PowerShell para implementar e gerir cópias de segurança no Azure para partilhas de ficheiros do Azure
services: backup
author: pvrk
manager: shivamg
keywords: PowersShell; Cópia de segurança de ficheiros do Azure; Restauro de ficheiros do Azure;
ms.service: backup
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: pullabhk
ms.assetid: 80da8ece-2cce-40dd-8dce-79960b6ae073
ms.openlocfilehash: 90623981f67bbed15ade743192525676e58a0a83
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53318711"
---
# <a name="use-powershell-to-back-up-and-restore-azure-file-shares"></a>Utilize o PowerShell para criar cópias de segurança e restaurar partilhas de ficheiros do Azure

Este artigo mostra como utilizar cmdlets do Azure PowerShell para criar cópias de segurança e recuperar uma partilha de ficheiros do Azure a partir de um cofre dos serviços de recuperação. Um cofre dos serviços de recuperação é um recurso do Azure Resource Manager utilizado para proteger dados e ativos nos serviços de cópia de segurança do Azure e o Azure Site Recovery.

## <a name="concepts"></a>Conceitos

Se não estiver familiarizado com o serviço de cópia de segurança do Azure, para uma descrição geral do serviço, consulte o artigo [o que é o Azure Backup?](backup-introduction-to-azure-backup.md). Antes de começar, certifique-se de que tome nota das funcionalidades de pré-visualização de segurança das partilhas de ficheiros do Azure documentadas [aqui](backup-azure-files.md).

Para utilizar o PowerShell com eficiência, é necessário compreender a hierarquia de objetos e o local começar.

![Hierarquia de objetos de serviços de recuperação](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Para ver a referência de cmdlets do PowerShell Recoveryservices, consulte a [cópia de segurança do Azure - Cmdlets de serviços de recuperação](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) na biblioteca do Azure.

## <a name="setup-and-registration"></a>Configuração e registo

> [!NOTE]
> Conforme observado [aqui](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.13.0), suporte para novos recursos no final do módulo AzureRM em Novembro de 2018. Portanto, estamos oferecendo suporte para cópia de segurança de partilhas de ficheiros do Azure com o novo módulo de "Az" PS. Estamos também a planear a carregar a versão de disponibilidade geral do módulo de Az.

Para começar:

1. [Baixe a versão mais recente do PowerShell de "Az"](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azurermps-6.13.0) (a versão mínima necessária é: 0.7.0)

2. Encontre os cmdlets do PowerShell de cópia de segurança do Azure disponíveis, escrevendo o seguinte comando:

    ```powershell
    Get-Command *azrecoveryservices*
    ```
    Os cmdlets de cópia de segurança do Azure, Azure Site Recovery e o Cofre dos serviços de recuperação e aliases são apresentados. A imagem seguinte é um exemplo de como o que verá. Não é a lista completa de cmdlets.

    ![lista de serviços de recuperação](./media/backup-azure-afs-automation/list-of-recoveryservices-ps-az.png)

3. Iniciar sessão no seu Azure conta utilizando **Connect-AzAccount**. Este cmdlet é exibida uma página da web pede-lhe as credenciais da conta:

    * Em alternativa, pode incluir as credenciais da conta como um parâmetro no **Connect-AzAccount** cmdlet, utilizando o **-credencial** parâmetro.
    * Se estiver a trabalhar em nome de um inquilino de parceiro CSP, especifica o cliente como um inquilino, utilizando o respetivo nome de domínio primário tenantID ou o inquilino. Por exemplo: **Ligar-AzAccount-inquilino "fabrikam.com"**

4. Associe a subscrição que pretende utilizar com a conta, uma vez que uma conta pode ter várias subscrições:

    ```powershell
    Select-AzureRmSubscription -SubscriptionName $SubscriptionName
    ```

5. Se estiver a utilizar o Azure Backup pela primeira vez, tem de utilizar o **Register-AzResourceProvider** cmdlet para registar o fornecedor de serviços de recuperação do Azure com a sua subscrição.

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. Pode verificar que os fornecedores registado com êxito, com os comandos seguintes:
    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
    No resultado do comando, o **RegistrationState** deve ser alterado para **registado**. Se não, basta executar o **Register-AzResourceProvider** cmdlet novamente.

As seguintes tarefas podem ser automatizadas com o PowerShell:

* Criar um cofre dos Serviços de Recuperação 
* Configurar cópia de segurança para partilhas de ficheiros do Azure
* Acionar uma tarefa de cópia de segurança
* Monitorizar uma tarefa de cópia de segurança
* Restaurar uma partilha de ficheiros do Azure
* Restaurar um arquivo do Azure individual a partir de uma partilha de ficheiros do Azure

## <a name="create-a-recovery-services-vault"></a>Criar um cofre dos Serviços de Recuperação 

Os seguintes passos levá-lo através da criação de um cofre dos serviços de recuperação.

1. O Cofre dos serviços de recuperação é um recurso do Resource Manager, por isso terá de colocá-lo dentro de um grupo de recursos. Pode utilizar um grupo de recursos existente ou crie um grupo de recursos com o **New-AzResourceGroup** cmdlet. Ao criar um grupo de recursos, especifique o nome e local para o grupo de recursos.  

    ```powershell
    New-AzResourceGroup -Name "test-rg" -Location "West US"
    ```
2. Utilize o **New-AzRecoveryServicesVault** cmdlet para criar o Cofre dos serviços de recuperação. Certifique-se de que especifique a mesma localização do cofre que foi utilizado para o grupo de recursos.

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```
3. Especifique o tipo de redundância de armazenamento a utilizar. Pode usar [armazenamento localmente redundante (LRS)](../storage/common/storage-redundancy-lrs.md) ou [armazenamento Georredundante (GRS)](../storage/common/storage-redundancy-grs.md). O exemplo seguinte mostra que a opção - BackupStorageRedundancy para testvault está definida como GeoRedundant.

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
    Set-AzRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a>Ver os cofres numa subscrição

Para ver todos os cofres na subscrição, utilize **Get-AzRecoveryServicesVault**:

```powershell
Get-AzRecoveryServicesVault
```

O resultado é semelhante ao seguinte exemplo, observe a que localização e ResourceGroupName associados são fornecidos.

```powershell
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```

Muitos cmdlets do Azure Backup requerem o objeto do cofre dos Serviços de Recuperação como entrada.

Uso **Set-AzRecoveryServicesVaultContext** para definir o contexto do cofre. Quando o contexto do cofre estiver definido, é aplicado a todos os cmdlets subsequentes. O exemplo seguinte define o contexto do cofre para o Cofre *testvault*.

```powershell
Get-AzRecoveryServicesVault -Name "testvault" | Set-AzRecoveryServicesVaultContext
```

> [!NOTE]
> Estamos a planear preterir a definição de contexto do Cofre de acordo com as diretrizes do PowerShell do Azure. Em vez disso, recomendamos que os usuários a passar o ID do cofre, conforme mencionado abaixo

Em alternativa, pode armazenamento/obtenção o ID do cofre ao qual pretende efetuar uma operação de PowerShell e passá-lo para o comando relevante.

```powershell
$vaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
```

## <a name="configure-backup-for-an-azure-file-share"></a>Configurar a cópia de segurança para uma partilha de ficheiros do Azure

### <a name="create-protection-policy"></a>Criar política de proteção

Uma política de proteção de cópia de segurança está associada a pelo menos uma política de retenção. Política de retenção define o tempo que um ponto de recuperação é mantido antes de ser eliminado. Uso **Get-AzRecoveryServicesBackupRetentionPolicyObject** para ver a política de retenção predefinida.  Da mesma forma, pode utilizar **Get-AzRecoveryServicesBackupSchedulePolicyObject** para obter a política de agenda predefinida. O **New-AzRecoveryServicesBackupProtectionPolicy** cmdlet cria um objeto do PowerShell que contém as informações de política de cópia de segurança. Os objetos de política de agendamento e retenção são utilizados como entradas para o **New-AzRecoveryServicesBackupProtectionPolicy** cmdlet. O exemplo seguinte armazena a política de agendamento e a política de retenção em variáveis. O exemplo utiliza essas variáveis para definir os parâmetros ao criar uma política de proteção *NewPolicy*.

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureFiles"
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureFiles"
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewAFSPolicy" -WorkloadType "AzureFiles" -RetentionPolicy $retPol -SchedulePolicy $schPol
```

O resultado é semelhante ao seguinte exemplo:

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewAFSPolicy           AzureFiles            AzureStorage              10/24/2017 1:30:00 AM
```

### <a name="enable-protection"></a>Ativar a proteção

Depois de definir a política de proteção, pode ativar a proteção para a partilha de ficheiros do Azure com esta política.

Primeiro obter o objeto de política relevante com o **Get-AzRecoveryServicesBackupProtectionPolicy** cmdlet. Pode utilizar este cmdlet para obter uma política específica ou para ver as políticas associadas um tipo de carga de trabalho.

O exemplo seguinte obtém as políticas para o tipo de carga de trabalho, AzureFiles.

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureFiles"
```

O resultado é semelhante ao seguinte exemplo:

```powershell
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
dailyafs             AzureFiles         AzureStorage         1/10/2018 12:30:00 AM
```

> [!NOTE]
> O fuso horário do campo BackupTime no PowerShell é UTC. No entanto, quando a hora da cópia é apresentada no portal do Azure, o tempo é ajustado para o fuso horário local.
>
>

A política de seguinte obtém a política de cópia de segurança com o nome como "dailyafs"

```powershell
$afsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "dailyafs"
```

Uso **Enable-AzRecoveryServicesBackupProtection** para ativar a proteção do item com a política determinada. Assim que a política foi associada com o cofre, o fluxo de trabalho de cópia de segurança é acionado ao tempo definido no agendamento de política.

O exemplo seguinte ativa a proteção para a partilha de ficheiros do Azure, "testAzureFileShare", sob o armazenamento conta "testStorageAcct", com o dailyafs de política.

```powershell
Enable-AzRecoveryServicesBackupProtection -StorageAccountName "testStorageAcct" -Name "testAzureFS" -Policy $afsPol
```

O comando tem de aguardar até que a tarefa de proteção de configuração está concluída e dá um resultado semelhante, conforme mostrado abaixo.

```cmd
WorkloadName       Operation            Status                 StartTime                                                                                                         EndTime                   JobID
------------             ---------            ------               ---------                                  -------                   -----
testAzureFS       ConfigureBackup      Completed            11/12/2018 2:15:26 PM     11/12/2018 2:16:11 PM     ec7d4f1d-40bd-46a4-9edb-3193c41f6bf6
```

### <a name="trigger-an-on-demand-backup"></a>Acionar uma cópia de segurança a pedido

Uso **AzRecoveryServicesBackupItem de cópia de segurança** para acionar uma tarefa de cópia de segurança para uma partilha de ficheiros do Azure protegidos. Recuperar a partilha de ficheiro e da conta de armazenamento no mesmo ao utilizar os seguintes comandos e acione uma cópia de segurança a pedido.

```powershell
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType "AzureFiles" -Name "testAzureFS"
$job =  Backup-AzRecoveryServicesBackupItem -Item $afsBkpItem
```

O comando devolve uma tarefa a ser monitorizado com o ID como no exemplo seguinte.

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS       Backup               Completed            11/12/2018 2:42:07 PM     11/12/2018 2:42:11 PM     8bdfe3ab-9bf7-4be6-83d6-37ff1ca13ab6
```

Podemos estão utilizando instantâneos de partilha de ficheiros do Azure ao mesmo tempo que as cópias de segurança e, por conseguinte, normalmente, a tarefa é concluída quando que o comando devolve esta saída

### <a name="modify-protection-policy"></a>Modificar a política de proteção

Se pretender alterar a política com que a partilha de ficheiros do Azure está protegida, utilize o **Enable-AzRecoveryServicesBackupProtection** com o item de cópia de segurança relevante e a nova política de proteção.

Os exemplos a seguir altera a política de proteção de "testAzureFS" de "dailyafs" para "monthlyafs"

```powershell
$monthlyafsPol =  Get-AzRecoveryServicesBackupProtectionPolicy -Name "monthlyafs"
$afsContainer = Get-AzRecoveryServicesBackupContainer -FriendlyName "testStorageAcct" -ContainerType AzureStorage
$afsBkpItem = Get-AzRecoveryServicesBackupItem -Container $afsContainer -WorkloadType AzureFiles -Name "testAzureFS"
Enable-AzRecoveryServicesBackupProtection -Item $afsBkpItem -Policy $monthlyafsPol
```

## <a name="restore-azure-file-sharesazure-files"></a>Restaurar as partilhas de ficheiros do Azure / Azure ficheiros

Pode restaurar uma partilha de ficheiros completa para o seu original ou uma localização alternativa. Da mesma forma arquivos individuais da partilha de ficheiros podem ser restaurados também.

### <a name="fetching-recovery-points"></a>A obter pontos de recuperação

Utilize o **Get-AzRecoveryServicesBackupRecoveryPoint** cmdlet para listar todos os pontos de recuperação para o item de cópia de segurança. No seguinte script, a variável **$rp**, é uma matriz de pontos de recuperação para o item de cópia de segurança selecionado, dos últimos sete dias. A matriz é ordenada pela ordem inversa de tempo com o ponto de recuperação mais recente no índice 0. Utilize a indexação da matriz de PowerShell padrão para escolher o ponto de recuperação. No exemplo, $rp [0] seleciona o ponto de recuperação mais recente.

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $afsBkpItem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()

$rp[0] | fl
```

O resultado é semelhante ao seguinte exemplo:

```powershell
FileShareSnapshotUri : https://testStorageAcct.file.core.windows.net/testAzureFS?sharesnapshot=2018-11-20T00:31:04.00000
                       00Z
RecoveryPointType    : FileSystemConsistent
RecoveryPointTime    : 11/20/2018 12:31:05 AM
RecoveryPointId      : 86593702401459
ItemName             : testAzureFS
Id                   : /Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Micros                      oft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;teststorageRG;testStorageAcct/protectedItems/AzureFileShare;testAzureFS/recoveryPoints/86593702401462
WorkloadType         : AzureFiles
ContainerName        : storage;teststorageRG;testStorageAcct
ContainerType        : AzureStorage
BackupManagementType : AzureStorage
```

Depois do ponto de recuperação relevante está selecionado, avance para restaurar o ficheiro/partilha de ficheiros para uma localização alternativa/original, conforme explicado abaixo.

### <a name="restore-azure-file-shares-to-an-alternate-location"></a>Restaurar as partilhas de ficheiros do Azure para uma localização alternativa

#### <a name="restoring-an-azure-file-share"></a>Restaurar uma partilha de ficheiros do Azure

Identificar a localização alternativa ao fornecer as seguintes informações:

* ***TargetStorageAccountName***: Conta de armazenamento para o qual é restaurado o conteúdo de cópia de segurança. A conta de armazenamento de destino deve ser na mesma localização do cofre.
* ***TargetFileShareName***: Partilhas de ficheiros na conta de armazenamento de destino para o qual é restaurado o conteúdo de cópia de segurança
* ***TargetFolder***: A pasta na partilha de ficheiros para o qual os dados são restaurados. Se o conteúdo de cópia de segurança deve ser restaurado para a pasta raiz, atribua valores de pasta de destino como cadeia de caracteres vazia
* ***ResolveConflict***: Instrução em caso de conflito com os dados restaurados. Aceita "Substituir" ou "Ignorar"

Fornece estes parâmetros para o comando restore para restaurar uma cópia de segurança de partilha de ficheiros para uma localização alternativa.

````powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -ResolveConflict Overwrite
````

O comando devolve uma tarefa com o ID ser controlados, como mostrado no exemplo a seguir.

````powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
testAzureFS        Restore              InProgress           12/10/2018 9:56:38 AM                               9fd34525-6c46-496e-980a-3740ccb2ad75
````

#### <a name="restoring-an-azure-file"></a>Restaurar um ficheiro do Azure

No caso queira restaurar um arquivo individual em vez de uma partilha de ficheiros completo, o arquivo individual deve ser identificado exclusivamente, fornecendo os seguintes parâmetros.

* ***TargetStorageAccountName***: Conta de armazenamento para o qual é restaurado o conteúdo de cópia de segurança. A conta de armazenamento de destino deve ser na mesma localização do cofre.
* ***TargetFileShareName***: Partilhas de ficheiros na conta de armazenamento de destino para o qual é restaurado o conteúdo de cópia de segurança
* ***TargetFolder***: A pasta na partilha de ficheiros para o qual os dados são restaurados. Se o conteúdo de cópia de segurança deve ser restaurado para a pasta raiz, atribua valores de pasta de destino como cadeia de caracteres vazia
* ***SourceFilePath***: O caminho absoluto do ficheiro, sejam restaurados em partilha de ficheiros, como uma cadeia de caracteres. Este é o mesmo caminho utilizado na ```Get-AzStorageFile``` PS cmdlet.
* ***SourceFileType***: Se for selecionado um diretório ou um ficheiro. Aceita o "Diretório" ou "Arquivo"
* ***ResolveConflict***: Instrução em caso de conflito com os dados restaurados. Aceita "Substituir" ou "Ignorar"

Como pode ver, os parâmetros adicionais são apenas relacionados com o arquivo individual a ser restaurada.

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -TargetStorageAccountName "TargetStorageAcct" -TargetFileShareName "DestAFS" -TargetFolder "testAzureFS_restored" -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

Isto também irá devolver uma tarefa a ser monitorizado com o ID, conforme mostrado acima.

### <a name="restore-azure-file-shares-to-original-location"></a>Restaurar as partilhas de ficheiros do Azure para a localização original

Em caso de restauro de localização original, todos os destino relacionados com parâmetros não tem de ser especificados. Apenas ```ResolveConflict``` tem de ser fornecido

#### <a name="overwrite-an-azure-file-share"></a>Substituir uma partilha de ficheiros do Azure

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -ResolveConflict Overwrite
```

#### <a name="overwrite-an-azure-file"></a>Substituir um ficheiro do Azure

```powershell
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -SourceFileType File -SourceFilePath "TestDir/TestDoc.docx" -ResolveConflict Overwrite
```

## <a name="track-backup-and-restore-jobs"></a>Controlar a cópia de segurança e restaurar trabalhos

Operações de cópia de segurança e restauro a pedido devolvam uma tarefa, juntamente com um ID, conforme mostrado [acima](#trigger-an-on-demand-backup). Utilize o ```Get-AzRecoveryServicesBackupJobDetails``` commandlet para acompanhar o progresso da tarefa e obter mais detalhes.

```powershell
$job = Get-AzRecoveryServicesBackupJob -JobId 00000000-6c46-496e-980a-3740ccb2ad75 -VaultId $vaultID

 $job | fl


IsCancellable        : False
IsRetriable          : False
ErrorDetails         : {Microsoft.Azure.Commands.RecoveryServices.Backup.Cmdlets.Models.AzureFileShareJobErrorInfo}
ActivityId           : 00000000-5b71-4d73-9465-8a4a91f13a36
JobId                : 00000000-6c46-496e-980a-3740ccb2ad75
Operation            : Restore
Status               : Failed
WorkloadName         : testAFS
StartTime            : 12/10/2018 9:56:38 AM
EndTime              : 12/10/2018 11:03:03 AM
Duration             : 01:06:24.4660027
BackupManagementType : AzureStorage

$job.ErrorDetails

 ErrorCode ErrorMessage                                          Recommendations
 --------- ------------                                          ---------------
1073871825 Microsoft Azure Backup encountered an internal error. Wait for a few minutes and then try the operation again. If the issue persists, please contact Microsoft support
```
