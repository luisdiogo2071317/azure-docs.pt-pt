---
title: Suporte de etiqueta do Azure Resource Manager para recursos
description: Mostra os tipos de recursos do Azure suportam etiquetas. Fornece detalhes sobre todos os serviços do Azure.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 01/02/2019
ms.author: tomfitz
ms.openlocfilehash: 50ea7a2446b5560bd208b2da128fa877068ce452
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000296"
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
| addsservices | Não |
| aadsupportcases | Não | 
| agentes | Não | 
| anonymousapiusers | Não | 
| configuração | Não | 
| registos | Não | 
| relatórios | Não | 
| services | Não | 
| servicehealthmetrics | Não | 

## <a name="aks"></a>AKS
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| managedClusters | Sim | 

## <a name="analysis-services"></a>Analysis Services
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| servidores | Sim | 

## <a name="api-hubs"></a>Hubs de API
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| apiManagementAccounts | Não | 
| apiManagementAccounts/apis | Não | 
| apiManagementAccounts/connectionAcls | Não | 
| apiManagementAccounts/connectionProviders | Não | 
| apiManagementAccounts/connectionProviderAcls | Não | 
| apiManagementAccounts/ligações | Não | 

## <a name="api-management"></a>Gestão de API
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| serviço | Sim | 

## <a name="automation"></a>Automatização
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| AutomationAccounts | Sim | 
| automationAccounts/configurações | Sim | 
| automationAccounts/tarefas | Não | 
| automationAccounts/runbooks | Sim | 
| automationAccounts/softwareUpdateConfigurations | Não | 
| automationAccounts/webhooks | Não | 

## <a name="azure-database-for-mariadb"></a>Azure Database for MariaDB
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| servidores | Sim | 
| / configurações de servidores | Não |
| bases de dados/servidores | Não |
| servidores/firewallRules | Não |
| servidores/recoverableServers | Não | 
| servidores/securityAlertPolicies | Não |
| servidores/virtualNetworkRules | Não | 

## <a name="azure-database-for-mysql"></a>Base de Dados do Azure para MySQL
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| servidores | Sim | 
| / configurações de servidores | Não |
| bases de dados/servidores | Não |
| servidores/firewallRules | Não |
| servidores/recoverableServers | Não | 
| servidores/securityAlertPolicies | Não |
| servidores/virtualNetworkRules | Não | 

## <a name="azure-database-for-postgresql"></a>Base de Dados do Azure para PostgreSQL
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| servidores | Sim | 
| servidores/consultores | Não | 
| / configurações de servidores | Não |
| bases de dados/servidores | Não |
| servidores/firewallRules | Não |
| servidores/queryTexts | Não | 
| servidores/recoverableServers | Não | 
| servidores/securityAlertPolicies | Não |
| servidores/topQueryStatistics | Não | 
| servidores/virtualNetworkRules | Não | 
| servidores/waitStatistics | Não | 

## <a name="batch"></a>Batch
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| batchAccounts | Sim | 

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
| edgenodes | Não | 
| Perfis | Sim | 
| perfis/pontos finais | Sim | 
| perfis/pontos finais/customdomains | Não | 
| perfis/pontos finais/origens | Não | 
| validateProbe | Não | 

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
| redes virtuais/remoteVirtualNetworkPeeringProxies | Não | 
| redes virtuais/virtualNetworkPeerings | Não | 

## <a name="classic-storage"></a>Armazenamento clássico
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| storageAccounts/serviços | Não | 
| serviços/storageAccounts/diagnosticSettings | Não | 

## <a name="compute"></a>Computação
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| availabilitySets | Sim | 
| Discos | Sim | 
| imagens | Sim | 
| restorePointCollections | Sim | 
| restorePointCollections/restorePoints | Não | 
| sharedVMImages | Sim | 
| sharedVMImages/versões | Sim | 
| instantâneos | Sim | 
| virtualMachines | Sim | 
| virtualMachines/diagnosticSettings | Não | 
| virtualMachines/extensões | Sim | 
| virtualMachines/metricDefinitions | Não | 
| virtualMachineScaleSets | Sim | 
| virtualMachineScaleSets/extensões | Não | 
| virtualMachineScaleSets/networkInterfaces | Não | 
| virtualMachineScaleSets/publicIPAddresses | Não | 
| virtualMachineScaleSets/virtualMachines | Não | 
| virtualMachineScaleSets/virtualMachines/networkInterfaces | Não | 

