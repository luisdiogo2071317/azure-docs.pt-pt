---
title: Registo de diagnóstico Cosmos BD do Azure | Microsoft Docs
description: Utilize este tutorial para ajudá-lo com a base de dados do Azure Cosmos registo.
services: cosmos-db
documentationcenter: ''
author: mimig1
manager: jhubbard
tags: azure-resource-manager
ms.assetid: ''
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: mimig
ms.openlocfilehash: b1921820b5a1d94c6f5d6413204ee7814cc25c74
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="azure-cosmos-db-diagnostic-logging"></a>Registo de diagnóstico Cosmos BD do Azure

Depois de começar a utilizar um ou mais bases de dados de BD do Cosmos do Azure, pode querer monitorizar como e quando as bases de dados são acedidos. Este artigo fornece uma descrição geral dos registos que estão disponíveis na plataforma do Azure. Saiba como ativar o registo de diagnóstico para a monitorização fins para enviar registos ao [Storage do Azure](https://azure.microsoft.com/services/storage/), como transmitir os registos para [Event Hubs do Azure](https://azure.microsoft.com/services/event-hubs/)e como exportar os registos para [Log Analytics do Azure ](https://azure.microsoft.com/services/log-analytics/), que faz parte de [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite).

## <a name="logs-available-in-azure"></a>Registos disponíveis no Azure

Antes de iremos falar sobre como monitorizar a sua conta de base de dados do Azure Cosmos, vamos clarificar alguns aspetos sobre o registo e monitorização. Existem diferentes tipos de registos na plataforma do Azure. Existem [registos de atividade do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), [registos de diagnóstico do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs), [métricas do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics), eventos, monitorização heartbeat, registos de operações e assim sucessivamente. Não há um plethora de registos. Pode ver a lista completa de registos no [Log Analytics do Azure](https://azure.microsoft.com/en-us/services/log-analytics/) no portal do Azure. 

A imagem seguinte mostra os diferentes tipos de registos do Azure que estão disponíveis:

![Diferentes tipos de registos do Azure](./media/logging/azurelogging.png)

Na imagem, o **recursos de computação** representam os recursos do Azure para o qual pode aceder a Microsoft do sistema operativo convidado. Por exemplo, máquinas virtuais do Azure, máquina virtual Dimensionar conjuntos, o serviço de contentor do Azure, e assim sucessivamente, são recursos de computação considerados. Recursos geram registos de atividade, os registos de diagnóstico e os registos de aplicações de computação. Para obter mais informações, consulte o [monitorização do Azure: recursos de computação](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#azure-monitor-sources---compute-subset) artigo.

O **recursos de computação não** são recursos que não é possível aceder ao sistema operativo subjacente e trabalhar diretamente com o recurso. Por exemplo, grupos de segurança de rede, as Logic Apps e assim sucessivamente. BD do Cosmos do Azure é um recurso de computação não. Pode ver os registos de recursos de computação não no registo de atividade ou ative a opção de registos de diagnóstico no portal. Para obter mais informações, consulte o [monitorização do Azure: não computação recursos](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#azure-monitor-sources---everything-else) artigo.

O registo de atividade regista as operações de um nível de subscrição para a base de dados do Azure Cosmos. Operações como ListKeys, escrever DatabaseAccounts e muito mais são registadas. Os registos de diagnóstico fornecem registo mais granular e permitem-lhe iniciar sessão DataPlaneRequests (criar, ler, consulta e assim sucessivamente) e MongoRequests.


Neste artigo, iremos focar-se no registo de atividade do Azure, os registos de diagnóstico do Azure e as métricas do Azure. O que é a diferença entre estes três registos? 

### <a name="azure-activity-log"></a>Registo de atividade do Azure

O registo de atividade do Azure é um registo de subscrição que fornece informações sobre os eventos de nível de subscrição ocorridos no Azure. O registo de atividade relatórios plane de controlo de eventos para as suas subscrições sob a categoria administrativa. Pode utilizar o registo de atividade para determinar o "o que, que, quando" para qualquer operação (PUT, POST, DELETE) nos recursos de escrita na sua subscrição. Também pode compreender o estado da operação e outras propriedades relevantes. 

O registo de atividade difere dos registos de diagnóstico. O registo de atividade fornecem dados sobre as operações num recurso do exterior (o _plane controlo_). O contexto de base de dados do Azure Cosmos plane controlo operações incluem criar coleção, chaves de lista, eliminar chaves, base de dados de lista e assim sucessivamente. São emitidos por um recurso e fornecer informações sobre o funcionamento do recurso de registos de diagnóstico (o _plane dados_). Alguns exemplos das operações de plane dados no registo de diagnóstico são Delete, Insert e ReadFeed.

Registos de atividade (controlo plane operações) podem ser mais rico natureza e podem incluir o endereço de e-mail completo do chamador, endereço IP do emissor, nome do recurso, nome da operação, TenantId e muito mais. O registo de atividade contém várias [categorias](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-activity-log-schema) de dados. Para obter detalhes completos na esquemas destas categorias, consulte [esquema de eventos de registo de atividade do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-activity-log-schema). No entanto, os registos de diagnóstico pode ser restritivos natureza como dados PII, muitas vezes, são eliminados destes registos. Poderá ter o endereço IP do chamador, mas o último octant é removido.

### <a name="azure-metrics"></a>Métricas do Azure

[As métricas do Azure](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-metrics) têm o tipo mais importante de dados de telemetria do Azure (também denominado _contadores de desempenho_) que é emitido pelo Azure mais recursos. Métricas permitem-lhe ver informações sobre débito, armazenamento, consistência, disponibilidade e a latência dos seus recursos Azure Cosmos DB. Para obter mais informações, consulte [monitorização e a depuração com métricas do BD Azure Cosmos](use-metrics.md).

### <a name="azure-diagnostic-logs"></a>Registos de diagnóstico do Azure

Registos de diagnóstico do Azure são emitidos por um recurso e fornecer dados avançados, frequentes sobre o funcionamento desse recurso. O conteúdo estes registos varia consoante o tipo de recurso. Os registos de diagnóstico de nível de recursos também diferem dos registos de diagnóstico de nível de SO convidado. Os registos de diagnóstico de SO convidado são recolhidos por um agente que está a ser executado no interior de uma máquina virtual ou outro suportado tipo de recurso. Os registos de diagnóstico ao nível do recurso necessitam sem dados de recursos específicos de agente e captura da própria plataforma do Azure. Registos de diagnóstico de nível de SO convidado capturam os dados do sistema operativo e aplicações em execução numa máquina virtual.

![Registo de diagnóstico para o armazenamento, os Event Hubs ou através de análise de registos do Operations Management Suite](./media/logging/azure-cosmos-db-logging-overview.png)

### <a name="what-is-logged-by-azure-diagnostic-logs"></a>O que é registado por registos de diagnóstico do Azure?

* Todos os pedidos de back-end autenticado (TCP/REST) em todas as APIs são registados, incluindo pedidos falhados resultantes de permissões de acesso, erros de sistema ou pedidos incorretos. Suporte para pedidos de gráfico, Cassandra e API de tabela iniciada pelo utilizador não estão atualmente disponíveis.
* Operações na base de dados autónomo, que incluem operações CRUD em todos os documentos, contentores e bases de dados.
* Operações sobre chaves de conta, que incluem a criar, modificar ou eliminar as chaves.
* Pedidos não autenticados que resultam numa resposta 401. Por exemplo, pedidos que não tem um token de portador ou incorretamente formulados ou expirados ou com um token inválido.

<a id="#turn-on"></a>
## <a name="turn-on-logging-in-the-azure-portal"></a>Ative o registo no portal do Azure

Para ativar o registo de diagnóstico, tem de ter os seguintes recursos:

* Um existente do Azure Cosmos DB conta, base de dados e contentor. Para obter instruções sobre como criar estes recursos, consulte [criar uma conta de base de dados utilizando o portal do Azure](create-sql-api-dotnet.md#create-a-database-account), [amostras da CLI do Azure](cli-samples.md), ou [exemplos do PowerShell](powershell-samples.md).

Para ativar o registo de diagnóstico no portal do Azure, efetue os seguintes passos:

1. No [portal do Azure](https://portal.azure.com), no seu Azure Cosmos DB conta, selecione **registos de diagnóstico** na navegação à esquerda e, em seguida, selecione **ative os diagnósticos**.

    ![Ativar o registo de diagnóstico para a BD do Cosmos do Azure no portal do Azure](./media/logging/turn-on-portal-logging.png)

2. No **definições de diagnóstico** página, efetue os seguintes passos: 

    * **Nome**: introduza um nome para os registos para o criar.

    * **Arquivo para uma conta de armazenamento**: para utilizar esta opção, terá de uma conta de armazenamento existente para ligar a. Para criar uma nova conta do storage no portal, consulte [criar uma conta de armazenamento](../storage/common/storage-create-storage-account.md) e siga as instruções para criar um Azure Resource Manager, a conta para fins gerais. Em seguida, volte a esta página no portal para selecionar a sua conta do storage. Poderá demorar alguns minutos para as contas de armazenamento recentemente criada aparece no menu pendente.
    * **Fluxo para um hub de eventos**: para utilizar esta opção, terá de um existente dos Event Hubs espaço de nomes e eventos hub para ligar a. Para criar um espaço de nomes de Event Hubs, consulte [criar um espaço de nomes de Event Hubs e um hub de eventos utilizando o portal do Azure](../event-hubs/event-hubs-create.md). Em seguida, regresse a esta página no portal para selecionar o nome de espaço de nomes e a política de Event Hubs.
    * **Enviar ao Log Analytics**: para utilizar esta opção, utilize uma área de trabalho existente ou crie uma nova área de trabalho de análise de registos, seguindo os passos para [criar uma nova área de trabalho](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace) no portal. Para obter mais informações sobre como ver os seus registos na análise de registos, consulte [ver os registos na análise de registos](#view-in-loganalytics).
    * **Inicie sessão DataPlaneRequests**: selecione esta opção para registar pedidos de back-end da plataforma de distribuídas de base de dados do Azure Cosmos subjacente para contas SQL, gráfico, MongoDB, Cassandra e API de tabela. Se estiver a arquivar a uma conta de armazenamento, pode selecionar o período de retenção para os registos de diagnóstico. Os registos são eliminados automaticamente após o período de retenção expira.
    * **Inicie sessão MongoRequests**: selecione esta opção para registar pedidos iniciada pelo utilizador da base de dados do Azure Cosmos front-end para servir de contas de API do MongoDB. Se estiver a arquivar a uma conta de armazenamento, pode selecionar o período de retenção para os registos de diagnóstico. Os registos são eliminados automaticamente após o período de retenção expira.
    * **Pedidos de métricos**: selecione esta opção para armazenar dados verbosos no [métricas do Azure](../monitoring-and-diagnostics/monitoring-supported-metrics.md). Se estiver a arquivar a uma conta de armazenamento, pode selecionar o período de retenção para os registos de diagnóstico. Os registos são eliminados automaticamente após o período de retenção expira.

3. Selecione **Guardar**.

    Se receber um erro que indica "Falha ao atualizar os diagnósticos para \<nome da área de trabalho >. A subscrição \<id de subscrição > não está registada para utilizar insights, "siga a [resolver problemas do diagnóstico do Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) instruções para registar a conta e, em seguida, repita este procedimento.

    Se pretender alterar a forma como em que os registos de diagnóstico são guardados no futuro em qualquer momento, volte a esta página para modificar as definições de registo de diagnóstico para a sua conta.

## <a name="turn-on-logging-by-using-azure-cli"></a>Ativar o registo, utilizando a CLI do Azure

Para ativar o registo de diagnóstico e de métricas ao utilizar a CLI do Azure, utilize os seguintes comandos:

- Para ativar o armazenamento dos registos de diagnóstico numa conta do storage, utilize este comando:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   O `resourceId` é o nome da conta de base de dados do Azure Cosmos. O `storageId` é o nome da conta do storage para o qual pretende enviar os registos.

- Para ativar a transmissão em fluxo de registos de diagnóstico para um hub de eventos, utilize este comando:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   O `resourceId` é o nome da conta de base de dados do Azure Cosmos. O `serviceBusRuleId` é uma cadeia com este formato:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Para ativar a enviar registos de diagnóstico para uma área de trabalho de análise de registos, utilize este comando:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

Pode combinar estes parâmetros para ativar várias opções de saída.

## <a name="turn-on-logging-by-using-powershell"></a>Ativar o registo com o PowerShell

Para ativar o registo de diagnóstico com o PowerShell, precisa do Azure Powershell com uma versão mínima 1.0.1.

Para instalar o Azure PowerShell e associá-lo à sua subscrição do Azure, consulte o artigo [Como instalar e configurar o Azure PowerShell](/powershell/azure/overview).

Se já tiver instalado o Azure PowerShell e não souber a versão, a partir do tipo de consola do PowerShell `(Get-Module azure -ListAvailable).Version`.  

### <a id="connect"></a>Ligar às suas subscrições
Abra uma sessão no Azure PowerShell e inicie sessão na sua conta do Azure com o seguinte comando:  

```powershell
Login-AzureRmAccount
```

Na janela pop-up do browser, introduza o seu nome de utilizador da conta do Azure e a palavra-passe. O Azure PowerShell obtém todas as subscrições que estão associados com esta conta e, por predefinição, utiliza o primeiro.

Se tiver mais do que uma subscrição, poderá ter de especificar a subscrição específica que foi utilizada para criar o seu Cofre de chaves do Azure. Para ver as subscrições da sua conta, escreva o seguinte comando:

```powershell
Get-AzureRmSubscription
```

Em seguida, para especificar a subscrição que está associada a conta de base de dados do Azure Cosmos que está a registar, escreva o seguinte comando:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Se tiver mais do que uma subscrição que está associado à sua conta, é importante especificar a subscrição que pretende utilizar.
>
>

Para obter mais informações sobre como configurar o Azure PowerShell, consulte [como instalar e configurar o Azure PowerShell](/powershell/azure/overview).

### <a id="storage"></a>Criar uma nova conta de armazenamento para os seus registos
Apesar de poder utilizar uma conta de armazenamento existente para os seus registos, neste tutorial, iremos criar uma nova conta de armazenamento que está dedicada a base de dados do Azure Cosmos registos. Para sua comodidade, armazenamos os detalhes da conta de armazenamento numa variável designada **sa**.

Para facilitar ainda mais a gestão, neste tutorial, utilizamos o mesmo grupo de recursos que contém a base de dados do Azure Cosmos DB. Substitua os valores para o **ContosoResourceGroup**, **contosocosmosdblogs**, e **Norte Central nos** parâmetros, conforme aplicável:

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup `
-Name contosocosmosdblogs -Type Standard_LRS -Location 'North Central US'
```

> [!NOTE]
> Se optar por utilizar uma conta de armazenamento existente, a conta tem de utilizar a mesma subscrição como a sua subscrição do Azure Cosmos DB. A conta também tem de utilizar o modelo de implementação Resource Manager, em vez de modelo de implementação clássica.
>
>

### <a id="identify"></a>Identificar a conta de base de dados do Azure Cosmos para os seus registos
Defina o nome da conta de base de dados do Azure Cosmos como uma variável com o nome **conta**, onde **ResourceName** é o nome da conta de base de dados do Azure Cosmos.

```powershell
$account = Get-AzureRmResource -ResourceGroupName ContosoResourceGroup `
-ResourceName contosocosmosdb -ResourceType "Microsoft.DocumentDb/databaseAccounts"
```

### <a id="enable"></a>Ativar registo
Para ativar o registo de base de dados do Azure Cosmos, utilize o `Set-AzureRmDiagnosticSetting` cmdlet com variáveis para a nova conta de armazenamento, a conta de base de dados do Azure Cosmos e a categoria para ativar o registo. Execute o seguinte comando e defina o **-ativado** sinalizador para **$true**:

```powershell
Set-AzureRmDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests
```

O resultado para o comando deve assemelhar-se o exemplo seguinte:

```powershell
    StorageAccountId            : /subscriptions/<subscription-ID>/resourceGroups/ContosoResourceGroup/providers`
    /Microsoft.Storage/storageAccounts/contosocosmosdblogs
    ServiceBusRuleId            :
    EventHubAuthorizationRuleId :
    Metrics
        TimeGrain       : PT1M
        Enabled         : False
        RetentionPolicy
        Enabled : False
        Days    : 0
    
    Logs
        Category        : DataPlaneRequests
        Enabled         : True
        RetentionPolicy
        Enabled : False
        Days    : 0
    
    WorkspaceId                 :
    Id                          : /subscriptions/<subscription-ID>/resourcegroups/ContosoResourceGroup/providers`
    /microsoft.documentdb/databaseaccounts/contosocosmosdb/providers/microsoft.insights/diagnosticSettings/service
    Name                        : service
    Type                        :
    Location                    :
    Tags                        :
```

O resultado do comando confirma que o registo está agora ativado para a base de dados e informações está a ser guardadas à sua conta de armazenamento.

Opcionalmente, também pode definir a política de retenção para os seus registos, que os registos antigos são eliminados automaticamente. Por exemplo, definir a política de retenção com a **- RetentionEnabled** sinalizador definido como **$true**. Definir o **- RetentionInDays** parâmetro **90** para que os registos com mais de 90 dias são eliminados automaticamente.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories DataPlaneRequests`
  -RetentionEnabled $true -RetentionInDays 90
```

### <a id="access"></a>Aceder aos seus registos
BD do Azure do Cosmos os registos para o **DataPlaneRequests** categoria são armazenadas no **insights-registos--plane-pedidos de dados** contentor na conta de armazenamento que indicou. 

Primeiro, crie uma variável para o nome do contentor. A variável é utilizada em toda a passagem.

```powershell
    $container = 'insights-logs-dataplanerequests'
```

Para listar todos os blobs neste contentor, escreva:

```powershell
Get-AzureStorageBlob -Container $container -Context $sa.Context
```

O resultado para o comando deve assemelhar-se o exemplo seguinte:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 10510193
ContentType       : application/octet-stream
LastModified      : 9/28/2017 7:49:04 PM +00:00
SnapshotTime      :
ContinuationToken:
Context           : Microsoft.WindowsAzure.Commands.Common.Storage.`
                    LazyAzureStorageContext
Name              : resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS`
/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/CONTOSOCOSMOSDB/y=2017/m=09/d=28/h=19/m=00/PT1H.json
```

Como pode neste resultado, os blobs seguem uma convenção de nomenclatura: `resourceId=/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DOCUMENTDB/DATABASEACCOUNTS/<Database Account Name>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json`

Os valores data e hora utilizam o UTC.

Porque a mesma conta de armazenamento pode ser utilizada para recolher registos para vários recursos, pode utilizar o ID de recurso completamente qualificado no nome do blob para aceder e transferir os blobs específicos que necessita. Antes de o fazermos, vamos abordar transferir todos os blobs.

Primeiro, crie uma pasta para transferir os blobs. Por exemplo:

```powershell
New-Item -Path 'C:\Users\username\ContosoCosmosDBLogs'`
 -ItemType Directory -Force
```

Em seguida, obter uma lista de todos os blobs:  

```powershell
$blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context
```

Encaminhe esta lista através de `Get-AzureStorageBlobContent` comando para transferir os blobs para a pasta de destino:

```powershell
$blobs | Get-AzureStorageBlobContent `
 -Destination 'C:\Users\username\ContosoCosmosDBLogs'
```

Quando executar este segundo comando, o **/** delimitador dos nomes blob cria uma estrutura de pasta completa sob a pasta de destino. Esta estrutura de pastas é utilizada para transferir e armazenar os blobs como ficheiros.

Para transferir seletivamente blobs, utilize carateres universais. Por exemplo:

* Se tiver várias bases de dados e pretender transferir registos para apenas uma base de dados denominada **CONTOSOCOSMOSDB3**, utilize o comando:

    ```powershell
    Get-AzureStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/DATABASEACCOUNTS/CONTOSOCOSMOSDB3
    ```

* Se tiver vários grupos de recursos e pretender transferir os registos para apenas um grupo de recursos, utilize o comando `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

    ```powershell
    Get-AzureStorageBlob -Container $container `
    -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
    ```
* Se pretende transferir todos os registos do mês de Julho de 2017, utilize o comando `-Blob '*/year=2017/m=07/*'`:

    ```powershell
    Get-AzureStorageBlob -Container $container `
     -Context $sa.Context -Blob '*/year=2017/m=07/*'
    ```

Também pode executar os seguintes comandos:

* Para consultar o estado das definições de diagnóstico para o seu recurso de base de dados, utilize o comando `Get-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`.
* Para desativar o registo do **DataPlaneRequests** categoria para o seu recurso de conta de base de dados, utilize o comando `Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories DataPlaneRequests`.


Os blobs que são devolvidos em cada uma das seguintes consultas são armazenados como texto e formatados como um blob JSON, conforme mostrado no seguinte código:

```json
{
    "records":
    [
        {
           "time": "Fri, 23 Jun 2017 19:29:50.266 GMT",
           "resourceId": "contosocosmosdb",
           "category": "DataPlaneRequests",
           "operationName": "Query",
           "resourceType": "Database",
           "properties": {"activityId": "05fcf607-6f64-48fe-81a5-f13ac13dd1eb",`
           "userAgent": "documentdb-dotnet-sdk/1.12.0 Host/64-bit MicrosoftWindowsNT/6.2.9200.0 AzureSearchIndexer/1.0.0",`
           "resourceType": "Database","statusCode": "200","documentResourceId": "",`
           "clientIpAddress": "13.92.241.0","requestCharge": "2.260","collectionRid": "",`
           "duration": "9250","requestLength": "72","responseLength": "209", "resourceTokenUserRid": ""}
        }
    ]
}
```

Para saber mais sobre os dados de cada blob JSON, consulte [interpretar os registos da base de dados do Azure Cosmos](#interpret).

## <a name="manage-your-logs"></a>Gerir os seus registos

Os registos de diagnóstico são disponibilizados na sua conta para duas horas desde o momento em que a operação de base de dados do Azure Cosmos foi efetuada. Cabe-lhe gerir os seus registos na sua conta de armazenamento:

* Utilize métodos de controlo padrão de acesso do Azure para proteger os seus registos e restringir quem podem aceder aos mesmos.
* Elimine os registos que já não pretende manter na sua conta de armazenamento.
* O período de retenção para pedidos de plane de dados que sejam arquivadas para uma conta de armazenamento está configurado no portal quando o **registo DataPlaneRequests** definição está selecionada. Para alterar essa definição, consulte [ative o registo no portal do Azure](#turn-on-logging-in-the-azure-portal).


<a id="#view-in-loganalytics"></a>
## <a name="view-logs-in-log-analytics"></a>Ver registos na análise de registos

Se tiver selecionado o **enviar ao Log Analytics** opção quando ativou o registo de diagnóstico, diagnóstico dados da sua coleção seja reencaminhados para a análise de registos dentro de duas horas. Quando observar a análise de registos imediatamente depois de ativar o registo, que não veja todos os dados. Apenas, aguarde duas horas e tente novamente. 

Antes de visualizar os seus registos, consulte e verifique se a sua área de trabalho de análise de registos foi atualizada para utilizar o novo idioma de consulta de análise de registos. Para verificar, abra o [portal do Azure](https://portal.azure.com), selecione **análise de registos** da extremidade esquerda, em seguida, selecione o nome da área de trabalho conforme mostrado na imagem seguinte. O **área de trabalho OMS** é apresentada a página:

![Análise de registos no portal do Azure](./media/logging/azure-portal.png)

Se vir a mensagem seguinte no **área de trabalho OMS** página, a sua área de trabalho não foi atualizada para utilizar o novo idioma. Para obter mais informações sobre como atualizar para o novo idioma de consulta, consulte [atualizar a sua área de trabalho do Log Analytics do Azure para a pesquisa do novo registo](../log-analytics/log-analytics-log-search-upgrade.md). 

![Análise de registos atualizar mensagem](./media/logging/upgrade-notification.png)

Para ver os seus dados de diagnóstico na análise de registos, abra o **pesquisa registo** página no menu esquerdo ou **gestão** área da página, conforme mostrado na imagem seguinte:

![Opções de pesquisa de registo no portal do Azure](./media/logging/log-analytics-open-log-search.png)

Agora que ativou a recolha de dados, execute o seguinte exemplo de pesquisa de registo utilizando o novo idioma de consulta para ver os 10 registos mais recentes `AzureDiagnostics | take 10`.

![Pesquisa de registo de exemplo para 10 registos mais recentes](./media/logging/log-analytics-query.png)

<a id="#queries"></a>
### <a name="queries"></a>Consultas

Seguem-se algumas consultas adicionais que pode introduzir no **pesquisa registo** caixa para ajudar a monitorizar os contentores de BD do Cosmos do Azure. Estas consultas trabalham com o [novo idioma](../log-analytics/log-analytics-log-search-upgrade.md). 

Para saber mais sobre o significado dos dados que são devolvidos pela pesquisa cada registo, consulte [interpretar os registos da base de dados do Azure Cosmos](#interpret).

* Para consultar todos os registos de diagnóstico da base de dados do Azure Cosmos durante um período de tempo especificado:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests"
    ```

* Para consultar a 10 mais recentemente registado eventos:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | take 10
    ```

* Para consultar todas as operações, agrupadas por tipo de operação:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName
    ```

* A consulta para todas as operações, agrupados por **recursos**:

    ```
    AzureActivity | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```

* Para consultar todas as atividades de utilizador, agrupadas por recurso:

    ```
    AzureActivity | where Caller == "test@company.com" and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by Resource
    ```
    > [!NOTE]
    > Este comando destina-se um registo de atividade, não um registo de diagnóstico.

* A consulta para o qual operações demorar mais do que 3 milissegundos:

    ```
    AzureDiagnostics | where toint(duration_s) > 30000 and ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by clientIpAddress_s, TimeGenerated
    ```

* A consulta para o qual o agente está a executar as operações:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | summarize count() by OperationName, userAgent_s
    ```

* Para consultar quando foram efetuadas as operações de execução longa:

    ```
    AzureDiagnostics | where ResourceProvider=="MICROSOFT.DOCUMENTDB" and Category=="DataPlaneRequests" | project TimeGenerated , toint(duration_s)/1000 | render timechart
    ```

Para obter mais informações sobre como utilizar o novo idioma de pesquisa de registo, consulte [pesquisas de registo de compreender na análise de registos](../log-analytics/log-analytics-log-search-new.md). 

## <a id="interpret"></a>Interpretar os seus registos

Dados de diagnóstico que são armazenados no armazenamento do Azure e análise de registos utilizam um esquema semelhante. 

A tabela seguinte descreve o conteúdo de cada entrada de registo.

| Campo de armazenamento do Azure ou uma propriedade | Propriedade de análise do registo | Descrição |
| --- | --- | --- |
| **Tempo** | **TimeGenerated** | A data e hora (UTC) quando ocorreu a operação. |
| **resourceId** | **Recurso** | A conta de base de dados do Azure Cosmos para o qual os registos estão ativados.|
| **category** | **Categoria** | Para os registos de base de dados do Azure Cosmos **DataPlaneRequests** é o valor só se encontra disponível. |
| **operationName** | **OperationName** | Nome da operação. Este valor pode ser qualquer uma das seguintes operações: criar, atualizar, leitura, ReadFeed, eliminar, substituir, executar, SqlQuery, consulta, JSQuery, Head, HeadFeed ou Upsert.   |
| **Propriedades** | n/d | O conteúdo deste campo é descrito nas linhas que se seguem. |
| **activityId** | **activityId_g** | O GUID exclusivo para a operação com sessão iniciada. |
| **userAgent** | **userAgent_s** | Uma cadeia que especifica o agente de utilizador do cliente que está a efetuar o pedido. O formato é {nome do agente de utilizador} / {version}.|
| **resourceType** | **ResourceType** | O tipo de recurso acedido. Este valor pode ser qualquer um dos seguintes tipos de recurso: base de dados, coleção, documentos, anexo, um utilizador, permissão, StoredProcedure, acionador, UserDefinedFunction ou oferta. |
| **statusCode** | **statusCode_s** | O estado de resposta da operação. |
| **requestResourceId** | **ResourceId** | O resourceId que diz respeito ao pedido. O valor pode apontar para databaseRid, collectionRid ou documentRid dependendo da operação efetuada.|
| **clientIpAddress** | **clientIpAddress_s** | Endereço IP do cliente. |
| **requestCharge** | **requestCharge_s** | O número de RUs que são utilizados pela operação |
| **collectionRid** | **collectionId_s** | O ID exclusivo para a coleção.|
| **duration** | **duration_s** | A duração da operação, numa ticks. |
| **requestLength** | **requestLength_s** | O comprimento do pedido, em bytes. |
| **responseLength** | **responseLength_s** | O comprimento da resposta, em bytes.|
| **resourceTokenUserRid** | **resourceTokenUserRid_s** | Este valor é não vazia quando [tokens de recurso](https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data#resource-tokens) são utilizados para autenticação. O valor aponta para o ID de recurso do utilizador. |

## <a name="next-steps"></a>Passos Seguintes

- Para compreender como ativar o registo bem como as categorias de métricas e registo que são suportadas pelos vários serviços do Azure, leia ambos o [descrição geral das métricas no Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) e [descrição geral do Azure os registos de diagnóstico ](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artigos.
- Estes artigos para saber mais sobre os event hubs de leitura:
   - [O que é Event Hubs do Azure?](../event-hubs/event-hubs-what-is-event-hubs.md)
   - [Introdução ao Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- Leitura [transferir métricas e registos de diagnóstico do armazenamento do Azure](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-blobs).
- Leitura [pesquisas de registo de compreender na análise de registos](../log-analytics/log-analytics-log-search-new.md).
