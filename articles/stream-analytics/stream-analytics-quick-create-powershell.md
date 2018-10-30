---
title: Criar um trabalho do Stream Analytics com o Azure PowerShell
description: Este início rápido disponibiliza detalhes através do módulo do Azure PowerShell para implementar e executar uma tarefa do Azure Stream Analytics.
services: stream-analytics
author: sidramadoss
ms.author: sidram
ms.date: 05/14/2018
ms.topic: quickstart
ms.service: stream-analytics
ms.custom: mvc
manager: kfile
ms.openlocfilehash: 126677df01ad34d488863dd83e2f8c9a2d947824
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49958895"
---
# <a name="quickstart-create-a-stream-analytics-job-by-using-azure-powershell"></a>Início Rápido: Criar uma tarefa do Stream Analytics com o Azure PowerShell

O módulo do Azure PowerShell é utilizado para criar e gerir recursos do Azure com cmdlets ou scripts do PowerShell. Este início rápido disponibiliza detalhes através do módulo do Azure PowerShell para implementar e executar uma tarefa do Azure Stream Analytics. 
 
O trabalho de exemplo lê dados de transmissão em fluxo num blob no armazenamento de blobs do Azure. O ficheiro de dados de entrada utilizado neste início rápido contém dados estáticos apenas para efeitos ilustrativos. Num cenário do mundo real, utiliza os dados de entrada de transmissão em fluxo para uma tarefa do Stream Analytics. Em seguida, utiliza a linguagem de consulta do Stream Analytics para transformar os dados, de modo a calcular a temperatura média quando for superior a 100°. Por fim, escreve os eventos de saída resultantes noutro ficheiro. 

## <a name="before-you-begin"></a>Antes de começar

* Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/)  

* Este início rápido requer a versão 3.6 ou posterior do módulo Azure PowerShell. Execute `Get-Module -ListAvailable AzureRM` para localizar a versão instalada no computador local. Se precisar de instalar ou atualizar, veja o artigo [Instalar o módulo do Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). 


## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição do Azure com o comando `Connect-AzureRmAccount` e introduza as credenciais do Azure no pop-up do browser:

```powershell
# Log in to your Azure account
Connect-AzureRmAccount
```

Depois de iniciar sessão, se tiver várias subscrições, selecione a subscrição que quer utilizar para este início rápido ao executar os cmdlets seguintes. Certifique-se de que substitui <your subscription name> pelo nome da sua subscrição:  

```powershell
# List all available subscriptions.
Get-AzureRmSubscription

# Select the Azure subscription you want to use to create the resource group and resources.
Get-AzureRmSubscription -SubscriptionName "<your subscription name>" | Select-AzureRmSubscription
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos do Azure com [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos.

```powershell
$resourceGroup = "StreamAnalyticsRG"
$location = "WestUS2"
New-AzureRmResourceGroup `
   -Name $resourceGroup `
   -Location $location 
```

## <a name="prepare-the-input-data"></a>Preparar os dados de entrada

Antes de definir o trabalho do Stream Analytics, prepare os dados configurados como entrada do mesmo.

1. Transfira os [dados de exemplo do sensor](https://raw.githubusercontent.com/Azure/azure-stream-analytics/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) a partir do GitHub. Clique com o botão direito do rato na ligação e **Save Link As...** (Guardar Ligação Como...) ou **Save target as** (Guardar destino como).

2. O bloco de código do PowerShell seguinte realiza vários comandos para preparar os dados de entrada de que o trabalho precisa. Reveja as secções para compreender o código. 

   1. Crie uma conta de armazenamento para fins gerais padrão com o cmdlet [New-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/New-AzureRmStorageAccount)  Este exemplo cria uma conta de armazenamento denominada mystorageaccount com armazenamento localmente redundante (LRS) e encriptação de blobs (ativada por predefinição).  

   2. Obtenha o contexto da conta de armazenamento `$storageAccount.Context` que define a conta de armazenamento a ser utilizada. Ao trabalhar com contas de armazenamento, referencia o contexto em vez de fornecer repetidamente as credenciais. 

   3. Utilize [New-AzureStorageContainer](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer) para criar um contentor de armazenamento e carregue os [dados de exemplo do sensor](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) que transferiu anteriormente. 

   4. Copie a chave de armazenamento que resulta do código e cole-a nos ficheiros JSON para criar as entradas e saídas do trabalho de transmissão em fluxo mais tarde.

   ```powershell
   $storageAccountName = "mystorageaccount"
   $storageAccount = New-AzureRmStorageAccount `
     -ResourceGroupName $resourceGroup `
     -Name $storageAccountName `
     -Location $location `
     -SkuName Standard_LRS `
     -Kind Storage
   
   $ctx = $storageAccount.Context
   $containerName = "streamanalytics"
   
   New-AzureStorageContainer `
     -Name $containerName `
     -Context $ctx
   
   Set-AzureStorageBlobContent `
     -File "c:\HelloWorldASA-InputStream.json" `
     -Blob "input/HelloWorldASA-InputStream.json" `
     -Container $containerName `
     -Context $ctx  
   
   $storageAccountKey = (Get-AzureRmStorageAccountKey `
     -ResourceGroupName $resourceGroup `
     -Name $storageAccountName).Value[0]
   
   Write-Host "The <storage account key> placeholder needs to be replaced in your input and output json files with this key value:" 
   Write-Host $storageAccountKey -ForegroundColor Cyan
   ```

