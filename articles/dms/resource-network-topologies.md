---
title: Topologias para migrações de instância gerida da base de dados SQL do Azure com o serviço de migração de base de dados do Azure de rede | Documentos da Microsoft
description: Saiba as configurações de origem e de destino para o serviço de migração de base de dados.
services: database-migration
author: HJToland3
ms.author: rajpo
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: cfbfd71f75cd3717392f9aa2c020cedda844d774
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803968"
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-the-azure-database-migration-service"></a>Topologias de rede para migrações de instância gerida BD SQL do Azure com o serviço de migração de base de dados do Azure
Este artigo aborda várias topologias de rede que funciona com o serviço de migração de base de dados do Azure para proporcionar uma experiência de migração abrangente de servidores do SQL Server no local para a instância gerida da base de dados SQL do Azure.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>O Azure SQL Database Managed Instance configurado para cargas de trabalho híbrida 
Utilize esta topologia se o seu Azure SQL Database Managed Instance está ligado à sua rede no local. Esta abordagem fornece o encaminhamento de rede mais simplificado e produz o débito de dados máximo durante a migração.

![Topologia de rede para cargas de trabalho híbrida](media\resource-network-topologies\hybrid-workloads.png)

**Requisitos**
- Neste cenário, o Azure SQL Database Managed Instance e a instância do serviço de migração de base de dados do Azure são criados na mesma VNET do Azure, mas usam diferentes sub-redes.  
- A VNET utilizada neste cenário também está ligada à rede no local através de um [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>O Azure SQL Database Managed Instance isolada da rede no local
Utilize esta topologia de rede se o seu ambiente requer um ou mais dos seguintes cenários:
- O Azure SQL Database Managed Instance é isolado da conectividade no local, mas sua instância do serviço de migração de base de dados do Azure está ligada à rede no local.
- Se as políticas de controlo de acesso baseado em ' (RBAC) da função estão em vigor e precisar de limitar os utilizadores a aceder à mesma subscrição que está a alojar o Azure SQL Database Managed Instance.
- As VNETs utilizado para o Azure SQL Database Managed Instance e o serviço de migração de base de dados do Azure estão em subscrições diferentes.

![Topologia de rede para a instância gerida isolada da rede no local](media\resource-network-topologies\mi-isolated-workload.png)

**Requisitos**
- A VNET que utiliza o Azure Database Migration Service para este cenário tem também de estar ligada à rede no local através de um (https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Configurar [VNET peering da rede](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) entre a VNET utilizada para a instância gerida da base de dados SQL do Azure e o serviço de migração de base de dados do Azure.


## <a name="cloud-to-cloud-migrations-shared-vnet"></a>Migrações de cloud para a cloud: partilhado VNET

Utilize esta topologia se a origem do SQL Server estiver alojada numa VM do Azure e compartilha a mesma VNET com a instância gerida da base de dados SQL do Azure e o serviço de migração de base de dados do Azure.

![Topologia de rede para migrações de Cloud para a Cloud com uma vnet partilhada](media\resource-network-topologies\cloud-to-cloud.png)

**Requisitos**
- Não existem requisitos adicionais.

## <a name="cloud-to-cloud-migrations-isolated-vnet"></a>Na cloud para migrações para a cloud: VNET em ambiente isolado

Utilize esta topologia de rede se o seu ambiente requer um ou mais dos seguintes cenários:
- O Azure SQL Database Managed Instance é aprovisionada numa VNET isolada.
- Se as políticas de controlo de acesso baseado em ' (RBAC) da função estão em vigor e precisar de limitar os utilizadores a aceder à mesma subscrição que está a alojar o Azure SQL Database Managed Instance.
- As VNETs utilizadas para a instância gerida da base de dados SQL do Azure e o serviço de migração de base de dados do Azure estão em subscrições diferentes.

![Topologia de rede para migrações de Cloud para a Cloud com uma vnet isolada](media\resource-network-topologies\cloud-to-cloud-isolated.png)

**Requisitos**
- Configurar [VNET peering da rede](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) entre a VNET utilizada para a instância gerida da base de dados SQL do Azure e o serviço de migração de base de dados do Azure.


## <a name="see-also"></a>Consultar Também
- [Migrar o SQL Server para instância gerida de base de dados SQL do Azure](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Descrição geral da pré-requisitos para utilizar o serviço de migração de base de dados do Azure](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Criar uma rede virtual com o portal do Azure](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Passos Seguintes
Para uma descrição geral do serviço de migração de base de dados do Azure e a disponibilidade regional durante a pré-visualização pública, consulte o artigo [o que é a pré-visualização de serviço de migração de base de dados do Azure](dms-overview.md). 