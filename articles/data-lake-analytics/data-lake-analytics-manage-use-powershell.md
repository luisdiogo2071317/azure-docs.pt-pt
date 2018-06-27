---
title: Gerir a Análise do Azure Data Lake com o Azure PowerShell
description: Este artigo descreve como utilizar o Azure PowerShell para gerir contas de Data Lake Analytics, origens de dados, os utilizadores e tarefas.
services: data-lake-analytics
ms.service: data-lake-analytics
author: matt1883
ms.author: mahi
manager: kfile
editor: jasonwhowell
ms.assetid: ad14d53c-fed4-478d-ab4b-6d2e14ff2097
ms.topic: conceptual
ms.date: 06/02/2018
ms.openlocfilehash: 560f36dc64480fd6aceaa50226b191ee40d2486f
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36959853"
---
# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a>Gerir a Análise do Azure Data Lake com o Azure PowerShell
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Este artigo descreve como gerir contas de Azure Data Lake Analytics, origens de dados, os utilizadores e tarefas utilizando o Azure PowerShell.

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar o PowerShell com o Data Lake Analytics, recolha os seguintes tipos de informações: 

* **ID de subscrição**: O ID da subscrição do Azure que contém a sua conta do Data Lake Analytics.
* **Grupo de recursos**: O nome do grupo de recursos do Azure que contém a sua conta do Data Lake Analytics.
* **O nome de conta do Data Lake Analytics**: O nome da sua conta do Data Lake Analytics.
* **Nome da conta do Data Lake Store da predefinição**: conta de cada Data Lake Analytics tem uma conta de Data Lake Store predefinida.
* **Localização**: A localização da sua conta do Data Lake Analytics, por exemplo, "EUA Leste 2" ou outro suportado localizações.

Os fragmentos de PowerShell neste tutorial utilizam estas variáveis para armazenar estas informações

```powershell
$subId = "<SubscriptionId>"
$rg = "<ResourceGroupName>"
$adla = "<DataLakeAnalyticsAccountName>"
$adls = "<DataLakeStoreAccountName>"
$location = "<Location>"
```

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

### <a name="log-in-using-interactive-user-authentication"></a>Inicie sessão utilizando a autenticação de utilizador interativo

Inicie sessão com um ID de subscrição ou pelo nome de subscrição

```powershell
# Using subscription id
Connect-AzureRmAccount -SubscriptionId $subId

# Using subscription name
Connect-AzureRmAccount -SubscriptionName $subname 
```

## <a name="saving-authenticaiton-context"></a>A guardar authenticaiton contexto

O `Connect-AzureRmAccount` cmdlet sempre pede-lhe credenciais. Pode evitar que está a ser solicitado, utilizando os seguintes cmdlets:

```powershell
# Save login session information
Save-AzureRmProfile -Path D:\profile.json  

# Load login session information
Select-AzureRmProfile -Path D:\profile.json 
```

### <a name="log-in-using-a-service-principal-identity-spi"></a>Inicie sessão com uma identidade de serviço Principal (SPI)

```powershell
$tenantid = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"  
$spi_appname = "appname" 
$spi_appid = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" 
$spi_secret = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" 

$pscredential = New-Object System.Management.Automation.PSCredential ($spi_appid, (ConvertTo-SecureString $spi_secret -AsPlainText -Force))
Login-AzureRmAccount -ServicePrincipal -TenantId $tenantid -Credential $pscredential -Subscription $subid
```

## <a name="manage-accounts"></a>Gerir contas


### <a name="list-accounts"></a>Contas de lista

```powershell
# List Data Lake Analytics accounts within the current subscription.
Get-AdlAnalyticsAccount

# List Data Lake Analytics accounts within a specific resource group.
Get-AdlAnalyticsAccount -ResourceGroupName $rg
```

### <a name="create-an-account"></a>Criar uma conta

Cada conta de Data Lake Analytics requer uma conta de Data Lake Store predefinida que utiliza para armazenar registos. Pode reutilizar uma conta existente ou criar uma conta. 

```powershell
# Create a data lake store if needed, or you can re-use an existing one
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

### <a name="get-account-information"></a>Obter informações de conta

Obter informações sobre a uma conta.

```powershell
Get-AdlAnalyticsAccount -Name $adla
```

### <a name="check-if-an-account-exists"></a>Verifique se existe uma conta

```powershell
Test-AdlAnalyticsAccount -Name $adla
```

## <a name="manage-data-sources"></a>Gerir origens de dados
Atualmente, o Azure Data Lake Analytics suporta as seguintes origens de dados:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Armazenamento do Azure](../storage/common/storage-introduction.md)

Cada conta de Data Lake Analytics tem uma conta de Data Lake Store predefinida. A conta de Data Lake Store predefinida é utilizada para armazenar os registos de auditoria de tarefa e os metadados de tarefa. 

### <a name="find-the-default-data-lake-store-account"></a>Localizar a conta de Data Lake Store predefinida

```powershell
$adla_acct = Get-AdlAnalyticsAccount -Name $adla
$dataLakeStoreName = $adla_acct.DefaultDataLakeAccount
```

Pode encontrar a conta de Data Lake Store predefinida por filtrar a lista de origens de dados pelo `IsDefault` propriedade:

```powershell
Get-AdlAnalyticsDataSource -Account $adla  | ? { $_.IsDefault } 
```

### <a name="add-a-data-source"></a>Adicionar uma origem de dados

```powershell

