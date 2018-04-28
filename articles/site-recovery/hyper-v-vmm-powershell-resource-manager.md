---
title: Replicar VMs de Hyper-V em nuvens do Virtual Machine Manager para um site secundário com o PowerShell (Azure Resource Manager) | Microsoft Docs
description: Descreve como replicar VMs de Hyper-V em nuvens do Virtual Machine Manager para um site secundário do Virtual Machine Manager utilizando o PowerShell (Resource Manager)
services: site-recovery
author: sujaytalasila
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/12/2018
ms.author: sutalasi
ms.openlocfilehash: 7c6af1b63d9e7904f5a397200c6950c62df08832
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="replicate-hyper-v-vms-to-a-secondary-site-by-using-powershell-resource-manager"></a>Replicar VMs Hyper-V para um site secundário através do PowerShell (Resource Manager)

Este artigo mostra como automatizar os passos para replicação de VMs de Hyper-V em nuvens do System Center Virtual Machine Manager para uma nuvem do Virtual Machine Manager num secundário no local site utilizando [do Azure Site Recovery](site-recovery-overview.md).

## <a name="prerequisites"></a>Pré-requisitos

- Reveja o [arquitetura do cenário e componentes](hyper-v-vmm-architecture.md).
- Reveja os [requisitos de suporte](site-recovery-support-matrix-to-sec-site.md) de todos os componentes.
- Certifique-se de que o Virtual Machine Manager servidores e anfitriões Hyper-V estão em conformidade com [suportar requisitos](site-recovery-support-matrix-to-sec-site.md).
- Verifique se as VMs que pretende replicar cumprem [replicado suporte máquina](site-recovery-support-matrix-to-sec-site.md).


## <a name="prepare-for-network-mapping"></a>Preparar o mapeamento da rede

[Mapeamento da rede](hyper-v-vmm-network-mapping.md) maps no local redes de VM do Virtual Machine Manager em nuvens de origem e de destino. Mapeamento faz o seguinte:

- Liga-se as VMs para redes VM de destino adequada após a ativação pós-falha. 
- As VMs da réplica, coloca forma ideal em servidores de anfitrião de Hyper-V de destino. 
- Se não configurar o mapeamento da rede, as VMs de réplica não será ligada a uma rede VM após a ativação pós-falha.

Prepare o Virtual Machine Manager da seguinte forma:

* Certifique-se de que tem [redes lógicas do Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/network-logical) nos servidores de Gestor de Máquina Virtual de origem e de destino:

    - A rede lógica no servidor de origem deve ser associada com a nuvem de origem no qual estão localizados os anfitriões de Hyper-V.
    - A rede lógica no servidor de destino deve ser associada com a nuvem de destino.
