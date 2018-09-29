---
title: Mover o suporte de operação por tipo de recurso do Azure | Documentos da Microsoft
description: Lista os tipos de recursos do Azure que podem ser movidos para um novo grupo de recursos ou subscrição.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/28/2018
ms.author: tomfitz
ms.openlocfilehash: 5bb820d816115dccf470b6c32d080862495e8310
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434981"
---
# <a name="move-operation-support-for-resources"></a>Mover o suporte de operação para recursos

Este artigo lista se um tipo de recurso do Azure suporta a operação de movimentação. Embora um tipo de recurso suporta a operação de movimentação, poderá haver condições que impedem que o recurso a ser movido. Para obter detalhes sobre as condições que afetem as operações de movimentação, consulte [mover recursos para um novo grupo de recursos ou subscrição](resource-group-move-resources.md).

## <a name="find-resource-provider-and-resource-type"></a>Encontrar o fornecedor de recursos e o tipo de recurso

Para determinar se um recurso pode ser movido, tem de encontrar o seu fornecedor de recursos e o tipo de recurso.

Para o PowerShell, utilize:

```azurepowershell-interactive
Get-AzureRmResource -ResourceGroupName demogroup | Select Name, ResourceType | Format-table
```

Para a CLI do Azure, utilize:

```azurecli-interactive
az resource list -g demogroup --query '[].{name:name, reourcetype:type}'
```

O tipo de recurso é devolvido no formato `<resource-provider>/<resource-type-name>`. Para isso, o valor `Microsoft.OperationalInsights/workspaces` tem um fornecedor de recursos de **Microsoft.OperationalInsights** e o nome de tipo de recurso de **áreas de trabalho**.

Depois de encontrar o fornecedor de recursos e o tipo de recurso, utilize as tabelas neste artigo para determinar se o tipo de recurso suporta a operação de movimentação.

## <a name="microsoftaad"></a>Microsoft.AAD

| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | --------------- | ----------- |
| domainservices | Não | Não |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| servidores | Sim | Sim |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | --------------- | ----------- |
| serviço | Sim | Sim |

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | --------------- | ----------- |
| policyassignments | Não | Não |

## <a name="microsoftautomation"></a>Microsoft.Automation
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| automationaccounts | Sim | Sim |
| automationaccounts/configurações | Sim | Sim |
| automationaccounts/runbooks | Sim | Sim |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| b2cdirectories | Sim | Sim |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| registos | Sim | Sim |

## <a name="microsoftbackup"></a>Microsoft.Backup
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| backupvault | Não | Não |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| batchaccounts | Sim | Sim |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| Clusters | Não | Não |
| fileservers | Não | Não |
| tarefas | Não | Não |
| áreas de trabalho | Não | Não |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| mapapis | Não | Não |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| biztalk | Sim | Sim |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| blueprintassignments | Não | Não |

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| botservices | Sim | Sim |

## <a name="microsoftcache"></a>Microsoft.Cache
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| De redis | Sim | Sim |

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| Perfis | Sim | Sim |
| perfis/pontos finais | Sim | Sim |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| certificateorders | Sim | Sim |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| domainnames | Sim | Não |
| virtualmachines | Sim | Não |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| networksecuritygroups | Não | Não |
| reservedips | Não | Não |
| redes virtuais | Não | Não |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| storageaccounts | Sim | Não |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| accounts | Sim | Sim |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| availabilitysets | Sim | Sim |
| Discos | Sim | Sim |
| galerias | Não | Não |
| galerias/imagens | Não | Não |
| galerias/imagens/versões | Não | Não |
| imagens | Sim | Sim |
| restorepointcollections | Não | Não |
| sharedvmimages | Não | Não |
| sharedvmimages/versões | Não | Não |
| instantâneos | Sim | Sim |
| virtualmachines | Sim | Sim |
| virtualmachines/extensões | Sim | Sim |
| virtualmachinescalesets | Sim | Sim |

## <a name="microsoftcontainer"></a>Microsoft.Container
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| containergroups | Não | Não |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| containergroups | Não | Não |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| registos | Sim | Sim |
| registos/buildtasks | Sim | Sim |
| replicações/registos | Não | Não |
| registos/tarefas | Sim | Sim |
| registos/webhooks | Sim | Sim |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| containerservices | Não | Não |
| managedclusters | Não | Não |
| openshiftmanagedclusters | Não | Não |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| aplicações | Sim | Sim |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| empresariais | Sim | Sim |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| hubs | Sim | Sim |

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| tarefas | Não | Não |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| databoxedgedevices | Não | Não |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| áreas de trabalho | Não | Não |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| catálogos | Sim | Sim |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| datafactories | Sim | Sim |
| fábricas | Sim | Sim |

