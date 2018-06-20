---
title: Descrição geral da pré-requisitos para utilizar o serviço de migração de base de dados do Azure | Microsoft Docs
description: Saiba mais sobre uma descrição geral dos pré-requisitos para utilizar o serviço de migração de base de dados do Azure para efetuar migrações de base de dados.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/19/2018
ms.openlocfilehash: 3dc449724e405f83ce976b9f8b01a89c25d693fe
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221255"
---
# <a name="overview-of-prerequisites-for-using-the-azure-database-migration-service"></a>Descrição geral da pré-requisitos para utilizar o serviço de migração de base de dados do Azure
Existem vários pré-requisitos necessários para garantir que o serviço de migração de base de dados do Azure é executada de facilmente quando efetuar as migrações de base de dados. Alguns dos pré-requisitos aplicam a todos os cenários (pares de destino de origem) suportados pelo serviço, enquanto outros pré-requisitos são exclusivos para um cenário específico.

Pré-requisitos associados utilizando o serviço de migração de base de dados do Azure são listadas nas seguintes secções.

## <a name="prerequisites-common-across-migration-scenarios"></a>Pré-requisitos comuns em cenários de migração
Os pré-requisitos de serviço de migração de base de dados do Azure que são comuns em todos os cenários de migração suportados incluem a necessidade de:
- Criar uma VNET para o serviço de migração de base de dados do Azure utilizando o modelo de implementação Azure Resource Manager, que fornece a conectividade de site a site para os seus servidores de origem no local através de um [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Certifique-se de que a sua escolha de regras do Azure (VNET) rede segurança grupo de rede Virtual bloquear a comunicação seguinte portas 443, 53, 9354, 445, 12000. Para mais detalhes sobre a filtragem de tráfego do Azure VNET NSG, consulte o artigo [filtrar o tráfego de rede com grupos de segurança de rede](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
- Quando utilizar uma aplicação de firewall à frente da sua bases de dados de origem, poderá ter de adicionar regras de firewall para permitir que o serviço de migração de base de dados do Azure para aceder a bases de dados de origem para migração.
- Configurar o seu [Firewall do Windows para acesso ao motor de base de dados](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Ative o protocolo TCP/IP, que está desativado por predefinição durante a instalação do SQL Server Express, ao seguir as instruções no artigo [ativar ou desativar um protocolo de rede do servidor](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database"></a>Pré-requisitos para migração do servidor SQL Server a SQL Database do Azure 
Para além dos pré-requisitos do serviço de migração de base de dados do Azure que são comuns a todos os cenários de migração, também existem pré-requisitos aplicáveis especificamente para um cenário ou outro.

Ao utilizar o serviço de migração de base de dados do Azure para executar o SQL Server para migrações de SQL Database do Azure, para além dos pré-requisitos que são comuns a todos os cenários de migração, é necessário resolver os seguintes pré-requisitos adicionais:

- Criar uma instância de instância de SQL Database do Azure, o que fazer, seguindo o detalhe no artigo C[riar uma base de dados SQL do Azure no portal do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- Transfira e instale o [Assistente de migração de dados](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 ou posterior.
- Abra a Firewall do Windows para permitir que o serviço de migração de base de dados do Azure para aceder à origem de SQL Server, que, por predefinição, é a porta TCP 1433.
- Se estiver a executar várias instâncias do SQL Server com nome utilizando as portas dinâmicas, poderá pretender ativar o serviço de Browser do SQL Server e permitir o acesso à porta UDP 1434 através das firewalls para que o serviço de migração de base de dados do Azure podem ligar a uma instância nomeada na sua origem servidor.
- Criar um nível de servidor [regra de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) para o servidor da SQL Database do Azure permitir o acesso de serviço de migração de base de dados do Azure para as bases de dados de destino. Forneça o intervalo de sub-rede da VNET utilizado para o serviço de migração de base de dados do Azure.
- Certifique-se de que as credenciais utilizadas para ligar à instância do SQL Server de origem têm [controlo servidor](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) permissões.
- Certifique-se de que as credenciais utilizadas para ligar à instância de SQL Database do Azure de destino tem a permissão controlar base de dados nas bases de dados de SQL do Azure de destino.

   > [!NOTE]
   > Para obter uma lista completa dos pré-requisitos necessários para utilizar o serviço de migração de base de dados do Azure para efetuar as migrações do SQL Server a SQL Database do Azure, consulte o tutorial [migrar o SQL Server a SQL Database do Azure](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-azure-sql).
   > 

## <a name="prerequisites-for-migrating-sql-server-to-azure-sql-database-managed-instance"></a>Pré-requisitos para migração do servidor SQL para a base de dados geridas por instância de SQL do Azure
- Criar uma instância de base de dados geridas por instância de SQL do Azure, seguindo o detalhe no artigo [criar uma instância de gerido da base de dados do Azure SQL no portal do Azure](https://aka.ms/sqldbmi).
- Abra as firewalls para permitirem o tráfego SMB numa porta 445 para o intervalo de endereço ou uma sub-rede de IP de serviço de migração de base de dados do Azure.
- Abra a Firewall do Windows para permitir que o serviço de migração de base de dados do Azure para aceder à origem de SQL Server, que, por predefinição, é a porta TCP 1433.
- Se estiver a executar várias instâncias do SQL Server com nome utilizando as portas dinâmicas, poderá pretender ativar o serviço de Browser do SQL Server e permitir o acesso à porta UDP 1434 através das firewalls para que o serviço de migração de base de dados do Azure podem ligar a uma instância nomeada na sua origem servidor.
- Certifique-se de que os inícios de sessão utilizados para ligar a origem de SQL Server e instância geridos de destino são membros da função de servidor sysadmin.
- Crie uma partilha de rede que pode utilizar o serviço de migração de base de dados do Azure para criar cópias de segurança da base de dados de origem.
- Certifique-se de que a conta de serviço com a origem de SQL Server, instância tem privilégios de escrita na partilha de rede que criou e de que a conta de computador para o servidor de origem tem acesso de leitura/escrita para a mesma partilha.
- Tome nota de um utilizador do Windows (e a palavra-passe) que tem o privilégio de controlo total na partilha de rede que criou anteriormente. O serviço de migração de base de dados do Azure representa tem as credenciais de utilizador para carregar os ficheiros de cópia de segurança no contentor de armazenamento do Azure para a operação de restauro.
- Criar um contentor de blob e obter o seu respetivo URI de SAS, utilizando os passos no artigo [recursos de gerir o armazenamento de Blobs do Azure com o Explorador de armazenamento](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container). Lembre-se de que selecione todas as permissões (leitura, escrita, eliminar, lista) na janela de política ao criar o URI de SAS.

   > [!NOTE]
   > Para obter uma lista completa dos pré-requisitos necessários para utilizar o serviço de migração de base de dados do Azure para efetuar migrações do SQL Server para instância geridos base de dados SQL do Azure, consulte o tutorial [migrar o SQL Server para a base de dados geridas por instância de SQL do Azure ](https://aka.ms/migratetomiusingdms).

## <a name="next-steps"></a>Passos Seguintes
Para obter uma descrição geral do serviço de migração de base de dados do Azure e regional disponibilidade, consulte o artigo [o que é o serviço de migração de base de dados do Azure](dms-overview.md). 