---
title: Criar uma tarefa do Stream Analytics com o Azure PowerShell | Microsoft Docs
description: Este início rápido disponibiliza detalhes através do módulo do Azure PowerShell para implementar e executar uma tarefa do Azure Stream Analytics.
services: stream-analytics
keywords: Stream analytics, tarefas na Cloud, Azure PowerShell, entrada da tarefa, saída da tarefa, transformação da tarefa
author: SnehaGunda
ms.author: sngun
ms.date: 03/16/2018
ms.topic: quickstart
ms.service: stream-analytics
ms.custom: mvc
manager: kfile
ms.openlocfilehash: 8a1036531ea0e7c1426224bc4d42c83e9049cabf
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="quickstart-create-a-stream-analytics-job-by-using-azure-powershell"></a>Início Rápido: Criar uma tarefa do Stream Analytics com o Azure PowerShell

O módulo do Azure PowerShell é utilizado para criar e gerir recursos do Azure com cmdlets ou scripts do PowerShell. Este início rápido disponibiliza detalhes através do módulo do Azure PowerShell para implementar e executar uma tarefa do Azure Stream Analytics.

## <a name="before-you-begin"></a>Antes de começar

* Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/)  

* Este início rápido requer a versão 3.6 ou posterior do módulo Azure PowerShell. Execute `Get-Module -ListAvailable AzureRM` para localizar a versão instalada no computador local. Se precisar de instalar ou atualizar, veja o artigo [Instalar o módulo do Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). O cenário neste artigo descreve a leitura de dados a partir do armazenamento de blobs, a transformação e a escrita dos dados num contentor diferente no mesmo armazenamento de blobs.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição do Azure com o comando `Connect-AzureRmAccount` e introduza as credenciais do Azure no pop-up do browser. Depois de iniciar sessão, se tiver várias subscrições, selecione a subscrição que quer utilizar para este início rápido ao executar os cmdlets seguintes. Certifique-se de que substitui <your subscription> pelo nome da sua subscrição:  

