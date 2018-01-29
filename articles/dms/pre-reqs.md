---
title: "Descrição geral da pré-requisitos para utilizar o serviço de migração de base de dados do Azure | Microsoft Docs"
description: "Saiba mais sobre uma descrição geral dos pré-requisitos para utilizar o serviço de migração de base de dados do Azure para efetuar migrações de base de dados."
services: database-migration
author: HJToland3
ms.author: jtoland
manager: 
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 01/25/2018
ms.openlocfilehash: a103bb4802bc4a20b6d82f0c6bb427133d9e5643
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="overview-of-prerequisites-for-using-the-azure-database-migration-service"></a>Descrição geral da pré-requisitos para utilizar o serviço de migração de base de dados do Azure
Existem vários pré-requisitos necessários para garantir que o serviço de migração de base de dados do Azure é executada de facilmente quando efetuar as migrações de base de dados. Alguns dos pré-requisitos aplicam a todos os cenários (pares de destino de origem) suportados pelo serviço, enquanto outros pré-requisitos são exclusivos para um cenário específico.

Pré-requisitos associados utilizando o serviço de migração de base de dados do Azure são listadas nas seguintes secções.

## <a name="prerequisites-common-across-migration-scenarios"></a>Pré-requisitos comuns em cenários de migração
Os pré-requisitos de serviço de migração de base de dados do Azure que são comuns em todos os cenários de migração suportados incluem a necessidade de:
- Criar uma VNET para o serviço de migração de base de dados do Azure utilizando o modelo de implementação Azure Resource Manager, que fornece a conectividade de site a site para os seus servidores de origem no local através de um [ExpressRoute](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-introduction) ou [VPN](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Certifique-se de que a sua escolha de regras do Azure (VNET) rede segurança grupo de rede Virtual bloquear a comunicação seguinte portas 443, 53, 9354, 445, 12000. Para mais detalhes sobre a filtragem de tráfego do Azure VNET NSG, consulte o artigo [filtrar o tráfego de rede com grupos de segurança de rede](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg).
- Quando utilizar uma aplicação de firewall à frente da sua bases de dados de origem, poderá ter de adicionar regras de firewall para permitir que o serviço de migração de base de dados do Azure para aceder a bases de dados de origem para migração.

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database"></a>Pré-requisitos para migração do servidor SQL Server a SQL Database do Azure 
Para além dos pré-requisitos do serviço de migração de base de dados do Azure que são comuns a todos os cenários de migração, também existem pré-requisitos aplicáveis especificamente para um cenário ou outro.

Ao utilizar o serviço de migração de base de dados do Azure para executar o SQL Server para migrações de SQL Database do Azure, para além dos pré-requisitos que são comuns a todos os cenários de migração, é necessário resolver os seguintes pré-requisitos adicionais:
- Configurar o seu [Firewall do Windows para acesso ao motor de base de dados](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Ative o protocolo TCP/IP, que está desativado por predefinição durante a instalação do SQL Server Express, ao seguir as instruções no artigo [ativar ou desativar um protocolo de rede do servidor](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Criar uma instância de instância de SQL Database do Azure, o que fazer, seguindo o detalhe no artigo C[riar uma base de dados SQL do Azure no portal do Azure](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-portal).
- Transfira e instale o [Assistente de migração de dados](https://www.microsoft.com/en-us/download/details.aspx?id=53595) v3.3 ou posterior.
- Abra a firewall do Windows para permitir que o serviço de migração de base de dados do Azure aceder a bases de dados de origem.
- Criar um nível de servidor [regra de firewall](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) para o servidor da SQL Database do Azure permitir o acesso de serviço de migração de base de dados do Azure para as bases de dados de destino. Forneça o intervalo de sub-rede da VNET utilizado para o serviço de migração de base de dados do Azure.
- Certifique-se de que as credenciais utilizadas para ligar à instância do SQL Server de origem têm [controlo servidor](https://docs.microsoft.com/en-us/sql/t-sql/statements/grant-server-permissions-transact-sql) permissões.
- Certifique-se de que as credenciais utilizadas para ligar à instância de SQL Database do Azure de destino tem a permissão controlar base de dados nas bases de dados de SQL do Azure de destino.

   > [!NOTE]
   > Para obter uma lista completa dos pré-requisitos necessários para utilizar o serviço de migração de base de dados do Azure para efetuar as migrações do SQL Server a SQL Database do Azure, consulte o tutorial [migrar o SQL Server a SQL Database do Azure](https://docs.microsoft.com/en-us/azure/dms/tutorial-sql-server-to-azure-sql).
   > 

## <a name="next-steps"></a>Passos Seguintes
Para obter uma descrição geral do serviço de migração de base de dados do Azure e disponibilidade regional durante a pré-visualização pública, consulte o artigo [o que é a pré-visualização de serviço de migração de base de dados do Azure](dms-overview.md). 