# Add an additional Storage (Blob) account.
$AzureStorageAccountName = "<AzureStorageAccountName>"
$AzureStorageAccountKey = "<AzureStorageAccountKey>"
Add-AdlAnalyticsDataSource -Account $adla -Blob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

# Add an additional Data Lake Store account.
$AzureDataLakeStoreName = "<AzureDataLakeStoreAccountName"
Add-AdlAnalyticsDataSource -Account $adla -DataLakeStore $AzureDataLakeStoreName 
```

### <a name="list-data-sources"></a>Origens de dados de lista

```powershell
# List all the data sources
Get-AdlAnalyticsDataSource -Name $adla

# List attached Data Lake Store accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "DataLakeStore"

# List attached Storage accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "Blob"
```

## <a name="submit-u-sql-jobs"></a>Submeter tarefas U-SQL

### <a name="submit-a-string-as-a-u-sql-job"></a>Uma cadeia de envio como uma tarefa de U-SQL

```powershell
$script = @"
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
"@

$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 

Submit-AdlJob -AccountName $adla -Script $script -Name "Demo"
```

### <a name="submit-a-file-as-a-u-sql-job"></a>Submeter um ficheiro como uma tarefa de U-SQL

```powershell
$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 
Submit-AdlJob -AccountName $adla –ScriptPath $scriptpath -Name "Demo"
```

### <a name="list-jobs"></a>Lista de tarefas

A saída inclui as tarefas atualmente em execução e as tarefas que foram concluídas recentemente.

```powershell
Get-AdlJob -Account $adla
```

### <a name="list-the-top-n-jobs"></a>Lista as tarefas principais N

Por predefinição, que a lista de tarefas é ordenada no envio de tempo. Por isso, as tarefas submetidas mais recentemente são apresentados primeiro. Por predefinição, a conta do ADLA memorizou tarefas durante 180 dias, mas o cmdlet Get-AdlJob por predefinição devolve apenas o primeiro 500. Utilize o - parâmetro superior para listar um número específico de tarefas.

```powershell
$jobs = Get-AdlJob -Account $adla -Top 10
```

### <a name="list-jobs-by-job-state"></a>Lista de tarefas por Estado da tarefa

Utilizar o `-State` parâmetro. Pode combinar qualquer um destes valores:

* `Accepted`
* `Compiling`
* `Ended`
* `New`
* `Paused`
* `Queued`
* `Running`
* `Scheduling`
* `Start`

```powershell
# List the running jobs
Get-AdlJob -Account $adla -State Running

# List the jobs that have completed
Get-AdlJob -Account $adla -State Ended

# List the jobs that have not started yet
Get-AdlJob -Account $adla -State Accepted,Compiling,New,Paused,Scheduling,Start
```

### <a name="list-jobs-by-job-result"></a>Lista de tarefas, o resultado da tarefa

Utilize o `-Result` parâmetro para detetar se as tarefas terminadas foi concluída com êxito. Tem estes valores:

* Cancelado
* Com Falhas
* Nenhuma
* Bem-sucedido

``` powershell
# List Successful jobs.
Get-AdlJob -Account $adla -State Ended -Result Succeeded

# List Failed jobs.
Get-AdlJob -Account $adla -State Ended -Result Failed
```

### <a name="list-jobs-by-job-submitter"></a>Lista de tarefas por submissor da tarefa

O `-Submitter` parâmetro ajuda-o a identificar quem submetida uma tarefa.

```powershell
Get-AdlJob -Account $adla -Submitter "joe@contoso.com"
```

### <a name="list-jobs-by-submission-time"></a>Lista de tarefas do tempo de submissão

O `-SubmittedAfter` é útil para filtrar para um intervalo de tempo.


```powershell
# List  jobs submitted in the last day.
$d = [DateTime]::Now.AddDays(-1)
Get-AdlJob -Account $adla -SubmittedAfter $d

