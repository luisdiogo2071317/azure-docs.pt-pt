---
title: Utilizar o PowerShell para criar e configurar uma área de trabalho do Log Analytics | Documentos da Microsoft
description: Log Analytics utiliza dados de servidores no seu local ou a infraestrutura na cloud. Pode recolher dados de máquina de armazenamento do Azure quando gerados pelo diagnóstico do Azure.
services: log-analytics
documentationcenter: ''
author: richrundmsft
manager: jochan
editor: ''
ms.assetid: 3b9b7ade-3374-4596-afb1-51b695f481c2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: powershell
ms.topic: conceptual
ms.date: 11/21/2016
ms.author: richrund
ms.openlocfilehash: b8b3b28d2bf7fc75b9f70d145290af1edf44c94f
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/07/2019
ms.locfileid: "54063195"
---
# <a name="manage-log-analytics-using-powershell"></a>Gerir o Log Analytics com o PowerShell
Pode utilizar o [cmdlets do PowerShell do Log Analytics](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/) para executar várias funções no Log Analytics numa linha de comandos ou como parte de um script.  Exemplos das tarefas que pode efetuar com o PowerShell:

* Criar uma área de trabalho
* Adicionar ou remover uma solução
* Importar e exportar pesquisas guardadas
* Criar um grupo de computadores
* Ativar a recolha de registos do IIS de computadores com o agente de Windows instalado
* Recolher contadores de desempenho de computadores com Linux e Windows
* Recolher eventos do syslog em computadores com Linux 
* Recolher eventos de registos de eventos do Windows
* Recolher registos de eventos personalizados
* Adicionar o log analytics agent para uma máquina virtual do Azure
* Configurar o log analytics para dados de índice recolhidos através dos diagnósticos do Azure

Este artigo fornece dois exemplos de código que mostram algumas das funções que pode efetuar a partir do PowerShell.  Pode consultar o [referência de cmdlets do PowerShell do Log Analytics](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/) para outras funções.

> [!NOTE]
> O log Analytics anteriormente chamado das informações operacionais, razão pela qual é o nome utilizado nos cmdlets.
> 
> 

## <a name="prerequisites"></a>Pré-requisitos
Esses exemplos funcionam com a versão 2.3.0 ou posterior do módulo azurerm. Operationalinsights.


## <a name="create-and-configure-a-log-analytics-workspace"></a>Criar e configurar uma área de trabalho do Log Analytics
O script de exemplo seguinte ilustra como:

1. Criar uma área de trabalho
2. Listar as soluções disponíveis
3. Adicionar soluções para a área de trabalho
4. Pesquisas guardada de importação
5. Pesquisas guardada de exportação
6. Criar um grupo de computadores
7. Ativar a recolha de registos do IIS de computadores com o agente de Windows instalado
8. Recolher contadores de desempenho disco lógico de computadores Linux (% de Inodes utilizados; Megabytes livres; % De espaço; utilizado Transferências/seg do disco; Leituras de disco/seg; Escritas de disco/seg)
9. Recolher eventos do syslog de computadores Linux
10. Recolher eventos de erro e aviso de Log de eventos de computadores Windows
11. Recolher contador de desempenho de memória utilizada em Mbytes disponíveis a partir de computadores Windows
12. Recolher um registo personalizado 

