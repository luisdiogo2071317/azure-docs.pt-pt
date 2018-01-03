---
title: Script do PowerShell do Azure de exemplo - migrar blobs entre contas de armazenamento utilizando o AzCopy no Windows | Microsoft Docs
description: "Utilizar o AzCopy, copia os conteúdos de uma conta de armazenamento do Azure para outro."
services: storage
documentationcenter: na
author: roygara
manager: jeconnoc
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/22/2017
ms.author: v-rogara
ms.openlocfilehash: 37b513c1bb76876b3846a8db9511f214ed628132
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/23/2017
---
# <a name="migrate-blobs-across-storage-accounts-using-azcopy-on-windows"></a>Migrar os blobs em contas de armazenamento utilizando o AzCopy no Windows

Este script aceita um nome de conta de armazenamento de origem e chave, um nome de conta de armazenamento de destino e chave e o filepath completo de AzCopy.exe. Em seguida, o script copia todos os conteúdos da conta de armazenamento de origem para a conta de armazenamento de destino. Neste momento, isto só é suportado no Windows.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

Este exemplo também requer o [versão mais recente do AzCopy no Windows](http://aka.ms/downloadazcopy). O diretório de instalação predefinida é`C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\`

Segue-se um exemplo de entrada para este script:

```Powershell
srcStorageAccountName: ExampleSourceStorageAccountName
srcStorageAccountKey: ExampleSourceStorageAccountKey
DestStorageAccountName: ExampleTargetStorageAccountName
DestStorageAccountKey: ExampleTargetStorageAccountKey
AzCopyPath: C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe
```


## <a name="sample-script"></a>Script de exemplo

```Powershell
# Run the script in a new open Powershell window, which has not run other cmdlets, or AzCopy might runs slow.
# Need install Azure PowerShell before run the script: https://github.com/Azure/azure-powershell/releases

 param (
    [Parameter(Mandatory = $true, 
    HelpMessage= "Source Storage account name.")]
    [ValidatePattern("^[a-z0-9`]{3,24}$")]
    [String]$srcStorageAccountName,

    [Parameter(Mandatory = $true, 
    HelpMessage= "Source Storage account key.")]
    [String]$srcStorageAccountKey,
    
    [Parameter(Mandatory = $true, 
    HelpMessage= "Destination Storage account name.")]
    [ValidatePattern("^[a-z0-9`]{3,24}$")]
    [String]$DestStorageAccountName,

    [Parameter(Mandatory = $true, 
    HelpMessage= "Destination Storage account key.")]
    [String]$DestStorageAccountKey,

    [Parameter(Mandatory = $true, 
    HelpMessage= "Input the Path of AzCopy.exe, e.g.: C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy.exe")]
    [String]$AzCopyPath,

    [Parameter(Mandatory = $false, 
    HelpMessage='Sets the number of retries in an event of failure. Set to 0 for no retry, set -1 for infinite retry.')]
    [Int32]$RetryTimes = 3,

    [Parameter(Mandatory = $false, 
    ValueFromPipeline = $true, 
    HelpMessage='Used for resume operation. When provided, the script will copy containers that are alphabetically after $LastSuccessContainerName')]
    [String]$LastSuccessContainerName = $null
 )


# Create transfer context
$srcCtx = New-AzureStorageContext -StorageAccountName $srcStorageAccountName -StorageAccountKey $srcStorageAccountKey
$destCtx = New-AzureStorageContext -StorageAccountName $destStorageAccountName -StorageAccountKey $destStorageAccountKey

$OutputLastSuccessContainer = $LastSuccessContainerName;
$HasReachedLastSuccessContainer = $false;

if ($LastSuccessContainerName -eq $null -or $LastSuccessContainerName -eq "")
{
    $HasReachedLastSuccessContainer = $true;
}

# For list container
$MaxReturn = 250
$Token = $Null

