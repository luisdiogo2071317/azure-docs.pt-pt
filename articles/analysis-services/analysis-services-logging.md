---
title: Diagnóstico de registo para o Azure Analysis Services | Documentos da Microsoft
description: Saiba mais sobre como configurar o registo de diagnósticos para o Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 480d453cc906fa1b1d93e00bd4a6d2b080768a47
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2019
ms.locfileid: "54105841"
---
# <a name="setup-diagnostic-logging"></a>Configurar registo de diagnósticos

Uma parte importante de qualquer solução de Analysis Services está a monitorizar o desempenho dos seus servidores. Com o [registos de diagnóstico de recursos do Azure](../azure-monitor/platform/diagnostic-logs-overview.md), pode monitorizar e enviar registos ao [armazenamento do Azure](https://azure.microsoft.com/services/storage/), transmiti-los para [Hubs de eventos do Azure](https://azure.microsoft.com/services/event-hubs/)e exportá-las para [registo Análise](https://azure.microsoft.com/services/log-analytics/), um serviço da [Azure](https://www.microsoft.com/cloud-platform/operations-management-suite). 

![Registo de diagnósticos para armazenamento, os Hubs de eventos ou do Log Analytics](./media/analysis-services-logging/aas-logging-overview.png)


## <a name="whats-logged"></a>O que é registado?

Pode selecionar **motor**, **Service**, e **métricas** categorias.

### <a name="engine"></a>Motor

Selecionando **motor** regista todos [xEvents](https://docs.microsoft.com/sql/analysis-services/instances/monitor-analysis-services-with-sql-server-extended-events). Não é possível selecionar eventos individuais. 

|Categorias de XEvent |Nome do evento  |
|---------|---------|
|Auditoria de Segurança    |   Início de sessão de auditoria      |
|Auditoria de Segurança    |   Fim de sessão de auditoria      |
|Auditoria de Segurança    |   Servidor de auditoria é iniciada e interrompida      |
|Relatórios de progresso     |   Início do relatório de progresso      |
|Relatórios de progresso     |   Fim do relatório de progresso      |
|Relatórios de progresso     |   Atual do relatório de progresso      |
|Consultas     |  Início da consulta       |
|Consultas     |   Fim da consulta      |
|Comandos     |  Início do comando       |
|Comandos     |  Fim do comando       |
|Erros e avisos     |   Erro      |
|Descobrir     |   Detetar final      |
|Notificação     |    Notificação     |
|Sessão     |  Inicialização de sessão       |
|Bloqueios    |  Deadlock       |
|Processamento de consultas     |   Início de consulta SE VertiPaq      |
|Processamento de consultas     |   VertiPaq SE Query End      |
|Processamento de consultas     |   Correspondência de Cache de consulta SE VertiPaq      |
|Processamento de consultas     |   Início da consulta direta      |
|Processamento de consultas     |  Fim da consulta direta       |

### <a name="service"></a>Serviço

|Nome da operação  |Ocorre quando  |
|---------|---------|
|ResumeServer     |    Retomar a um servidor     |
|SuspendServer    |   Colocar em pausa um servidor      |
|DeleteServer     |    Eliminar um servidor     |
|RestartServer    |     Utilizador reinicia um servidor através do SSMS ou o PowerShell    |
|GetServerLogFiles    |    Utilizador exporta o registo do servidor através do PowerShell     |
|ExportModel     |   Utilizador exporta um modelo no portal usando aberto no Visual Studio     |

### <a name="all-metrics"></a>Todas as métricas

A categoria de métricas, registos a mesma [métricas do servidor](analysis-services-monitor.md#server-metrics) apresentados nas métricas.

## <a name="setup-diagnostics-logging"></a>Configurar registo de diagnóstico

### <a name="azure-portal"></a>Portal do Azure

1. Na [portal do Azure](https://portal.azure.com) > servidor, clique em **registos de diagnóstico** na navegação à esquerda e, em seguida, clique **ativar diagnósticos**.

    ![Ativar o registo de diagnósticos para o Azure Cosmos DB no portal do Azure](./media/analysis-services-logging/aas-logging-turn-on-diagnostics.png)

2. Na **das definições de diagnóstico**, especifique as seguintes opções: 

    * **Nome**. Introduza um nome para os registos criar.

    * **Arquivo para uma conta de armazenamento**. Para utilizar esta opção, terá de uma conta de armazenamento existente para ligar a. Ver [criar uma conta de armazenamento](../storage/common/storage-create-storage-account.md). Siga as instruções para criar um Gerenciador de recursos, conta para fins gerais, em seguida, selecione a sua conta de armazenamento por meio do retorno a esta página no portal. Pode demorar alguns minutos para as contas de armazenamento recentemente criada aparece no menu pendente.
    * **Stream para um hub de eventos**. Para utilizar esta opção, terá de um Hub de eventos espaço de nomes e o event hub para ligar a. Para obter mais informações, consulte [criar um espaço de nomes de Hubs de eventos e um hub de eventos com o portal do Azure](../event-hubs/event-hubs-create.md). Em seguida, regresse a esta página no portal para selecionar o nome de espaço de nomes e a política do Hub de eventos.
    * **Enviar para o Log Analytics**. Para utilizar esta opção, utilize uma área de trabalho existente ou crie uma nova área de trabalho do Log Analytics ao seguir os passos para [criar uma nova área de trabalho](../azure-monitor/learn/quick-collect-azurevm.md#create-a-workspace) no portal. Para obter mais informações sobre a visualização dos registos no Log Analytics, consulte [vista de registos no Log Analytics](#view-logs-in-log-analytics) neste artigo.

    * **Motor**. Selecione esta opção para registar xEvents. Se estiver arquivando para uma conta de armazenamento, pode selecionar o período de retenção para os registos de diagnóstico. Os registos estão autodeleted após o período de retenção expira.
    * **Serviço**. Selecione esta opção para registar eventos de nível de serviço. Se estiver a arquivamento para uma conta de armazenamento, pode selecionar o período de retenção para os registos de diagnóstico. Os registos estão autodeleted após o período de retenção expira.
    * **Métricas**. Selecione esta opção para armazenar dados detalhados no [métricas](analysis-services-monitor.md#server-metrics). Se estiver a arquivamento para uma conta de armazenamento, pode selecionar o período de retenção para os registos de diagnóstico. Os registos estão autodeleted após o período de retenção expira.

3. Clique em **Guardar**.

    Se receber um erro que diz "Falha ao atualizar diagnósticos para \<nome de área de trabalho >. A subscrição \<id da subscrição > não está registada para utilizar o Microsoft. insights. " Siga os [resolver problemas relacionados com o Azure Diagnostics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) as instruções para registar a conta, em seguida, repita este procedimento.

    Se pretender alterar a forma como em que seus registos de diagnóstico são guardados no futuro em qualquer momento, pode regressar a esta página para modificar as definições.

### <a name="powershell"></a>PowerShell

Aqui estão os comandos básicos para o ajudar. Se pretender que o ajuda passo a passo sobre como configurar o registo para uma conta de armazenamento com o PowerShell, veja o tutorial neste artigo.

Para ativar as métricas e diagnósticos de registro com o PowerShell, utilize os seguintes comandos:

- Para ativar o armazenamento de registos de diagnóstico numa conta de armazenamento, utilize este comando:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   O ID de conta de armazenamento é o ID de recurso para a conta de armazenamento onde pretende enviar os registos.

- Para ativar a transmissão em fluxo de registos de diagnóstico para um hub de eventos, use este comando:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   O ID de regra de Azure Service Bus é uma cadeia de caracteres com este formato:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- Para ativar o envio de registos de diagnóstico para uma área de trabalho do Log Analytics, use este comando:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- Pode obter o ID de recurso da sua área de trabalho do Log Analytics, utilizando o seguinte comando:

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

Pode combinar estes parâmetros para ativar várias opções de saída.

### <a name="rest-api"></a>API REST

Saiba como [alterar as definições de diagnóstico, utilizando a API de REST do Azure Monitor](https://docs.microsoft.com/rest/api/monitor/). 

### <a name="resource-manager-template"></a>Modelo do Resource Manager

Saiba como [ative as definições de diagnóstico durante a criação de recursos com um modelo do Resource Manager](../azure-monitor/platform/diagnostic-logs-stream-template.md). 

## <a name="manage-your-logs"></a>Gerir os seus registos

Os registos estão normalmente disponíveis dentro de duas horas de configuração de registo. Cabe-lhe gerir os seus registos na sua conta de armazenamento:

* Utilize métodos padrão de controlo de acesso do Azure para proteger os seus registos, restringindo o seu acesso.
* Elimine os registos que já não pretende manter na sua conta de armazenamento.
* Certifique-se de que definir um período de retenção para que os registos antigos serão eliminados da conta de armazenamento.

## <a name="view-logs-in-log-analytics"></a>Ver registos no Log Analytics

Eventos de métricas e servidor estão integrados xEvents no Log Analytics para análise de lado a lado. O log Analytics também pode ser configurado para receber eventos dos outros serviços do Azure, oferecendo uma visão holística dos dados de registo de diagnóstico em sua arquitetura.

Para ver os dados de diagnóstico no Log Analytics, abra a página de pesquisa de registos do menu à esquerda ou área de gestão, conforme mostrado abaixo.

![Opções de pesquisa de registo no portal do Azure](./media/analysis-services-logging/aas-logging-open-log-search.png)

Agora que ativou a recolha de dados, no **pesquisa de registos**, clique em **todos os dados recolhidos**.

Na **tipo**, clique em **AzureDiagnostics**e, em seguida, clique em **aplicar**. AzureDiagnostics inclui o mecanismo e eventos de serviço. Observe que uma consulta do Log Analytics é criado no momento. A registar\_campo s contém nomes de xEvent, que podem parecer familiares se já usou xEvents para iniciar sessão no local.

Clique em **registar\_s** ou um dos nomes de eventos e do Log Analytics continua a construir uma consulta. Certifique-se de que guardar as suas consultas para reutilização posterior.

Certifique-se de que consulte o Log Analytics, que fornece um Web site com as capacidades de alertas sobre os dados recolhidos, vistas e de consultas avançada.

### <a name="queries"></a>Consultas

Há centenas de consultas que pode utilizar. Aqui estão alguns para começar.
Para saber mais sobre como utilizar a nova linguagem de consulta de pesquisa de registos, consulte [compreender a pesquisa do Log Analytics](../log-analytics/log-analytics-log-search-new.md). 

* Consulta devolver consultas submetidas para o Azure Analysis Services que demorou mais de cinco minutos (300.000 milissegundos) para concluir.

    ```
    search * | where ( Type == "AzureDiagnostics" ) | where ( EventClass_s == "QUERY_END" ) | where toint(Duration_s) > 300000
    ```

* Identifique as réplicas de escalamento horizontal.

    ```
    search * | summarize count() by ServerName_s
    ```
    Ao utilizar o Escalamento horizontal, pode identificar réplicas só de leitura porque o ServerName\_valores de campo s tem o número de instância da réplica acrescentado ao nome. O campo de recursos contém o nome de recurso do Azure, que corresponde ao nome do servidor que os utilizadores veem. O campo de IsQueryScaleoutReadonlyInstance_s é igual a VERDADEIRO para réplicas.



> [!TIP]
> Tem uma consulta do Log Analytics excelente que pretende partilhar? Se tiver uma conta do GitHub, pode adicioná-lo a este artigo. Basta clicar **editar** no canto superior direito desta página.


## <a name="tutorial---turn-on-logging-by-using-powershell"></a>Tutorial - ativar o registo com o PowerShell
Este tutorial rápido, vai criar uma conta de armazenamento na mesma subscrição e grupo de recursos que o seu servidor do serviço de análise. Em seguida, utilizar Set-AzureRmDiagnosticSetting para ativar os diagnósticos de log e enviar a saída para a nova conta de armazenamento.

### <a name="prerequisites"></a>Pré-requisitos
Para concluir este tutorial, tem de ter os seguintes recursos:

* Um servidor existente do Azure Analysis Services. Para obter instruções sobre como criar um recurso de servidor, consulte [criar um servidor no portal do Azure](analysis-services-create-server.md), ou [criar um servidor Azure Analysis Services com o PowerShell](analysis-services-create-powershell.md).

### <a name="aconnect-to-your-subscriptions"></a></a>Ligar às suas subscrições

Abra uma sessão no Azure PowerShell e inicie sessão na sua conta do Azure com o seguinte comando:  

```powershell
Connect-AzureRmAccount
```

Na janela pop-up do browser, introduza o seu nome de utilizador da conta do Azure e a palavra-passe. O Azure PowerShell obtém todas as subscrições associadas a esta conta e, por defeito, utiliza a primeira.

Se tiver várias subscrições, poderá ter de especificar uma subscrição utilizada para criar o seu Cofre de Chaves do Azure. Escreva o seguinte para ver as subscrições da sua conta:

```powershell
Get-AzureRmSubscription
```

Em seguida, para especificar a subscrição que está associada a conta do Azure Analysis Services que está a iniciar sessão, escreva:

```powershell
Set-AzureRmContext -SubscriptionId <subscription ID>
```

> [!NOTE]
> Se tiver várias subscrições associadas à sua conta, é importante especificar a subscrição.
>
>

### <a name="create-a-new-storage-account-for-your-logs"></a>Criar uma nova conta de armazenamento para os seus registos

Pode utilizar uma conta de armazenamento existente para os seus registos, desde que ele está na mesma subscrição que o seu servidor. Para este tutorial cria uma nova conta de armazenamento dedicada aos registos de Analysis Services. Para que seja fácil, está armazenando os detalhes da conta de armazenamento numa variável chamada **sa**.

Também usar o mesmo grupo de recursos como aquela que contém o servidor do Analysis Services. Substitua os valores para `awsales_resgroup`, `awsaleslogs`, e `West Central US` pelos seus próprios valores:

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName awsales_resgroup `
-Name awsaleslogs -Type Standard_LRS -Location 'West Central US'
```

### <a name="identify-the-server-account-for-your-logs"></a>Identificar a conta de servidor para os seus registos

Defina o nome de conta como uma variável chamada **conta**, onde ResourceName é o nome da conta.

```powershell
$account = Get-AzureRmResource -ResourceGroupName awsales_resgroup `
-ResourceName awsales -ResourceType "Microsoft.AnalysisServices/servers"
```

### <a name="enable-logging"></a>Ativar registo

Para ativar o registo, utilize o cmdlet Set-AzureRmDiagnosticSetting, juntamente com as variáveis para a nova conta de armazenamento, conta de servidor e a categoria. Execute o seguinte comando, definindo a **-ativado** sinalizador para **$true**:

```powershell
Set-AzureRmDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories Engine
```

O resultado deverá ser semelhante ao seguinte:

```powershell
StorageAccountId            : 
/subscriptions/a23279b5-xxxx-xxxx-xxxx-47b7c6d423ea/resourceGroups/awsales_resgroup/providers/Microsoft.Storage/storageAccounts/awsaleslogs
ServiceBusRuleId            :
EventHubAuthorizationRuleId :
Metrics                    
    TimeGrain       : PT1M
    Enabled         : False
    RetentionPolicy
    Enabled : False
    Days    : 0


Logs                       
    Category        : Engine
    Enabled         : True
    RetentionPolicy
    Enabled : False
    Days    : 0


    Category        : Service
    Enabled         : False
    RetentionPolicy
    Enabled : False
    Days    : 0


WorkspaceId                 :
Id                          : /subscriptions/a23279b5-xxxx-xxxx-xxxx-47b7c6d423ea/resourcegroups/awsales_resgroup/providers/microsoft.analysisservic
es/servers/awsales/providers/microsoft.insights/diagnosticSettings/service
Name                        : service
Type                        :
Location                    :
Tags                        :
```

Isto confirma que o registo está agora ativado para o servidor, ao guardar as informações para a conta de armazenamento.

Também pode definir a política de retenção para os seus registos, para que os registos mais antigos são automaticamente eliminados. Por exemplo, definir a política de retenção utilizando **- RetentionEnabled** sinalizador para **$true**e defina **- RetentionInDays** parâmetro **90**. Registos de mais de 90 dias são automaticamente eliminados.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories Engine`
  -RetentionEnabled $true -RetentionInDays 90
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre [registo de diagnósticos de recursos do Azure](../azure-monitor/platform/diagnostic-logs-overview.md).

Ver [Set-AzureRmDiagnosticSetting](https://docs.microsoft.com/powershell/module/azurerm.insights/Set-AzureRmDiagnosticSetting) na ajuda do PowerShell.