## <a name="microsoftdatalake"></a>Microsoft.DataLake
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| datalakeaccounts | Não | Não |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| accounts | Sim | Sim |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| accounts | Sim | Sim |

## <a name="microsoftdatamigration"></a>Microsoft. datamigration
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| services | Não | Não |
| projetos/Services | Não | Não |
| ranhuras | Não | Não |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| servidores | Não | Não |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| servidores | Sim | Sim |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| servergroups | Não | Não |
| servidores | Sim | Sim |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| artifactsources | Não | Não |
| Implementações | Não | Não |
| servicetopologies | Não | Não |
| servicetopologies/serviços | Não | Não |
| servicetopologies/serviços/serviceunits | Não | Não |

## <a name="microsoftdevices"></a>Microsoft.Devices
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| iothubs | Sim | Sim |
| provisioningservices | Sim | Sim |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| labcenters | Não | Não |
| Laboratórios | Sim | Não |
| laboratórios/servicerunners | Sim | Sim |
| laboratórios/virtualmachines | Sim | Não |
| Agendas | Não | Não |

## <a name="microsoftdns"></a>Microsoft.DNS
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| dnszones | Não | Não |
| dnszones ou uma | Não | Não |
| dnszones/aaaa | Não | Não |
| dnszones/cname | Não | Não |
| dnszones/mx | Não | Não |
| dnszones/ptr | Não | Não |
| dnszones/srv | Não | Não |
| dnszones/txt | Não | Não |
| trafficmanagerprofiles | Não | Não |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| databaseaccounts | Sim | Sim |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| domínios | Sim | Sim |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| Tópicos | Sim | Sim |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| Clusters | Sim | Sim |
| espaços de nomes | Sim | Sim |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| hanainstances | Sim | Sim |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| dedicatedhsms | Não | Não |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| Clusters | Sim | Sim |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| tarefas | Sim | Sim |

## <a name="microsoftinsights"></a>Microsoft. insights
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| actiongroups | Sim | Sim |
| activitylogalerts | Não | Não |
| alertrules | Sim | Sim |
| autoscalesettings | Sim | Sim |
| Componentes | Sim | Sim |
| metricalerts | Não | Não |
| scheduledqueryrules | Sim | Sim |
| testes Web | Sim | Sim |
| pastas de trabalho | Sim | Sim |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| iotapps | Sim | Sim |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| cofres | Sim | Sim |

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| labaccounts | Sim | Sim |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| accounts | Sim | Sim |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| accounts | Sim | Sim |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| integrationaccounts | Sim | Sim |
| Fluxos de trabalho | Sim | Sim |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| commitmentplans | Sim | Sim |
| webservices | Sim | Não |
| áreas de trabalho | Sim | Sim |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| operationalizationclusters | Sim | Sim |

## <a name="microsoftmachinelearningexperimentation"></a>Machinelearningexperimentation
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| accounts | Sim | Sim |
| as contas/áreas de trabalho | Sim | Sim |
| as contas/áreas de trabalho/projetos | Sim | Sim |
| teamaccounts | Sim | Sim |
| teamaccounts/áreas de trabalho | Sim | Sim |
| áreas de trabalho/teamaccounts/projetos | Sim | Sim |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| accounts | Sim | Sim |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| áreas de trabalho | Sim | Sim |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| userassignedidentities | Sim | Sim |

## <a name="microsoftmaps"></a>Microsoft.Maps
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| accounts | Sim | Sim |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| classicdevservices | Não | Não |

## <a name="microsoftmedia"></a>Microsoft.Media
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| mediaservices | Sim | Sim |
| mediaservices/liveevents | Sim | Sim |
| mediaservices/pontos finais | Sim | Sim |

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| projetos | Não | Não |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| applicationgateways | Não | Não |
| applicationsecuritygroups | Sim | Sim |
| azurefirewalls | Sim | Sim |
| ligações | Sim | Sim |
| ddosprotectionplans | Não | Não |
| dnszones | Sim | Sim |
| expressroutecircuits | Não | Não |
| expressroutecrossconnections | Não | Não |
| expressroutegateways | Não | Não |
| expressrouteports | Não | Não |
| frontdoors | Sim | Sim |
| frontdoorwebapplicationfirewallpolicies | Sim | Sim |
| interfaceendpoints | Não | Não |
| balanceadores de carga | Sim | Sim |
| localnetworkgateways | Sim | Sim |
| networkintentpolicies | Sim | Sim |
| networkinterfaces | Sim | Sim |
| networkprofiles | Não | Não |
| networksecuritygroups | Sim | Sim |
| networkwatchers | Sim | Sim |
| networkwatchers/connectionmonitors | Sim | Sim |
| networkwatchers/lentes | Sim | Sim |
| networkwatchers/pingmeshes | Sim | Sim |
| publicipaddresses | Sim | Sim |
| publicipprefixes | Sim | Sim |
| routefilters | Não | Não |
| routetables | Sim | Sim |
| serviceendpointpolicies | Sim | Sim |
| trafficmanagerprofiles | Sim | Sim |
| virtualhubs | Sim | Sim |
| virtualnetworkgateways | Sim | Sim |
| redes virtuais | Sim | Sim |
| virtualnetworktaps | Não | Não |
| virtualwans | Sim | Sim |
| vpngateways | Sim | Sim |
| vpnsites | Sim | Sim |
| webapplicationfirewallpolicies | Sim | Sim |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| espaços de nomes | Sim | Sim |
| espaços de nomes/notificationhubs | Sim | Sim |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| áreas de trabalho | Sim | Sim |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| managementconfigurations | Sim | Sim |
| soluções | Sim | Sim |
| visualizações | Sim | Sim |

