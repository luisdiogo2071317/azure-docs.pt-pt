---
title: Suporte de etiqueta do Azure Resource Manager para recursos
description: Mostra os tipos de recursos do Azure suportam etiquetas. Fornece detalhes sobre todos os serviços do Azure.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 11/20/2018
ms.author: tomfitz
ms.openlocfilehash: 99bac69d096095f660e2624194b72e1390c6fc22
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52275569"
---
# <a name="tag-support-for-azure-resources"></a>Suporte a marcas para recursos do Azure
Este artigo descreve se um tipo de recurso suporta [marcação](resource-group-using-tags.md).

## <a name="aad-domain-services"></a>Serviços de domínio do AAD
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| domínios | Não | 

## <a name="ad-hybrid-health-service"></a>Serviço de estado de funcionamento do AD híbrido
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| services | Não | 
| addsservices | Não | 
| configuração | Não | 
| agentes | Não | 
| aadsupportcases | Não | 
| Relatórios | Não | 
| servicehealthmetrics | Não | 
| registos | Não | 
| anonymousapiusers | Não | 

## <a name="analysis-services"></a>Analysis Services
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| servidores | Sim | 

## <a name="api-hubs"></a>Hubs de API
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| apiManagementAccounts | Não | 
| apiManagementAccounts/connectionProviders | Não | 
| apiManagementAccounts/ligações | Não | 
| apiManagementAccounts/connectionAcls | Não | 
| apiManagementAccounts/connectionProviderAcls | Não | 
| apiManagementAccounts/apis | Não | 

## <a name="api-management"></a>Gestão de API
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| serviço | Sim | 

## <a name="automation"></a>Automatização
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| AutomationAccounts | Sim | 
| automationAccounts/runbooks | Sim | 
| automationAccounts/configurações | Sim | 
| automationAccounts/webhooks | Não | 
| automationAccounts/softwareUpdateConfigurations | Não | 
| automationAccounts/tarefas | Não | 

## <a name="batch"></a>Batch
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| batchAccounts | Sim | 

## <a name="batch-ai"></a>Batch AI
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| Clusters | Sim | 
| tarefas | Sim | 
| fileservers | Sim | 
| áreas de trabalho | Sim | 
| áreas de trabalho/clusters | Não | 
| áreas de trabalho/fileservers | Não | 
| áreas de trabalho/experimentações | Não | 
| áreas de trabalho/experimentações/tarefas | Não | 

## <a name="bing-maps"></a>Mapas Bing
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| mapApis | Sim | 

## <a name="biztalk-services"></a>Serviços Biztalk
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| BizTalk | Sim | 

## <a name="cache"></a>Cache
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| Redis | Sim | 

## <a name="cdn"></a>CDN
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| Perfis | Sim | 
| perfis/pontos finais | Sim | 
| perfis/pontos finais/origens | Não | 
| perfis/pontos finais/customdomains | Não | 
| validateProbe | Não | 
| edgenodes | Não | 

## <a name="classic-compute"></a>Computação Clássica
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| domainNames | Não | 
| domainNames/ranhuras | Não | 
| domainNames/ranhuras/funções | Não | 
| virtualMachines | Não | 
| virtualMachines/diagnosticSettings | Não | 
| virtualMachines/metricDefinitions | Não | 
| virtualMachines/métricas | Não | 

## <a name="classic-infrastructure-migrate"></a>Migrar de clássica infraestrutura
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| classicInfrastructureResources | Não | 

## <a name="classic-network"></a>Rede clássica
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| redes virtuais | Não | 
| redes virtuais/virtualNetworkPeerings | Não | 
| redes virtuais/remoteVirtualNetworkPeeringProxies | Não | 

## <a name="classic-storage"></a>Armazenamento clássico
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| storageAccounts/serviços | Não | 
| serviços/storageAccounts/diagnosticSettings | Não | 

## <a name="compute"></a>Computação
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| availabilitySets | Sim | 
| virtualMachines | Sim | 
| virtualMachines/extensões | Sim | 
| virtualMachineScaleSets | Sim | 
| virtualMachineScaleSets/extensões | Não | 
| virtualMachineScaleSets/virtualMachines | Não | 
| virtualMachineScaleSets/networkInterfaces | Não | 
| virtualMachineScaleSets/virtualMachines/networkInterfaces | Não | 
| virtualMachineScaleSets/publicIPAddresses | Não | 
| restorePointCollections | Sim | 
| restorePointCollections/restorePoints | Não | 
| virtualMachines/diagnosticSettings | Não | 
| virtualMachines/metricDefinitions | Não | 
| sharedVMImages | Sim | 
| sharedVMImages/versões | Sim | 
| Discos | Sim | 
| instantâneos | Sim | 
| imagens | Sim | 