## <a name="create-a-stream-analytics-job"></a>Criar uma tarefa do Stream Analytics

Crie um trabalho do Stream Analytics com o cmdlet [New-AzureRmStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsjob?view=azurermps-5.4.0). Este cmdlet utiliza o nome da tarefa, o nome do grupo de recursos e a definição de tarefa como parâmetros. O nome do trabalho pode ser qualquer nome amigável que o identifique. Pode conter carateres alfanuméricos, hífenes e carateres de sublinhado e tem de ter entre 3 a 63 carateres. A definição de trabalho é um ficheiro JSON que contém as propriedades necessárias para criar um trabalho. No computador local, crie um ficheiro denominado `JobDefinition.json` e adicione os seguintes dados JSON:

```json
{    
   "location":"WestUS2",  
   "properties":{    
      "sku":{    
         "name":"standard"  
      },  
      "eventsOutOfOrderPolicy":"adjust",  
      "eventsOutOfOrderMaxDelayInSeconds":10,  
      "compatibilityLevel": 1.1
   }
}
```

Em seguida, execute o cmdlet `New-AzureRmStreamAnalyticsJob`. Confirme que substitui o valor da variável `jobDefinitionFile` pelo caminho onde armazenou o ficheiro JSON de definição do trabalho. 

```powershell
$jobName = "MyStreamingJob"
$jobDefinitionFile = "C:\JobDefinition.json"
New-AzureRmStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -File $jobDefinitionFile `
  -Name $jobName `
  -Force 
```

## <a name="configure-input-to-the-job"></a>Configurar a entrada da tarefa

Utilize o cmdlet [New-AzureRmStreamAnalyticsInput](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsinput?view=azurermps-5.4.0) para adicionar uma entrada ao trabalho. Este cmdlet utiliza o nome da tarefa, o nome da entrada da tarefa, o nome do grupo de recursos e a definição de entrada da tarefa como parâmetros. A definição da entrada do trabalho é um ficheiro JSON que contém as propriedades necessárias para configurar essa entrada. Neste exemplo, vai criar um armazenamento de blobs como entrada. 

No computador local, crie um ficheiro denominado `JobInputDefinition.json` e adicione os seguintes dados JSON ao mesmo. Certifique-se de que substitui o valor de `accountKey` pela chave de acesso da conta de armazenamento, que é o valor armazenado em $storageAccountKey. 

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
                   "accountKey":"<storage account key>"
                }],
                "container": "streamanalytics",
                "pathPattern": "input/",
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

Em seguida, execute o cmdlet `New-AzureRmStreamAnalyticsInput`, confirme que substitui o valor da variável `jobDefinitionFile` pelo caminho onde armazenou o ficheiro JSON da definição da entrada do trabalho. 

```powershell
$jobInputName = "MyBlobInput"
$jobInputDefinitionFile = "C:\JobInputDefinition.json"
New-AzureRmStreamAnalyticsInput `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobInputDefinitionFile `
  -Name $jobInputName 
```

## <a name="configure-output-to-the-job"></a>Configurar a saída da tarefa

Adicione uma saída à tarefa com o cmdlet [New-AzureRmStreamAnalyticsOutput](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsoutput?view=azurermps-5.4.0). Este cmdlet utiliza o nome da tarefa, o nome da saída da tarefa, o nome do grupo de recursos e a definição de saída da tarefa como parâmetros. A definição da saída do trabalho é um ficheiro JSON que contém as propriedades necessárias para configurar essa saída. Este exemplo utiliza o armazenamento de blobs como saída. 