* Certifique-se de que tem [redes VM](https://docs.microsoft.com/system-center/vmm/network-virtual) nos servidores de Gestor de Máquina Virtual de origem e de destino. Redes VM devem ser ligadas à rede lógica em cada localização.
* Ligar as VMs nos anfitriões de Hyper-V de origem à rede VM de origem. 

## <a name="prepare-for-powershell"></a>Preparar para o PowerShell

Certifique-se de que tem o Azure PowerShell pronto para começar:

- Se já utilizar o PowerShell, atualize para versão 0.8.10 ou posterior. [Saiba mais](/powershell/azureps-cmdlets-docs) sobre como configurar o PowerShell.
- Depois de definir e configurar o PowerShell, reveja o [service cmdlets](/powershell/azure/overview).
- Para saber mais sobre como utilizar os valores de parâmetros, entradas e saídas no PowerShell, leia o [começar](/powershell/azure/get-started-azureps) guia.

## <a name="set-up-a-subscription"></a>Configurar uma subscrição
1. A partir do PowerShell, inicie sessão sua conta do Azure.

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Connect-AzureRmAccount #-Credential $Cred
2. Obter uma lista das suas subscrições, com a ID de subscrição. Tenha em atenção o ID da subscrição na qual pretende criar o Cofre dos serviços de recuperação. 

        Get-AzureRmSubscription
3. Defina a subscrição do cofre.

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="create-a-recovery-services-vault"></a>Criar um cofre dos Serviços de Recuperação 
1. Se não tiver uma, crie um grupo de recursos do Azure Resource Manager.

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Crie um novo cofre de serviços de recuperação. Guarde o objeto de cofre numa variável a ser utilizado mais tarde. 

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location
   
    Pode obter o objeto de cofre depois de criar utilizando o cmdlet Get-AzureRMRecoveryServicesVault.

## <a name="set-the-vault-context"></a>Definir o contexto do Cofre
1. Obter um cofre existente.

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Defina o contexto do cofre.

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="install-the-site-recovery-provider"></a>Instale o fornecedor da recuperação de sites
1. Na máquina Virtual Machine Manager, crie um diretório, executando o seguinte comando:

       New-Item c:\ASR -type directory
2. Extraia os ficheiros utilizando o ficheiro de configuração do fornecedor transferido.

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Instale o fornecedor e aguarde pela conclusão da instalação.

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

4. Registe o servidor no cofre.

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="create-and-associate-a-replication-policy"></a>Crie e associe uma política de replicação
1. Crie uma política de replicação para o Hyper-V 2012 R2, da seguinte forma e de neste caso:

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
    > A nuvem do Virtual Machine Manager pode conter os anfitriões de Hyper-V versões diferentes do Windows Server em execução, mas a política de replicação é para uma versão específica de um sistema operativo. Se tiver de anfitriões diferentes em execução em sistemas operativos diferentes, crie políticas de replicação separados para cada sistema. Por exemplo, se tiver cinco anfitriões que executem Windows Server 2012 e três anfitriões em execução no Windows Server 2012 R2, crie duas políticas de replicação. Criar um para cada tipo de sistema operativo.

2. Obter o contentor de proteção primário (primário do Virtual Machine Manager nuvem) e o contentor de proteção de recuperação (recovery cloud do Virtual Machine Manager).

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
3. Obter a política de replicação que criou utilizando o nome amigável.

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
4. Começar a associação do contentor de proteção (nuvem do Virtual Machine Manager) com a política de replicação.

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
5. Aguarde que a tarefa de associação de política concluir. Para verificar se a tarefa estiver concluída, utilize o seguinte fragmento de PowerShell:

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

6. Depois do processamento de conclusão da tarefa, execute o seguinte comando:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

Para verificar a conclusão da operação, siga os passos no [monitorizar a atividade](#monitor-activity).

##  <a name="configure-network-mapping"></a>Configurar o mapeamento da rede
1. Utilize este comando para obter os servidores do cofre atual. O comando armazena os servidores do Site Recovery na variável de matriz $Servers.

        $Servers = Get-AzureRmSiteRecoveryServer
2. Execute este comando para obter as redes para o servidor do Virtual Machine Manager de origem e o servidor do Virtual Machine Manager de destino.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > O servidor do Virtual Machine Manager de origem pode ser o primeiro ou segundo na matriz de servidor. Verifique os nomes de servidor do Virtual Machine Manager e obter as redes adequadamente.


3. Este cmdlet cria um mapeamento entre a rede principal e a rede de recuperação. Especifica a rede principal como o primeiro elemento da $PrimaryNetworks. Especifica a rede de recuperação como o primeiro elemento da $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]


## <a name="enable-protection-for-vms"></a>Ativar a proteção para VMs
Depois de servidores, nuvens e redes estão configuradas corretamente, ative a proteção para VMs na nuvem.

1. Para ativar a proteção, execute o seguinte comando para obter o contentor de proteção:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Obter a entidade de proteção (VM), da seguinte forma:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Ative a replicação para a VM.

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="run-a-test-failover"></a>Executar uma ativação pós-falha de teste

Para testar a implementação, execute uma ativação pós-falha de teste para uma única máquina virtual. Também pode criar um plano de recuperação que contém várias VMs e executar uma ativação pós-falha de teste para o plano. A ativação pós-falha de teste simula o mecanismo de ativação pós-falha e recuperação numa rede isolada.

1. Obter a VM para o qual as VMs serão ativadas pós-falha.

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

2. Efetue uma ativação pós-falha de teste.

   Para uma única VM:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
    
   Para um plano de recuperação:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

Para verificar a conclusão da operação, siga os passos no [monitorizar a atividade](#monitor-activity).

## <a name="run-planned-and-unplanned-failovers"></a>Executar as ativações pós-falha planeadas e não

1. Execute uma ativação pós-falha.

   Para uma única VM:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

   Para um plano de recuperação:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

2. Execute uma ativação pós-falha não planeada.

   Para uma única VM:
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

   Para um plano de recuperação:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <a name="monitor-activity"></a>Monitorizar a atividade
Utilize os seguintes comandos para monitorizar a atividade de ativação pós-falha. Aguarde que o processamento concluir entre as tarefas.

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

[Saiba mais](/powershell/module/azurerm.recoveryservices.backup/#recovery) sobre a recuperação de Site com os cmdlets do PowerShell do Resource Manager.
