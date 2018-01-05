---
title: O registo para o Azure Analysis Services Diganostic | Microsoft Docs
description: "Saiba mais sobre como configurar o registo de diagnóstico para o Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: 
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 12/29/2017
ms.author: owend
ms.openlocfilehash: 02c25de980b399812676285ad3f87f60af93265f
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="setup-diagnostic-logging"></a>Registo de diagnóstico de configuração

Uma parte importante de qualquer solução de Analysis Services está a monitorizar como os servidores estão a funcionar. Com [registos de diagnóstico de recursos do Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md), pode monitorizar e enviar registos ao [Storage do Azure](https://azure.microsoft.com/services/storage/), transmiti-los para [Event Hubs do Azure](https://azure.microsoft.com/services/event-hubs/)e exportá-las para [registo Análise de](https://azure.microsoft.com/services/log-analytics/), que fazem parte da [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite). 

![Registo de diagnóstico para o armazenamento, os Event Hubs ou através de análise de registos do Operations Management Suite](./media/analysis-services-logging/aas-logging-overview.png)


## <a name="whats-logged"></a>O que é registado?

Pode selecionar **motor**, **serviço**, e **métricas** categorias.

### <a name="engine"></a>Motor

Selecionar **motor** regista todos os [xEvents](https://docs.microsoft.com/sql/analysis-services/instances/monitor-analysis-services-with-sql-server-extended-events). Não é possível selecionar eventos individuais. 

|Categorias de XEvent |Nome do evento  |
|---------|---------|
|Auditoria de Segurança    |   Início de sessão de auditoria      |
|Auditoria de Segurança    |   Fim de sessão de auditoria      |
|Auditoria de Segurança    |   Servidor de auditoria é iniciado e deixa de      |
|Relatórios de progresso     |   Comece de relatório de progresso      |
|Relatórios de progresso     |   Fim de relatório de progresso      |
|Relatórios de progresso     |   Atual do relatório de progresso      |
|Consultas     |  Comece de consulta       |
|Consultas     |   Fim de consulta      |
|Comandos     |  Início de comando       |
|Comandos     |  Fim de comando       |
|Erros & avisos     |   Erro      |
|Descobrir     |   Fim de detetar      |
|Notificação     |    Notificação     |
|Sessão     |  Inicialização de sessão       |
|Bloqueios    |  impasse       |
|Processamento da consulta     |   Comece de consulta SE VertiPaq      |
|Processamento da consulta     |   Fim de consulta SE VertiPaq      |
|Processamento da consulta     |   Correspondência de Cache de consulta SE VertiPaq      |
|Processamento da consulta     |   Comece de consulta direta      |
|Processamento da consulta     |  Fim de consulta direta       |

### <a name="service"></a>Serviço

|Nome da operação  |Ocorre quando  |
|---------|---------|
|CreateGateway     |   Utilizador configura um gateway no servidor      |
|ResumeServer     |    Retomar um servidor     |
|SuspendServer    |   Colocar em pausa um servidor      |
|DeleteServer     |    Eliminar um servidor     |
|RestartServer    |     Utilizador reinicia um servidor através do SSMS ou o PowerShell    |
|GetServerLogFiles    |    Utilizador exporta o registo de servidor através do PowerShell     |
|ExportModel     |   Utilizador exporta um modelo no portal utilizando abrir no Visual Studio     |

### <a name="all-metrics"></a>Todas as métricas

A categoria de métricas regista o mesmo [métricas do servidor](analysis-services-monitor.md#server-metrics) apresentado no métricas.

## <a name="setup-diagnostics-logging"></a>Configurar o registo de diagnóstico

### <a name="azure-portal"></a>Portal do Azure

1. No [portal do Azure](https://portal.azure.com) > servidor, clique em **registos de diagnóstico** na navegação à esquerda e, em seguida, clique em **ative os diagnósticos**.

    ![Ativar o registo de diagnóstico para a BD do Cosmos do Azure no portal do Azure](./media/analysis-services-logging/aas-logging-turn-on-diagnostics.png)

2. No **definições de diagnóstico**, efetue o seguinte procedimento: 

    * **Nome**. Introduza um nome para os registos para o criar.

    * **Arquivo para uma conta de armazenamento**. Para utilizar esta opção, terá de uma conta de armazenamento existente para ligar a. Consulte [criar uma conta de armazenamento](../storage/common/storage-create-storage-account.md). Siga as instruções para criar uma conta para fins gerais do Resource Manager. Em seguida, regresse a esta página no portal para selecionar a sua conta do storage. Pode demorar alguns minutos para as contas de armazenamento recentemente criada aparece no menu pendente.
    * **Fluxo para um hub de eventos**. Para utilizar esta opção, terá de um existente Hub de eventos espaço de nomes e event hub para ligar a. Para criar um espaço de nomes de Event Hubs, consulte [criar um espaço de nomes de Event Hubs e um hub de eventos no portal do Azure](../event-hubs/event-hubs-create.md). Em seguida, regresse a esta página no portal para selecionar o nome de espaço de nomes e a política de Hub de eventos.
    * **Enviar ao Log Analytics**. Para utilizar esta opção, utilize uma área de trabalho existente ou crie uma nova área de trabalho de análise de registos, seguindo os passos para [criar uma nova área de trabalho](../log-analytics/log-analytics-quick-collect-azurevm.md#create-a-workspace) no portal. Para obter mais informações sobre a visualização dos registos na análise de registos, consulte [ver os registos na análise de registos](#view-in-loganalytics).

    * **Motor**. Selecione esta opção para registar xEvents. Se estiver a arquivar a uma conta de armazenamento, pode selecionar o período de retenção para os registos de diagnóstico. Os registos são autodeleted após o período de retenção expira.
    * **Serviço**. Selecione esta opção para registar eventos de nível de serviço. Se estiver arquivar a uma conta de armazenamento, pode selecionar o período de retenção para os registos de diagnóstico. Os registos são autodeleted após o período de retenção expira.
    * **Métricas**. Selecione esta opção para armazenar dados verbosos no [métricas](analysis-services-monitor.md#server-metrics). Se estiver arquivar a uma conta de armazenamento, pode selecionar o período de retenção para os registos de diagnóstico. Os registos são autodeleted após o período de retenção expira.

3. Clique em **Guardar**.

    Se receber um erro que indica "Falha ao atualizar os diagnósticos para \<nome da área de trabalho >. A subscrição \<id de subscrição > não está registada para utilizar insights. " Siga o [resolver problemas do diagnóstico do Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) as instruções para registar a conta, em seguida, repita este procedimento.

    Se pretender alterar a forma como em que os registos de diagnóstico são guardados no futuro em qualquer momento, pode regressar a esta página para modificar as definições.

### <a name="powershell"></a>PowerShell

Seguem-se os comandos básicos para ajudá-lo a aceder. Se pretender que o ajuda passo a passo sobre como configurar o registo para uma conta de armazenamento utilizando o PowerShell, consulte o tutorial neste artigo.

Para ativar as métricas e ao diagnóstico do registo utilizando o PowerShell, utilize os seguintes comandos:

- Para ativar o armazenamento dos registos de diagnóstico numa conta do storage, utilize este comando:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   O ID de conta de armazenamento é o ID de recurso para a conta de armazenamento onde pretende enviar os registos.

- Para ativar a transmissão em fluxo de registos de diagnóstico para um hub de eventos, utilize este comando:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   O ID de regra de Service Bus do Azure é uma cadeia com este formato:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- Para ativar o envio de registos de diagnóstico para uma área de trabalho de análise de registos, utilize este comando:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- Pode obter o ID de recurso da sua área de trabalho de análise de registos ao utilizar o seguinte comando:

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

Pode combinar estes parâmetros para ativar várias opções de saída.

### <a name="rest-api"></a>API REST

Saiba como [alterar as definições de diagnóstico ao através da API de REST do Azure Monitor](https://msdn.microsoft.com/library/azure/dn931931.aspx). 

### <a name="resource-manager-template"></a>Modelo do Resource Manager

Saiba como [ativar as definições de diagnóstico durante a criação de recursos através de um modelo do Resource Manager](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="manage-your-logs"></a>Gerir os seus registos

Os registos estão normalmente disponíveis dentro de alguns horas de configuração de registo. Cabe-lhe gerir os seus registos na sua conta de armazenamento:

* Utilize métodos padrão de controlo de acesso do Azure para proteger os seus registos, restringindo o seu acesso.
* Elimine os registos que já não pretende manter na sua conta de armazenamento.
* Não se esqueça de definir um período de retenção para para que registos antigos são eliminados da sua conta de armazenamento.

## <a name="view-logs-in-log-analytics"></a>Ver registos na análise de registos

Eventos de métricas e servidor estão integrados no xEvents na análise de registos de análise de lado a lado. Análise de registos pode também ser configurado para receber eventos de outros serviços do Azure que fornece uma vista holística de dados de registo de diagnóstico em sua arquitetura.

Para ver os dados de diagnóstico na análise de registos, abra a página de pesquisa de registo do menu à esquerda ou a área de gestão, como mostrado abaixo.

![Opções de pesquisa de registo no portal do Azure](./media/analysis-services-logging/aas-logging-open-log-search.png)

Agora que ativou a recolha de dados, no **pesquisa registo**, clique em **todos os dados de recolhidos**.

No **tipo**, clique em **AzureDiagnostics**e, em seguida, clique em **aplicar**. AzureDiagnostics inclui eventos de serviço e de motor. Tenha em atenção de que uma consulta de análise de registos é criada no momento. O EventClass\_campo s contém nomes de xEvent, que podem ter um aspeto familiares se utilizou xEvents para registo no local.

Clique em **EventClass\_s** ou um dos nomes do evento e análise de registos continua a construção de uma consulta. Lembre-se de que a guardar as suas consultas reutilizar mais tarde.

Lembre-se de que finalizar Operations Management Suite, que fornece um Web site com consulta avançada, dashboarding e as capacidades de alertas do dados de análise de registos.

### <a name="queries"></a>Consultas

Existem centenas de consultas que pode utilizar. Seguem-se alguns para ajudar a começar.
Para saber mais sobre como utilizar o novo idioma de consulta de pesquisa de registo, consulte [registo compreender procura na análise de registos](../log-analytics/log-analytics-log-search-new.md). 

* Consulta devolver consultas submetidas para o Azure Analysis Services que demorou mais de cinco minutos (300.000 milissegundos) para concluir.

    ```
    search * | where ( Type == "AzureDiagnostics" ) | where ( EventClass_s == "QUERY_END" ) | where toint(Duration_s) > 300000
    ```

* Identifica as réplicas de escalamento horizontal.

    ```
    search * | summarize count() by ServerName_s
    ```
    Quando utilizar Escalamento horizontal, pode identificar as réplicas de só de leitura porque o ServerName\_valores de campo s tem o número de instância da réplica acrescentado ao nome. O campo de recursos contém o nome de recurso do Azure, que corresponde ao nome de servidor que os utilizadores veem. O campo de IsQueryScaleoutReadonlyInstance_s é igual a true para as réplicas.



> [!TIP]
> Tem uma grande consulta de análise de registos que pretende partilhar? Se tiver uma conta GitHub, pode adicioná-lo a este artigo. Basta clicar **editar** na parte superior direita desta página.


## <a name="tutorial---turn-on-logging-by-using-powershell"></a>Tutorial - ativar o registo com o PowerShell
Neste tutorial rápido, crie uma conta do storage na mesma subscrição e grupo de recursos como o servidor do Analysis Service. Em seguida, utilize Set-AzureRmDiagnosticSetting para ativar o diagnóstico de registo, enviar o resultado para a nova conta de armazenamento.

### <a name="prerequisites"></a>Pré-requisitos
Para concluir este tutorial, tem de ter os seguintes recursos:

* Um servidor de Analysis Services do Azure existente. Para obter instruções sobre como criar um recurso de servidor, consulte [criar um servidor no portal do Azure](analysis-services-create-server.md), ou [criar um servidor de Analysis Services do Azure utilizando o PowerShell](analysis-services-create-powershell.md).

### <a name="aconnect-to-your-subscriptions"></a></a>Ligar a subscrições do

Abra uma sessão no Azure PowerShell e inicie sessão na sua conta do Azure com o seguinte comando:  

```powershell
Login-AzureRmAccount
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

Pode utilizar uma conta de armazenamento existente para os seus registos, fornecido na mesma subscrição do seu servidor. Para este tutorial cria uma nova conta de armazenamento dedicada aos registos de Analysis Services. Para facilitar, que está a armazenar os detalhes da conta de armazenamento numa variável designada **sa**.

Também é utilizar o mesmo grupo de recursos que contém o servidor de Analysis Services. Substitua os valores para `awsales_resgroup`, `awsaleslogs`, e `West Central US` com os seus próprios valores:

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName awsales_resgroup `
-Name awsaleslogs -Type Standard_LRS -Location 'West Central US'
```

### <a name="identify-the-server-account-for-your-logs"></a>Identificar a conta de servidor para os seus registos

Defina o nome da conta a uma variável com o nome **conta**, onde ResourceName é o nome da conta.

```powershell
$account = Get-AzureRmResource -ResourceGroupName awsales_resgroup `
-ResourceName awsales -ResourceType "Microsoft.AnalysisServices/servers"
```

### <a name="enable-logging"></a>Ativar registo

Para ativar o registo, utilize o cmdlet Set-AzureRmDiagnosticSetting, juntamente com as variáveis para a nova conta de armazenamento, a conta de servidor e a categoria. Execute o seguinte comando, a definição de **-ativado** sinalizador para **$true**:

```powershell
Set-AzureRmDiagnosticSetting  -ResourceId $account.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories Engine
```

O resultado deverá ter um aspeto semelhante ao seguinte:

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

Isto confirma que o registo está agora ativado para o servidor, guardar a informação para a conta de armazenamento.

Também pode definir a política de retenção para os seus registos para que os registos antigos são eliminados automaticamente. Por exemplo, definir a política de retenção utilizar **- RetentionEnabled** sinalizador para **$true**e definir **- RetentionInDays** parâmetro **90**. Os registos com mais de 90 dias são eliminados automaticamente.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId $account.ResourceId`
 -StorageAccountId $sa.Id -Enabled $true -Categories Engine`
  -RetentionEnabled $true -RetentionInDays 90
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre [registo de diagnóstico de recursos do Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

Consulte [Set-AzureRmDiagnosticSetting](https://docs.microsoft.com/powershell/module/azurerm.insights/Set-AzureRmDiagnosticSetting) na ajuda do PowerShell.