## <a name="container"></a>Contentor
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| containerGroups | Sim | 

## <a name="container-instance"></a>Instância de Contentor
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| containerGroups | Sim | 
| serviceAssociationLinks | Não | 

## <a name="container-registry"></a>Registo de Contentor
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| registos | Sim | 
| replicações/registos | Sim |
| registos/tarefas | Sim |
| registos/webhooks | Sim |

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

## <a name="data-box"></a>Data Box
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| tarefas | Sim | 

## <a name="data-box-edge"></a>Data Box Edge
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| DataBoxEdgeDevices | Sim | 

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
| dataFactories/diagnosticSettings | Não | 
| dataFactories/metricDefinitions | Não | 
| dataFactorySchema | Não | 
| fábricas | Sim | 
| fábricas/integrationRuntimes | Não | 

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
| laboratórios/artifactsources | Sim |
| laboratórios/custos | Sim |
| laboratórios/customimages | Sim |
| laboratórios/fórmulas | Sim |
| laboratórios/notificationchannels | Sim |
| laboratórios/policysets/políticas | Sim |
| laboratórios/agendas | Sim |
| laboratórios/serviceRunners | Sim | 
| laboratórios/utilizadores | Sim |
| laboratórios/utilizadores/discos | Sim |
| laboratórios/utilizadores/ambientes | Sim |
| laboratórios/utilizadores/segredos | Sim |
| laboratórios/utilizadores/servicefabrics | Sim |
| laboratórios/utilizadores/servicefabrics/agendas | Sim |
| laboratórios/virtualMachines | Sim | 
| laboratórios/virtualmachines/agendas | Sim |
| laboratórios/redes virtuais | Sim |
| Agendas | Sim | 

## <a name="dynamics-lcs"></a>Dynamics LCS
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| lcsprojects | Não | 
| lcsprojects/conectores | Não | 
| lcsprojects/clouddeployments | Não | 

## <a name="event-grid"></a>Event Grid
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| domínios | Sim | 
| domínios/tópicos | Não | 
| eventSubscriptions | Não | 
| extensionTopics | Não | 
| Tópicos | Sim | 
| topicTypes | Não | 

## <a name="event-hub"></a>Hub de Eventos
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| Clusters | Sim | 
| espaços de nomes | Sim | 
| espaços de nomes/AuthorizationRules | Não |
| espaços de nomes/disasterRecoveryConfigs | Não |
| espaços de nomes/eventhubs | Não |
| espaços de nomes/eventhubs/authorizationRules | Não |
| espaços de nomes/eventhubs/consumergroups | Não |

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
| actionGroups | Sim |
| activityLogAlerts | Sim |
| alertrules | Sim |
| automatedExportSettings | Não | 
| Componentes | Sim | 
| componentes/eventos | Não | 
| componentes/métricas | Não | 
| componentes/pricingPlans | Não | 
| componentes/consulta | Não | 
| registos | Não | 
| metricAlerts | Sim |
| migrateToNewPricingModel | Não | 
| myWorkbooks | Não | 
| consultas de | Não | 
| rollbackToLegacyPricingModel | Não | 
| scheduledqueryrules | Sim | 
| testes Web | Sim | 
| pastas de trabalho | Sim | 

## <a name="key-vault"></a>Cofre de Chaves
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| deletedVaults | Não | 
| cofres | Sim | 
| cofres/accessPolicies | Não | 
| Cofres/segredos | Não | 

## <a name="log-analytics"></a>Log Analytics
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| registos | Não | 

## <a name="logic"></a>Lógica
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| integrationAccounts | Sim | 
| Fluxos de trabalho | Sim | 

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
| mediaservices/accountFilters | Não | 
| mediaservices/ativos | Não | 
| mediaservices/ativos/assetFilters | Não | 
| mediaservices/contentKeyPolicies | Não | 
| mediaservices/eventGridFilters | Não | 
| mediaservices/liveEventOperations | Não | 
| mediaservices/liveEvents | Sim | 
| mediaservices/liveEvents/liveOutputs | Não | 
| mediaservices/liveOutputOperations | Não | 
| mediaservices/pontos finais | Sim | 
| mediaservices/streamingEndpointOperations | Não | 
| mediaservices/streamingLocators | Não | 
| mediaservices/streamingPolicies | Não | 
| mediaservices/transformações | Não | 
| mediaservices/transformações/tarefas | Não | 

