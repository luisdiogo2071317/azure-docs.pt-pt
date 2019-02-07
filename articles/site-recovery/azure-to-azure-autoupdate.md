---
title: Atualização automática do serviço de mobilidade na recuperação após desastre do Azure | Documentos da Microsoft
description: Fornece uma descrição geral da atualização automática do serviço de mobilidade, ao replicar VMs do Azure com o Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: 3f0f28ca22321b537ab7e8911c5cbb513a1ade81
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55818929"
---
# <a name="automatic-update-of-the-mobility-service-in-azure-to-azure-replication"></a>Atualização automática do serviço de mobilidade na replicação do Azure para o Azure

O Azure Site Recovery tem uma cadência de lançamento mensal onde melhoramentos às funcionalidades existentes ou novos são adicionados e problemas conhecidos, se houver são fixos. Isso significaria que permaneça atualizado com o serviço, precisa planejar a implantação desses patches, mensalmente. Para evitar o cabeçalho de ativação pós-falha associado com a atualização, os utilizadores em vez disso, podem optar por permitir a recuperação de Site Gerir atualizações dos componentes. Conforme detalhado no [referência de arquitetura](azure-to-azure-architecture.md) para recuperação após desastre do Azure, instalar o serviço de mobilidade em todas as máquinas virtuais do Azure para o qual os replicação é ativada ao replicar máquinas virtuais do um Azure região para outra. Depois de ativar a atualização automática, a extensão de serviço de mobilidade é atualizada com cada nova versão. Este documento detalha o seguinte:

- Como funciona a atualização automática?
- Ativar as atualizações automáticas
- Problemas comuns e resolução de problemas
 
## <a name="how-does-automatic-update-work"></a>Como funciona a atualização automática

Permitir a recuperação de sites gerir as atualizações, um runbook global (que é utilizado pelos serviços do Azure) é implementado através de uma conta de automatização, que é criada na mesma subscrição que o cofre. Uma conta de automatização é utilizada para um cofre específico. O runbook verifica a existência de cada VM num cofre para o qual é atualizado automaticamente está ativados e inicia uma atualização da extensão do serviço de mobilidade, se uma versão mais recente está disponível. A agenda predefinida do runbook voltar a ocorrer diariamente às 12:00 AM de acordo com o fuso horário da geo da máquina virtual replicada. O agendamento do runbook também pode ser modificado por meio da conta de automatização pelo usuário, se necessário. 

> [!NOTE]
> Ativar atualizações automáticas não requer um reinício das suas VMs do Azure e não afeta a replicação em curso.

> [!NOTE]
> A faturação das tarefas utilizados pela conta de automatização baseia-se no número de minutos utilizados no mês e, por predefinição, 500 minutos estão incluídos como unidades gratuitas para uma conta de automatização de tempo de execução da tarefa. A execução das quantidades diárias de tarefa de um **alguns segundos para cerca de um minuto** e será **abordados os créditos gratuitos**.

UNIDADES gratuitas incluídas (por mês) * * ₹0.14 de 500 minutos de tempo de execução de tarefa de preço / minuto

## <a name="enable-automatic-updates"></a>Ativar as atualizações automáticas

Pode optar por permitir a recuperação de Site gerir as atualizações das seguintes formas:-

