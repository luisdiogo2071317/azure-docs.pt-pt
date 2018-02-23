---
title: "Replicar VMs de Hyper-V em nuvens VMM para um site secundário com o PowerShell (Azure Resource Manager) | Microsoft Docs"
description: "Descreve como replicar VMs de Hyper-V em nuvens VMM para um site VMM secundário com o PowerShell (Resource Manager)"
services: site-recovery
author: sujaytalasila
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: sutalasi
ms.openlocfilehash: a5e36546494223b20844303f3f76782746658411
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="replicate-hyper-v-vms-to-a-secondary-site-using-powershell-resource-manager"></a>Replicar VMs Hyper-V para um site secundário através do PowerShell (Resource Manager)

Este artigo mostra para automatizar passos para a replicação de VMs de Hyper-V em nuvens do System Center Virtual Machine Manager (VMM) a uma nuvem VMM num site secundário no local, utilizar [do Azure Site Recovery](site-recovery-overview.md).

## <a name="prerequisites"></a>Pré-requisitos

- Reveja o [arquitetura do cenário e componentes](hyper-v-vmm-architecture.md).
- Reveja os [requisitos de suporte](site-recovery-support-matrix-to-sec-site.md) de todos os componentes.
- Certifique-se de que servidores do VMM e anfitriões Hyper-V estão em conformidade com [suportar requisitos](site-recovery-support-matrix-to-sec-site.md).
- Verifique se as VMs que pretende replicar cumprem [replicado suporte de máquina](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions)


## <a name="prepare-for-network-mapping"></a>Preparar o mapeamento da rede

[Mapeamento da rede](hyper-v-vmm-network-mapping.md) maps no local redes de VM do VMM em nuvens de origem e de destino. Mapeamento faz o seguinte:

- Liga-se as VMs para redes VM de destino adequada após a ativação pós-falha. 
- As VMs da réplica, coloca forma ideal em servidores de anfitrião de Hyper-V de destino. 
- Se não configurar o mapeamento da rede, as VMs de réplica não será ligada a uma rede VM após a ativação pós-falha.

Prepare VMM da seguinte forma:

1. Certifique-se de que tem [redes lógicas o VMM](https://docs.microsoft.com/system-center/vmm/network-logical) nos servidores do VMM de origem e de destino.
    - A rede lógica no servidor de origem deve ser associada com a nuvem de origem no qual estão localizados os anfitriões de Hyper-V.
    - A rede lógica no servidor de destino deve ser associada com a nuvem de destino.
1. Certifique-se de que tem [redes VM](https://docs.microsoft.com/system-center/vmm/network-virtual) nos servidores do VMM de origem e de destino. Redes VM devem ser ligadas à rede lógica em cada localização.
2. Ligar as VMs nos anfitriões de Hyper-V de origem à rede VM de origem. 

## <a name="prepare-for-powershell"></a>Preparar para o PowerShell

Certifique-se de que tem o Azure PowerShell pronto para começar.

- Se já estiver a utilizar o PowerShell, terá de atualizar para a versão 0.8.10 ou posterior.  [Saiba mais](/powershell/azureps-cmdlets-docs) sobre como configurar o PowerShell.
- Depois de ter de configurar e configurado o PowerShell, reveja o [service cmdlets](/powershell/azure/overview).
- Para saber mais sobre como utilizar os valores de parâmetros, entradas e saídas no Azure PowerShell, leia o [introdução](/powershell/azure/get-started-azureps) guia.

## <a name="set-up-a-subscription"></a>Configurar uma subscrição
1. A partir do Azure PowerShell, inicie sessão na sua conta do Azure:

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. Obter uma lista das suas subscrições, com a ID de subscrição. Anotar o ID da subscrição na qual pretende criar o Cofre de serviço de recuperação.   

        Get-AzureRmSubscription
3. Defina a subscrição do cofre:

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="create-a-recovery-services-vault"></a>Criar um cofre dos Serviços de Recuperação 
1. Se não tiver uma, crie um grupo de recursos do Azure Resource Manager.

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Criar um novo cofre de serviços de recuperação e guarde o objeto de cofre numa variável que será utilizada posteriormente: 

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location
   
    Pode obter o objeto de cofre depois de a criar, utilizando o cmdlet Get-AzureRMRecoveryServicesVault.

## <a name="set-the-vault-context"></a>Definir o contexto do Cofre
1. Obter um cofre existente:

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Defina o contexto de cofre:

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="install-the-azure-site-recovery-provider"></a>Instalar o fornecedor do Azure Site Recovery
1. Na máquina do VMM, crie um diretório, executando o seguinte comando:

       New-Item c:\ASR -type directory
2. Extraia os ficheiros utilizando o ficheiro de configuração do fornecedor transferido: pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Instale o fornecedor e aguarde pela conclusão da instalação:

       .\SetupDr.exe /i
       $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
       do
       {
         $isNotInstalled = $true;
         if(Test-Path $installationRegPath)
         {
           $isNotInstalled = $false;
         }
       }While($isNotInstalled)

4. Registe o servidor no cofre:

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="create-and-associate-a-replication-policy"></a>Crie e associe uma política de replicação
1. Crie uma política de replicação (neste caso para Hyper-V R2 de 2012), da seguinte forma:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod

    > [!NOTE]
    > A nuvem do VMM pode conter os anfitriões de Hyper-V versões diferentes do Windows Server em execução, mas a política de replicação é para uma versão específica de um sistema operativo. Se tiver de anfitriões diferentes em execução em sistemas operativos diferentes, em seguida, crie políticas de replicação separada para cada sistema. Por exemplo, se tiver cinco anfitriões, em execução nos servidores do Windows 2012 e três no Windows Server 2012 R2, crie duas políticas de replicação – um para cada tipo de sistema operativo.

2. Obter o contentor de proteção primário (primário VMM c) e o contentor de proteção de recuperação (nuvem VMM de recuperação):

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. Obter a política de replicação que criou, utilizando o nome amigável:

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. Começar a associação do contentor de proteção (nuvem VMM) com a política de replicação:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. Aguarde a conclusão da tarefa de associação da política. Pode verificar se a tarefa foi concluída através do seguinte fragmento de PowerShell:

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

   Depois de concluída a tarefa de processamento, execute o seguinte comando:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

Para verificar a conclusão da operação, siga os passos no [monitorizar a atividade](#monitor-activity).

##  <a name="configure-network-mapping"></a>Configurar o mapeamento da rede
1. Utilize este comando para obter os servidores do cofre atual. O comando armazena os servidores do Azure Site Recovery na variável de matriz $Servers.

        $Servers = Get-AzureRmSiteRecoveryServer
2. Execute este comando para retrievet as redes para o servidor do VMM de origem e o servidor do VMM de destino.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > O servidor do VMM de origem pode ser o primeiro ou segundo na matriz de servidores. Verifique os nomes de servidor do VMM e obter adequadamente as redes.


3. Este cmdlet cria um mapeamento entre a rede principal e a rede de recuperação. Especifica a rede principal como o primeiro elemento da $PrimaryNetworks e a rede de recuperação como o primeiro elemento da $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]


## <a name="enable-protection-for-vms"></a>Ativar a proteção para VMs
Depois de servidores, nuvens e redes estão configuradas corretamente, pode ativar a proteção para VMs na nuvem.

1. Para ativar a proteção, execute o seguinte comando para obter o contentor de proteção:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Obter a entidade de proteção (VM) da seguinte forma:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Ative a replicação para a VM:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="run-a-test-failover"></a>Executar uma ativação pós-falha de teste

Para testar a implementação pode executar uma ativação pós-falha de teste para uma única máquina virtual ou criar um plano de recuperação que contém várias VMs, e e executar uma ativação pós-falha de teste para o plano. A ativação pós-falha de teste simula o mecanismo de ativação pós-falha e recuperação numa rede isolada.

1. Obter a VM para o qual as VMs irão efetuar a ativação pós-falha:

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. Efetue uma ativação pós-falha de teste da seguinte forma:
    - Para uma única VM

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
    
    - Para um plano de recuperação:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

Para verificar a conclusão da operação, siga os passos no [monitorizar a atividade](#monitor-activity).

## <a name="run-planned-and-unplanned-failovers"></a>Executar as ativações pós-falha planeadas e não

1. Efetue uma ativação pós-falha planeada da seguinte forma:
    -  Para uma única VM:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
    - Para um plano de recuperação

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan
2. Efetue uma ativação pós-falha não planeada da seguinte forma:
    - Para uma única VM
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

    - Para um plano de recuperação:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <a name="monitor-activity"></a>Monitorizar a atividade
Utilize os seguintes comandos para monitorizar a atividade de failver. Tenha em atenção que poderá ter de aguardar entre tarefas de processamento concluir.

    Do
    {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

    if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Passos Seguintes

[Saiba mais](/powershell/module/azurerm.recoveryservices.backup/#recovery) sobre recuperação de sites com os cmdlets do PowerShell do Azure Resource Manager.