## <a name="microsoftportal"></a>Microsoft.Portal
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| Dashboards | Sim | Sim |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| workspacecollections | Sim | Sim |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| Capacidades | Sim | Sim |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| cofres | Sim | Sim |

## <a name="microsoftrelay"></a>Microsoft.Relay
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| espaços de nomes | Sim | Sim |

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| aplicações | Sim | Não |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| fluxos | Sim | Sim |
| jobcollections | Sim | Sim |

## <a name="microsoftsearch"></a>Microsoft.Search
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| searchservices | Sim | Sim |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| espaços de nomes | Sim | Sim |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| Clusters | Sim | Sim |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| aplicações | Sim | Sim |
| Redes | Sim | Sim |
| volumes | Sim | Sim |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| signalr | Sim | Sim |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| siterecoveryvault | Não | Não |

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| appliancedefinitions | Não | Não |
| aplicações | Não | Não |
| applicationdefinitions | Não | Não |
| aplicações | Não | Não |

## <a name="microsoftsql"></a>Microsoft.Sql
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| managedinstances | Sim | Sim |
| managedinstances/bases de dados | Sim | Sim |
| servidores | Sim | Sim |
| bases de dados/servidores | Sim | Sim |
| servidores/elasticpools | Sim | Sim |
| virtualclusters | Sim | Sim |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| storageaccounts | Sim | Sim |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| storagesyncservices | Sim | Sim |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| gerentes | Não | Não |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| streamingjobs | Sim | Sim |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| Ambientes | Sim | Sim |
| ambientes/eventsources | Sim | Sim |
| ambientes/referencedatasets | Sim | Sim |

## <a name="microsoftvisualstudio"></a>Microsoft.visualstudio
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| conta | Sim | Sim |
| extensão da conta / | Sim | Sim |
| conta/projeto | Sim | Sim |

## <a name="microsoftweb"></a>Microsoft. Web
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| certificados | Não | Sim |
| classicmobileservices | Não | Não |
| connectiongateways | Sim | Sim |
| ligações | Sim | Sim |
| customapis | Sim | Sim |
| hostingenvironments | Não | Não |
| serverfarms | Sim | Sim |
| Sites | Sim | Sim |
| sites/premieraddons | Sim | Sim |
| sites/ranhuras | Sim | Sim |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Tipo de recurso | Grupo de recursos | Subscrição |
| ------------- | -------------- | ------------ |
| deviceservices | Sim | Sim |


## <a name="third-party-services"></a>Serviços de terceiros

Serviços de terceiros atualmente não é suportada a operação de movimentação. Esses provedores de recursos são:

* 84codes. CloudAMQP
* AppDynamics.APM
* Aspera.Transfers
* Auth0.cloud
* Citrix.Cloud
* Citrix.Services
* CloudSimple.PrivateCloudIaaS
* Cloudyn.Analytics
* Conexlink.MyCloudIT
* Crypteron.DataSecurity
* Dynatrace.DynatraceSaaS
* Dynatrace.Ruxit
* LiveArena.Broadcast
* Lombiq.DotNest
* Mailjet.Email
* Myget.PackageManagement
* NewRelic.APM
* nuubit.nextgencdn
* Paraleap.CloudMonix
* Pokitdok.Platform
* RavenHq.Db
* Raygun.CrashReporting
* RevAPM.MobileCDN
* Sendgrid.Email
* Sparkpost.Basic
* stackify.retrace
* SuccessBricks.ClearDB
* TrendMicro.DeepSecurity
* U2uconsult.TheIdentityHub


## <a name="next-steps"></a>Passos Seguintes

* Para obter comandos mover os recursos, veja [mover recursos para um novo grupo de recursos ou subscrição](resource-group-move-resources.md).
