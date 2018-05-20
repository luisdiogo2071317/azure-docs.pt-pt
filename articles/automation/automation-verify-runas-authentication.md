---
title: Validar a configuração de conta de automatização do Azure
description: Este artigo descreve como confirmar que a configuração da sua conta de automatização está corretamente configurada.
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9bf5a3a1dc67752e3f911d07f23c7b408e6d38c3
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="test-azure-automation-run-as-account-authentication"></a>Testar a autenticação da conta Run As de Automatização do Azure
Depois de criar uma conta de Automatização com êxito, pode executar um teste simples para confirmar que consegue autenticar com êxito no Azure Resource Manager ou na implementação clássica do Azure com a sua conta Run As de Automatização recentemente criada ou atualizada.    

## <a name="automation-run-as-authentication"></a>Autenticação Run As de Automatização
Utilize o código de exemplo abaixo para [criar um runbook do PowerShell](automation-creating-importing-runbook.md) para verificar a autenticação com a conta Run As e também nos runbooks personalizados para autenticar e gerir os recursos do Resource Manager com a sua conta de Automatização.   

    $connectionName = "AzureRunAsConnection"
    try
    {
        # Get the connection "AzureRunAsConnection "
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

        "Logging in to Azure..."
        Connect-AzureRmAccount `
           -ServicePrincipal `
           -TenantId $servicePrincipalConnection.TenantId `
           -ApplicationId $servicePrincipalConnection.ApplicationId `
           -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    }
    catch {
       if (!$servicePrincipalConnection)
       {
          $ErrorMessage = "Connection $connectionName not found."
          throw $ErrorMessage
      } else{
          Write-Error -Message $_.Exception
          throw $_.Exception
      }
    }

    #Get all ARM resources from all resource groups
    $ResourceGroups = Get-AzureRmResourceGroup 

    foreach ($ResourceGroup in $ResourceGroups)
    {    
       Write-Output ("Showing resources in resource group " + $ResourceGroup.ResourceGroupName)
       $Resources = Find-AzureRmResource -ResourceGroupNameContains $ResourceGroup.ResourceGroupName | Select ResourceName, ResourceType
       ForEach ($Resource in $Resources)
       {
          Write-Output ($Resource.ResourceName + " of type " +  $Resource.ResourceType)
       }
       Write-Output ("")
    } 

Repare que o cmdlet utilizado para autenticar no runbook - **Connect-AzureRmAccount**, utiliza o *ServicePrincipalCertificate* conjunto de parâmetros.  Autentica utilizando o certificado de serviço principal, não as credenciais.  

Quando a [executar o runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) para validar a sua conta Run As, um [tarefa de runbook](automation-runbook-execution.md) é criado, a tarefa é apresentada a página e o estado da tarefa apresentados no **resumo da tarefa** mosaico. O estado da tarefa começará como *Em fila* com a indicação de que está à espera que uma função de trabalho de runbook na nuvem fique disponível. Em seguida, irá mudar para *A iniciar* quando uma função de trabalho reivindicar a tarefa e, em seguida, *A executar* quando o runbook começar a ser executado.  Quando tiver concluído a tarefa de runbook, vemos um estado de **Concluído**.

Para ver os resultados detalhados do runbook, clique no mosaico **Saída**.  Na página **Saída**, deve verificar se foi autenticado com êxito e devolveu uma lista de todos os recursos em todos os grupos de recursos na sua subscrição.  

Não se esqueça de remover o bloco de código que começa com o comentário `#Get all ARM resources from all resource groups` quando reutilizar o código para os runbooks.

## <a name="classic-run-as-authentication"></a>Autenticação Run As clássica
Utilize o código de exemplo abaixo para [criar um runbook do PowerShell](automation-creating-importing-runbook.md) para verificar a autenticação com a conta Run As Clássica e também nos runbooks personalizados para autenticar e gerir os recursos no modelo de implementação clássico.  

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get the connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate to Azure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in the Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting the certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in the Automation account."
    }

    Write-Verbose "Authenticating to Azure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID
    
    #Get all VMs in the subscription and return list with name of each
    Get-AzureVM | ft Name

Quando [executar o runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) para validar a conta Run As, é criada uma [tarefa de runbook](automation-runbook-execution.md), é apresentada a página Tarefa, bem como o estado da tarefa no mosaico **Resumo da Tarefa**. O estado da tarefa começará como *Em fila* com a indicação de que está à espera que uma função de trabalho de runbook na nuvem fique disponível. Em seguida, irá mudar para *A iniciar* quando uma função de trabalho reivindicar a tarefa e, em seguida, *A executar* quando o runbook começar a ser executado.  Quando tiver concluído a tarefa de runbook, vemos um estado de **Concluído**.

Para ver os resultados detalhados do runbook, clique no mosaico **Saída**.  Na página **Saída**, deve verificar se foi autenticado com êxito e devolveu uma lista de todas as VMs do Azure por VMName implementadas na sua subscrição.  

Não se esqueça de remover o cmdlet **Get-AzureVM** quando reutilizar o código para os runbooks.

## <a name="next-steps"></a>Passos Seguintes
* Para começar com runbooks do PowerShell, consulte [My first PowerShell runbook (O meu primeiro runbook do PowerShell)](automation-first-runbook-textual-powershell.md).
* Para obter mais informações sobre a Criação de Gráficos, veja [Graphical authoring in Azure Automation](automation-graphical-authoring-intro.md) (Criação de gráficos na Automatização do Azure).