```

$ResourceGroup = "oms-example"
$WorkspaceName = "log-analytics-" + (Get-Random -Maximum 99999) # workspace names need to be unique - Get-Random helps with this for the example code
$Location = "westeurope"

# List of solutions to enable
$Solutions = "Security", "Updates", "SQLAssessment"

# Saved Searches to import
$ExportedSearches = @"
[
    {
        "Category":  "My Saved Searches",
        "DisplayName":  "WAD Events (All)",
        "Query":  "Type=Event SourceSystem:AzureStorage ",
        "Version":  1
    },
    {        
        "Category":  "My Saved Searches",
        "DisplayName":  "Current Disk Queue Length",
        "Query":  "Type=Perf ObjectName=LogicalDisk InstanceName=\"C:\" CounterName=\"Current Disk Queue Length\"",
        "Version":  1
    }
]
"@ | ConvertFrom-Json

# Custom Log to collect
$CustomLog = @"
{
    "customLogName": "sampleCustomLog1", 
    "description": "Example custom log datasource", 
    "inputs": [
        { 
            "location": { 
            "fileSystemLocations": { 
                "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ], 
                "linuxFileTypeLogPaths": [ "/var/logs" ] 
                } 
            }, 
        "recordDelimiter": { 
            "regexDelimiter": { 
                "pattern": "\\n", 
                "matchIndex": 0, 
                "matchIndexSpecified": true, 
                "numberedGroup": null 
                } 
            } 
        }
    ], 
    "extractions": [
        { 
            "extractionName": "TimeGenerated", 
            "extractionType": "DateTime", 
            "extractionProperties": { 
                "dateTimeExtraction": { 
                    "regex": null, 
                    "joinStringRegex": null 
                    } 
                } 
            }
        ] 
    }
"@

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

# Create the workspace
New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup

# List all solutions and their installation status
Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Add solutions
foreach ($solution in $Solutions) {
    Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true
}

# List enabled solutions
(Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Where({($_.enabled -eq $true)})

# Import Saved Searches
foreach ($search in $ExportedSearches) {
    $id = $search.Category + "|" + $search.DisplayName
    New-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId $id -DisplayName $search.DisplayName -Category $search.Category -Query $search.Query -Version $search.Version
}

# Export Saved Searches
(Get-AzureRmOperationalInsightsSavedSearch -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName).Value.Properties | ConvertTo-Json 

# Create Computer Group based on a query
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Web Servers" -DisplayName "Web Servers" -Category "My Saved Searches" -Query "Computer=""web*"" | distinct Computer" -Version 1

# Create a computer group based on names (up to 5000)
$computerGroup = """servername1.contoso.com"",""servername2.contoso.com"",""servername3.contoso.com"",""servername4.contoso.com"""
New-AzureRmOperationalInsightsComputerGroup -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -SavedSearchId "My Named Servers" -DisplayName "Named Servers" -Category "My Saved Searches" -Query $computerGroup -Version 1

# Enable IIS Log Collection using agent
Enable-AzureRmOperationalInsightsIISLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Perf
New-AzureRmOperationalInsightsLinuxPerformanceObjectDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Logical Disk" -InstanceName "*"  -CounterNames @("% Used Inodes", "Free Megabytes", "% Used Space", "Disk Transfers/sec", "Disk Reads/sec", "Disk Reads/sec", "Disk Writes/sec") -IntervalSeconds 20  -Name "Example Linux Disk Performance Counters"
Enable-AzureRmOperationalInsightsLinuxCustomLogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Linux Syslog
New-AzureRmOperationalInsightsLinuxSyslogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -Facility "kern" -CollectEmergency -CollectAlert -CollectCritical -CollectError -CollectWarning -Name "Example kernal syslog collection"
Enable-AzureRmOperationalInsightsLinuxSyslogCollection -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName

# Windows Event
New-AzureRmOperationalInsightsWindowsEventDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -EventLogName "Application" -CollectErrors -CollectWarnings -Name "Example Application Event Log"

# Windows Perf
New-AzureRmOperationalInsightsWindowsPerformanceCounterDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -ObjectName "Memory" -InstanceName "*" -CounterName "Available MBytes" -IntervalSeconds 20 -Name "Example Windows Performance Counter"

# Custom Logs
New-AzureRmOperationalInsightsCustomLogDataSource -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -CustomLogRawJson "$CustomLog" -Name "Example Custom Log Collection"

```
No exemplo acima regexDelimiter foi definida como "\\n" para a nova linha. O delimitador de registo também pode ser um carimbo.  Estes são os formatos suportados:

| Formato | Formato de JSON RegEx usa dois \\ para cada \ num padrão RegEx por isso, se o teste num aplicativo de RegEx reduzir \\ para \ |
| --- | --- |
| HH: MM: DE AAAA-MM-DD | ((\\\\1!d{2})\|(\\\\1!d{4}))-([0-1]\\\\d) (([0-3]\\\\d)\|(\\ \\1!d)) \\ \\s ((\\\\1!d)\|([0-1]\\\\1!d)\|(2[0-4])):[0-5][0-9]:[0-5][0-9] |
| M/AAAA HH: MM: SS AM/PM | (([0-1]\\\\1!d)\|[0-9]) / (([0-3]\\\\1!d)\|(\\\\1!d)) / ((\\\\1!d{2})\|( \\ \\1!d{4}))\\\\s ((\\\\1!d)\|([0-1]\\\\1!d)\|(2[0-4])): [0-5] [0-9]: [ 0-5] [0-9]\\\\s (AM\|PM\|estou\|pm) |
| MMM/dd/aaaa hh: mm: | ((([0-3]\\\\1!d)\|(\\\\1!d)) / (Jan\|Fevereiro\|Mar\|Maio\|Abr\|Jul\|Jun\|Ago\|Oct\|Sep\|Nov\|Dec\|jan\|Fevereiro\|mar\|pode\|Abr\|jul\|jun\|Ago\|oct\|sep\|nov\|dez) / ((\\\\1!d{2})\|(\\\\1!d{4})) \\ \\s ((\\\\1!d)\|([0-1]\\\\1!d)\|(2[0-4])):[0-5][0-9]:[0-5][0-9]) |
| MMM dd aaaa hh: mm: | (((?: Jan(?:uary)? \|Feb(?:ruary)? \|Mar(?:ch)? \|Apr(?:il)? \|Poderá\|Jun(?:e)?\| Jul(?:y)? \|Aug(?:ust)? \|Sep(?:tember)? \|Setembro\|Oct(?:o ero)? \|Nov(?:ember)? \|Dec(?:ember)?)). *? ((?: (?: [0 2]? \\ \\1!d{1})\|(?: [3] [01]{1}))) (?! [\\\\1!d]).* ? ((?: (?: [1]{1}\\\\1!d{1}\\\\1!d{1}\\\\1!d{1})\|(?: [2]{1} \\ \\1!d{3}))) (?! [\\\\1!d]). *? ((?: (?: [0-1][0-9])\|(?: [2][0-3])\|(?: [0-9])):(?:[0-5][0-9])(?::[0-5][0-9])? (?:\\\\s? (?: am\|AM\|pm\|PM))?)) |
| yyMMdd hh: mm: | ([0-9]{2}([0] [1 a 9]\|[1][0-2]) ([0-2] [0-9]\|[3][0-1])\\\\s\\\\s? ( [0-1]? [0-9] \|[2][0-3]):[0-5][0-9]:[0-5][0-9]) |
| ddMMyy hh: mm: | (([0-2] [0-9]\|[3][0-1]) ([0] [1 a 9]\|[1][0-2]) [0-9]{2}\\\\s\\\\s? ( [0-1]? [0-9] \|[2][0-3]):[0-5][0-9]:[0-5][0-9]) |
| MMM 1!d hh: mm: | (Jan\|Fevereiro\|Mar\|Abr\|pode\|Jun\|Jul\|Ago\|Sep\|Oct\|Nov\|dez)\\ \\s\\\\s? ( [0]? [1 a 9] \|[1 – 2] [0-9]\|[3][0-1])\\\\s ([0-1]? [ 0-9]\|[2][0-3]):([0-5][0-9]):([0-5][0-9]) |
| MMM 1!d hh: mm:<br> dois espaços após MMM | (Jan\|Fevereiro\|Mar\|Abr\|pode\|Jun\|Jul\|Ago\|Sep\|Oct\|Nov\|dez)\\ \\s\\\\s ([0]? [ 1 a 9]\|[1 – 2] [0-9]\|[3][0-1])\\\\s ([0] [0-9]\|[1][0-2]):([0-5][0-9]):([0-5][0-9]) |
| MMM 1!d hh: mm: | (Jan\|Fevereiro\|Mar\|Abr\|pode\|Jun\|Jul\|Ago\|Sep\|Oct\|Nov\|dez)\\ \\s ([0]? [ 1 a 9]\|[1 – 2] [0-9]\|[3][0-1])\\\\s ([0] [0-9]\|[1][0-2]):([0-5][0-9]):([0-5][0-9]) |
| dd/MMM/yyyy:HH:mm:ss + zzzz<br> onde + é + ou a -<br> onde zzzz tempo de deslocamento | (([0-2] [1 a 9]\|[3][0-1])\\\\/ (Jan\|Fevereiro\|Mar\|Abr\|Maio\|Jun\|Jul\|Ago\|Sep \|Oct\|Nov\|dez)\\\\/((19\|20) [0-9] [0-9]): ([0] [0-9]\|[1][0-2]):([0-5][0-9]):([0-5][0-9])\\ \\s [\\\\+\|\\\\-] [0-9]{4}) |
| aaaa-MM-ddTHH<br> O T é uma literal letra T | ((\\\\1!d{2})\|(\\\\1!d{4}))-([0-1]\\\\d) (([0-3]\\\\d)\|(\\ \\1!d)) T ((\\\\1!d)\|([0-1]\\\\1!d)\|(2[0-4])):[0-5][0-9]:[0-5][0-9] |