No computador local, crie um ficheiro denominado `JobOutputDefinition.json` e adicione os seguintes dados JSON ao mesmo. Certifique-se de que substitui o valor de `accountKey` pela chave de acesso da conta de armazenamento, que é o valor armazenado em $storageAccountKey. 

```json
{
    "properties": {
        "datasource": {
            "type": "Microsoft.Storage/Blob",
            "properties": {
                "storageAccounts": [
                    {
                      "accountName": "mystorageaccount",
                      "accountKey": "<storage account key>"
                    }],
                "container": "streamanalytics",
                "pathPattern": "output/",
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

Em seguida, execute o cmdlet `New-AzureRmStreamAnalyticsOutput`. Confirme que substitui o valor da variável `jobOutputDefinitionFile` pelo caminho em que armazenou o ficheiro JSON da definição da saída do trabalho. 

```powershell
$jobOutputName = "MyBlobOutput"
$jobOutputDefinitionFile = "C:\JobOutputDefinition.json"
New-AzureRmStreamAnalyticsOutput `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobOutputDefinitionFile `
  -Name $jobOutputName -Force 
```

## <a name="define-the-transformation-query"></a>Definir a consulta de transformação

Adicione uma transformação à tarefa com o cmdlet [New-AzureRmStreamAnalyticsTransformation](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticstransformation?view=azurermps-5.4.0). Este cmdlet utiliza o nome da tarefa, o nome da transformação da tarefa, o nome do grupo de recursos e a definição de transformação da tarefa como parâmetros. No computador local, crie um ficheiro denominado `JobTransformationDefinition.json` e adicione os seguintes dados JSON ao mesmo. O ficheiro JSON contém um parâmetro de consulta que define a consulta de transformação:

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

Execute novamente o cmdlet `New-AzureRmStreamAnalyticsTransformation`. Certifique-se de que substitui o valor da variável `jobTransformationDefinitionFile` pelo caminho onde armazenou o ficheiro JSON da definição da transformação do trabalho. 

```powershell
$jobTransformationName = "MyJobTransformation"
$jobTransformationDefinitionFile = "C:\JobTransformationDefinition.json"
New-AzureRmStreamAnalyticsTransformation `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobTransformationDefinitionFile `
  -Name $jobTransformationName -Force
```

## <a name="start-the-stream-analytics-job-and-check-the-output"></a>Iniciar a tarefa do Stream Analytics e verificar a saída

Utilize o cmdlet [Start-AzureRmStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/start-azurermstreamanalyticsjob?view=azurermps-5.4.0). Este cmdlet utiliza o nome da tarefa, o nome do grupo de recursos, o modo de início da saída e a hora de início como parâmetros. `OutputStartMode` aceita os valores `JobStartTime` `CustomTime` ou `LastOutputEventTime`. Para obter mais informações sobre ao que cada um destes valores se refere, veja a secção [parameters](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/start-azurermstreamanalyticsjob?view=azurermps-5.4.0) (parâmetros) na documentação do PowerShell. Neste exemplo, especifique o modo como `CustomTime` e indique um valor para `OutputStartTime`. 

Para o valor de tempo, selecione `2018-01-01`. Esta data de início é escolhida porque precede o carimbo de data/hora do evento dos dados de exemplo. Depois de executar o cmdlet seguinte, devolve `True` como a saída, se o trabalho for iniciado. No contentor de armazenamento, é criada uma pasta de saída com os dados transformados. 

```powershell
Start-AzureRmStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode CustomTime `
  -OutputStartTime 2018-01-01T00:00:00Z 
```

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o grupo de recursos, a tarefa de transmissão em fluxo e todos os recursos relacionados. A eliminação da tarefa evita a faturação das unidades de transmissão em fluxo consumidas pela tarefa. Se estiver a planear utilizar o trabalho no futuro, pode ignorar a eliminação e parar o trabalho por agora. Se não quiser continuar a utilizar esta tarefa, elimine todos os recursos criados por este início rápido ao executar o seguinte cmdlet:

```powershell
Remove-AzureRmResourceGroup `
  -Name $resourceGroup 
```

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, implementou uma tarefa simples do Stream Analytics com o PowerShell. Também pode implementar tarefas do Stream Analytics com o [portal do Azure](stream-analytics-quick-create-portal.md) e o [Visual Studio](stream-analytics-quick-create-vs.md).

Para saber mais sobre como configurar outras origens de entrada e efetuar a deteção em tempo real, avance para o seguinte artigo:

> [!div class="nextstepaction"]
> [Deteção de fraudes em tempo real com o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