do{
    # List source containers
    $retry = 1
    $srcContainers = Get-AzureStorageContainer -MaxCount $MaxReturn -ContinuationToken $Token -Context $srcCtx

    # If list container fail, retry it
    while(($srcContainers -eq $null) -and ($RetryTimes -eq -1 -or $retry -le $retryTimes))
    {
        Write-Host "Retry List containers $retry"
        $srcContainers = Get-AzureStorageContainer -MaxCount $MaxReturn -ContinuationToken $Token -Context $srcCtx
        $retry++
    }

    # If list container fail after retry, break script
    if ($srcContainers -eq $null){
        Write-Error "List container failed, no container is listed.";
        $CopyContainerFail = $true
        break;
    }
    $Token = $srcContainers[$srcContainers.Count -1].ContinuationToken;

    # Transfer containers one by one
    $CopyContainerFail = $false
    foreach ($container in $srcContainers)
    {
        if (!$HasReachedLastSuccessContainer)
        {
            if ($container.Name -eq $LastSuccessContainerName)
            {
                $HasReachedLastSuccessContainer = $true;
            }
            Write-Host "Skipping container copy: $($container.Name)"
            Continue;
        }

        Write-Host "Start copying container: $($container.Name)"
        $retry = 1

        # Get AzCopy command for transfer one container
        $destContainer = $destCtx.StorageAccount.CreateCloudBlobClient().GetContainerReference($container.Name)
        $azCopyCmd = [string]::Format("""{0}"" /source:{1} /dest:{2} /sourcekey:""{3}"" /destkey:""{4}"" /snapshot /y /s",$AzCopyPath, $container.CloudBlobContainer.Uri.AbsoluteUri, $destContainer.Uri.AbsoluteUri, $srcStorageAccountKey, $DestStorageAccountKey)
    
        # Execute the AzCopy command first time
        Write-Host "$azCopyCmd"
        $result = cmd /c $azCopyCmd
        foreach($s in $result)
        {
            Write-Host $s 
        }

        # If transfer failed, retry it
        while(($LASTEXITCODE -ne 0) -and ($RetryTimes -eq -1 -or $retry -le $retryTimes))
        {
            Write-Host "Retry $retry : $azCopyCmd"
            $result = cmd /c $azCopyCmd
            foreach($s in $result)
            {
                Write-Host $s 
            }
            $retry++
        }

        # If tranfer failed after retry, print error
        if ($LASTEXITCODE -ne 0){
            Write-Error "Container copy failed: $($container.Name)";
            $CopyContainerFail = $true
            break;
        }
        else
        {
            Write-Host "Finish copying container: $($container.Name)"
            Write-Host ""
            $OutputLastSuccessContainer = $container.Name
        }
    }
    if($CopyContainerFail)
    {
        break;
    }
}
While ($Token -ne $Null)

# Summary the copy result
if ($CopyContainerFail)
{
    if(($OutputLastSuccessContainer -ne $null) -and ($OutputLastSuccessContainer -ne ""))
    {
        Write-Warning "To resume, rerun the script by appending the option: ""-LastSuccessContainer $OutputLastSuccessContainer"""
        return $OutputLastSuccessContainer
    }
    else
    {
        Write-Warning "To resume, rerun the script."
        return $null
    }
}
else
{
    Write-Host "All Containers copied successfully."
    return ""
}
```

## <a name="script-explanation"></a>Explicação de script

Este script utiliza os seguintes comandos para copiar dados de uma conta de armazenamento para outro. Cada item nas ligações de tabela para a documentação específica do comando.

| Comando | Notas |
|---|---|
| [Get-AzureStorageContainer](/powershell/module/azure.storage/Get-AzureStorageContainer) | Devolve os contentores associados a esta conta de armazenamento. |
| [Novo AzureStorageContext](/powershell/module/azure.storage/New-AzureStorageContext) | Cria um contexto de armazenamento do Azure. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o módulo Azure PowerShell, consulte [documentação do Azure PowerShell](/powershell/azure/overview).

Exemplos de script do PowerShell de armazenamento adicional que podem ser encontrados na [exemplos do PowerShell para o Blob storage do Azure](../blobs/storage-samples-blobs-powershell.md).