- [Como parte do passo ativar replicação](#as-part-of-the-enable-replication-step)
- [Ativar/desativar a extensão de atualizar as definições no interior do Cofre](#toggle-the-extension-update-settings-inside-the-vault)

### <a name="as-part-of-the-enable-replication-step"></a>Como parte do passo ativar replicação:

Quando ativa a replicação para a máquina virtual a partir de qualquer [da vista de máquina virtual](azure-to-azure-quickstart.md), ou [partir do Cofre de serviços de recuperação](azure-to-azure-how-to-enable-replication.md), obterá uma opção para optar por permitir a recuperação de sites para Gerir atualizações para a extensão de recuperação de sites ou gerir manualmente a mesma.

![enable-replication-auto-update](./media/azure-to-azure-autoupdate/enable-rep.png)

### <a name="toggle-the-extension-update-settings-inside-the-vault"></a>Ativar/desativar a extensão de atualizar as definições no interior do Cofre

1. No interior do cofre, navegue até **Manage**-> **infraestrutura do Site Recovery**
2. Sob **máquinas virtuais do Azure para**-> **definições de atualização da extensão**, clique o botão de alternar para escolher se pretende permitir *ASR para gerir atualizações* ou *gerir manualmente*. Clique em **Guardar**.

![cofre-Ativar/desativar-atualização automática](./media/azure-to-azure-autoupdate/vault-toggle.png)

> [!Important] 
> Quando escolhe *permitir que o ASR para gerir*, a configuração é aplicada a todas as máquinas virtuais no cofre correspondente.


> [!Note] 
> Ambas as opções a notificá-lo a conta de automatização que é usada para gerenciar as atualizações. Se pretende ativar esta funcionalidade pela primeira vez num cofre, será criada uma nova conta de automatização. Todas as replicações de enable subsequentes no mesmo cofre usará o criado anteriormente.

**Se pretender utilizar uma conta de automatização personalizada, utilize o script abaixo:-**

```azurepowershell
param(
    [Parameter(Mandatory=$true)]
    [String] $VaultResourceId,

    [Parameter(Mandatory=$true)]
    [ValidateSet("Enabled",'Disabled')]
    [Alias("Enabled or Disabled")]
    [String] $AutoUpdateAction,

    [Parameter(Mandatory=$false)]
    [String] $AutomationAccountArmId
)

$SiteRecoveryRunbookName = "Modify-AutoUpdateForVaultForPatner"
$TaskId = [guid]::NewGuid().ToString()
$SubscriptionId = "00000000-0000-0000-0000-000000000000"
$AsrApiVersion = "2018-01-10"
$RunAsConnectionName = "AzureRunAsConnection"
$ArmEndPoint = "https://management.azure.com"
$AadAuthority = "https://login.windows.net/"
$AadAudience = "https://management.core.windows.net/"
$AzureEnvironment = "AzureCloud"
$Timeout = "160"

function Throw-TerminatingErrorMessage
{
    Param
    (
        [Parameter(Mandatory=$true)]
        [String]
        $Message
    )

    throw ("Message: {0}, TaskId: {1}.") -f $Message, $TaskId
}

function Write-Tracing
{
    Param
    (
        [Parameter(Mandatory=$true)]      
        [ValidateSet("Informational", "Warning", "ErrorLevel", "Succeeded", IgnoreCase = $true)]
        [String]
        $Level,

        [Parameter(Mandatory=$true)]
        [String]
        $Message,

        [Switch]
        $DisplayMessageToUser
    )

    Write-Output $Message

}

function Write-InformationTracing
{
    Param
    (
        [Parameter(Mandatory=$true)]
        [String]
        $Message
    )

    Write-Tracing -Message $Message -Level Informational -DisplayMessageToUser
}

function ValidateInput()
{
    try
    {
        if(!$VaultResourceId.StartsWith("/subscriptions", [System.StringComparison]::OrdinalIgnoreCase))
        {
            $ErrorMessage = "The vault resource id should start with /subscriptions."
            throw $ErrorMessage
        }

        $Tokens = $VaultResourceId.SubString(1).Split("/")
        if(!($Tokens.Count % 2 -eq 0))
        {
            $ErrorMessage = ("Odd Number of tokens: {0}." -f $Tokens.Count)
            throw $ErrorMessage
        }

        if(!($Tokens.Count/2 -eq 4))
        {
            $ErrorMessage = ("Invalid number of resource in vault ARM id expected:4, actual:{0}." -f ($Tokens.Count/2))
            throw $ErrorMessage
        }

        if($AutoUpdateAction -ieq "Enabled" -and [string]::IsNullOrEmpty($AutomationAccountArmId))
        {
            $ErrorMessage = ("The automation account ARM id should not be null or empty when AutoUpdateAction is enabled.")
            throw $ErrorMessage
        }
    }
    catch
    {
        $ErrorMessage = ("ValidateInput failed with [Exception: {0}]." -f $_.Exception)
        Write-Tracing -Level ErrorLevel -Message $ErrorMessage -DisplayMessageToUser
        Throw-TerminatingErrorMessage -Message $ErrorMessage
    }
}

function Initialize-SubscriptionId()
{
    try
    {
        $Tokens = $VaultResourceId.SubString(1).Split("/")

        $Count = 0
        $ArmResources = @{}
        while($Count -lt $Tokens.Count)
        {
            $ArmResources[$Tokens[$Count]] = $Tokens[$Count+1]
            $Count = $Count + 2
        }
        
        return $ArmResources["subscriptions"]
    }
    catch
    {
        Write-Tracing -Level ErrorLevel -Message ("Initialize-SubscriptionId: failed with [Exception: {0}]." -f $_.Exception) -DisplayMessageToUser
        throw
    }
}

function Invoke-InternalRestMethod($Uri, $Headers, [ref]$Result)
{
    $RetryCount = 0
    $MaxRetry = 3
    do
    {
        try
        {
            $ResultObject = Invoke-RestMethod -Uri $Uri -Headers $Headers    
            ($Result.Value) += ($ResultObject)
            break
        }
        catch
        {
            Write-InformationTracing ("Retry Count: {0}, Exception: {1}." -f $RetryCount, $_.Exception)
            $RetryCount++
            if(!($RetryCount -le $MaxRetry))
            {
                throw
            }

            Start-Sleep -Milliseconds 2000
        }
    }while($true)
}

function Invoke-InternalWebRequest($Uri, $Headers, $Method, $Body, $ContentType, [ref]$Result)
{
    $RetryCount = 0
    $MaxRetry = 3
    do
    {
        try
        {
            $ResultObject = Invoke-WebRequest -Uri $UpdateUrl -Headers $Header -Method 'PATCH' `
                -Body $InputJson  -ContentType "application/json" -UseBasicParsing
            ($Result.Value) += ($ResultObject)
            break
        }
        catch
        {
            Write-InformationTracing ("Retry Count: {0}, Exception: {1}." -f $RetryCount, $_.Exception)
            $RetryCount++
            if(!($RetryCount -le $MaxRetry))
            {
                throw
            }

            Start-Sleep -Milliseconds 2000
        }
    }while($true)
}