## <a name="network"></a>Rede
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| applicationGateways | Sim | 
| applicationSecurityGroups | Sim | 
| azureFirewalls | Sim | 
| ligações | Sim | 
| ddosProtectionPlans | Sim | 
| expressRouteCircuits | Sim | 
| frontdoors | Sim | 
| frontdoorWebApplicationFirewallPolicies | Sim | 
| interfaceEndpoints | Sim | 
| balanceadores de carga | Sim | 
| localNetworkGateways | Sim | 
| networkIntentPolicies | Sim | 
| networkInterfaces | Sim | 
| networkProfiles | Sim | 
| networkSecurityGroups | Sim | 
| networkWatchers | Sim | 
| networkWatchers/connectionMonitors | Sim | 
| networkWatchers/lentes | Sim | 
| networkWatchers/pingMeshes | Sim | 
| privateLinkServices | Sim | 
| publicIPAddresses | Sim | 
| publicIPPrefixes | Sim | 
| routeFilters | Sim | 
| routeTables | Sim | 
| serviceEndpointPolicies | Sim | 
| virtualHubs | Sim | 
| redes virtuais | Sim | 
| virtualNetworkGateways | Sim | 
| virtualNetworkTaps | Sim | 
| virtualWans | Sim | 
| vpnGateways | Sim | 
| vpnSites | Sim | 
| webApplicationFirewallPolicies | Sim | 

## <a name="notification-hubs"></a>Hubs de Notificação
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| espaços de nomes | Sim | 
| espaços de nomes/notificationHubs | Sim | 

## <a name="operational-insights"></a>Informações Operacionais
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| áreas de trabalho | Sim |
| áreas de trabalho/origens de dados | Sim |
| áreas de trabalho/linkedServices | Sim |
| áreas de trabalho/savedSearches | Não |
| áreas de trabalho/storageInsightConfigs | Sim |

## <a name="operations-management"></a>Gestão de Operações
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| soluções | Não |

## <a name="portal"></a>Portal
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| Dashboards | Sim | 

## <a name="portal-sdk"></a>Portal de SDK
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| rootResources | Sim | 

## <a name="power-bi"></a>Power BI
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| workspaceCollections | Sim | 

## <a name="recovery-services"></a>Serviços de Recuperação
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| backupProtectedItems | Não | 
| cofres | Sim | 

## <a name="relay"></a>Reencaminhamento
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| espaços de nomes | Sim | 

## <a name="resources"></a>Recursos
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| resourceGroups | Sim | 
| subscrições/resourceGroups | Sim | 

## <a name="scheduler"></a>Scheduler
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| jobcollections | Sim | 
| fluxos | Sim | 

## <a name="search"></a>Pesquisa
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| resourceHealthMetadata | Não | 
| searchServices | Sim | 

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

## <a name="sql"></a>SQL
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| localizações/instanceFailoverGroups | Não |
| managedInstances | Sim |
| managedInstances/bases de dados | Sim |
| bases de dados/managedInstances/backupShortTermRetentionPolicies | Não |
| bases de dados/managedInstances/esquemas/colunas/tabelas/sensitivityLabels | Não |
| bases de dados/managedInstances/vulnerabilityAssessments | Não |
| managedInstances/bases de dados/vulnerabilityAssessments/regras/linhas de base | Não |
| managedInstances/encryptionProtector | Não |
| managedInstances/chaves | Não |
| managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Não |
| managedInstances/vulnerabilityAssessments | Não |
| servidores | Sim |
| servidores/administradores | Não |
| servidores/consultores | Não |
| servidores/auditingSettings | Não |
| servidores/backupLongTermRetentionVaults | Não |
| servidores/communicationLinks | Não |
| servidores/connectionPolicies | Não |
| bases de dados/servidores | Sim |
| bases de dados/servidores/consultores | Não |
| bases de dados/servidores/auditingSettings | Não |
| bases de dados/servidores/backupLongTermRetentionPolicies | Não |
| bases de dados/servidores/backupShortTermRetentionPolicies | Não |
| bases de dados/servidores/connectionPolicies | Não |
| bases de dados/servidores/dataMaskingPolicies | Não |
| servidores/bases de dados/dataMaskingPolicies/regras | Não |
| bases de dados/servidores/extendedAuditingSettings | Não |
| bases de dados/servidores/extensões | Não |
| bases de dados/servidores/geoBackupPolicies | Não |
| bases de dados/servidores/esquemas/colunas/tabelas/sensitivityLabels | Não |
| bases de dados/servidores/securityAlertPolicies | Não |
| bases de dados/servidores/syncGroups | Não |
| servidores/bases de dados/syncGroups/syncMembers | Não |
| bases de dados/servidores/transparentDataEncryption | Não |
| bases de dados/servidores/vulnerabilityAssessments | Não |
| servidores/bases de dados/vulnerabilityAssessments/regras/linhas de base | Não |
| servidores/disasterRecoveryConfiguration | Não |
| servidores/dnsAliases | Não |
| servidores/elasticPools | Sim |
| servidores/encryptionProtector | Não |
| servidores/extendedAuditingSettings | Não |
| servidores/failoverGroups | Sim |
| servidores/firewallRules | Não |
| servidores/jobAgents | Sim |
| servidores/jobAgents/credenciais | Não |
| servidores/jobAgents/tarefas | Não |
| servidores/jobAgents/tarefas/execuções | Não |
| servidores/jobAgents/tarefas/passos | Não |
| servidores/jobAgents/targetGroups | Não |
| servidores/chaves | Não |
| servidores/securityAlertPolicies | Não |
| servidores/syncAgents | Não |
| servidores/virtualNetworkRules | Não |
| servidores/vulnerabilityAssessments | Não |