# List  jobs submitted in the last seven day.
$d = [DateTime]::Now.AddDays(-7)
Get-AdlJob -Account $adla -SubmittedAfter $d
```

### <a name="get-job-status"></a>Obter estado da tarefa

Obtenha o estado de uma tarefa específica.

```powershell
Get-AdlJob -AccountName $adla -JobId $job.JobId
```


### <a name="cancel-a-job"></a>Cancelar uma tarefa

```powershell
Stop-AdlJob -Account $adla -JobID $jobID
```

### <a name="wait-for-a-job-to-finish"></a>Aguarde a conclusão de conclusão

Em vez de repetir `Get-AdlAnalyticsJob` até que seja uma tarefa concluída, pode utilizar o `Wait-AdlJob` cmdlet para aguardar a tarefa terminar.

```powershell
Wait-AdlJob -Account $adla -JobId $job.JobId
```

## <a name="analyzing-job-history"></a>Analisar o histórico de tarefas

Com o Azure PowerShell para analisar o histórico de tarefas que foram executados no Data Lake analytics é uma técnica de elevado desempenho. Pode utilizá-lo para obter informações acerca da utilização e o custo. Pode saber mais observando a [repositório de exemplo de análise de histórico da tarefa](https://github.com/Azure-Samples/data-lake-analytics-powershell-job-history-analysis)  

## <a name="list-job-pipelines-and-recurrences"></a>lista tarefa pipelines e recurrences

Utilize o `Get-AdlJobPipeline` cmdlet para ver as informações de pipeline previamente submetido tarefas.

```powershell
$pipelines = Get-AdlJobPipeline -Account $adla
$pipeline = Get-AdlJobPipeline -Account $adla -PipelineId "<pipeline ID>"
```

Utilize o `Get-AdlJobRecurrence` cmdlet para ver as informações de periodicidade para anteriormente submetido tarefas.

```powershell
$recurrences = Get-AdlJobRecurrence -Account $adla

$recurrence = Get-AdlJobRecurrence -Account $adla -RecurrenceId "<recurrence ID>"
```


## <a name="manage-compute-policies"></a>Gerir políticas de computação

### <a name="list-existing-compute-policies"></a>Lista as políticas existentes de computação

O `Get-AdlAnalyticsComputePolicy` cmdlet obtém informações sobre as políticas de computação para uma conta de Data Lake Analytics.

```powershell
$policies = Get-AdlAnalyticsComputePolicy -Account $adla
```

### <a name="create-a-compute-policy"></a>Criar uma política de computação

O `New-AdlAnalyticsComputePolicy` cmdlet cria uma nova política de computação para uma conta de Data Lake Analytics. Neste exemplo define os AUs máximas disponíveis para o utilizador especificado para 50 e a prioridade do trabalho mínimo para 250.

```powershell
$userObjectId = (Get-AzureRmAdUser -SearchString "garymcdaniel@contoso.com").Id

New-AdlAnalyticsComputePolicy -Account $adla -Name "GaryMcDaniel" -ObjectId $objectId -ObjectType User -MaxDegreeOfParallelismPerJob 50 -MinPriorityPerJob 250
```
## <a name="manage-files"></a>Gerir ficheiros

### <a name="check-for-the-existence-of-a-file"></a>Verifique a existência de um ficheiro.

```powershell
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

### <a name="uploading-and-downloading"></a>Carregar e transferir

Carregar um ficheiro.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\data.tsv" -Destination "/data_copy.csv" 
```

Carregar um recursivamente pasta inteira.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\myData\" -Destination "/myData/" -Recurse
```

Transferir um ficheiro.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "c:\data.csv"
```

Transferir um recursivamente pasta inteira.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/" -Destination "c:\myData\" -Recurse
```

> [!NOTE]
> Se o processo de carregamento ou transferência for interrompido, pode tentar retomar o processo executando o cmdlet novamente com o ``-Resume`` sinalizador.

## <a name="manage-the-u-sql-catalog"></a>Gerir o catálogo U-SQL

O catálogo U-SQL é utilizado para a estrutura de dados e o código para que possa ser partilhados por scripts U-SQL. O catálogo permite que o desempenho mais elevado possível com os dados no Azure Data Lake. Para obter mais informações, veja [Utilizar catálogo U-SQL](data-lake-analytics-use-u-sql-catalog.md).

### <a name="list-items-in-the-u-sql-catalog"></a>Itens de lista no catálogo U-SQL

```powershell
# List U-SQL databases
Get-AdlCatalogItem -Account $adla -ItemType Database 

# List tables within a database
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database"

# List tables within a schema.
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database.schema"
```

### <a name="list-all-the-assemblies-the-u-sql-catalog"></a>Listar todos os o catálogo de assemblagens U-SQL

```powershell
$dbs = Get-AdlCatalogItem -Account $adla -ItemType Database

foreach ($db in $dbs)
{
    $asms = Get-AdlCatalogItem -Account $adla -ItemType Assembly -Path $db.Name

    foreach ($asm in $asms)
    {
        $asmname = "[" + $db.Name + "].[" + $asm.Name + "]"
        Write-Host $asmname
    }
}
```

