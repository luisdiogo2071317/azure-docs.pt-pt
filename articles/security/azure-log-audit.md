---
title: Azure de registo e auditoria | Microsoft Docs
description: Saiba mais sobre como pode utilizar dados de registo para obter conhecimentos aprofundados sobre a sua aplicação.
services: security
documentationcenter: na
author: UnifyCloud
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: e4144ca0d87abda3d9f8de47e56af59d0e4af312
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938371"
---
# <a name="azure-logging-and-auditing"></a>Registo e auditoria do Azure

O Azure oferece uma grade diversidade de auditoria e registo opções para o ajudar a identificar lacunas na sua políticas de segurança e mecanismos de segurança configurável. Este artigo aborda gerar, recolha e análise de registos de segurança de serviços alojados no Azure.

> [!Note]
> Algumas recomendações neste artigo poderão resultar numa maior dados, a rede ou a utilização de recursos de computação e aumentar os custos de licenciamento ou de subscrição.

## <a name="types-of-logs-in-azure"></a>Tipos de registos no Azure
Aplicações em nuvem são complexas, com várias partes mover. Os registos de fornecerem dados para ajudar a manter as suas aplicações e funcional. Os registos de ajudam a resolver passado problemas ou impedir a potencial aqueles. E pode ajudar a melhorar o desempenho da aplicação ou maintainability ou automatizar ações que caso contrário necessitem intervenção manual.

Os registos do Azure são classificados nos seguintes tipos:
* **Registos de controlo/gestão** fornecem informações sobre as operações do Azure Resource Manager criar, ATUALIZAR e eliminar. Para obter mais informações, consulte [registos de atividade do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).

* **Dados plane registos** fornecem informações sobre eventos gerados como parte utilização de recursos do Azure. Os exemplos deste tipo de registo são o sistema de eventos do Windows, segurança, e os registos de aplicação numa máquina virtual (VM) e o [registos de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) que estão configurados por meio do Monitor do Azure.

* **Processar eventos** fornecem informações sobre analisados/alertas de eventos que foram processados em seu nome. Os exemplos deste tipo são [alertas do Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) onde [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro) tem processado e analisados a sua subscrição e fornece alertas de segurança concisa.

A tabela seguinte lista os tipos mais importantes de registos disponíveis no Azure:

| Categoria de registo | Tipo de registo | Utilização | Integração |
| ------------ | -------- | ------ | ----------- |
|[Registos de atividade](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|Eventos de controlo plane nos recursos do Azure Resource Manager|   Fornece informações sobre as operações que foram executadas no recursos na sua subscrição.|    REST API, [Monitor do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|
|[Registos de diagnóstico do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|Dados frequentes sobre o funcionamento dos recursos do Azure Resource Manager na subscrição|    Fornece informações sobre as operações que o recurso do próprio efetuado.| Monitor do Azure, [fluxo](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|
|[Relatórios do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)|Os registos e relatórios | Relatórios de atividades de início de sessão de utilizador e as informações de atividade do sistema sobre utilizadores e gestão de grupo.|[Graph API](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api-quickstart)|
|[Máquinas virtuais e serviços em nuvem](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-collect-azurevm)|Serviço de registo de eventos do Windows e Linux Syslog|    Captura de dados de sistema e dados de registo nas máquinas virtuais e transferências de dados para uma conta de armazenamento à sua escolha.|   Windows (com o Windows Azure Diagnostics [[WAD](https://docs.microsoft.com/azure/azure-diagnostics)] armazenamento) e Linux no Monitor do Azure|
|[Análise de armazenamento do Azure](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)|O registo de armazenamento, fornece dados de métricas para uma conta de armazenamento|Fornece informações sobre os pedidos de rastreio, analisa as tendências de utilização e diagnoses problemas com a sua conta de armazenamento.|   REST API ou o [biblioteca de clientes](https://msdn.microsoft.com/library/azure/mt347887.aspx)|
|[Registos de fluxo do grupo de segurança de rede (NSG)](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview)|Formato JSON, mostra fluxos de saída e entrados numa base por regras|Mostra informações sobre o tráfego IP de entrada e de saída através de um grupo de segurança de rede.|[Observador de rede do Azure](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)|
|[Informações de aplicação](https://docs.microsoft.com/azure/application-insights/app-insights-overview)|Os registos, exceções e diagnóstico personalizado|   Fornece um serviço (APM) para programadores de web em várias plataformas de monitorização do desempenho de aplicações.| REST API, [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)|
|Processar os dados / alertas de segurança|    Alertas do Centro de segurança do Azure, os alertas de Log Analytics do Azure|   Fornece informações de segurança e alertas.|  REST APIs, JSON|

### <a name="activity-logs"></a>Registos de atividade
[Registos de atividade do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) fornecem informações aprofundadas as operações que foram executadas no recursos na sua subscrição. Registos de atividade foram anteriormente conhecidos como "registos de auditoria" ou "registos operacionais", porque estes relatórios [eventos de controlo plane](https://driftboatdave.com/2016/10/13/azure-auditing-options-for-your-custom-reporting-needs/) para as suas subscrições. 

Os registos de atividade ajuda a determinar o "o que, quem e quando" para operações de escrita (ou seja, PUT, POST ou eliminar). Atividade regista também ajuda a compreender o estado da operação e outras propriedades relevantes. Registos de atividade não incluir operações de leitura (GET).

Neste artigo, PUT, POST e DELETE referem-se a todas as operações de escrita que contém um registo de atividade nos recursos. Por exemplo, pode utilizar os registos de atividade para localizar um erro quando estiver a resolver problemas ou para monitorizar a forma como um utilizador na sua organização modificou um recurso.

![Diagrama do registo de atividade](./media/azure-log-audit/azure-log-audit-fig1.png)

Pode obter eventos de um registo de atividade com o portal do Azure, [CLI do Azure](https://docs.microsoft.com/azure/storage/storage-azure-cli), cmdlets do PowerShell, e [API REST da Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough). Registos de atividade tem período de retenção de dados 19 dia.

Cenários de integração para um registo de eventos de atividade:

* [Criar um alerta de e-mail ou webhook que é acionado por um evento de registo de atividade](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-auditlog-to-webhook-email).

* [Transmiti-lo para um hub de eventos](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs) para ingestão por um serviço independente ou uma solução de análise personalizada, tais como o Power BI.

* Analisá-lo no Power BI utilizando o [pacote de conteúdos do Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).

* [Guarde-o para uma conta de armazenamento de arquivo ou manual inspeção](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-activity-log). Pode especificar o período de retenção (em dias) através da utilização de perfis de registo.

* Consultar e vê-la no portal do Azure.

* Consultá-lo através do cmdlet do PowerShell, a CLI do Azure ou a REST API.

* Exportar o registo de atividade com perfis de registo para [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).

Pode utilizar uma conta de armazenamento ou [espaço de nomes de hub de eventos](https://docs.microsoft.com/azure/event-hubs/event-hubs-resource-manager-namespace-event-hub-enable-archive) que não está na mesma subscrição que está a emitir o registo. Quem configura a definição tem de ter o adequado [controlo de acesso baseado em funções (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) acesso para ambas as subscrições.

### <a name="azure-diagnostics-logs"></a>Registos de diagnóstico do Azure
Registos de diagnóstico do Azure são emitidos por um recurso que fornece dados avançados, frequentes sobre o funcionamento desse recurso. O conteúdo estes registos varia consoante o tipo de recurso. Por exemplo, [registos de sistema de eventos do Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) são uma categoria de registos de diagnóstico para VMs, e [blob, tabela e fila registos](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account) são categorias de registos de diagnóstico para contas de armazenamento. Registos de diagnóstico diferem dos registos de atividade, que fornecem informações aprofundadas as operações que foram executadas no recursos na sua subscrição.

![Diagramas de registos de diagnóstico do Azure](./media/azure-log-audit/azure-log-audit-fig2.png)

Registos de diagnóstico do Azure oferecem várias opções de configuração, tais como o portal do Azure, PowerShell, CLI do Azure e a API REST.

**Cenários de integração**

* Guardá-las para um [conta de armazenamento](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs) de inspeção de auditoria ou manual. Pode especificar o período de retenção (em dias) utilizando as definições de diagnóstico.

* [Transmiti-las para os event hubs](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs) para ingestão por um serviço independente ou uma solução de análise personalizada, tal como [PowerBI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/).

* Analisá-los com [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).

**Serviços suportados, o esquema para os registos de diagnóstico e categorias de registo suportado por tipo de recurso**


| Serviço | Esquema e a documentação | Tipo de recurso | Categoria |
| ------- | ------------- | ------------- | -------- |
|Azure Load Balancer| [Análise de registos para o Balanceador de carga (pré-visualização)](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log)|Microsoft.Network/loadBalancers<br>Microsoft.Network/loadBalancers| LoadBalancerAlertEvent<br>LoadBalancerProbeHealthStatus|
|Grupos de Segurança de Rede|[Análise de registos para grupos de segurança de rede](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)|Microsoft.Network/networksecuritygroups<br>Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent<br>NetworkSecurityGroupRuleCounter|
|Gateway de Aplicação do Azure|[Registo de diagnóstico para o Gateway de aplicação](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)|Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog<br>ApplicationGatewayPerformanceLog<br>ApplicationGatewayFirewallLog|
|Azure Key Vault|[Registos do Cofre de chaves](https://docs.microsoft.com/azure/key-vault/key-vault-logging)|Microsoft.KeyVault/vaults|AuditEvent|
|Azure Search|[Ativar e utilizar a análise de tráfego de pesquisa](https://docs.microsoft.com/azure/search/search-traffic-analytics)|Microsoft.Search/searchServices|OperationLogs|
|Azure Data Lake Store|[Registos de diagnóstico de acesso para o Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-diagnostic-logs)|Microsoft.DataLakeStore/accounts<br>Microsoft.DataLakeStore/accounts|Auditoria<br>Pedidos|
|Azure Data Lake Analytics|[Registos de diagnóstico de acesso para o Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-diagnostic-logs)|Microsoft.DataLakeAnalytics/accounts<br>Microsoft.DataLakeAnalytics/accounts|Auditoria<br>Pedidos|
|Azure Logic Apps|[Esquema de controlo personalizado B2B de Aplicações Lógicas](https://docs.microsoft.com/azure/logic-apps/logic-apps-track-integration-account-custom-tracking-schema)|Microsoft.Logic/workflows<br>Microsoft.Logic/integrationAccounts|WorkflowRuntime<br>IntegrationAccountTrackingEvents|
|Azure Batch|[Registos de diagnóstico do Azure Batch](https://docs.microsoft.com/azure/batch/batch-diagnostics)|Microsoft.Batch/batchAccounts|ServiceLog|
|Automatização do Azure|[Análise de registos para a automatização do Azure](https://docs.microsoft.com/azure/automation/automation-manage-send-joblogs-log-analytics)|Microsoft.Automation/automationAccounts<br>Microsoft.Automation/automationAccounts|JobLogs<br>JobStreams|
|Azure Event Hubs|[Registos de diagnóstico de Hubs de eventos](https://docs.microsoft.com/azure/event-hubs/event-hubs-diagnostic-logs)|Microsoft.EventHub/namespaces<br>Microsoft.EventHub/namespaces|ArchiveLogs<br>OperationalLogs|
|Azure Stream Analytics|[Registos de diagnóstico da tarefa](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-job-diagnostic-logs)|Microsoft.StreamAnalytics/streamingjobs<br>Microsoft.StreamAnalytics/streamingjobs|Execução<br>Criação de conteúdos|
|Service Bus do Azure|[Registos de diagnóstico do Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-diagnostic-logs)|Microsoft.ServiceBus/namespaces|OperationalLogs|

### <a name="azure-active-directory-reporting"></a>Relatórios do Azure Active Directory
Azure Active Directory (Azure AD) inclui a segurança, atividade e relatórios de auditoria para o diretório de um utilizador. O [relatório de auditoria do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) ajuda-o a identificar ações privilegiadas que ocorreram na instância do Azure AD do utilizador. Ações privilegiadas incluem alterações de elevação (por exemplo, a criação da função ou a reposição de palavra-passe), alterar as configurações da política (por exemplo, políticas de palavra-passe) ou as alterações à configuração do diretório (por exemplo, alterações às definições de Federação do domínio).

Os relatórios fornecem o registo de auditoria para o nome de evento, o utilizador que efetuou a ação, o recurso de destino afetado por alteração e a data e hora (em UTC). Os utilizadores podem obter a lista de eventos de auditoria para o Azure AD através do [portal do Azure](https://portal.azure.com/), conforme descrito nas [ver os registos de auditoria](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

Os relatórios incluídos estão listados na seguinte tabela:

| Relatórios de segurança | Relatórios de atividade | Relatórios de auditoria |
| :--------------- | :--------------- | :------------ |
|Inícios de sessão de fontes desconhecidas| Utilização da aplicação: resumo| Relatório de auditoria de diretório|
|Inícios de sessão após várias falhas|  Utilização da aplicação: detalhado||
|Inícios de sessão de várias localizações geográficas|    Dashboard de aplicações||
|Inícios de sessão de endereços IP com atividade suspeita|   Erros de aprovisionamento de contas||
|Atividades irregulares de início de sessão|    Dispositivos de utilizadores individuais||
|Inícios de sessão de dispositivos possivelmente infetados|   Atividade de utilizador individuais||
|Utilizadores com atividade anómala de início de sessão| Relatório de atividade de grupos||
||Relatório de registo de atividade de reposição de palavra-passe||
||Atividade de reposição de palavra-passe|||

Os dados nestes relatórios podem ser úteis para as suas aplicações, como sistemas Security Information and Event Management (SIEM), auditoria e ferramentas de business intelligence. As APIs dos relatórios do Azure AD proporcionam acesso programático aos dados através de um conjunto de APIs baseadas em REST. Pode chamar estas [APIs](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) de vários idiomas e as ferramentas de programação.

Eventos de relatório de auditoria do Azure AD são retidos durante 180 dias.

> [!Note]
> Para obter mais informações sobre a retenção de relatórios, consulte [políticas de retenção de relatórios do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention).

Se estiver interessado em reter os eventos de auditoria mais, utilize a API de relatórios para solicitar a regularmente [eventos de auditoria](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) para um arquivo de dados separada.

### <a name="virtual-machine-logs-that-use-azure-diagnostics"></a>Registos de máquina virtual que utilizam o diagnóstico do Azure
[Diagnóstico do Azure](https://docs.microsoft.com/azure/azure-diagnostics) é a capacidade no Azure que permite a recolha de dados de diagnóstico sobre uma aplicação implementada. Pode utilizar a extensão de diagnóstico de qualquer uma das várias origens. São atualmente suportados [funções relativos à web e de trabalho do serviço em nuvem do Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me).

![Registos de máquina virtual que utilizam o diagnóstico do Azure](./media/azure-log-audit/azure-log-audit-fig3.png)

### <a name="azure-virtual-machineshttpsazuremicrosoftcomdocumentationlearning-pathsvirtual-machines-that-are-running-microsoft-windows-and-service-fabrichttpsdocsmicrosoftcomazureservice-fabricservice-fabric-overview"></a>[Máquinas virtuais do Azure](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/) que estiver a executar o Microsoft Windows e [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview)

Pode ativar o diagnóstico do Azure numa máquina virtual efetuando qualquer um dos seguintes procedimentos:

* [Utilizar o Visual Studio para rastrear máquinas virtuais do Azure](https://docs.microsoft.com/azure/vs-azure-tools-debug-cloud-services-virtual-machines)

* [Configurar remotamente o diagnóstico do Azure numa máquina virtual do Azure](https://docs.microsoft.com/azure/virtual-machines-dotnet-diagnostics)

* [Utilize o PowerShell para configurar diagnósticos em máquinas virtuais do Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-ps-extensions-diagnostics?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

* [Criar uma máquina virtual do Windows com a monitorização e diagnóstico utilizando um modelo Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="storage-analytics"></a>Análise de Armazenamento
[Análise de armazenamento do Azure](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) os registos e fornece dados de métricas para uma conta de armazenamento. Pode utilizar estes dados para rastrear pedidos, analisar tendências de utilização e diagnosticar problemas relacionados com a sua conta de armazenamento. Registo de análise de armazenamento está disponível para o [serviços de armazenamento de tabelas do Azure, Azure fila e Blob do Azure](https://docs.microsoft.com/azure/storage/storage-introduction). Análise de armazenamento regista informações detalhadas sobre pedidos com êxito e a um serviço de armazenamento.

Pode utilizar estas informações para monitorizar os pedidos individuais e diagnosticar problemas com um serviço de armazenamento. Pedidos são registados numa base de melhor esforço. Entradas de registo são criadas apenas se não existirem pedidos efetuados relativamente ao ponto final de serviço. Por exemplo, se uma conta de armazenamento tem de atividade no respetivo ponto final do blob, mas não no respetivos pontos finais de tabelas ou filas, são criados apenas os registos que dizem respeito ao serviço de armazenamento de Blobs.

Para utilizar a análise de armazenamento, ativá-la individualmente para cada serviço que pretende monitorizar. Pode ativá-la no [portal do Azure](https://portal.azure.com/). Para obter mais informações, consulte [monitorizar uma conta de armazenamento no portal do Azure](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). Também pode ativar a análise de armazenamento através de programação através da API REST ou biblioteca de clientes. Utilize a operação definir propriedades de serviço para ativar a análise de armazenamento individualmente para cada serviço.

Os dados agregados são armazenados num blob bem conhecido (para o registo) e em tabelas bem conhecidas (para métricas), que pode aceder ao utilizar o serviço de armazenamento de BLOBs e APIs do serviço de armazenamento de tabela.

Análise de armazenamento tem um limite de 20 com vários terabytes (TB), a quantidade de dados armazenados, que é independentes do limite total para a sua conta de armazenamento. Todos os registos são armazenados no [blobs de blocos](https://docs.microsoft.com/azure/storage/storage-analytics) num contentor com o nome $logs, que é criado automaticamente ao ativar a análise de armazenamento para uma conta de armazenamento.

> [!Note]
> * Para obter mais informações sobre as políticas de retenção de dados e de faturação, consulte [faturação e de análise de armazenamento](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
> * Para mais informações sobre limites de conta de armazenamento, consulte [metas de desempenho e escalabilidade do Storage do Azure](https://docs.microsoft.com/azure/storage/storage-scalability-targets).

Análise de armazenamento regista os seguintes tipos de pedidos autenticados e anónimos:

| Autenticados  | Anónimo|
| :------------- | :-------------|
| Pedidos com êxito | Pedidos com êxito |
|Falha de pedidos, incluindo o tempo limite, limitação, rede, autorização e outros erros | Pedidos com uma assinatura de acesso partilhado, incluindo pedidos falhados e bem-sucedidas |
| Pedidos com uma assinatura de acesso partilhado, incluindo pedidos falhados e bem-sucedidas |Erros de tempo limite para o cliente e servidor |
|   Pedidos de dados de análise |    Pedidos falhados de GET com o código de erro 304 (não modificados) |
| Pedidos efetuados pelo armazenamento Analytics ela própria, tal como criação de registo ou eliminação, não são registados. Uma lista completa dos dados com sessão iniciada está documentada na [análise de armazenamento registadas operações e mensagens de estado](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) e [formato de registo de análise de armazenamento](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). | Todos os outros pedidos anónimos falhados não são registados. Uma lista completa dos dados com sessão iniciada está documentada na [análise de armazenamento registadas operações e mensagens de estado](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) e [formato de registo de análise de armazenamento](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |

### <a name="azure-networking-logs"></a>Registos de rede do Azure
Rede de registo e monitorização no Azure é abrangente e abrange duas amplas categorias:

* [Observador de rede](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher): a monitorização de rede com base no cenário é fornecido com as funcionalidades do observador de rede. Este serviço inclui a captura de pacotes, o salto seguinte, o fluxo IP verificar, a vista de grupo de segurança, os registos de fluxo NSG. A monitorização ao nível do cenário fornece uma vista de ponto a ponto dos recursos de rede, ao contrário de monitorização de recursos de rede individuais.

* [Monitorização de recursos](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-resource-level-monitoring): monitorização de nível de recursos inclui funcionalidades de quatro, registos de diagnóstico, métricas, resolução de problemas e estado de funcionamento do recurso. Todas estas funcionalidades são criadas ao nível de recursos de rede.

![Registos de rede do Azure](./media/azure-log-audit/azure-log-audit-fig4.png)

Observador de rede é um serviço regional que lhe permite monitorizar e diagnosticar condições a um nível de cenário rede, para e do Azure. Diagnóstico de rede e ferramentas de visualização disponíveis com o observador de rede ajudam-na compreender, diagnosticar e obter informações sobre a sua rede no Azure.

### <a name="network-security-group-flow-logging"></a>Registo de fluxo do grupo de segurança de rede

[Os registos de fluxo NSG](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) são uma funcionalidade do observador de rede que pode utilizar para ver informações sobre o tráfego IP de entrada e de saída através de um NSG. Estes registos de fluxo são escritos no formato JSON e mostram:
* Fluxos de saída e entrados numa base por regras.
* O NIC que se aplica o fluxo.
* 5 cadeias de identificação informações sobre o fluxo: o IP de origem ou de destino, a porta de origem ou de destino e o protocolo.
* Indica se o tráfego foi permitido ou negado.

Apesar de os registos de fluxo de destino NSGs, não são apresentadas da mesma forma que os outros registos. Registos de fluxo são armazenados apenas dentro de uma conta de armazenamento.

As políticas de retenção mesmo que o se verificam no outros registos aplicam-se nos registos de fluxo. Os registos de tem uma política de retenção que pode definir do dia 1 a 365 dias. Se não definir uma política de retenção, os registos são mantidos para sempre.

**Registos de diagnóstico**

Eventos periódicos e spontaneous são criados pelos recursos de rede e registados em contas do storage e enviados para um hub de eventos ou análise de registos. Os registos de fornecem informações sobre o estado de funcionamento de um recurso. Estes podem ser visualizados no ferramentas como o Power BI e análise de registos. Para saber como ver registos de diagnóstico, consulte [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics).

![Registos de diagnósticos](./media/azure-log-audit/azure-log-audit-fig5.png)

Estão disponíveis para os registos de diagnóstico [Balanceador de carga](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [grupos de segurança de rede](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), rotas, e [Gateway de aplicação](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).

Observador de rede fornece que registos de diagnóstico de um vista. Esta vista contém todos os recursos de rede que suportam o registo de diagnóstico. Desta vista, pode ativar e desativar os recursos de rede rápida e convenientemente.


Além das funcionalidades mencionadas anteriormente de registo, o observador de rede atualmente tem as seguintes capacidades:
- [Topologia](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview): fornece uma vista de nível de rede que mostra as várias interconnections e as associações entre recursos de rede num grupo de recursos.

- [Captura de pacotes variável](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview): captura dados de pacote e para uma máquina virtual. Opções de filtragem e controlos fine-tuning, tais como definições de limitação de tempo e tamanho, avançadas fornecem versatility. Os dados de pacote podem ser armazenados num arquivo de blob ou no disco local no *.cap* formato de ficheiro.

* [Verificação de fluxo IP](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview): verifica se um pacote é permitido ou negado com base nos parâmetros de pacotes de 5 cadeias de identificação do fluxo informações (ou seja, IP de destino, IP de origem, porta de destino, porta de origem e protocolo). Se o pacote é negado por um grupo de segurança, a regra e o grupo negado o pacote é devolvido.

* [Próximo salto](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview): determina o salto seguinte para pacotes a ser encaminhados nos recursos de infraestrutura de rede do Azure, para que pode diagnosticar qualquer configurada incorretamente rotas definidas pelo utilizador.

* [Vista do grupo de segurança](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview): obtém as regras de segurança eficaz e aplicados que são aplicadas numa VM.

* [Gateway de rede virtual e a resolução de problemas de ligação](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest): ajuda a resolver problemas de ligações e gateways de rede virtual.

* [Limites de subscrição de rede](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-subscription-limits): permite-lhe ver utilização de recursos de rede contra os limites.

### <a name="application-insights"></a>Application Insights

[Azure Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) é um serviço APM extensível para programadores de web em várias plataformas. Utilizá-lo para monitorizar aplicações web em direto. Deteta automaticamente anomalias de desempenho. inclui ferramentas de análise poderosas para o ajudar a diagnosticar problemas e a compreender o que os utilizadores efetivamente fazem com a sua aplicação.

Application Insights foi concebido para ajudar a melhorar continuamente o desempenho e a facilidade de utilização.

Funciona para as aplicações numa ampla variedade de plataformas, incluindo .NET, Node.js e J2EE, se estiverem alojados no local ou na nuvem. Integra-se com o processo de DevOps e pontos de ligação com várias ferramentas de desenvolvimento.

![Diagrama do Application Insights](./media/azure-log-audit/azure-log-audit-fig6.png)

O Application Insights destina-se à equipa de programação, para o ajudar a compreender o desempenho da aplicação e como está a ser utilizada. Monitoriza:

* **Pedido de taxas, tempos de resposta e taxas de falha**: saber quais as páginas são mais populares, em que momento do dia, e onde são os seus utilizadores. Veja que páginas têm o melhor desempenho. Se os tempos de resposta e taxas de falha aceda elevadas quando existirem mais pedidos, poderá ter um problema resourcing.

* **Taxas de dependência, tempos de resposta e taxas de falha**: saber se os serviços externos são abrandamento.

* **Exceções**: analisar as estatísticas agregadas, ou escolha instâncias específicas e explorar o rastreio da pilha e os pedidos relacionados. São reportadas exceções do servidor e do browser.

* **Vistas de página e o desempenho da carga**: obter relatórios browsers dos seus utilizadores.

* **Chamadas AJAX**: obter taxas de página Web, tempos de resposta e taxas de falha.

* **Contagens de utilizadores e sessões**.

* **Contadores de desempenho**: obter dados das suas máquinas de servidor Windows ou Linux, tais como CPU, memória e a utilização de rede.

* **Diagnóstico de anfitrião**: obter dados a partir de Docker ou do Azure.

* **Registos de rastreio de diagnóstico**: obter dados da sua aplicação, para que pode correlacionar eventos de rastreio com pedidos.

* **Métricas e eventos personalizados**: obter os dados que escrever pessoalmente o código de cliente ou servidor, para controlar eventos empresariais, tais como vendidos ou jogos won.

A tabela seguinte lista e descreve cenários de integração:

| Cenário de integração | Descrição |
| --------------------- | :---------- |
|[Mapeamento de aplicações](https://docs.microsoft.com/azure/application-insights/app-insights-app-map)|Os componentes da sua aplicação, com as principais métricas e alertas.||
|[Diagnóstico de pesquisa para a instância dados](https://docs.microsoft.com/azure/application-insights/app-insights-diagnostic-search)| Procure e filtre eventos como pedidos, exceções, chamadas de dependências, rastreios de registo e visualizações de página.||
|[Explorador de métricas para os dados agregados](https://docs.microsoft.com/azure/application-insights/app-insights-metrics-explorer)|Explore, filtre e segmente dados agregados, como taxas de pedidos, falhas e exceções, tempos de resposta e tempos de carregamento de páginas.||
|[Dashboards](https://docs.microsoft.com/azure/application-insights/app-insights-dashboards#dashboards)|Combine dados de vários recursos e partilhe-os com outras pessoas. Ideais para aplicações com vários componentes e para visualização contínua na sala de equipa.||
|[Transmissão de métricas em direto](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream)|Quando implementa uma compilação nova, veja estes indicadores de desempenho em tempo quase real, para ter a certeza de que está tudo a funcionar conforme esperado.||
|[Análise](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)|Responda a perguntas difíceis sobre o desempenho e a utilização da sua aplicação através desta poderosa linguagem de consultas.||
|[Manuais e automáticas de alertas](https://docs.microsoft.com/azure/application-insights/app-insights-alerts)|Alertas automáticas se adaptam a padrões normal da sua aplicação de telemetria e são acionadas quando há algo fora o padrão normal. Também pode definir alertas em níveis específicos de métricas personalizadas ou padrão.||
|[Visual Studio](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio)|Ver dados de desempenho no código. Aceda ao código a partir dos rastreios de pilha.||
|[Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi)|Integre métricas de utilização com outras métricas de business intelligence.||
|[API REST](https://dev.applicationinsights.io/)|Escreva código para executar consultas nas métricas e nos dados não processados.||
|[Exportação contínua](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry)|Exportação em massa de dados não processados para o armazenamento quando este chega.||

### <a name="azure-security-center-alerts"></a>Alertas do Centro de segurança do Azure
A deteção de ameaças do Centro de segurança do Azure funciona através da recolha automática de informações de segurança dos seus recursos do Azure, rede e soluções de parceiros ligadas. Analisa esta informação, muitas vezes correlacionando informações de várias origens, para identificar ameaças. Os alertas de segurança são prioritários no Centro de Segurança, juntamente com recomendações sobre como remediar a ameaça. Para obter mais informações, consulte [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro).

![Diagrama de centro de segurança do Azure](./media/azure-log-audit/azure-log-audit-fig7.png)

O Centro de Segurança emprega análises de segurança avançadas, que ultrapassam as abordagens baseadas na assinatura. Aplica-se avanços em dados de grandes dimensões e [machine learning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) tecnologias para avaliar eventos em recursos de infraestrutura de nuvem de todo. Desta forma, detetar ameaças que seriam impossíveis de identificar através de abordagens manuais e previsão da evolução de ataques. Estas análises de segurança incluem:

* **Ameaças integradas**: procura mal-intencionadas conhecidas aplicando ameaças globais de produtos da Microsoft e serviços, a unidade Microsoft Crimes digitais (DCU), o Microsoft Security Response Center (MSRC) e de feeds externos.

* **A análise comportamental**: aplica padrões conhecidos para detetar um comportamento malicioso.

* **Deteção de anomalias**: utiliza a criação de perfis estatística para criar uma linha de base histórica. Alerta sobre os desvios de linhas de base estabelecidos em conformidade com um potencial vetor de ataque.

Muitas operações de segurança e as equipas de resposta a incidentes dependem de uma solução SIEM como ponto de partida para a triagem e investigar alertas de segurança. Com a integração de registo do Azure, pode sincronizar alertas do Centro de segurança e eventos de segurança de máquina virtual, recolhidos por registos de diagnóstico e de auditoria do Azure, com a sua solução de análise de registos ou SIEM em tempo real.

## <a name="log-analytics"></a>Log Analytics 

Análise de registos é um serviço no Azure ajuda-o a recolher e analisar os dados que são gerados pelo recursos na sua nuvem e no local ambientes. Proporciona informações em tempo real, utilizando a pesquisa integrada e dashboards personalizados para analisar prontamente milhões de registos em todas as suas cargas de trabalho e servidores, independentemente da respetiva localização física.

![Diagrama de análise do registo](./media/azure-log-audit/azure-log-audit-fig8.png)

No centro do Log Analytics é a área de trabalho de análise de registos, que está alojada no Azure. Análise de registos recolhe dados na área de trabalho do origens ligadas ao configurar origens de dados e adicionar soluções à sua subscrição. Origens de dados e soluções de cada criar diferentes tipos, cada um com o seu próprio conjunto de propriedades de registo. Mas origens e soluções podem ser analisadas em conjunto nas consultas para a área de trabalho. Esta capacidade permite-lhe utilizar as mesmas ferramentas e métodos para trabalhar com uma variedade de dados recolhidos por uma variedade de origens.

Ligado origens são os computadores e outros recursos que geram os dados recolhidos pelo registo de análise. Origens podem incluir os agentes que estão instalados no [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) e [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) computadores que estabelecem ligação diretamente ou agentes num [um grupo de gestão do System Center Operations Managerligado](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents). Análise de registos também pode recolher dados a partir de um [conta do storage do Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage).

[Origens de dados](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources) vários tipos de dados que são recolhidos a partir de cada origem ligada. Origens de incluem eventos e [dados de desempenho](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-performance-counters) de [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) e agentes do Linux, para além das origens, tal como [registos de IIS](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs) e [registos de texto personalizado](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-custom-logs). Pode configurar cada origem de dados que pretende recolher e a configuração é entregue automaticamente a cada origem ligada.

Existem quatro formas de [recolher registos e as métricas para serviços do Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage):
* Diagnóstico do Azure diretamente ao Log Analytics (**diagnóstico** na tabela seguinte)

* Diagnóstico do Azure para armazenamento do Azure ao Log Analytics (**armazenamento** na tabela seguinte)

* Os conectores para serviços do Azure (**conector** na tabela seguinte)

* Scripts para recolher e, em seguida, colocar dados no Log Analytics (células em branco na seguinte tabela e a serviços não listados)

| Serviço | Tipo de recurso | Registos | Métricas | Solução |
| :------ | :------------ | :--- | :------ | :------- |
|Gateway de Aplicação do Azure| Microsoft.Network/<br>applicationGateways|  Diagnóstico|Diagnóstico|    [Aplicação Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics) [análise de Gateway](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics)|
|Application Insights||     Conector|  Conector|  [Application Insights](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) [conector (pré-visualização)](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)|
|Contas de automatização do Azure| Microsoft.Automation/<br>AutomationAccounts|    Diagnóstico||       [Obter mais informações](https://docs.microsoft.com/azure/automation/automation-manage-send-joblogs-log-analytics)|
|Contas de lote do Azure|  Microsoft.Batch/<br>batchAccounts|  Diagnóstico|    Diagnóstico||
|Serviços cloud clássico||       Armazenamento||       [Obter mais informações](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage-iis-table)|
|Serviços Cognitivos|    Microsoft.CognitiveServices/<br>accounts|       Diagnóstico|||
|Azure Data Lake Analytics| Microsoft.DataLakeAnalytics/<br>accounts|   Diagnóstico|||
|Azure Data Lake Store| Microsoft.DataLakeStore/<br>accounts|   Diagnóstico|||
|Espaço de nomes de Hub de eventos do Azure| Microsoft.EventHub/<br>espaços de nomes|  Diagnóstico|    Diagnóstico||
|Azure IoT Hub| Microsoft.Devices/<br>IotHubs||     Diagnóstico||
|Azure Key Vault|   Microsoft.KeyVault/<br>cofres|  Diagnóstico  || [Análise do Cofre de Chaves](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-key-vault)|
|Azure Load Balancer|   Microsoft.Network/<br>loadBalancers|    Diagnóstico|||
|Azure Logic Apps|  Microsoft.Logic/<br>Fluxos de trabalho|  Diagnóstico|    Diagnóstico||
||Microsoft.Logic/<br>integrationAccounts||||
|Grupos de Segurança de Rede|   Microsoft.Network/<br>networksecuritygroups|Diagnóstico||   [Análise de grupo de segurança de rede do Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics#azure-network-security-group-analytics-solution-in-log-analytics)|
|Cofres de recuperação|   Microsoft.RecoveryServices/<br>cofres|||[O Azure Recovery Services análise (pré-visualização)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
|Procurar serviços|   Microsoft.Search/<br>searchServices|    Diagnóstico|    Diagnóstico||
|Espaço de nomes do Service Bus| Microsoft.ServiceBus/<br>espaços de nomes|    Diagnóstico|Diagnóstico|    [Análise de barramento de serviço (pré-visualização)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
|Service Fabric||       Armazenamento||    [Análise de recursos de infraestrutura de serviço (pré-visualização)](https://docs.microsoft.com/azure/log-analytics/log-analytics-service-fabric)|
|SQL (v12)| Microsoft.Sql/<br>servidores /<br>bases de dados||       Diagnóstico||
||Microsoft.Sql/<br>servidores /<br>elasticPools||||
|Armazenamento|||         Script| [Análise de armazenamento do Azure (pré-visualização)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution)|
|Máquinas Virtuais do Azure|    Microsoft.Compute/<br>virtualMachines|  Extensão|  Extensão||
||||Diagnóstico||
|Conjuntos de dimensionamento de máquinas virtuais|    Microsoft.Compute/<br>virtualMachines    ||Diagnóstico||
||Microsoft.Compute/<br>virtualMachineScaleSets/<br>virtualMachines||||
|Farms de servidores Web|Microsoft.Web/<br>serverfarms||   Diagnóstico
|Sites|  Microsoft.Web/<br>sites ||      Diagnóstico|    [Obter mais informações](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webappazure-oms-monitoring)|
||Microsoft.Web/<br>sites /<br>ranhuras|||||


## <a name="log-integration-with-on-premises-siem-systems"></a>Integração de registo com sistemas SIEM no local
Com [a integração de registo do Azure](https://www.microsoft.com/download/details.aspx?id=53324), pode integrar registos não processados a partir dos seus recursos do Azure com o sistema SIEM no local.

![Diagrama de integração de registo](./media/azure-log-audit/azure-log-audit-fig9.png)

Integração de registo recolhe diagnóstico do Azure do seu máquinas virtuais do Windows, os registos de atividade do Azure, alertas do Centro de segurança do Azure e os registos do fornecedor de recursos do Azure. Esta integração proporciona um dashboard unificado para todos os elementos, se de que estão no local ou na nuvem, para que pode agregar, correlacionar, analisar e alerta para eventos de segurança.

Integração de registo atualmente suporta a integração de registos de atividade do Azure, registos de auditoria de registos de eventos do Windows provenientes de máquinas de virtuais do Windows com a sua subscrição do Azure, alertas do Centro de segurança do Azure, os registos de diagnóstico do Azure e do Azure AD.

| Tipo de registo | Análise de registos que suportam JSON (Splunk, ArcSight e IBM QRadar) |
| :------- | :-------------------------------------------------------- |
|Os registos de auditoria do Azure AD|   Sim|
|Registos de atividade| Sim|
|Alertas do Centro de segurança |Sim|
|Registos de diagnóstico (registos de recursos)|  Sim|
|Registos VM|   Sim, através de eventos reencaminhados e não através de JSON|

[Introdução ao Azure registo integração](https://docs.microsoft.com/azure/security/security-azure-log-integration-get-started): Este tutorial explica-lhe instalar a integração de registo do Azure e integrar registos a partir do armazenamento do Azure, os registos de atividade do Azure, alertas do Centro de segurança do Azure e o Azure AD registos de auditoria.

Cenários de integração para o SIEM:

* [Passos de configuração do parceiro](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): Este blogue mostra-lhe como configurar a integração de registo do Azure para trabalhar com soluções de parceiros Splunk, HP ArcSight e IBM QRadar.

* [FAQ sobre integração do registo do Azure](https://docs.microsoft.com/azure/security/security-azure-log-integration-faq): Este artigo responde a questões sobre a integração de registo do Azure.

* [Integração de alertas do Centro de segurança com a integração de registo do Azure](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration): Este artigo descreve como sincronizar alertas do Centro de segurança, eventos de segurança de máquina virtual recolhidos por registos de diagnóstico do Azure, e os registos de auditoria do Azure com a análise de registos ou SIEM solução.

## <a name="next-steps"></a>Passos Seguintes

- [Auditoria e registo](https://docs.microsoft.com/azure/security/security-management-and-monitoring-overview): proteger os dados, mantendo em simultâneo visibilidade e rapidamente a responder a alertas de segurança atempadamente.

- [Coleção de registo e o registo de auditoria de segurança no Azure](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/): impor estas definições para garantir que as instâncias do Azure estão a recolher os registos de auditoria e segurança corretos.

- [Configurar definições de auditoria para uma coleção de sites](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US): se for um administrador de coleção de sites, obter o histórico das ações de utilizadores individuais e o histórico de ações executadas durante um intervalo de datas específico. 

- [Procurar o registo de auditoria de segurança do Office 365 & conformidade Center](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US): Utilize a segurança do Office 365 e o Centro de conformidade para procurar o registo de auditoria unificada e ver a atividade de utilizador e administrador na organização do Office 365.