## <a name="sql-virtual-machine"></a>Máquina virtual do SQL
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| DWVM | Sim | 

## <a name="storage"></a>Armazenamento
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| storageAccounts | Sim | 
| storageAccounts/blobServices | Não | 
| storageAccounts/fileServices | Não | 
| storageAccounts/queueServices | Não | 
| storageAccounts/serviços | Não | 
| serviços/storageAccounts/metricDefinitions | Não | 
| storageAccounts/tableServices | Não | 

## <a name="storage-sync"></a>Sincronização de armazenamento
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| storageSyncServices | Sim | 
| storageSyncServices/registeredServers | Não | 
| storageSyncServices/syncGroups | Não | 
| storageSyncServices/syncGroups/cloudEndpoints | Não | 
| storageSyncServices/syncGroups/serverEndpoints | Não | 
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
| extensão da conta / | Sim | 
| conta/projeto | Sim | 

## <a name="web"></a>Web
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| apiManagementAccounts | Não | 
| apiManagementAccounts/apiAcls | Não | 
| apiManagementAccounts/apis | Não | 
| apiManagementAccounts/apis/apiAcls | Não | 
| apiManagementAccounts/apis/connectionAcls | Não | 
| apiManagementAccounts/apis/ligações | Não | 
| apiManagementAccounts/apis/ligações/connectionAcls | Não | 
| apiManagementAccounts/apis/localizedDefinitions | Não | 
| apiManagementAccounts/connectionAcls | Não | 
| apiManagementAccounts/ligações | Não | 
| billingMeters | Não | 
| certificados | Sim | 
| connectionGateways | Sim | 
| ligações | Sim | 
| customApis | Sim | 
| deletedSites | Não | 
| functions | Não | 
| hostingEnvironments | Sim | 
| hostingEnvironments/métricas | Não | 
| hostingEnvironments/multiRolePools | Não | 
| hostingEnvironments/workerPools | Não | 
| publishingUsers | Não | 
| serverFarms | Sim | 
| serverFarms/funções de trabalho | Não | 
| Sites | Sim | 
| sites/domainOwnershipIdentifiers | Não | 
| sites/hostNameBindings | Não | 
| sites/instâncias | Não | 
| sites/instâncias/extensões | Não | 
| sites/métricas | Não | 
| sites/premieraddons | Sim | 
| sites/ranhuras | Sim | 
| sites/ranhuras/hostNameBindings | Não | 
| sites/ranhuras/instâncias | Não | 
| sites/ranhuras/instâncias/extensões | Não | 
| sites/ranhuras/métricas | Não | 
| sourceControls | Não | 
| Validar | Não | 
| verifyHostingEnvironmentVnet | Não | 

## <a name="xrm"></a>XRM
| Tipo de recurso | Suporta etiquetas |
| ------------- | ----------- |
| organizações | Não | 

## <a name="next-steps"></a>Passos Seguintes
Para saber como aplicar etiquetas a recursos, veja [utilizar etiquetas para organizar os recursos do Azure](resource-group-using-tags.md).