## <a name="container"></a>Contentor
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| containerGroups | Sim | 

## <a name="container-instance"></a>Instância de Contentor
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| containerGroups | Sim | 
| serviceAssociationLinks | Não | 

## <a name="container-service"></a>Serviço de Contentor
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| containerServices | Sim | 

## <a name="cortana-analytics"></a>Cortana Analytics
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| accounts | Sim | 

## <a name="cosmos-db"></a>BD do Cosmos
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| databaseAccounts | Sim | 
| databaseAccountNames | Não | 

## <a name="cost-management"></a>Gestão de Custos
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| Conectores | Sim | 

## <a name="data-box-edge"></a>Data Box Edge
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| DataBoxEdgeDevices | Sim | 
| DataBoxEdgeDevices/checkNameAvailability | Não | 

## <a name="data-catalog"></a>Catálogo de Dados
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| catálogos | Sim | 

## <a name="data-connect"></a>Ligar dados
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| connectionManagers | Sim | 

## <a name="data-factory"></a>Data Factory
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| dataFactories | Sim | 
| fábricas | Sim | 
| fábricas/integrationRuntimes | Não | 
| dataFactories/diagnosticSettings | Não | 
| dataFactories/metricDefinitions | Não | 
| dataFactorySchema | Não | 

## <a name="devices"></a>Dispositivos
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| IotHubs | Sim | 
| IotHubs/eventGridFilters | Não | 
| ProvisioningServices | Sim | 

## <a name="devspaces"></a>Devspaces
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| Controladores | Sim | 

## <a name="devtest-lab"></a>Laboratório DevTest
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| Laboratórios | Sim | 
| Agendas | Sim | 
| laboratórios/virtualMachines | Sim | 
| laboratórios/serviceRunners | Sim | 

## <a name="dynamics-lcs"></a>Dynamics LCS
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| lcsprojects | Não | 
| lcsprojects/conectores | Não | 
| lcsprojects/clouddeployments | Não | 

## <a name="event-grid"></a>Event Grid
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| eventSubscriptions | Não | 
| Tópicos | Sim | 
| domínios | Sim | 
| domínios/tópicos | Não | 
| topicTypes | Não | 
| extensionTopics | Não | 

## <a name="event-hub"></a>Hub de Eventos
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| espaços de nomes | Sim | 
| Clusters | Sim | 

## <a name="hana-on-azure"></a>Hana no Azure
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| hanaInstances | Sim | 

## <a name="hdinsight"></a>HDInsight
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| Clusters | Sim | 
| clusters/aplicações | Não | 

## <a name="import-export"></a>Importação e Exportação
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| tarefas | Sim | 

## <a name="insights"></a>Informações
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| Componentes | Sim | 
| componentes/consulta | Não | 
| componentes/métricas | Não | 
| componentes/eventos | Não | 
| testes Web | Sim | 
| consultas de | Não | 
| scheduledqueryrules | Sim | 
| componentes/pricingPlans | Não | 
| migrateToNewPricingModel | Não | 
| rollbackToLegacyPricingModel | Não | 
| automatedExportSettings | Não | 
| pastas de trabalho | Sim | 
| myWorkbooks | Não | 
| registos | Não | 

## <a name="key-vault"></a>Cofre de Chaves
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| cofres | Sim | 
| Cofres/segredos | Não | 
| cofres/accessPolicies | Não | 
| deletedVaults | Não | 

## <a name="log-analytics"></a>Log Analytics
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| registos | Não | 

## <a name="logic"></a>Lógica
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| Fluxos de trabalho | Sim | 
| integrationAccounts | Sim | 

## <a name="machine-learning-services"></a>Machine Learning Services
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| áreas de trabalho | Sim | 
| áreas de trabalho/computações | Não | 

## <a name="managed-identity"></a>Identidade Gerida
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| Identidades | Não | 
| userAssignedIdentities | Sim | 