### <a name="get-details-about-a-catalog-item"></a>Obter detalhes sobre um item de catálogo

```powershell
# Get details of a table
Get-AdlCatalogItem  -Account $adla -ItemType Table -Path "master.dbo.mytable"

# Test existence of a U-SQL database.
Test-AdlCatalogItem  -Account $adla -ItemType Database -Path "master"
```

### <a name="store-credentials-in-the-catalog"></a>Credenciais do arquivo no catálogo

Dentro de uma base de dados do U-SQL, crie um objeto de credencial para uma base de dados alojado no Azure. Atualmente, as credenciais de U-SQL são o único tipo de item de catálogo que pode criar através do PowerShell.

```powershell
$dbName = "master"
$credentialName = "ContosoDbCreds"
$dbUri = "https://contoso.database.windows.net:8080"

New-AdlCatalogCredential -AccountName $adla `
          -DatabaseName $db `
          -CredentialName $credentialName `
          -Credential (Get-Credential) `
          -Uri $dbUri
```

## <a name="manage-firewall-rules"></a>Gerir regras de firewall

### <a name="list-firewall-rules"></a>Lista regras de firewall

```powershell
Get-AdlAnalyticsFirewallRule -Account $adla
```

### <a name="add-a-firewall-rule"></a>Adicionar uma regra de firewall

```powershell
$ruleName = "Allow access from on-prem server"
$startIpAddress = "<start IP address>"
$endIpAddress = "<end IP address>"

Add-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

### <a name="modify-a-firewall-rule"></a>Modificar uma regra de firewall

```powershell
Set-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

### <a name="remove-a-firewall-rule"></a>Remover uma regra de firewall

```powershell
Remove-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName
```

### <a name="allow-azure-ip-addresses"></a>Permitir que os endereços IP do Azure

```powershell
Set-AdlAnalyticsAccount -Name $adla -AllowAzureIpState Enabled
```

```powershell
Set-AdlAnalyticsAccount -Name $adla -FirewallState Enabled
Set-AdlAnalyticsAccount -Name $adla -FirewallState Disabled
```


## <a name="working-with-azure"></a>Trabalhar com o Azure

### <a name="get-details-of-azurerm-errors"></a>Obter os detalhes de erros de AzureRm

```powershell
Resolve-AzureRmError -Last
```

### <a name="verify-if-you-are-running-as-an-administrator-on-your-windows-machine"></a>Certifique-se de que se estiver a executar como administrador no seu computador Windows

```powershell
function Test-Administrator  
{  
    $user = [Security.Principal.WindowsIdentity]::GetCurrent();
    $p = New-Object Security.Principal.WindowsPrincipal $user
    $p.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)  
}
```

### <a name="find-a-tenantid"></a>Localizar um TenantID

A partir de um nome de subscrição:

```powershell
function Get-TenantIdFromSubcriptionName( [string] $subname )
{
    $sub = (Get-AzureRmSubscription -SubscriptionName $subname)
    $sub.TenantId
}

Get-TenantIdFromSubcriptionName "ADLTrainingMS"
```

A partir de um ID de subscrição:

```powershell
function Get-TenantIdFromSubcriptionId( [string] $subid )
{
    $sub = (Get-AzureRmSubscription -SubscriptionId $subid)
    $sub.TenantId
}

$subid = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
Get-TenantIdFromSubcriptionId $subid
```

De um endereço de domínio, como "contoso.com"

```powershell
function Get-TenantIdFromDomain( $domain )
{
    $url = "https://login.windows.net/" + $domain + "/.well-known/openid-configuration"
    return (Invoke-WebRequest $url|ConvertFrom-Json).token_endpoint.Split('/')[3]
}

$domain = "contoso.com"
Get-TenantIdFromDomain $domain
```

### <a name="list-all-your-subscriptions-and-tenant-ids"></a>Listar todas as subscrições e IDs de inquilino

```powershell
$subs = Get-AzureRmSubscription
foreach ($sub in $subs)
{
    Write-Host $sub.Name "("  $sub.Id ")"
    Write-Host "`tTenant Id" $sub.TenantId
}
```

## <a name="create-a-data-lake-analytics-account-using-a-template"></a>Criar uma conta de Data Lake Analytics utilizando um modelo

Também pode utilizar um modelo de grupo de recursos do Azure utilizando o seguinte exemplo: [criar uma conta de Data Lake Analytics utilizando um modelo](https://github.com/Azure-Samples/data-lake-analytics-create-account-with-arm-template)

## <a name="next-steps"></a>Passos Seguintes
* [Descrição geral do Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* Introdução ao Data Lake Analytics com [portal do Azure](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI 2.0](data-lake-analytics-get-started-cli2.md)
* Gerir o Azure Data Lake Analytics com [portal do Azure](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI](data-lake-analytics-manage-use-cli.md) 