## <a name="configuring-log-analytics-to-index-azure-diagnostics"></a>Configurar o Log Analytics para indexar o diagnóstico do Azure
Para a monitorização sem agente de recursos do Azure, os recursos tem de ter o diagnóstico do Azure ativada e configurada para gravar numa área de trabalho do Log Analytics. Esta abordagem envia dados diretamente para o Log Analytics e não necessita de dados a ser escritos para uma conta de armazenamento. Os recursos suportados incluem:

| Tipo de Recurso | Registos | Métricas |
| --- | --- | --- |
| Gateways de Aplicação    | Sim | Sim |
| Contas de Automatização     | Sim | |
| Contas de Batch          | Sim | Sim |
| Data Lake analytics     | Sim | | 
| Arquivo do Data Lake         | Sim | |
| Conjunto elástico de SQL        |     | Sim |
| Espaço de nomes do hub de eventos     |     | Sim |
| Hubs IoT                |     | Sim |
| Cofre de Chaves               | Sim | |
| Balanceador de Carga          | Sim | |
| Aplicações Lógicas              | Sim | Sim |
| Grupos de Segurança de Rede | Sim | |
| Cache do Azure para Redis             |     | Sim |
| Procurar serviços         | Sim | Sim |
| Espaço de nomes do Service Bus   |     | Sim |
| SQL (v12)               |     | Sim |
| Web Sites               |     | Sim |
| Farms de servidores Web        |     | Sim |

Para obter os detalhes das métricas disponíveis, consulte [suportado métricas com o Azure Monitor](../../azure-monitor/platform/metrics-supported.md).

Para obter os detalhes dos registos disponíveis, consulte [suportada serviços e o esquema para os registos de diagnóstico](../../azure-monitor/platform/tutorial-dashboards.md).

```
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO" 

Set-AzureRmDiagnosticSetting -ResourceId $resourceId -WorkspaceId $workspaceId -Enabled $true
```

Também pode utilizar o cmdlet anterior para recolher registos de recursos que estão em subscrições diferentes. O cmdlet é capaz de trabalhar em várias subscrições, uma vez que está a fornecer o id do recurso a criar registos e os registos são enviados para a área de trabalho.


## <a name="configuring-log-analytics-to-index-azure-diagnostics-from-storage"></a>Configurar o Log Analytics para o diagnóstico do Azure do armazenamento de índice
Para recolher dados de registos de dentro de uma instância em execução de um serviço cloud clássico ou um cluster do service fabric, precisa primeiro escrever os dados ao armazenamento do Azure. Log Analytics, em seguida, é configurado para recolher os registos da conta de armazenamento. Os recursos suportados incluem:

* Serviços de cloud clássico (funções web e de trabalho)
* Clusters do Service fabric

A exemplo a seguir mostra como:

1. Listar as contas de armazenamento existentes e localizações do Log Analytics será indexar dados a partir de
2. Criar uma configuração para ler a partir de uma conta de armazenamento
3. Atualizar a configuração do recentemente criada para dados de índice a partir de localizações adicionais
4. Eliminar a configuração criada recentemente

```
# validTables = "WADWindowsEventLogsTable", "LinuxsyslogVer2v0", "WADServiceFabric*EventTable", "WADETWEventTable" 
$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq "your workspace name"})

# Update these two lines with the storage account resource ID and the storage account key for the storage account you want to Log Analytics to  
$storageId = "/subscriptions/ec11ca60-1234-491e-5678-0ea07feae25c/resourceGroups/demo/providers/Microsoft.Storage/storageAccounts/wadv2storage"
$key = "abcd=="

# List existing insights
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name

# Create a new insight
New-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -StorageAccountResourceId $storageId -StorageAccountKey $key -Tables @("WADWindowsEventLogsTable") -Containers @("wad-iis-logfiles")

# Update existing insight
Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" -Tables @("WADWindowsEventLogsTable", "WADETWEventTable") -Containers @("wad-iis-logfiles")

# Remove the insight
Remove-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -Name "newinsight" 

```

Também pode utilizar o script anterior para recolher registos de contas de armazenamento em subscrições diferentes. O script é capaz de trabalhar em várias subscrições, uma vez que está a fornecer o id de recurso de conta de armazenamento e uma chave de acesso correspondente. Quando alterar a chave de acesso, tem de atualizar a informação de armazenamento para que a nova chave.


## <a name="next-steps"></a>Passos Seguintes
* [Reveja o Log Analytics dos cmdlets PowerShell](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/) para obter mais informações sobre como utilizar o PowerShell para a configuração do Log Analytics.