## <a name="mariadb"></a>MariaDB
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| servidores | Sim | 
| servidores/recoverableServers | Não | 
| servidores/virtualNetworkRules | Não | 

## <a name="marketplace-apps"></a>Aplicações do Marketplace
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| classicDevServices | Sim | 

## <a name="marketplace-ordering"></a>Ordenação de Marketplace
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| Contratos | Não | 
| offertypes | Não | 

## <a name="media"></a>Meios de comunicação social
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| mediaservices | Sim | 
| mediaservices/ativos | Não | 
| mediaservices/contentKeyPolicies | Não | 
| mediaservices/streamingLocators | Não | 
| mediaservices/streamingPolicies | Não | 
| mediaservices/eventGridFilters | Não | 
| mediaservices/transformações | Não | 
| mediaservices/transformações/tarefas | Não | 
| mediaservices/pontos finais | Sim | 
| mediaservices/liveEvents | Sim | 
| mediaservices/liveEvents/liveOutputs | Não | 
| mediaservices/streamingEndpointOperations | Não | 
| mediaservices/liveEventOperations | Não | 
| mediaservices/liveOutputOperations | Não | 
| mediaservices/ativos/assetFilters | Não | 
| mediaservices/accountFilters | Não | 

## <a name="mysql"></a>MySQL
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| servidores | Sim | 
| servidores/recoverableServers | Não | 
| servidores/virtualNetworkRules | Não | 

## <a name="network"></a>Rede
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| redes virtuais | Sim | 
| publicIPAddresses | Sim | 
| networkInterfaces | Sim | 
| interfaceEndpoints | Sim | 
| balanceadores de carga | Sim | 
| networkSecurityGroups | Sim | 
| applicationSecurityGroups | Sim | 
| serviceEndpointPolicies | Sim | 
| networkIntentPolicies | Sim | 
| routeTables | Sim | 
| publicIPPrefixes | Sim | 
| networkWatchers | Sim | 
| networkWatchers/connectionMonitors | Sim | 
| networkWatchers/lentes | Sim | 
| networkWatchers/pingMeshes | Sim | 
| virtualNetworkGateways | Sim | 
| localNetworkGateways | Sim | 
| ligações | Sim | 
| applicationGateways | Sim | 
| expressRouteCircuits | Sim | 
| routeFilters | Sim | 
| virtualWans | Sim | 
| vpnSites | Sim | 
| virtualHubs | Sim | 
| vpnGateways | Sim | 
| azureFirewalls | Sim | 
| virtualNetworkTaps | Sim | 
| privateLinkServices | Sim | 
| ddosProtectionPlans | Sim | 
| networkProfiles | Sim | 
| frontdoors | Sim | 
| frontdoorWebApplicationFirewallPolicies | Sim | 
| webApplicationFirewallPolicies | Sim | 

## <a name="notification-hubs"></a>Hubs de Notificação
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| espaços de nomes | Sim | 
| espaços de nomes/notificationHubs | Sim | 

## <a name="portal"></a>Portal
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| Dashboards | Sim | 

## <a name="portal-sdk"></a>Portal de SDK
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| rootResources | Sim | 

## <a name="postgresql"></a>PostgreSQL
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| servidores | Sim | 
| servidores/recoverableServers | Não | 
| servidores/virtualNetworkRules | Não | 
| servidores/topQueryStatistics | Não | 
| servidores/queryTexts | Não | 
| servidores/waitStatistics | Não | 
| servidores/consultores | Não | 

## <a name="power-bi"></a>Power BI
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| workspaceCollections | Sim | 

## <a name="recovery-services"></a>Serviços de Recuperação
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| cofres | Sim | 
| backupProtectedItems | Não | 

## <a name="relay"></a>Reencaminhamento
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| espaços de nomes | Sim | 

## <a name="resources"></a>Recursos
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| resourceGroups | Não | 
| subscrições/resourceGroups | Não | 

## <a name="scheduler"></a>Scheduler
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| jobcollections | Sim | 
| fluxos | Sim | 

## <a name="search"></a>Pesquisa
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| searchServices | Sim | 
| resourceHealthMetadata | Não | 

## <a name="security"></a>Segurança
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| dataCollectionAgents | Não | 

## <a name="service-bus"></a>Service Bus
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| espaços de nomes | Sim | 
| espaços de nomes/eventgridfilters | Não | 