function Get-Header([ref]$Header, $AadAudience, $AadAuthority, $RunAsConnectionName){
    try 
    {
        $RunAsConnection = Get-AutomationConnection -Name $RunAsConnectionName
        $TenantId = $RunAsConnection.TenantId
        $ApplicationId = $RunAsConnection.ApplicationId
        $CertificateThumbprint = $RunAsConnection.CertificateThumbprint
        $Path = "cert:\CurrentUser\My\{0}" -f $CertificateThumbprint
        $Secret = Get-ChildItem -Path $Path
        $ClientCredential = New-Object Microsoft.IdentityModel.Clients.ActiveDirectory.ClientAssertionCertificate(
                $ApplicationId,
                $Secret)

        # Trim the forward slash from the AadAuthority if it exist.
        $AadAuthority = $AadAuthority.TrimEnd("/")
        $AuthContext = New-Object Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext(
            "{0}/{1}" -f $AadAuthority, $TenantId )
        $AuthenticationResult = $authContext.AcquireToken($AadAudience, $Clientcredential)
        $Header.Value['Content-Type'] = 'application\json'
        $Header.Value['Authorization'] = $AuthenticationResult.CreateAuthorizationHeader()
        $Header.Value["x-ms-client-request-id"] = $TaskId + "/" + (New-Guid).ToString() + "-" + (Get-Date).ToString("u")
    }
    catch
    {
        $ErrorMessage = ("Get-BearerToken: failed with [Exception: {0}]." -f $_.Exception)
        Write-Tracing -Level ErrorLevel -Message $ErrorMessage -DisplayMessageToUser
        Throw-TerminatingErrorMessage -Message $ErrorMessage
    }
}

