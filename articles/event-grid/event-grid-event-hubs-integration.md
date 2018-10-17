---
title: Integração do Azure Event Grid e dos Hubs de Eventos
description: Descreve como utilizar o Azure Event Grid e os Hubs de Eventos para migrar dados para um SQL Data Warehouse
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: tutorial
ms.date: 08/22/2018
ms.author: tomfitz
ms.openlocfilehash: aad7a24d8b0e0bc74815cad3604db1cc21a6db96
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44163231"
---
# <a name="stream-big-data-into-a-data-warehouse"></a>Transmitir macrodados em fluxo para um armazém de dados

O Azure [Event Grid](overview.md) é um serviço de encaminhamento de eventos inteligente que lhe permite reagir a notificações de aplicações e serviços. Por exemplo, ele pode acionar uma Função do Azure para processar dados de Hubs de Eventos que foram capturados para um armazenamento de Blobs do Azure ou o Data Lake Store e migrar os dados para outros repositórios de dados. Este [exemplo de Recolha e Event Grid dos Hubs de Eventos](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) mostra como utilizar a Recolha dos Hubs de Eventos com o Event Grid para migrar dados de forma integrada de Hubs de Eventos a partir do armazenamento de blobs para um SQL Data Warehouse.

![Descrição geral da aplicação](media/event-grid-event-hubs-integration/overview.png)

Como os dados são enviados para o hub de eventos, a Recolha obtém dados a partir do fluxo e gera blobs de armazenamento com os dados no formato Avro. Quando a Recolha gera o blob, aciona um evento. O Event Grid distribui dados sobre o evento para subscritores. Neste caso, os dados do evento são enviados para o ponto final das Funções do Azure. Os dados do evento incluem o caminho do blob gerado. A função utiliza esse URL para obter o ficheiro e enviá-lo para o armazém de dados.

Neste artigo, irá:

* Implementar a seguinte infraestrutura:
  * Hub de eventos com a Recolha ativada
  * Conta de armazenamento para os ficheiros de Recolha
  * Plano do serviço de aplicações do Azure para alojar a aplicação de funções
  * Aplicação de funções para processar o evento
  * SQL Server para alojar o armazém de dados
  * SQL Data Warehouse para armazenar os dados migrados
* Criar uma tabela no armazém de dados
* Adicionar código para a aplicação de funções
* Subscrever o evento
* Executar a aplicação que envia dados para o hub de eventos
* Ver os dados migrados no armazém de dados

## <a name="about-the-event-data"></a>Sobre os dados do evento

O Event Grid distribui dados de eventos para os subscritores. O exemplo seguinte mostra os dados de eventos para criar um ficheiro de Recolha. Em particular, observe a propriedade `fileUrl` no objeto `data`. A aplicação de funções obtém este valor e utiliza-o para obter o ficheiro de Recolha.

```json
[
    {
        "topic": "/subscriptions/<guid>/resourcegroups/rgDataMigrationSample/providers/Microsoft.EventHub/namespaces/tfdatamigratens",
        "subject": "eventhubs/hubdatamigration",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-08-31T19:12:46.0498024Z",
        "id": "14e87d03-6fbf-4bb2-9a21-92bd1281f247",
        "data": {
            "fileUrl": "https://tf0831datamigrate.blob.core.windows.net/windturbinecapture/tfdatamigratens/hubdatamigration/1/2017/08/31/19/11/45.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 249168,
            "eventCount": 1500,
            "firstSequenceNumber": 2400,
            "lastSequenceNumber": 3899,
            "firstEnqueueTime": "2017-08-31T19:12:14.674Z",
            "lastEnqueueTime": "2017-08-31T19:12:44.309Z"
        }
    }
]
```

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, tem de ter:

