---
title: "Rede topologias para migrações de base de dados geridas por instância de SQL do Azure utilizando o serviço de migração de base de dados do Azure | Microsoft Docs"
description: "Saiba as configurações de origem e de destino para o serviço de migração de base de dados."
services: database-migration
author: HJToland3
ms.author: jtoland
manager: 
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/06/2018
ms.openlocfilehash: 892cff02b5b70f09236bb37ae786f180ddca9316
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-the-azure-database-migration-service"></a>Topologias de rede para migrações de base de dados geridas por instância de SQL do Azure utilizando o serviço de migração de base de dados do Azure
Neste artigo, ficará a saber sobre as várias topologias de rede que o serviço de migração de base de dados do Azure pode trabalhar com a fornecer experiência totalmente integrada de migração para a base de dados geridas por instância de SQL do Azure a partir de servidores do SQL Server no local.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Instância do Azure SQL da base de dados geridos configurados para cargas de trabalho híbrida 
Utilize esta topologia se a instância de gerido da base de dados do Azure SQL Server está ligada à sua rede no local. Esta abordagem fornece o encaminhamento de rede mais simplificada e gera o débito de dados durante a migração.

![Topologia de rede para cargas de trabalho híbrida](media\resource-network-topologies\hybrid-workloads.png)

**Requisitos**
- Neste cenário, a instância de gerido da base de dados do Azure SQL e a instância de serviço de migração de base de dados do Azure são criados na mesma VNET do Azure, mas utilizam sub-redes diferentes.  
- A VNET utilizada neste cenário também está ligada à rede no local ao utilizar o ExpressRoute ou VPN.

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Instância do Azure SQL da base de dados geridos isolada da rede no local
Utilize esta topologia de rede se o seu ambiente requer um ou mais dos seguintes cenários:
- A instância de gerido da base de dados do Azure SQL está isolada da conectividade no local, mas a instância de serviço de migração de base de dados do Azure está ligada à rede no local.
- Políticas de controlo de acesso baseado em (RBAC) de função estão assegurados e limitar o acesso de utilizador na mesma subscrição que está a alojar a instância de gerido da base de dados do Azure SQL.
- As VNETs utilizadas para a base de dados geridas por instância de SQL do Azure e o serviço de migração de base de dados do Azure estão em subscrições diferentes.

![Topologia de rede para a instância geridos isolada da rede no local](media\resource-network-topologies\mi-isolated-workload.png)

**Requisitos**
- A VNET que utiliza o serviço de migração de base de dados do Azure para este cenário também tem de estar ligada à rede no local ao utilizar o ExpressRoute ou VPN.
- Crie uma rede VNET peering entre a VNET utilizada para a base de dados geridas por instância de SQL do Azure e o serviço de migração de base de dados do Azure.


## <a name="cloud-to-cloud-migrations"></a>Nuvem para as migrações de nuvem
Utilize esta topologia se a origem de SQL Server estiver alojada numa máquina virtual do Azure.

![Topologia de rede para migrações de nuvem para a nuvem](media\resource-network-topologies\cloud-to-cloud.png)

**Requisitos**
- Crie uma rede VNET peering entre a VNET utilizada para a base de dados geridas por instância de SQL do Azure e o serviço de migração de base de dados do Azure.

## <a name="see-also"></a>Consultar Também
- [Migrar do SQL Server para instância gerida de base de dados SQL do Azure](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Descrição geral da pré-requisitos para utilizar o serviço de migração de base de dados do Azure](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Criar uma rede virtual com o portal do Azure](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Passos Seguintes
Para obter uma descrição geral do serviço de migração de base de dados do Azure e disponibilidade regional durante a pré-visualização pública, consulte o artigo [o que é a pré-visualização de serviço de migração de base de dados do Azure](dms-overview.md). 