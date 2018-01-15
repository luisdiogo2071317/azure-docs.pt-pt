---
title: "Replicação e ativação pós-falha em máquinas virtuais VMware para o Azure com o Azure Site Recovery PowerShell | Microsoft Docs"
description: "Replicação e ativação pós-falha em máquinas virtuais VMware no Azure através do PowerShell do Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhemraj
editor: raynew
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/02/2018
ms.author: bsiva
ms.openlocfilehash: ee4847a61392a8eacde82ea62c3812d601b489f3
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/13/2018
---
# <a name="replicate-and-failover-vmware-virtual-machines-to-azure-using-azure-site-recovery-powershell"></a>Replicação e ativação pós-falha em máquinas virtuais VMware no Azure através do PowerShell do Azure Site Recovery

Neste artigo, verá como a replicar e ativação pós-falha em máquinas virtuais VMware para o Azure com o Azure PowerShell. 

Saiba como:

> [!div class="checklist"]
> - Crie um cofre dos Serviços de Recuperação.
> - Defina o contexto do cofre.
> - Confirme se o servidor de configuração e a ampliação de servidores de processos estão registados no cofre.
> - Criar uma política de replicação e mapeá-lo para utilização com o servidor de configuração.
> - Adicionar um servidor vCenter e detetar máquinas virtuais VMware.
> - Crie contas de armazenamento para replicar as máquinas virtuais.
> - Replica máquinas virtuais VMware para contas do storage do Azure.
> - Configure definições de ativação pós-falha para replicar máquinas virtuais.
> - Execute uma ativação pós-falha de teste, validar e ativação pós-falha de teste de limpeza.
> - Ativação pós-falha para o Azure.

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar:
- Certifique-se de que compreende o [arquitetura do cenário e componentes](concepts-vmware-to-azure-architecture.md).
- Reveja o [suportar requisitos](site-recovery-support-matrix-to-azure.md) para todos os componentes.
- Tem a versão 5.0.1 ou superior do módulo do AzureRm PowerShell. Se precisar de instalar ou atualizar o Azure PowerShell, siga este [guia para instalar e configurar o Azure PowerShell](/powershell/azureps-cmdlets-docs).

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Inicie sessão na sua subscrição do Microsoft Azure

Inicie sessão na sua subscrição do Azure utilizando o cmdlet Login-AzureRmAccount

```azurepowershell
Login-AzureRmAccount
```
Selecione a subscrição do Azure que pretende replicar as máquinas virtuais VMware. Utilize o cmdlet Get-AzureRmSubscription para obter a lista de subscrições do Azure que tem acesso a. Selecione a subscrição do Azure para trabalhar com utilizando o cmdlet Select-AzureRmSubscription.

```azurepowershell
Select-AzureRmSubscription -SubscriptionName "ASR Test Subscription"
```
## <a name="create-a-recovery-services-vault"></a>Criar um cofre dos Serviços de Recuperação 

* Crie um grupo de recursos no qual pretende criar o Cofre dos serviços de recuperação. No exemplo abaixo, o grupo de recursos com o nome VMwareDRtoAzurePS e é criado na região Ásia Oriental.

```azurepowershell
New-AzureRmResourceGroup -Name "VMwareDRtoAzurePS" -Location "East Asia"
```
```
ResourceGroupName : VMwareDRtoAzurePS
Location          : eastasia
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRtoAzurePS
```
   
* Crie um cofre dos serviços de recuperação. No exemplo abaixo, o Cofre de serviços de recuperação com o nome VMwareDRToAzurePs e é criado na região Oriental e no grupo de recursos criado no passo anterior.

```azurepowershell
New-AzureRmRecoveryServicesVault -Name "VMwareDRToAzurePs" -Location "East Asia" -ResourceGroupName "VMwareDRToAzurePs"
```
```
Name              : VMwareDRToAzurePs
ID                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs
Type              : Microsoft.RecoveryServices/vaults
Location          : eastasia
ResourceGroupName : VMwareDRToAzurePs
SubscriptionId    : xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
``` 

* Transferir a chave de registo do cofre do cofre. A chave de registo do Cofre é utilizada para registar o servidor de configuração no local neste cofre. O registo é parte do processo de instalação do software de servidor de configuração.