function Get-ProtectionContainerToBeModified([ref] $ContainerMappingList)
{
    try 
    {
        Write-InformationTracing ("Get protection container mappings : {0}." -f $VaultResourceId)
        $ContainerMappingListUrl = $ArmEndPoint + $VaultResourceId + "/replicationProtectionContainerMappings" + "?api-version=" + $AsrApiVersion
        
        Write-InformationTracing ("Getting the bearer token and the header.")
        Get-Header ([ref]$Header) $AadAudience $AadAuthority $RunAsConnectionName
        
        $Result = @()
        Invoke-InternalRestMethod -Uri $ContainerMappingListUrl -Headers $header -Result ([ref]$Result)
        $ContainerMappings = $Result[0]

        Write-InformationTracing ("Total retrieved container mappings: {0}." -f $ContainerMappings.Value.Count)
        foreach($Mapping in $ContainerMappings.Value)
        {
            if(($Mapping.properties.providerSpecificDetails -eq $null) -or ($Mapping.properties.providerSpecificDetails.instanceType -ine "A2A"))
            {
                Write-InformationTracing ("Mapping properties: {0}." -f ($Mapping.properties))
                Write-InformationTracing ("Ignoring container mapping: {0} as the provider does not match." -f ($Mapping.Id))
                continue;
            }

            if($Mapping.Properties.State -ine "Paired")
            {
                Write-InformationTracing ("Ignoring container mapping: {0} as the the state is not paired." -f ($Mapping.Id))
                continue;
            }

            Write-InformationTracing ("Provider specific details {0}." -f ($Mapping.properties.providerSpecificDetails))
            $MappingAutoUpdateStatus = $Mapping.properties.providerSpecificDetails.agentAutoUpdateStatus
            $MappingAutomationAccountArmId = $Mapping.properties.providerSpecificDetails.automationAccountArmId
            $MappingHealthErrorCount = $Mapping.properties.HealthErrorDetails.Count

            if($AutoUpdateAction -ieq "Enabled" -and
                ($MappingAutoUpdateStatus -ieq "Enabled") -and
                ($MappingAutomationAccountArmId -ieq $AutomationAccountArmId) -and
                ($MappingHealthErrorCount -eq 0))
            {
                Write-InformationTracing ("Provider specific details {0}." -f ($Mapping.properties))
                Write-InformationTracing ("Ignoring container mapping: {0} as the auto update is already enabled and is healthy." -f ($Mapping.Id))
                continue;
            }

            ($ContainerMappingList.Value).Add($Mapping.id)
        }
    }
    catch
    {
        $ErrorMessage = ("Get-ProtectionContainerToBeModified: failed with [Exception: {0}]." -f $_.Exception)
        Write-Tracing -Level ErrorLevel -Message $ErrorMessage -DisplayMessageToUser
        Throw-TerminatingErrorMessage -Message $ErrorMessage
    }
}

$OperationStartTime = Get-Date
$ContainerMappingList = New-Object System.Collections.Generic.List[System.String]
$JobsInProgressList = @()
$JobsCompletedSuccessList = @()
$JobsCompletedFailedList = @()
$JobsFailedToStart = 0
$JobsTimedOut = 0
$Header = @{}

$AzureRMProfile = Get-Module -ListAvailable -Name AzureRM.Profile | Select Name, Version, Path
$AzureRmProfileModulePath = Split-Path -Parent $AzureRMProfile.Path
Add-Type -Path (Join-Path $AzureRmProfileModulePath "Microsoft.IdentityModel.Clients.ActiveDirectory.dll")

$Inputs = ("Tracing inputs VaultResourceId: {0}, Timeout: {1}, AutoUpdateAction: {2}, AutomationAccountArmId: {3}." -f $VaultResourceId, $Timeout, $AutoUpdateAction, $AutomationAccountArmId)
Write-Tracing -Message $Inputs -Level Informational -DisplayMessageToUser
$CloudConfig = ("Tracing cloud configuration ArmEndPoint: {0}, AadAuthority: {1}, AadAudience: {2}." -f $ArmEndPoint, $AadAuthority, $AadAudience)
Write-Tracing -Message $CloudConfig -Level Informational -DisplayMessageToUser
$AutomationConfig = ("Tracing automation configuration RunAsConnectionName: {0}." -f $RunAsConnectionName)
Write-Tracing -Message $AutomationConfig -Level Informational -DisplayMessageToUser

ValidateInput
$SubscriptionId = Initialize-SubscriptionId
Get-ProtectionContainerToBeModified ([ref]$ContainerMappingList)

$Input = @{
  "properties"= @{
    "providerSpecificInput"= @{
        "instanceType" = "A2A"
        "agentAutoUpdateStatus" = $AutoUpdateAction
        "automationAccountArmId" = $AutomationAccountArmId
    }
  }
}
$InputJson = $Input |  ConvertTo-Json

