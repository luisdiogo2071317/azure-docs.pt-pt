---
title: Rede topologias para migrações de base de dados geridas por instância de SQL do Azure utilizando o serviço de migração de base de dados do Azure | Microsoft Docs
description: Saiba as configurações de origem e de destino para o serviço de migração de base de dados.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: 9fcee103854209016d73e29b598c9f33d35c4b6c
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316872"
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-the-azure-database-migration-service"></a>Topologias de rede para migrações de base de dados geridas por instância de SQL do Azure utilizando o serviço de migração de base de dados do Azure
Este artigo aborda as várias topologias de rede que funcione com o serviço de migração de base de dados do Azure para fornecer uma experiência de migração completa dos servidores de SQL no local para a base de dados geridas por instância de SQL do Azure.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Instância do Azure SQL da base de dados geridos configurados para cargas de trabalho híbrida 
Utilize esta topologia se a instância de gerido da base de dados do Azure SQL Server está ligada à sua rede no local. Esta abordagem fornece o encaminhamento de rede mais simplificada e gera o débito de dados durante a migração.

![Topologia de rede para cargas de trabalho híbrida](media\resource-network-topologies\hybrid-workloads.png)

**Requisitos**
- Neste cenário, a instância de gerido da base de dados do Azure SQL e a instância de serviço de migração de base de dados do Azure são criados na mesma VNET do Azure, mas utilizam sub-redes diferentes.  
- A VNET utilizada neste cenário também está ligada à rede no local através de um [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Instância do Azure SQL da base de dados geridos isolada da rede no local
Utilize esta topologia de rede se o seu ambiente requer um ou mais dos seguintes cenários:
- A instância de gerido da base de dados do Azure SQL está isolada da conectividade no local, mas a instância de serviço de migração de base de dados do Azure está ligada à rede no local.
- Se as políticas de controlo de acesso baseado em ' (RBAC) da função estão assegurados e pretender limitar os utilizadores a aceder à mesma subscrição que está a alojar a instância de gerido da base de dados do Azure SQL.
- As VNETs utilizado para a instância de gerido da base de dados do Azure SQL e o serviço de migração de base de dados do Azure estão em subscrições diferentes.

![Topologia de rede para a instância geridos isolada da rede no local](media\resource-network-topologies\mi-isolated-workload.png)

**Requisitos**
- A VNET que utiliza o serviço de migração de base de dados do Azure para este cenário tem também de estar ligada à rede no local através de um (https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Configurar [o VNET peering da rede](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) entre a VNET utilizada para a base de dados geridas por instância de SQL do Azure e o serviço de migração de base de dados do Azure.


## <a name="cloud-to-cloud-migrations-shared-vnet"></a>Migrações de nuvem para a nuvem: partilhado VNET

Utilize esta topologia se a origem de SQL Server estiver alojada numa VM do Azure e partilha a mesma VNET com a base de dados geridas por instância de SQL do Azure e o serviço de migração de base de dados do Azure.

![Topologia de rede para migrações de nuvem para a nuvem com uma vnet partilhada](media\resource-network-topologies\cloud-to-cloud.png)

**Requisitos**
- Não existem requisitos adicionais.

## <a name="cloud-to-cloud-migrations-isolated-vnet"></a>Nuvem para migrações de nuvem: isolada VNET

Utilize esta topologia de rede se o seu ambiente requer um ou mais dos seguintes cenários:
- A instância de gerido da base de dados do Azure SQL é aprovisionada numa VNET isolada.
- Se as políticas de controlo de acesso baseado em ' (RBAC) da função estão assegurados e pretender limitar os utilizadores a aceder à mesma subscrição que está a alojar a instância de gerido da base de dados do Azure SQL.
- As VNETs utilizadas para a base de dados geridas por instância de SQL do Azure e o serviço de migração de base de dados do Azure estão em subscrições diferentes.

![Topologia de rede para migrações de nuvem para a nuvem com uma vnet isolada](media\resource-network-topologies\cloud-to-cloud-isolated.png)

**Requisitos**
- Configurar [o VNET peering da rede](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) entre a VNET utilizada para a base de dados geridas por instância de SQL do Azure e o serviço de migração de base de dados do Azure.


## <a name="see-also"></a>Consultar Também
- [Migrar do SQL Server para instância gerida de base de dados SQL do Azure](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Descrição geral da pré-requisitos para utilizar o serviço de migração de base de dados do Azure](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Criar uma rede virtual com o portal do Azure](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Passos Seguintes
Para obter uma descrição geral do serviço de migração de base de dados do Azure e disponibilidade regional durante a pré-visualização pública, consulte o artigo [o que é a pré-visualização de serviço de migração de base de dados do Azure](dms-overview.md). 