## <a name="service-fabric"></a>Service Fabric
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| Clusters | Sim | 
| clusters/aplicações | Não | 

## <a name="service-fabric-mesh"></a>Service Fabric Mesh
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| aplicações | Sim | 
| Redes | Sim | 
| volumes | Sim | 

## <a name="signalr-service"></a>Serviço SignalR
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| SignalR | Sim | 

## <a name="site-recovery"></a>Site Recovery
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| SiteRecoveryVault | Sim | 

## <a name="solutions"></a>Soluções
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| aplicações | Sim | 
| applicationDefinitions | Sim | 
| jitRequests | Sim | 

## <a name="sql-virtual-machine"></a>Máquina virtual do SQL
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| DWVM | Sim | 

## <a name="storage"></a>Armazenamento
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| storageAccounts | Sim | 
| storageAccounts/blobServices | Não | 
| storageAccounts/tableServices | Não | 
| storageAccounts/queueServices | Não | 
| storageAccounts/fileServices | Não | 
| storageAccounts/serviços | Não | 
| serviços/storageAccounts/metricDefinitions | Não | 

## <a name="storage-sync"></a>Sincronização de armazenamento
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| storageSyncServices | Sim | 
| storageSyncServices/syncGroups | Não | 
| storageSyncServices/syncGroups/cloudEndpoints | Não | 
| storageSyncServices/syncGroups/serverEndpoints | Não | 
| storageSyncServices/registeredServers | Não | 
| storageSyncServices/fluxos de trabalho | Não | 

## <a name="storsimple"></a>Storsimple
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| gerentes | Sim | 

## <a name="stream-analytics"></a>Stream Analytics
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| streamingjobs | Sim | 
| streamingjobs/diagnosticSettings | Não | 
| streamingjobs/metricDefinitions | Não | 

## <a name="subscription"></a>Subscrição
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| SubscriptionDefinitions | Não | 
| SubscriptionOperations | Não | 

## <a name="support"></a>Suporte
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| supporttickets | Não | 

## <a name="visual-studio"></a>Visual Studio
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| conta | Sim | 
| conta/projeto | Sim | 
| extensão da conta / | Sim | 
| conta | Sim | 
| conta/projeto | Sim | 
| extensão da conta / | Sim | 

## <a name="web"></a>Web
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| sites/instâncias | Não | 
| sites/ranhuras/instâncias | Não | 
| sites/instâncias/extensões | Não | 
| sites/ranhuras/instâncias/extensões | Não | 
| publishingUsers | Não | 
| Validar | Não | 
| sourceControls | Não | 
| sites/hostNameBindings | Não | 
| sites/domainOwnershipIdentifiers | Não | 
| sites/ranhuras/hostNameBindings | Não | 
| certificados | Sim | 
| serverFarms | Sim | 
| serverFarms/funções de trabalho | Não | 
| Sites | Sim | 
| sites/ranhuras | Sim | 
| sites/métricas | Não | 
| sites/ranhuras/métricas | Não | 
| sites/premieraddons | Sim | 
| hostingEnvironments | Sim | 
| hostingEnvironments/multiRolePools | Não | 
| hostingEnvironments/workerPools | Não | 
| hostingEnvironments/métricas | Não | 
| functions | Não | 
| deletedSites | Não | 
| apiManagementAccounts | Não | 
| apiManagementAccounts/ligações | Não | 
| apiManagementAccounts/connectionAcls | Não | 
| apiManagementAccounts/apis/ligações/connectionAcls | Não | 
| apiManagementAccounts/apis/connectionAcls | Não | 
| apiManagementAccounts/apiAcls | Não | 
| apiManagementAccounts/apis/apiAcls | Não | 
| apiManagementAccounts/apis | Não | 
| apiManagementAccounts/apis/localizedDefinitions | Não | 
| apiManagementAccounts/apis/ligações | Não | 
| ligações | Sim | 
| customApis | Sim | 
| connectionGateways | Sim | 
| billingMeters | Não | 
| verifyHostingEnvironmentVnet | Não | 

## <a name="xrm"></a>XRM
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| organizações | Não | 

## <a name="next-steps"></a>Passos Seguintes
Para saber como aplicar etiquetas a recursos, veja [utilizar etiquetas para organizar os recursos do Azure](resource-group-using-tags.md).