if ($ContainerMappingList.Count -eq 0)
{
    Write-Tracing -Level Succeeded -Message ("Exiting as there are no container mappings to be modified.") -DisplayMessageToUser
    exit
}

Write-InformationTracing ("Container mappings to be updated has been retrieved with count: {0}." -f $ContainerMappingList.Count)

try
{
    Write-InformationTracing ("Start the modify container mapping jobs.")
    ForEach($Mapping in $ContainerMappingList)
    {
    try {
            $UpdateUrl = $ArmEndPoint + $Mapping + "?api-version=" + $AsrApiVersion
            Get-Header ([ref]$Header) $AadAudience $AadAuthority $RunAsConnectionName
            
            $Result = @()
            Invoke-InternalWebRequest -Uri $UpdateUrl -Headers $Header -Method 'PATCH' `
                -Body $InputJson  -ContentType "application/json" -Result ([ref]$Result)
            $Result = $Result[0]

            $JobAsyncUrl = $Result.Headers['Azure-AsyncOperation']
            Write-InformationTracing ("The modify container mapping job invoked with async url: {0}." -f $JobAsyncUrl)
            $JobsInProgressList += $JobAsyncUrl;

            # Rate controlling the set calls to maximum 60 calls per minute.
            # ASR throttling for set calls is 200 in 1 minute.
            Start-Sleep -Milliseconds 1000
        }
        catch{
            Write-InformationTracing ("The modify container mappings job creation failed for: {0}." -f $Ru)
            Write-InformationTracing $_
            $JobsFailedToStart++
        }
    }

    Write-InformationTracing ("Total modify container mappings has been initiated: {0}." -f $JobsInProgressList.Count)
}
catch
{
    $ErrorMessage = ("Modify container mapping jobs failed with [Exception: {0}]." -f $_.Exception)
    Write-Tracing -Level ErrorLevel -Message $ErrorMessage -DisplayMessageToUser
    Throw-TerminatingErrorMessage -Message $ErrorMessage
}

try
{
    while($JobsInProgressList.Count -ne 0)
    {
        Sleep -Seconds 30
        $JobsInProgressListInternal = @()
        ForEach($JobAsyncUrl in $JobsInProgressList)
        {
            try
            {
                Get-Header ([ref]$Header) $AadAudience $AadAuthority $RunAsConnectionName
                $Result = Invoke-RestMethod -Uri $JobAsyncUrl -Headers $header
                $JobState = $Result.Status
                if($JobState -ieq "InProgress")
                {
                    $JobsInProgressListInternal += $JobAsyncUrl
                }
                elseif($JobState -ieq "Succeeded" -or `
                    $JobState -ieq "PartiallySucceeded" -or `
                    $JobState -ieq "CompletedWithInformation")
                {
                    Write-InformationTracing ("Jobs succeeded with state: {0}." -f $JobState)
                    $JobsCompletedSuccessList += $JobAsyncUrl
                }
                else
                {
                    Write-InformationTracing ("Jobs failed with state: {0}." -f $JobState)
                    $JobsCompletedFailedList += $JobAsyncUrl
                }
            }
            catch
            {
                Write-InformationTracing ("The get job failed with: {0}. Ignoring the exception and retrying the next job." -f $_.Exception)

                # The job on which the tracking failed, will be considered in progress and tried again later.
                $JobsInProgressListInternal += $JobAsyncUrl
            }

            # Rate controlling the get calls to maximum 120 calls per minute.
            # ASR throttling for get calls is 10000 in 60 minutes.
            Start-Sleep -Milliseconds 500
        }

        Write-InformationTracing ("Jobs remaining {0}." -f $JobsInProgressListInternal.Count)

        $CurrentTime = Get-Date
        if($CurrentTime -gt $OperationStartTime.AddMinutes($Timeout))
        {
            Write-InformationTracing ("Tracing modify cloud pairing jobs has timed out.")
            $JobsTimedOut = $JobsInProgressListInternal.Count
            $JobsInProgressListInternal = @()
        }

        $JobsInProgressList = $JobsInProgressListInternal
    }
}
catch
{
    $ErrorMessage = ("Tracking modify cloud pairing jobs failed with [Exception: {0}]." -f $_.Exception)
    Write-Tracing -Level ErrorLevel -Message $ErrorMessage  -DisplayMessageToUser
    Throw-TerminatingErrorMessage -Message $ErrorMessage 
}

Write-InformationTracing ("Tracking modify cloud pairing jobs completed.")
Write-InformationTracing ("Modify cloud pairing jobs success: {0}." -f $JobsCompletedSuccessList.Count)
Write-InformationTracing ("Modify cloud pairing jobs failed: {0}." -f $JobsCompletedFailedList.Count)
Write-InformationTracing ("Modify cloud pairing jobs failed to start: {0}." -f $JobsFailedToStart)
Write-InformationTracing ("Modify cloud pairing jobs timedout: {0}." -f $JobsTimedOut)

if($JobsTimedOut -gt  0)
{
    $ErrorMessage = "One or more modify cloud pairing jobs has timedout."
    Write-Tracing -Level ErrorLevel -Message ($ErrorMessage)   
    Throw-TerminatingErrorMessage -Message $ErrorMessage
}
elseif($JobsCompletedSuccessList.Count -ne $ContainerMappingList.Count)
{
    $ErrorMessage = "One or more modify cloud pairing jobs failed."
    Write-Tracing -Level ErrorLevel -Message ($ErrorMessage)
    Throw-TerminatingErrorMessage -Message $ErrorMessage
}

Write-Tracing -Level Succeeded -Message ("Modify cloud pairing completed.") -DisplayMessageToUser
```

### <a name="manage-manually"></a>Gerir manualmente

1. Se existem novas atualizações disponíveis para o serviço de mobilidade instalado em suas VMs do Azure, verá uma notificação que lê a "atualização do agente de replicação de recuperação do novo Site está disponível. Clique para instalar."

     ![Janela de itens replicados](./media/vmware-azure-install-mobility-service/replicated-item-notif.png)
3. Selecione a notificação para abrir a página de seleção de máquina virtual.
4. Selecione as máquinas virtuais que pretende atualizar o serviço de mobilidade no e selecione **OK**.

     ![Lista de VMS de itens de replicados](./media/vmware-azure-install-mobility-service/update-okpng.png)

A tarefa de serviço de mobilidade de atualização é iniciada para cada uma das máquinas virtuais selecionadas.


## <a name="common-issues--troubleshooting"></a>Problemas comuns e resolução de problemas

Se houver um problema com as atualizações automáticas, será notificado das mesmas em "Problemas de configuração" no dashboard do cofre. 

No caso de tentou ativar as atualizações automáticas e falha, consulte abaixo para resolução de problemas.

**Erro**: Não tem permissões para criar uma conta Run As do Azure (principal de serviço) e conceder a função de Contribuinte ao principal de serviço. 
- Ação recomendada: Certifique-se de que a conta com sessão iniciada é atribuída a "contribuinte" e repita a operação. Consulte a [isso](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) documento para obter mais informações sobre a atribuir as permissões corretas.
 
Depois das atualizações automáticas são ativadas, a maioria dos problemas pode ser recuperada pelo serviço Site Recovery e requer que clicar no '**reparação**' botão.

![botão de reparação](./media/azure-to-azure-autoupdate/repair.png)

No caso do botão de reparação não estiver disponível, consulte a mensagem de erro exibida no painel de definições de extensão.

 - **Erro**: A conta Run As não tem permissão para aceder ao recurso de serviços de recuperação.

    **Ação recomendada**: Eliminar e, em seguida [voltar a criar a conta Run As](https://docs.microsoft.com/azure/automation/automation-create-runas-account) ou certifique-se de que o do Azure Active Directory do aplicativo Run As de automatização da conta tem acesso ao recurso de serviços de recuperação.

- **Erro**: Não foi encontrada a conta Run As. Um destes foi eliminado ou não criado - aplicação de diretório do Azure Active Directory, Principal de serviço, função, recurso de certificado de automatização, recurso de ligação de automatização - ou o Thumbprint não é idêntico entre o certificado e a ligação. 

    **Ação recomendada**: Eliminar e [, em seguida, voltar a criar a conta Run As](https://docs.microsoft.com/azure/automation/automation-create-runas-account).