* Uma subscrição do Azure. Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.
* [Visual studio 2017 Versão 15.3.2 ou superior](https://www.visualstudio.com/vs/) com cargas de trabalho para: desenvolvimento de ambiente de trabalho .NET, desenvolvimento do Azure, desenvolvimento de ASP.NET e web, desenvolvimento de Node.js e desenvolvimento de Python.
* O [projeto de exemplo EventHubsCaptureEventGridDemo](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) transferido para o seu computador.

## <a name="deploy-the-infrastructure"></a>Implementar a infraestrutura

Para simplificar este artigo, implemente a infraestrutura necessária com um modelo do Resource Manager. Para ver os recursos que estão implementados, veja o [modelo](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/EventHubsDataMigration.json).

Para a CLI do Azure, utilize:

```azurecli-interactive
az group create -l westcentralus -n rgDataMigrationSample

az group deployment create \
  --resource-group rgDataMigrationSample \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json \
  --parameters eventHubNamespaceName=<event-hub-namespace> eventHubName=hubdatamigration sqlServerName=<sql-server-name> sqlServerUserName=<user-name> sqlServerPassword=<password> sqlServerDatabaseName=<database-name> storageName=<unique-storage-name> functionAppName=<app-name>
```

Para o PowerShell, utilize:

```powershell
New-AzureRmResourceGroup -Name rgDataMigration -Location westcentralus

New-AzureRmResourceGroupDeployment -ResourceGroupName rgDataMigration -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/EventHubsDataMigration.json -eventHubNamespaceName <event-hub-namespace> -eventHubName hubdatamigration -sqlServerName <sql-server-name> -sqlServerUserName <user-name> -sqlServerDatabaseName <database-name> -storageName <unique-storage-name> -functionAppName <app-name>
```

Conceda um valor de palavra-passe quando pedido.

## <a name="create-a-table-in-sql-data-warehouse"></a>Criar uma tabela no SQL Data Warehouse

Adicionar uma tabela para o armazém de dados ao executar o script [CreateDataWarehouseTable.sql](https://github.com/Azure/azure-event-hubs/blob/master/samples/e2e/EventHubsCaptureEventGridDemo/scripts/CreateDataWarehouseTable.sql). Para executar o script, utilize o Visual Studio ou o Editor de Consultas no portal.

O script a executar é:

```sql
CREATE TABLE [dbo].[Fact_WindTurbineMetrics] (
    [DeviceId] nvarchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL, 
    [MeasureTime] datetime NULL, 
    [GeneratedPower] float NULL, 
    [WindSpeed] float NULL, 
    [TurbineSpeed] float NULL
)
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
```

## <a name="publish-the-azure-functions-app"></a>Publicar a aplicação de Funções do Azure

1. Abra o [projeto de exemplo EventHubsCaptureEventGridDemo](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo) no Visual Studio 2017 (15.3.2 ou superior).

1. No Explorador de Soluções, clique com o botão direito do rato em **FunctionEGDWDumper** e selecione **Publicar**.

   ![Publicar aplicação de funções](media/event-grid-event-hubs-integration/publish-function-app.png)

1. Selecione **Aplicação de Funções do Azure** e **Selecionar Existente**. Selecione **Publicar**.

   ![Aplicação de funções de destino](media/event-grid-event-hubs-integration/pick-target.png)

1. Selecione a aplicação de funções que implementou através do modelo. Selecione **OK**.

   ![Selecionar aplicação de funções](media/event-grid-event-hubs-integration/select-function-app.png)

1. Quando o Visual Studio tiver configurado o perfil, selecione **Publicar**.

   ![Selecione publicar](media/event-grid-event-hubs-integration/select-publish.png)

Depois de publicar a função, está pronto para subscrever o evento.

## <a name="subscribe-to-the-event"></a>Subscrever o evento

1. Aceda ao [Portal do Azure](https://portal.azure.com/). Selecione o grupo de recursos e a aplicação de funções.

   ![Ver aplicação de funções](media/event-grid-event-hubs-integration/view-function-app.png)

1. Selecione a função.

   ![Selecionar função](media/event-grid-event-hubs-integration/select-function.png)

1. Selecione **Adicionar subscrição do Event Grid**.

   ![Adicionar subscrição](media/event-grid-event-hubs-integration/add-event-grid-subscription.png)

9. Conceda um nome à subscrição da grelha de eventos. Utilize **Espaços de Nomes de Hubs de Eventos** como o tipo de evento. Indique os valores para selecionar a instância do espaço de nomes dos Hubs de Eventos. Deixe o ponto final do subscritor como o valor indicado. Selecione **Criar**.

   ![Criar subscrição](media/event-grid-event-hubs-integration/set-subscription-values.png)

## <a name="run-the-app-to-generate-data"></a>Executar a aplicação para gerar dados

Acabou de configurar o seu hub de eventos, o armazém de dados SQL, aplicação de funções do Azure e a subscrição do evento. A solução está pronta para migrar os dados do hub de eventos para o armazém de dados. Antes de executar uma aplicação que gera dados para o hub de eventos, tem de configurar alguns valores.

1. No portal, selecione o seu espaço de nomes do hub de eventos. Selecione **Cadeias de Ligação**.

   ![Selecione cadeias de ligação](media/event-grid-event-hubs-integration/event-hub-connection.png)

2. Selecione **RootManageSharedAccessKey**

   ![Selecionar chave](media/event-grid-event-hubs-integration/show-root-key.png)

3. Cópia **Cadeia de ligação - chave primária**

   ![Chave de cópia](media/event-grid-event-hubs-integration/copy-key.png)

4. Volte ao seu projeto do Visual Studio. No projeto WindTurbineDataGenerator, abra **program.cs**.

5. Substitua os dois valores constantes. Utilize o valor copiado para **EventHubConnectionString**. Utilize **hubdatamigration**, o nome do hub de eventos.

   ```cs
   private const string EventHubConnectionString = "Endpoint=sb://demomigrationnamespace.servicebus.windows.net/...";
   private const string EventHubName = "hubdatamigration";
   ```

6. Compilar a solução. Execute a aplicação WindTurbineGenerator.exe. Após alguns minutos, consulte a tabela no seu armazém de dados para os dados migrados.

## <a name="next-steps"></a>Passos seguintes

* Para saber mais sobre as diferenças dos serviços de mensagens do Azure, veja [Escolher entre os serviços do Azure que entregam mensagens](compare-messaging-services.md).
* Para obter uma introdução ao Event Grid, veja [Sobre o Azure Event Grid](overview.md).
* Para obter uma introdução à Recolha dos Hubs de Eventos, veja [Ativar Recolha dos Hubs de Eventos com o portal do Azure](../event-hubs/event-hubs-capture-enable-through-portal.md).
* Para obter mais informações sobre como configurar e executar o exemplo, veja [Exemplo do Event Grid e da Recolha dos Hubs de Eventos](https://github.com/Azure/azure-event-hubs/tree/master/samples/e2e/EventHubsCaptureEventGridDemo).