```powershell
# Log in to your Azure account
Connect-AzureRmAccount

# Select the Azure subscription you want to use to create the resource group.
Get-AzureRmSubscription `
  -SubscriptionName “<your subscription>” | Select-AzureRmSubscription
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos do Azure com [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos.

```powershell
$resourceGroup = "ASAPSRG"
$location = "WestUS2"
New-AzureRMResourceGroup `
  -Name $resourceGroup `
  -Location $location 
```

## <a name="prepare-the-input-data"></a>Preparar os dados de entrada

Antes de definir a tarefa do Stream Analytics, deve preparar os dados configurados como entrada da tarefa. Execute os seguintes passos para preparar os dados de entrada necessários para a tarefa: 

1. Transfira os [dados de exemplo do sensor](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) a partir do GitHub.  

2. Crie uma conta de armazenamento para fins gerais padrão com replicação LRS através do cmdlet [New-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/New-AzureRmStorageAccount)  

3. Obtenha o contexto da conta de armazenamento que define a conta de armazenamento a ser utilizada. Ao trabalhar com contas de armazenamento, referencia o contexto em vez de fornecer repetidamente as credenciais. Este exemplo cria uma conta de armazenamento denominada mystorageaccount com armazenamento localmente redundante (LRS) e encriptação de blobs (ativada por predefinição).  

4. Em seguida, crie um contentor com [New-AzureStorageContainer](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer), defina as permissões para "blob" para permitir o acesso público dos ficheiros e carregue os [dados de exemplo do sensor](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) que transferiu anteriormente. 

Estes passos são alcançados ao executar o seguinte script do PowerShell:

```powershell
$storageAccountName = "mystorageaccount"
$storageAccount = New-AzureRmStorageAccount `
  -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
$containerName = "myinputcontainer"

New-AzureStorageContainer `
  -Name $containerName `
  -Context $ctx `
  -Permission blob

Set-AzureStorageBlobContent `
  -File "C:\HelloWorldASA-InputStream.json" `
  -Container $containerName `
  -Context $ctx  

$storageAccountKey = (Get-AzureRmStorageAccountKey `
  -ResourceGroupName $resourceGroup `
  -Name $storageAccountName).Value[0]
```

## <a name="create-a-stream-analytics-job"></a>Criar uma tarefa do Stream Analytics

Crie uma tarefa do Stream Analytics com o cmdlet [New-AzureRMStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsjob?view=azurermps-5.4.0). Este cmdlet utiliza o nome da tarefa, o nome do grupo de recursos e a definição de tarefa como parâmetros. O nome da tarefa pode ser qualquer nome amigável que identifique a tarefa; o nome da tarefa do Stream Analytics apenas pode conter carateres alfanuméricos, hífenes e carateres de sublinhado e tem de ter entre 3 e 63 carateres. A definição de tarefa é um ficheiro JSON que contém as propriedades necessárias para criar uma tarefa. No computador local, crie um ficheiro denominado "JobDefinition.json" e adicione os seguintes dados JSON:

```json
{    
   "location":"Central US",  
   "properties":{    
      "sku":{    
         "name":"standard"  
      },  
      "eventsOutOfOrderPolicy":"drop",  
      "eventsOutOfOrderMaxDelayInSeconds":10,  
      "compatibilityLevel": 1.1
}
}
```

Em seguida, execute o cmdlet New-AzureRMStreamAnalyticsJob, certifique-se de que substitui o valor da variável jobDefinitionFile pelo caminho onde armazenou o ficheiro JSON da definição de tarefa. 

```powershell
$jobName = "MyStreamingJob"
$jobDefinitionFile = "C:\JobDefinition.json"
New-AzureRMStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  –File $jobDefinitionFile `
  –Name $jobName -Force 
```

## <a name="configure-input-to-the-job"></a>Configurar a entrada da tarefa

Adicione uma entrada à tarefa com o cmdlet [New-AzureRMStreamAnalyticsInput](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsinput?view=azurermps-5.4.0). Este cmdlet utiliza o nome da tarefa, o nome da entrada da tarefa, o nome do grupo de recursos e a definição de entrada da tarefa como parâmetros. A definição de entrada da tarefa é um ficheiro JSON que contém as propriedades necessárias para configurar a entrada da tarefa. Neste exemplo, irá criar um armazenamento de blobs como entrada. No computador local, crie um ficheiro denominado "JobInputDefinition.json", adicione os seguintes dados JSON e certifique-se de que substitui o valor **accountKey** pela chave de acesso da conta de armazenamento que é o valor armazenado no valor $storageAccountKey. 

```json
{
    "properties": {
        "type": "Stream",
        "datasource": {
            "type": "Microsoft.Storage/Blob",
            "properties": {
                "storageAccounts": [
                {
                   "accountName": "mystorageaccount",
                   "accountKey":"<Your storage account key>"
                }],
                "container": "myinputcontainer",
                "pathPattern": "",
                "dateFormat": "yyyy/MM/dd",
                "timeFormat": "HH"
            }
        },
        "serialization": {
            "type": "Json",
            "properties": {
                "encoding": "UTF8"
            }
        }
    },
    "name": "MyBlobInput",
    "type": "Microsoft.StreamAnalytics/streamingjobs/inputs"
}
```

Em seguida, execute o cmdlet New-AzureRMStreamAnalyticsInput, certifique-se de que substitui o valor da variável jobDefinitionFile pelo caminho onde armazenou o ficheiro JSON da definição de entrada da tarefa. 

```powershell
$jobInputName = "MyBlobInput"
$jobInputDefinitionFile = "C:\JobInputDefinition.json"
New-AzureRMStreamAnalyticsInput `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobInputDefinitionFile `
  -Name $jobInputName 
```

## <a name="configure-output-to-the-job"></a>Configurar a saída da tarefa

Adicione uma saída à tarefa com o cmdlet [New-AzureRmStreamAnalyticsOutput](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsoutput?view=azurermps-5.4.0). Este cmdlet utiliza o nome da tarefa, o nome da saída da tarefa, o nome do grupo de recursos e a definição de saída da tarefa como parâmetros. A definição de saída da tarefa é um ficheiro JSON que contém as propriedades necessárias para configurar a saída da tarefa. Neste exemplo, irá criar um armazenamento de blobs como saída. No computador local, crie um ficheiro denominado "JobOutputDefinition.json", adicione os seguintes dados JSON e certifique-se de que substitui o valor **accountKey** pela chave de acesso da conta de armazenamento que é o valor armazenado no valor $storageAccountKey. 

```json
{
    "properties": {
        "datasource": {
            "type": "Microsoft.Storage/Blob",
            "properties": {
                "storageAccounts": [
                    {
                      "accountName": "mystorageaccount",
                      "accountKey": "<Your storage account key>"
                    }],
                "container": "myoutputcontainer",
                "pathPattern": "",
                "dateFormat": "yyyy/MM/dd",
                "timeFormat": "HH"
            }
        },
        "serialization": {
            "type": "Json",
            "properties": {
                "encoding": "UTF8",
                "format": "LineSeparated"
            }
        }
    },
    "name": "MyBlobOutput",
    "type": "Microsoft.StreamAnalytics/streamingjobs/outputs"
}
```

Em seguida, execute o cmdlet New-AzureRMStreamAnalyticsOutput, certifique-se de que substitui o valor da variável jobOutputDefinitionFile pelo caminho onde armazenou o ficheiro JSON da definição de saída da tarefa. 

```powershell
$jobOutputName = "MyBlobOutput"
$jobOutputDefinitionFile = "C:\JobOutputDefinition.json"
New-AzureRMStreamAnalyticsOutput `
  -ResourceGroupName $resourceGroup `
  –JobName $jobName `
  –File $jobOutputDefinitionFile `
  –Name $jobOutputName -Force 
```

## <a name="define-the-transformation-query"></a>Definir a consulta de transformação

Adicione uma transformação à tarefa com o cmdlet [New-AzureRmStreamAnalyticsTransformation](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticstransformation?view=azurermps-5.4.0). Este cmdlet utiliza o nome da tarefa, o nome da transformação da tarefa, o nome do grupo de recursos e a definição de transformação da tarefa como parâmetros. No computador local, crie um ficheiro denominado "JobTransformationDefinition.json" e adicione os seguintes dados JSON. O ficheiro JSON contém um parâmetro de consulta que define a consulta de transformação:

```json
{     
   "name":"MyTransformation",  
   "type":"Microsoft.StreamAnalytics/streamingjobs/transformations",  
   "properties":{    
      "streamingUnits":1,  
      "script":null,  
      "query":" SELECT System.Timestamp AS OutputTime, dspl AS SensorName, Avg(temp) AS AvgTemperature INTO MyBlobOutput FROM MyBlobInput TIMESTAMP BY time GROUP BY TumblingWindow(second,30),dspl HAVING Avg(temp)>100"  
   }  
}
```

Em seguida, execute o cmdlet New-AzureRMStreamAnalyticsTransformation, certifique-se de que substitui o valor da variável jobTransformationDefinitionFile pelo caminho onde armazenou o ficheiro JSON da definição de transformação da tarefa. 

```powershell
$jobTransformationName = "MyJobTransformation"
$jobTransformationDefinitionFile = "C:\JobTransformationDefinition.json"
New-AzureRMStreamAnalyticsTransformation `
  -ResourceGroupName $resourceGroup `
  –JobName $jobName `
  –File $jobTransformationDefinitionFile `
  –Name $jobTransformationName -Force
```

## <a name="start-the-stream-analytics-job-and-check-the-output"></a>Iniciar a tarefa do Stream Analytics e verificar a saída

Inicie a tarefa com o cmdlet [Start-AzureRMStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/start-azurermstreamanalyticsjob?view=azurermps-5.4.0). Este cmdlet utiliza o nome da tarefa, o nome do grupo de recursos, o modo de início da saída e a hora de início como parâmetros. OUtpputStartMode aceita dois valores JobStartTime, CustomTime ou LastOutputEventTime. Para saber mais sobre o que cada um destes valores está a referenciar, veja a secção [Parâmetros](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/start-azurermstreamanalyticsjob?view=azurermps-5.4.0) na documentação do PowerShell. Neste exemplo, pode especificar o modo como CustomTime e fornecer um valor para OutputStartTime. 

Para o valor de tempo, selecione um dia antes do momento em que carregou o ficheiro para o armazenamento de blobs, uma vez que a hora de carregamento do ficheiro é anterior à hora atual. Depois de executar o cmdlet seguinte, é devolvido "True" como saída se a tarefa for iniciada. É criado um contentor com o nome "myoutputcontainer" na conta de armazenamento com os dados transformados. 

```powershell
Start-AzureRMStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode CustomTime `
  -OutputStartTime 2018-03-11T14:45:12Z 
```

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o grupo de recursos, a tarefa de transmissão em fluxo e todos os recursos relacionados. A eliminação da tarefa evita a faturação das unidades de transmissão em fluxo consumidas pela tarefa. Se estiver a planear utilizar a tarefa no futuro, pode pará-la e reiniciá-la mais tarde, quando for necessário. Se não quiser continuar a utilizar esta tarefa, elimine todos os recursos criados por este início rápido ao executar o seguinte cmdlet:

```powershell
Remove-AzureRmResourceGroup `
  -Name $resourceGroup 
```

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, implementou uma tarefa do Stream Analytics simples. Para saber mais sobre como configurar outras origens de entrada e efetuar a deteção em tempo real, avance para o seguinte artigo:

> [!div class="nextstepaction"]
> [Deteção de fraudes em tempo real com o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