```azurepowershell
#Get the vault object by name and resource group and save it to the $vault PowerShell variable 
$vault = Get-AzureRmRecoveryServicesVault -Name "VMwareDRToAzurePS" -ResourceGroupName "VMwareDRToAzurePS"

#Download vault registration key to the path C:\Work
Get-AzureRmRecoveryServicesVaultSettingsFile -SiteRecovery -Vault $Vault -Path "C:\Work\"
```
```
FilePath
--------
C:\Work\VMwareDRToAzurePs_2017-11-23T19-52-34.VaultCredentials
```

Utilize a chave de registo do cofre transferidas e siga os passos nos artigos indicados abaixo para concluir a instalação e o registo do servidor de configuração.
* [Escolha os seus objetivos de proteção](site-recovery-set-up-vmware-to-azure.md#choose-your-protection-goals)
* [Configurar o ambiente de origem](site-recovery-set-up-vmware-to-azure.md#set-up-the-source-environment) 

## <a name="set-the-vault-context"></a>Definir o contexto do Cofre

> [!TIP]
> O módulo PowerShell do Azure Site Recovery (módulo AzureRm.RecoveryServices.SiteRecovery) é fornecido com aliases fáceis de utilizar para a maioria dos cmdlets. Os cmdlets no módulo de ter a forma  *\<operação >-**AzureRmRecoveryServicesAsr**\<objeto >* e tem o equivalentes aliases que ter a forma  *\<Operação >-**ASR**\<objeto >*. Este artigo utiliza os aliases de cmdlet para facilitar a leitura.

Defina o contexto de cofre utilizando o cmdlet Set-ASRVaultContext. Uma vez definida, são executadas operações subsequentes do Azure Site Recovery na sessão do PowerShell no contexto do cofre selecionado. No exemplo abaixo, os detalhes do cofre do $vault variável é utilizada para especificar o contexto do cofre para a sessão do PowerShell.
 ```azurepowershell
Set-ASRVaultContext -Vault $vault
```
```
ResourceName      ResourceGroupName ResourceNamespace          ResouceType
------------      ----------------- -----------------          -----------
VMwareDRToAzurePs VMwareDRToAzurePs Microsoft.RecoveryServices vaults
```

As seções subsequentes deste artigo partem do princípio de que foi definido o contexto do cofre para operações do Azure Site Recovery.

## <a name="validate-that-your-configuration-server-and-scale-out-process-servers-are-registered-to-the-vault"></a>Validar se o servidor de configuração e a ampliação de servidores de processos estão registados no Cofre

 Partem do princípio de:
- Um servidor de configuração com o nome *ConfigurationServer* foi registado neste Cofre
- Um servidor de processos adicional com o nome *ScaleOut ProcessServer* ter sido registado *ConfigurationServer*
- Contas com o nome *vCenter_account*, *WindowsAccount*, e *LinuxAccount* tiverem sido configuradas no servidor de configuração. Estas contas são utilizadas para adicionar o vCenter server para detetar máquinas virtuais e para software do serviço de mobilidade em servidores Windows e Linux que estão a ser replicadas de instalação push.

Servidores de configuração registados são representados por um objeto de recursos de infraestrutura no Azure Site Recovery. Neste passo, obtenha a lista de recursos de infraestrutura objetos no cofre e identificar o servidor de configuração.

```azurepowershell
# Verify that the Configuration server is successfully registered to the vault
$ASRFabrics = Get-ASRFabric
$ASRFabrics.count
```
```
1
```
```azurepowershell
#Print details of the Configuration Server
$ASRFabrics[0]
```
```
Name                  : 2c33d710a5ee6af753413e97f01e314fc75938ea4e9ac7bafbf4a31f6804460d
FriendlyName          : ConfigurationServer
ID                    : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationFabrics
                        /2c33d710a5ee6af753413e97f01e314fc75938ea4e9ac7bafbf4a31f6804460d
Type                  : Microsoft.RecoveryServices/vaults/replicationFabrics
FabricType            : VMware
SiteIdentifier        : ef7a1580-f356-4a00-aa30-7bf80f952510
FabricSpecificDetails : Microsoft.Azure.Commands.RecoveryServices.SiteRecovery.ASRVMWareSpecificDetails
```

* Identifique os servidores de processo que pode ser utilizado para replicar máquinas.

```azurepowershell
$ProcessServers = $ASRFabrics[0].FabricSpecificDetails.ProcessServers
for($i=0; $i -lt $ProcessServers.count; $i++) {
 "{0,-5} {1}" -f $i, $ProcessServers[$i].FriendlyName
}
```
```
0     ScaleOut-ProcessServer
1     ConfigurationServer
```

Da saída acima ***$ProcessServers [0]*** corresponde à *ScaleOut ProcessServer* e ***$ProcessServers [1]*** corresponde à função de servidor de processos no *ConfigurationServer*

* Identifica as contas que configurar no servidor de configuração.

```azurepowershell
$AccountHandles = $ASRFabrics[0].FabricSpecificDetails.RunAsAccounts
#Print the account details
$AccountHandles
```
```
AccountId AccountName
--------- -----------
1         vCenter_account
2         WindowsAccount
3         LinuxAccount
```

Da saída acima ***$AccountHandles [0]*** corresponde à conta *vCenter_account*, ***$AccountHandles [1]*** conta *WindowsAccount*, e ***$AccountHandles [2]*** conta *LinuxAccount*

## <a name="create-a-replication-policy-and-map-it-for-use-with-the-configuration-server"></a>Criar uma política de replicação e mapeá-lo para utilização com o servidor de configuração

Neste passo, são criadas duas políticas de replicação. A ativação pós-falha uma política para replicar máquinas virtuais VMware para Azure e outra para replicar máquinas virtuais em execução no Azure de volta para o site de VMware no local.

> [!NOTE]
> A maioria das operações do Azure Site Recovery são executadas no modo assíncrono. Quando iniciar uma operação, é submetida uma tarefa de recuperação de sites do Azure e uma tarefa de controlar o objeto é devolvida. Esta tarefa de objeto de controlo pode ser utilizada para monitorizar o estado da operação.

* Criar uma política de replicação com o nome *ReplicationPolicy* replicar máquinas virtuais VMware no Azure com as propriedades especificadas.

```azurepowershell
$Job_PolicyCreate = New-ASRPolicy -VMwareToAzure -Name "ReplicationPolicy" -RecoveryPointRetentionInHours 24 -ApplicationConsistentSnapshotFrequencyInHours 4 -RPOWarningThresholdInMinutes 60

# Track Job status to check for completion
while (($Job_PolicyCreate.State -eq "InProgress") -or ($Job_PolicyCreate.State -eq "NotStarted")){ 
        sleep 10; 
        $Job_PolicyCreate = Get-ASRJob -Job $Job_PolicyCreate
}

#Display job status
$Job_PolicyCreate
```
```
Name             : 8d18e2d9-479f-430d-b76b-6bc7eb2d0b3e
ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationJobs/8d18e2d
                   9-479f-430d-b76b-6bc7eb2d0b3e
Type             :
JobType          : AddProtectionProfile
DisplayName      : Create replication policy
ClientRequestId  : a162b233-55d7-4852-abac-3d595a1faac2 ActivityId: 9895234a-90ea-4c1a-83b5-1f2c6586252a
State            : Succeeded
StateDescription : Completed
StartTime        : 11/24/2017 2:49:24 AM
EndTime          : 11/24/2017 2:49:23 AM
TargetObjectId   : ab31026e-4866-5440-969a-8ebcb13a372f
TargetObjectType : ProtectionProfile
TargetObjectName : ReplicationPolicy
AllowedActions   :
Tasks            : {Prerequisites check for creating the replication policy, Creating the replication policy}
Errors           : {}
```

* Crie uma política de replicação a utilizar para reativação pós-falha a partir do Azure para o site de VMware no local.

```azurepowershell
$Job_FailbackPolicyCreate = New-ASRPolicy -AzureToVMware -Name "ReplicationPolicy-Failback" -RecoveryPointRetentionInHours 24 -ApplicationConsistentSnapshotFrequencyInHours 4 -RPOWarningThresholdInMinutes 60
```

Utilize os detalhes da tarefa no *$Job_FailbackPolicyCreate* para controlar a operação para conclusão.

* Crie um mapeamento de contentor de proteção para mapear as políticas de replicação com o servidor de configuração.

```azurepowershell
#Get the protection container corresponding to the Configuration Server
$ProtectionContainer = Get-ASRProtectionContainer -Fabric $ASRFabrics[0]

#Get the replication policies to map by name.
$ReplicationPolicy = Get-ASRPolicy -Name "ReplicationPolicy"
$FailbackReplicationPolicy = Get-ASRPolicy -Name "ReplicationPolicy-Failback"

# Associate the replication policies to the protection container corresponding to the Configuration Server. 

$Job_AssociatePolicy = New-ASRProtectionContainerMapping -Name "PolicyAssociation1" -PrimaryProtectionContainer $ProtectionContainer -Policy $Policy[0]

# Check the job status
while (($Job_AssociatePolicy.State -eq "InProgress") -or ($Job_AssociatePolicy.State -eq "NotStarted")){ 
        sleep 10; 
        $Job_AssociatePolicy = Get-ASRJob -Job $Job_AssociatePolicy
}
$Job_AssociatePolicy.State

$Job_AssociateFailbackPolicy = New-ASRProtectionContainerMapping -Name "FailbackPolicyAssociation" -PrimaryProtectionContainer $ProtectionContainer -Policy $Policy[0]

# Check the job status
while (($Job_AssociateFailbackPolicy.State -eq "InProgress") -or ($Job_AssociateFailbackPolicy.State -eq "NotStarted")){ 
        sleep 10; 
        $Job_AssociateFailbackPolicy = Get-ASRJob -Job $Job_AssociateFailbackPolicy
}
$Job_AssociateFailbackPolicy.State

```

## <a name="add-a-vcenter-server-and-discover-vmware-virtual-machines"></a>Adicionar um servidor vCenter e detetar máquinas virtuais VMware

Adicione um vCenter Server por endereço IP ou nome de anfitrião. O **-porta** parâmetro especifica a porta no vCenter server para ligar, **-nome** parâmetro especifica um nome amigável para a utilizar para o servidor vCenter e o **-conta** parâmetro especifica o identificador de conta no servidor de configuração a utilizar para detetar máquinas virtuais geridas pelo servidor vCenter.

```azurepowershell
# The $AccountHandles[0] variable holds details of vCenter_account

$Job_AddvCenterServer = New-ASRvCenter -Fabric $ASRFabrics[0] -Name "MyvCenterServer" -IpOrHostName "10.150.24.63" -Account $AccountHandles[0] -Port 443

#Wait for the job to complete and ensure it completed successfully

while (($Job_AddvCenterServer.State -eq "InProgress") -or ($Job_AddvCenterServer.State -eq "NotStarted")) { 
        sleep 30; 
        $Job_AddvCenterServer = Get-ASRJob -Job $Job_AddvCenterServer
}
$Job_AddvCenterServer
```
```
Name             : 0f76f937-f9cf-4e0e-bf27-10c9d1c252a4
ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationJobs/0f76f93
                   7-f9cf-4e0e-bf27-10c9d1c252a4
Type             :
JobType          : DiscoverVCenter
DisplayName      : Add vCenter server
ClientRequestId  : a2af8892-5686-4d64-a528-10445bc2f698 ActivityId: 7ec05aad-002e-4da0-991f-95d0de7a9f3a
State            : Succeeded
StateDescription : Completed
StartTime        : 11/24/2017 2:41:47 AM
EndTime          : 11/24/2017 2:44:37 AM
TargetObjectId   : 10.150.24.63
TargetObjectType : VCenter
TargetObjectName : MyvCenterServer
AllowedActions   :
Tasks            : {Adding vCenter server}
Errors           : {}
```

## <a name="create-storage-accounts"></a>Criar contas de armazenamento

Neste passo, são criadas as contas de armazenamento a ser utilizada para replicação. Estas contas de armazenamento são utilizadas mais tarde para replicar máquinas virtuais. Certifique-se de que as contas de armazenamento são criadas na mesma região do Azure que o cofre. Pode ignorar este passo se pretender utilizar uma conta de armazenamento existente para replicação.

> [!NOTE]
> Ao replicar máquinas virtuais no local para uma conta de armazenamento premium, tem de especificar uma conta de armazenamento padrão adicional (conta de armazenamento de registo). A conta de armazenamento de registo contém registos de replicação como um arquivo intermédio até que os registos podem ser aplicados no destino de armazenamento premium.
>

```azurepowershell

$PremiumStorageAccount = New-AzureRmStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "premiumstorageaccount1" -Location "East Asia" -SkuName Premium_LRS

$LogStorageAccount = New-AzureRmStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "logstorageaccount1" -Location "East Asia" -SkuName Standard_LRS

$ReplicationStdStorageAccount= New-AzureRmStorageAccount -ResourceGroupName "VMwareDRToAzurePs" -Name "replicationstdstorageaccount1" -Location "East Asia" -SkuName Standard_LRS
```

## <a name="replicate-vmware-virtual-machines"></a>Replicar máquinas virtuais VMware

Demora cerca de 15 a 20 minutos para máquinas virtuais para ser detetado a partir do servidor vCenter. Depois de detetado, é criado um objeto de item protegível no Azure Site Recovery para cada máquina virtual detetada. Neste passo, três das máquinas virtuais detetadas são replicadas para as contas de armazenamento do Azure criadas no passo anterior.

Terá dos detalhes seguintes para proteger uma máquina virtual detetada:
* O item protegível a replicar.
* A conta de armazenamento para replicar a máquina virtual. Além disso, é necessário um armazenamento de registo para proteger máquinas virtuais para uma conta de armazenamento premium.
* O servidor de processos a serem utilizadas para replicação. A lista de servidores de processos disponíveis foi obtida e guardou no ***$ProcessServers [0]****(ScaleOut ProcessServer)* e ***$ProcessServers [1]*** *(ConfigurationServer)* variáveis.
* A conta a utilizar para o software do serviço de mobilidade para as máquinas de instalação push. A lista de contas disponíveis foi obtida e armazenada no ***$AccountHandles*** variável.
* O mapeamento de contentor de proteção para a política de replicação a ser utilizada para replicação.
* O grupo de recursos no qual as máquinas virtuais têm de ser criadas na ativação pós-falha.
* Opcionalmente, a rede virtual do Azure e a sub-rede à qual a falha na ativação pós-falha da máquina virtual deve estar ligado.

Agora replicar as seguintes máquinas virtuais utilizando as definições especificadas nesta tabela


|Máquina virtual  |Servidor de processos        |Conta de Armazenamento              |Conta de armazenamento de registo  |Política           |Conta de instalação do serviço de mobilidade|Grupo de recursos de destino  | Rede virtual de destino  |Sub-rede de destino  |
|-----------------|----------------------|-----------------------------|---------------------|-----------------|-----------------------------------------|-----------------------|-------------------------|---------------|
|Win2K12VM1       |ScaleOut ProcessServer|premiumstorageaccount1       |logstorageaccount1   |ReplicationPolicy|WindowsAccount                           |VMwareDRToAzurePs      |A ASR vnet                 |Sub-rede 1       |
|CentOSVM1       |ConfigurationServer   |replicationstdstorageaccount1| N/A                 |ReplicationPolicy|LinuxAccount                             |VMwareDRToAzurePs      |A ASR vnet                 |Sub-rede 1       |   
|CentOSVM2       |ConfigurationServer   |replicationstdstorageaccount1| N/A                 |ReplicationPolicy|LinuxAccount                             |VMwareDRToAzurePs      |A ASR vnet                 |Sub-rede 1       |   

 
```azurepowershell

#Get the target resource group to be used
$ResourceGroup = Get-AzureRmResourceGroup -Name "VMwareToAzureDrPs"

#Get the target virtual network to be used
$RecoveryVnet = Get-AzureRmVirtualNetwork -Name "ASR-vnet" -ResourceGroupName "asrrg" 

#Get the protection container mapping for replication policy named ReplicationPolicy
$PolicyMap  = Get-ASRProtectionContainerMapping -ProtectionContainer $ProtectionContainer | where PolicyFriendlyName -eq "ReplicationPolicy"

#Get the protectable item corresponding to the virtual machine Win2K12VM1
$VM1 = Get-ASRProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "Win2K12VM1"

# Enable replication for virtual machine Win2K12VM1
# The name specified for the replicated item needs to be unique within the protection container. Using a random GUID to ensure uniqueness
$Job_EnableRepication1 = New-ASRReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM1 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -RecoveryAzureStorageAccountId $PremiumStorageAccount.Id -LogStorageAccountId $LogStorageAccount.Id -ProcessServer $ProcessServers[0] -Account $AccountHandles[1] -RecoveryResourceGroupId $ResourceGroup.ResourceId -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1" 

#Get the protectable item corresponding to the virtual machine CentOSVM1
$VM2 = Get-ASRProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "CentOSVM1"

# Enable replication for virtual machine CentOSVM1
$Job_EnableRepication2 = New-ASRReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM2 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -RecoveryAzureStorageAccountId $ReplicationStdStorageAccount.Id  -ProcessServer $ProcessServers[1] -Account $AccountHandles[2] -RecoveryResourceGroupId $ResourceGroup.ResourceId -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1"

#Get the protectable item corresponding to the virtual machine CentOSVM2
$VM3 = Get-ASRProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "CentOSVM2"

# Enable replication for virtual machine CentOSVM2
$Job_EnableRepication3 = New-ASRReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM3 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -RecoveryAzureStorageAccountId $ReplicationStdStorageAccount.Id  -ProcessServer $ProcessServers[1] -Account $AccountHandles[2] -RecoveryResourceGroupId $ResourceGroup.ResourceId -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1" 

```

Depois da tarefa de replicação de ativação estiver concluída com êxito, é iniciada a replicação inicial para as máquinas virtuais. A replicação inicial pode demorar algum tempo consoante a quantidade de dados a replicar e a largura de banda disponível para replicação. Após a conclusão da replicação inicial, a máquina virtual é movida para um estado protegido. Quando a máquina virtual atingirem um estado protegido, que pode realizar uma ativação pós-falha de teste para a máquina virtual, adicioná-lo a planos de recuperação etc.

Pode verificar o estado de replicação e estado de funcionamento de replicação da máquina virtual com o cmdlet Get-ASRReplicationProtectedItem.

```azurepowershell
Get-ASRReplicationProtectedItem -ProtectionContainer $ProtectionContainer | Select FriendlyName, ProtectionState, ReplicationHealth
```
```
FriendlyName ProtectionState                 ReplicationHealth
------------ ---------------                 -----------------
CentOSVM1    Protected                       Normal
CentOSVM2    InitialReplicationInProgress    Normal
Win2K12VM1   Protected                       Normal
```

## <a name="configure-failover-settings-for-replicating-virtual-machines"></a>Configurar definições de ativação pós-falha para replicar máquinas virtuais

Definições de ativação pós-falha para máquinas protegidas podem ser atualizadas utilizando o cmdlet Set-ASRReplicationProtectedItem. Algumas das definições que podem ser atualizadas através deste cmdlet são:
* Nome da máquina virtual a ser criado na ativação pós-falha
* Tamanho VM da máquina virtual a ser criado na ativação pós-falha
* Rede virtual do Azure e a sub-rede que os NICs da máquina virtual devem estar ligados na ativação pós-falha
* Ativação pós-falha em discos geridos
* Aplicar a vantagem de utilizar híbridos do Azure
* Atribua um endereço IP estático da rede virtual de destino a ser atribuída à máquina virtual na ativação pós-falha.

Neste exemplo, iremos atualizar o tamanho VM da máquina virtual a ser criado na ativação pós-falha da máquina virtual *Win2K12VM1* e especifique que a utilização de máquina virtual gerida discos na ativação pós-falha.

```azurepowershell
$ReplicatedVM1 = Get-ASRReplicationProtectedItem -FriendlyName "Win2K12VM1" -ProtectionContainer $ProtectionContainer

Set-ASRReplicationProtectedItem -InputObject $ReplicatedVM1 -Size "Standard_DS11" -UseManagedDisk True
```
```
Name             : cafa459c-44a7-45b0-9de9-3d925b0e7db9
ID               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VMwareDRToAzurePs/providers/Microsoft.RecoveryServices/vaults/VMwareDRToAzurePs/replicationJobs/cafa459
                   c-44a7-45b0-9de9-3d925b0e7db9
Type             :
JobType          : UpdateVmProperties
DisplayName      : Update the virtual machine
ClientRequestId  : b0b51b2a-f151-4e9a-a98e-064a5b5131f3 ActivityId: ac2ba316-be7b-4c94-a053-5363f683d38f
State            : InProgress
StateDescription : InProgress
StartTime        : 11/24/2017 2:04:26 PM
EndTime          :
TargetObjectId   : 88bc391e-d091-11e7-9484-000c2955bb50
TargetObjectType : ProtectionEntity
TargetObjectName : Win2K12VM1
AllowedActions   :
Tasks            : {Update the virtual machine properties}
Errors           : {}
```

## <a name="perform-a-test-failover-validate-and-cleanup-test-failover"></a>Execute uma ativação pós-falha de teste, validar e ativação pós-falha de teste de limpeza

```azurepowershell
#Test failover of Win2K12VM1 to the test virtual network "V2TestNetwork"

#Get details of the test failover virtual network to be used
TestFailovervnet = Get-AzureRmVirtualNetwork -Name "V2TestNetwork" -ResourceGroupName "asrrg" 

#Start the test failover operation
$TFOJob = Start-ASRTestFailoverJob -ReplicationProtectedItem $ReplicatedVM1 -AzureVMNetworkId $TestFailovervnet.Id -Direction PrimaryToRecovery
```
Depois da tarefa de ativação pós-falha de teste é concluído com êxito, irá reparar que uma máquina virtual com o sufixo *"-teste"* (Win2K12VM1-teste neste caso) para o seu nome é criada no Azure. 

Pode agora ligar para o teste de efetuar a ativação pós-falha da máquina virtual e validar a ativação pós-falha de teste.

Limpeza ativação pós-falha de teste utilizando o cmdlet Start-ASRTestFailoverCleanupJob. Esta operação elimina a máquina virtual criada como parte da operação de ativação pós-falha de teste.

```azurepowershell
$Job_TFOCleanup = Start-ASRTestFailoverCleanupJob -ReplicationProtectedItem $ReplicatedVM1
```

## <a name="failover-to-azure"></a>Ativação pós-falha para o Azure

Neste passo, vamos ativação pós-falha da máquina virtual Win2K12VM1 para um ponto de recuperação específico.

```azurepowershell
# Get the list of available recovery points for Win2K12VM1
$RecoveryPoints = Get-ASRRecoveryPoint -ReplicationProtectedItem $ReplicatedVM1
"{0} {1}" -f $RecoveryPoints[0].RecoveryPointType, $RecoveryPoints[0].RecoveryPointTime
```
```
CrashConsistent 11/24/2017 5:28:25 PM
```
```azurepowershell

#Start the failover job
$Job_Failover = Start-ASRUnplannedFailoverJob -ReplicationProtectedItem $ReplicatedVM1 -Direction PrimaryToRecovery -RecoveryPoint $RecoveryPoints[0]
do {
        $Job_Failover = Get-ASRJob -Job $Job_Failover;
        sleep 60;
} while (($Job_Failover.State -eq "InProgress") -or ($JobFailover.State -eq "NotStarted"))
$Job_Failover.State
```
```
Succeeded
```

Assim que a ativação pós-falha com êxito, pode confirmar a ativação pós-falha operação e a replicação inversa do programa de configuração do Azure de volta para o site de VMware no local.

## <a name="next-steps"></a>Passos Seguintes
Ver o [referência do PowerShell do Azure Site Recovery ](https://docs.microsoft.com/powershell/module/AzureRM.RecoveryServices.SiteRecovery) para saber como pode realizar outras tarefas, tais como criar planos de recuperação e o teste de ativação pós-falha de planos de recuperação através do PowerShell.
