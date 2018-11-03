---
title: Quotas e limites de subscrição do Azure
description: Fornece uma lista de subscrição do Azure comuns e limites do serviço, quotas e restrições. Isto inclui informações sobre como aumentar os limites juntamente com os valores máximos.
services: multiple
author: rothja
manager: jeffreyg
tags: billing
ms.assetid: 60d848f9-ff26-496e-a5ec-ccf92ad7d125
ms.service: billing
ms.topic: article
ms.date: 10/19/2018
ms.author: byvinyal
ms.openlocfilehash: 702783ce581ba59b64892035eca3fb28787eec0f
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958972"
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Subscrição do Azure e limites de serviço, quotas e restrições
Este documento lista algumas das limitações mais comuns do Microsoft Azure, que também são chamadas quotas. Este documento atualmente não abrange todos os serviços do Azure. Ao longo do tempo, a lista será expandida e atualizada para abordar mais da plataforma.

Visite [descrição geral de preços do Azure](https://azure.microsoft.com/pricing/) para saber mais sobre os preços do Azure. Lá, pode estimar os custos com o [Calculadora de preços](https://azure.microsoft.com/pricing/calculator/) ou ao visitar a página de detalhes de preços para um serviço (por exemplo, [Windows VMs](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)). Para obter dicas ajudar a gerir os custos, veja [evitar custos inesperados com a faturação do Azure e a gestão de custos](billing/billing-getting-started.md).

> [!NOTE]
> Se deseja elevar o limite ou quota acima a **limite predefinido**, [abra um pedido de suporte do cliente online sem custos](azure-resource-manager/resource-manager-quota-errors.md). Os limites não podem ser aumentados acima a **limite máximo** valor mostrado nas tabelas seguintes. Se não existir nenhuma **limite máximo** coluna, em seguida, o recurso não tem limites ajustável.
>
> [As subscrições de avaliação de gratuito](https://azure.microsoft.com/offers/ms-azr-0044p) não são elegíveis para aumentos de limite ou quota. Se tiver um [subscrição de avaliação gratuita](https://azure.microsoft.com/offers/ms-azr-0044p), pode atualizar para uma [pay as you go](https://azure.microsoft.com/offers/ms-azr-0003p/) subscrição. Para obter mais informações, consulte [atualizar avaliação gratuita do Azure para pay as you go](billing/billing-upgrade-azure-subscription.md) e [perguntas Freqüentes da assinatura de avaliação gratuita](https://azure.microsoft.com/free/free-account-faq).
>

## <a name="limits-and-the-azure-resource-manager"></a>Limites e o Gestor de recursos do Azure
Agora, é possível combinar vários recursos do Azure num único grupo de recursos do Azure. Quando utilizar grupos de recursos, a limites de que uma vez foram global ser geridas num nível regional com o Azure Resource Manager. Para obter mais informações sobre grupos de recursos do Azure, consulte [descrição geral do Azure Resource Manager](azure-resource-manager/resource-group-overview.md).

Os limites abaixo, uma nova tabela foi adicionada para refletir as diferenças em limites ao utilizar o Azure Resource Manager. Por exemplo, há uma **limites de subscrição** tabela e uma **limites de subscrição - Azure Resource Manager** tabela. Quando um limite aplica-se a ambos os cenários, só é exibido na primeira tabela. A menos que indicado de outra forma, os limites são globais em todas as regiões.

> [!NOTE]
> É importante enfatizar que as quotas para recursos em grupos de recursos do Azure são por região acessível pela sua subscrição e não estão por subscrição, assim como as quotas de gestão de serviço. Vamos usar quotas de vCPU como exemplo. Se precisar de pedir um aumento de quota com suporte para vCPUs, precisa decidir quantos vCPUs que pretende utilizar em que regiões e, em seguida, faça uma solicitação específica de quotas de vCPU de grupo de recursos do Azure para os volumes e regiões que pretende. Por conseguinte, se precisar de utilizar 30 vCPUs na Europa Ocidental para executar a sua aplicação lá, especificamente, deve pedir 30 vCPUs na Europa Ocidental. Mas não terá uma quota de vCPU aumentam em qualquer outra região--apenas Europa Ocidental terão a quota de vCPU de 30.
> <!-- -->
> Como resultado, poderá considerar útil considerar decidir o que as suas quotas de grupo de recursos do Azure que precisam ser para a sua carga de trabalho em qualquer região de um e esse valor em cada região na qual estiver a considerar implementação da solicitação. Ver [solucionando problemas de implantação](resource-manager-common-deployment-errors.md) para obter mais ajuda a detetar as suas quotas atuais para regiões específicas.
>
>

## <a name="service-specific-limits"></a>Limites de serviços específicos
* [Active Directory](#active-directory-limits)
* [Gestão de API](#api-management-limits)
* [Serviço de Aplicações](#app-service-limits)
* [Gateway de Aplicação](#application-gateway-limits)
* [Application Insights](#application-insights-limits)
* [Automatização](#automation-limits)
* [BD do Cosmos para o Azure](#azure-cosmos-db-limits)
* [Base de Dados do Azure para MySQL](#azure-database-for-mysql)
* [Base de Dados do Azure para PostgreSQL](#azure-database-for-postgresql)
* [Azure Event Grid](#azure-event-grid-limits)
* [Azure Maps](#azure-maps-limits)
* [Azure Monitor](#monitor-limits)
* [Azure Policy](#azure-policy-limits)
* [Cache de Redis do Azure](#azure-redis-cache-limits)
* [Cópia de segurança](#backup-limits)
* [Batch](#batch-limits)
* [Batch AI](#batch-ai-limits)
* [Serviços BizTalk](#biztalk-services-limits)
* [CDN](#cdn-limits)
* [Serviços Cloud](#cloud-services-limits)
* [Container Instances](#container-instances-limits)
* [Container Registry](#container-registry-limits)
* [Serviço do Kubernetes](#kubernetes-service-limits)
* [Data Factory](#data-factory-limits)
* [Data Lake Analytics](#data-lake-analytics-limits)
* [Data Lake Store](#data-lake-store-limits)
* [Serviço de migração de base de dados](#database-migration-service-limits)
* [DNS](#dns-limits)
* [Hubs de Eventos](#event-hubs-limits)
* [Firewall do Azure](#azure-firewall-limits)
* [Porta de entrada](#azure-front-door-service-limits)
* [Hub IoT](#iot-hub-limits)
* [Serviço de Aprovisionamento de Dispositivos no Hub IoT](#iot-hub-device-provisioning-service-limits)
* [Cofre de Chaves](#key-vault-limits)
* [Log Analytics](#log-analytics-limits)
* [Identidade gerida](#managed-identity-limits)
* [Serviços de Multimédia](#media-services-limits)
* [Mobile Engagement](#mobile-engagement-limits)
* [Serviços móveis](#mobile-services-limits)
* [Multi-Factor Authentication](#multi-factor-authentication)
* [Redes](#networking-limits)
* [Observador de rede](#network-watcher-limits)
* [Serviço do Hub de notificação](#notification-hub-service-limits)
* [Grupo de Recursos](#resource-group-limits)
* [Controlo de acesso baseado em funções](#role-based-access-control-limits)
* [Scheduler](#scheduler-limits)
* [Pesquisa](#search-limits)
* [Service Bus](#service-bus-limits)
* [Serviço SignalR](#signalr-service-limits)
* [Site Recovery](#site-recovery-limits)
* [Base de Dados SQL](#sql-database-limits)
* [SQL Data Warehouse](#sql-data-warehouse-limits)
* [Armazenamento](#storage-limits)
* [Sistema de StorSimple](#storsimple-system-limits)
* [Stream Analytics](#stream-analytics-limits)
* [Subscrição](#subscription-limits)
* [Gestor de Tráfego](#traffic-manager-limits)
* [Máquinas Virtuais](#virtual-machines-limits)
* [Conjuntos de Dimensionamento de Máquinas Virtuais](#virtual-machine-scale-sets-limits)

### <a name="subscription-limits"></a>Limites da subscrição
#### <a name="subscription-limits---azure-service-management-classic-resources"></a>Limites de subscrição - gestão de serviço do Azure (recursos clássicos)
[!INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Limites de subscrição - Azure Resource Manager
Os limites seguintes aplicam-se ao utilizar o Azure Resource Manager e os grupos de recursos do Azure. Limites de que não foram alterados com o Azure Resource Manager não estão listadas abaixo. Consulte a tabela anterior para esses limites.

Para obter informações sobre a API do Resource Manager ler e escrever os limites, consulte [Gestor de recursos de limitação de pedidos](resource-manager-request-limits.md).

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a>Limites do grupo de recursos
[!INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Limites de máquinas virtuais
#### <a name="virtual-machine-limits"></a>Limites de máquina virtual
[!INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]

#### <a name="virtual-machines-limits---azure-resource-manager"></a>Limites de máquinas virtuais - Azure Resource Manager
Os limites seguintes aplicam-se ao utilizar o Azure Resource Manager e os grupos de recursos do Azure. Limites de que não foram alterados com o Azure Resource Manager não estão listadas abaixo. Consulte a tabela anterior para esses limites.

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Limites de conjuntos de dimensionamento de máquina virtual
[!INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="container-instances-limits"></a>Limites de instâncias de contentor
[!INCLUDE [container-instances-limits](../includes/container-instances-limits.md)]

### <a name="container-registry-limits"></a>Limites de registo de contentor
A tabela seguinte fornece detalhes sobre os recursos e limites do básico, Standard e Premium [escalões de serviço](./container-registry/container-registry-skus.md).

[!INCLUDE [container-registry-limits](../includes/container-registry-limits.md)]

### <a name="kubernetes-service-limits"></a>Limites de serviço do Kubernetes
[!INCLUDE [container-service-limits](../includes/container-service-limits.md)]

### <a name="networking-limits"></a>Limites de rede
[!INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Limites de rede
[!INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Limites do Gateway de aplicação

A tabela seguinte aplica-se para a v1, v2, padrão e SKUs do WAF, a menos que indicado de outra forma.
[!INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="network-watcher-limits"></a>Limites do observador de rede
[!INCLUDE [network-watcher-limits](../includes/network-watcher-limits.md)]

#### <a name="traffic-manager-limits"></a>Limites do Gestor de tráfego
[!INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>Limites DNS
[!INCLUDE [dns-limits](../includes/dns-limits.md)]

#### <a name="azure-firewall-limits"></a>Limites da Firewall do Azure
[!INCLUDE [azure-firewall-limits](../includes/firewall-limits.md)]

#### <a name="azure-front-door-service-limits"></a>Limites de serviço de porta de entrada do Azure
[!INCLUDE [azure-front-door-service-limits](../includes/front-door-limits.md)]

### <a name="storage-limits"></a>Limites de armazenamento
<!--like # storage accts -->
[!INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

Para obter mais detalhes sobre os limites de conta de armazenamento, consulte [metas de desempenho e escalabilidade do armazenamento do Azure](storage/common/storage-scalability-targets.md).

#### <a name="storage-resource-provider-limits"></a>Limites de fornecedor de recursos de armazenamento 

[!INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]

#### <a name="azure-blob-storage-limits"></a>Limites de armazenamento de Blobs do Azure
[!INCLUDE [storage-blob-scale-targets](../includes/storage-blob-scale-targets.md)]

#### <a name="azure-files-limits"></a>Limites de ficheiros do Azure
Para obter detalhes adicionais sobre os limites de ficheiros do Azure, consulte [metas de escalabilidade e desempenho de ficheiros do Azure](storage/files/storage-files-scale-targets.md).

[!INCLUDE [storage-files-scale-targets](../includes/storage-files-scale-targets.md)]

#### <a name="azure-file-sync-limits"></a>Limites de sincronização de ficheiros do Azure
[!INCLUDE [storage-sync-files-scale-targets](../includes/storage-sync-files-scale-targets.md)]

#### <a name="azure-queue-storage-limits"></a>Limites de armazenamento de filas do Azure
[!INCLUDE [storage-queues-scale-targets](../includes/storage-queues-scale-targets.md)]

#### <a name="azure-table-storage-limits"></a>Limites de armazenamento de tabela do Azure
[!INCLUDE [storage-tables-scale-targets](../includes/storage-tables-scale-targets.md)]

<!-- conceptual info about disk limits -- applies to unmanaged and managed -->
#### <a name="virtual-machine-disk-limits"></a>Limites de disco da máquina virtual
[!INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Ver [tamanhos de máquinas virtuais](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) para obter mais detalhes.

#### <a name="managed-virtual-machine-disks"></a>Discos da máquina virtual gerida

[!INCLUDE [azure-storage-limits-vm-disks-managed](../includes/azure-storage-limits-vm-disks-managed.md)]

#### <a name="unmanaged-virtual-machine-disks"></a>Discos de máquinas de virtuais não geridos

[!INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

### <a name="cloud-services-limits"></a>Limites de serviços cloud
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]

### <a name="app-service-limits"></a>Limites do serviço de aplicações
Os seguintes limites de serviço de aplicações incluem limites para aplicações Web, aplicações móveis e API Apps.

[!INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Limites do Scheduler
[!INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Limites do batch
[!INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

### <a name="batch-ai-limits"></a>Limites de IA do batch
[!INCLUDE [azure-batch-ai-limits](../includes/azure-batch-ai-limits.md)]

### <a name="biztalk-services-limits"></a>Limites dos serviços BizTalk
A tabela seguinte mostra os limites de serviços Biztalk do Azure.

[!INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]

### <a name="azure-cosmos-db-limits"></a>O Azure Cosmos DB limita
O Azure Cosmos DB é uma base de dados de escala global em que o débito e armazenamento podem ser dimensionados para lidar com tudo o que requer a sua aplicação. Se tiver dúvidas sobre o dimensionamento do Azure Cosmos DB oferece, envie um email para askcosmosdb@microsoft.com.

### <a name="azure-database-for-mysql"></a>Base de Dados do Azure para MySQL
Para a base de dados do Azure para MySQL limites, consulte [limitações na base de dados do Azure para MySQL](mysql/concepts-limits.md).

### <a name="azure-database-for-postgresql"></a>Base de Dados do Azure para PostgreSQL
Para a base de dados do Azure para PostgreSQL limites, consulte [limitações na base de dados do Azure para PostgreSQL](postgresql/concepts-limits.md).

### <a name="search-limits"></a>Limites de pesquisa
Escalões de preço determinam a capacidade e limites do seu serviço de pesquisa. Os escalões incluem:

* *Gratuito* serviço de multi-inquilino, partilhado com outros subscritores do Azure, concebida para avaliação e de desenvolvimento pequena projetos.
* *Básico* fornece recursos de computação dedicados para cargas de trabalho de produção em escala mais pequena, com até três réplicas para cargas de trabalho de consulta de elevada disponibilidade.
* *Standard (S1, S2, S3, S3 alta densidade)* é para cargas de trabalho de produção maiores. Vários níveis existem no escalão standard, para que pode escolher uma configuração de recursos que melhor corresponda ao seu perfil de carga de trabalho.

**Limites por subscrição**

[!INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Limites por serviço de pesquisa**

[!INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Para saber mais sobre os limites num nível mais granular, tais como o tamanho do documento, as consultas por segundo, as chaves, pedidos e respostas, veja [limites de serviço do Azure Search](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Limites de serviços de multimédia
[!INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>Limites da CDN
[!INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Limites de serviços móveis
[!INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitor-limits"></a>Limites de monitor
[!INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Limites de serviço do Hub de notificação
[!INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Limites de Hubs de eventos
[!INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Limites do Service Bus
[!INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>Limites do IoT Hub
[!INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="iot-hub-device-provisioning-service-limits"></a>Limites do serviço de aprovisionamento de dispositivos IoT Hub
[!INCLUDE [azure-iotdps-limits](../includes/iot-dps-limits.md)]

### <a name="data-factory-limits"></a>Limites do Data Factory
[!INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Limites do Data Lake Analytics
[!INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="data-lake-store-limits"></a>Limites do Data Lake Store
[!INCLUDE [azure-data-lake-store-limits](../includes/azure-data-lake-store-limits.md)]

### <a name="database-migration-service-limits"></a>Limites de serviço de migração de base de dados
[!INCLUDE [database-migration-service-limits](../includes/database-migration-service-limits.md)]

### <a name="stream-analytics-limits"></a>Limites do Stream Analytics
[!INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Limites do Active Directory
[!INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]

### <a name="azure-event-grid-limits"></a>Limites do Azure Event Grid
[!INCLUDE [event-grid-limits](../includes/event-grid-limits.md)]

### <a name="azure-maps-limits"></a>Limites de mapas do Azure
[!INCLUDE [maps-limits](../includes/maps-limits.md)]

### <a name="azure-policy-limits"></a>Limites de política do Azure
[!INCLUDE [policy-limits](../includes/azure-policy-limits.md)]

### <a name="storsimple-system-limits"></a>Limites de sistema do StorSimple
[!INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]

### <a name="log-analytics-limits"></a>Limites do log Analytics
[!INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Limites de cópia de segurança
[!INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="signalr-service-limits"></a>Limites de serviço do SignalR
[!INCLUDE [signalr-service-limits](../includes/signalr-service-limits.md)]

### <a name="site-recovery-limits"></a>Limites do Site Recovery
[!INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Limites do Application Insights
[!INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>Limites de gestão de API
[!INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Limites de Cache de Redis do Azure
[!INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Limites do Key Vault
[!INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication
[!INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Limites de automatização
[!INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="managed-identity-limits"></a>Limites de identidade geridos
[!INCLUDE [automation-limits](~/includes/managed-identity-limits.md)]

### <a name="role-based-access-control-limits"></a>Limites de controlo de acesso baseado em funções
[!INCLUDE [role-based-access-control-limits](../includes/role-based-access-control-limits.md)]

### <a name="sql-database-limits"></a>Limites de base de dados SQL
Para limites de base de dados SQL, consulte [limites de recursos de base de dados de SQL para bases de dados individuais](sql-database/sql-database-vcore-resource-limits-single-databases.md) e [limites de recursos do SQL da base de dados para conjuntos elásticos e bases de dados agrupadas](sql-database/sql-database-vcore-resource-limits-elastic-pools.md).

### <a name="sql-data-warehouse-limits"></a>Limites do SQL Data Warehouse
Para os limites do SQL Data Warehouse, consulte [limites de recursos do SQL Data Warehouse](sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md).

## <a name="see-also"></a>Consulte também
[Compreender os limites do Azure e aumenta](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Máquina virtual e tamanhos do serviço Cloud do Azure](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Tamanhos de serviços Cloud](cloud-services/cloud-services-sizes-specs.md)
