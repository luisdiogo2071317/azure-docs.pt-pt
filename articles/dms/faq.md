---
title: "FAQ sobre como utilizar o serviço de migração de base de dados do Azure | Microsoft Docs"
description: "Saiba mais perguntas mais frequentes sobre como utilizar o serviço de migração de base de dados do Azure para efetuar migrações de base de dados."
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
ms.openlocfilehash: 87bd27147d20fec8c5839b744d70f215e2c1ec47
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="faq-about-using-the-azure-database-migration-service"></a>FAQ sobre como utilizar o serviço de migração de base de dados do Azure
Este artigo apresenta uma lista de perguntas mais comuns sobre como utilizar o serviço de migração de base de dados do Azure, juntamente com respostas relacionadas.

### <a name="q-what-is-azure-database-migration-service"></a>Q. O que é o serviço de migração de base de dados do Azure?
O serviço de migração de base de dados do Azure é um serviço completamente gerido concebido para permitir migrações totalmente integradas de várias origens de base de dados para plataformas de dados do Azure com o período de indisponibilidade mínimo. O serviço está atualmente em pré-visualização pública, com esforços de desenvolvimento concentra-se em:
- Fiabilidade e desempenho.
- Iterativa adição de pares de destino de origem.
- Contínuo investimento em migrações friction-livre.

### <a name="q-what-source-target-pairs-does-the-azure-database-migration-service-currently-support"></a>Q. Os pares de destino origem o serviço de migração de base de dados do Azure suporta atualmente?
O serviço em pré-visualização pública atualmente suporta migrações do SQL Server para a SQL Database do Azure e pode ir para o portal do Azure agora para começar a utilizar o serviço de migração de base de dados do Azure para este cenário. Outros pares de destino de origem, como o SQL Server para a base de dados geridas por instância de SQL do Azure e Oracle a SQL Database do Azure estão disponíveis através de uma versão de pré-visualização privada limitado. Para uma oportunidade de participar na pré-visualização privada limitado um destes cenários, inscreva-se [aqui](https://sqldatabase-migrationpreview.azurewebsites.net/).

### <a name="q-how-does-the-azure-database-migration-service-compare-to-other-microsoft-database-migration-tools-such-as-the-database-migration-assistant-dma-or-sql-server-migration-assistant-ssma"></a>Q. Como o serviço de migração de base de dados do Azure comparar com outras ferramentas de migração de base de dados do Microsoft, como o Assistente de base de dados de migração (DMA) ou o SQL Server migração Assistente (SSMA)?
O serviço de migração de base de dados do Azure é o método preferencial de migração de base de dados para o Microsoft Azure à escala. Para mais detalhes sobre como o serviço de migração de base de dados do Azure compara para outro Microsoft ferramentas de migração de base de dados e para as recomendações sobre como utilizar o serviço para vários cenários, consulte a publicação do blogue [base de dados Differentiating Microsoft As ferramentas de migração e serviços](https://blogs.msdn.microsoft.com/datamigration/2017/10/13/differentiating-microsofts-database-migration-tools-and-services/).

### <a name="q-how-does-the-azure-database-migration-service-compare-to-the-azure-migrate-offering"></a>Q. Como comparar o serviço de migração de base de dados do Azure para a oferta Azure migrar?
O serviço de migração do Azure ajuda com a migração de máquinas virtuais no local IaaS do Azure. O serviço avalia a adequabilidade de migração e dimensionamento com base no desempenho e fornece estimativas de custos para executar máquinas virtuais no local no Azure. Migrar do Azure é útil para migrações de comparação de precisão e shift de cargas de trabalho baseados em VM para VMs IaaS do Azure no local. No entanto, ao contrário do serviço de migração da base de dados do Azure, Azure migrar não é um serviço de migração de base de dados especializadas oferta para plataformas de base de dados relacional de Azure PaaS, tais como SQL Database do Azure ou SQL Azure ou a base de dados geridas por instância de SQL do Azure.

### <a name="q-what-is-a-summary-of-the-steps-required-to-use-the-azure-database-migration-service-to-perform-a-database-migration"></a>Q. O que é um resumo dos passos necessários para utilizar o serviço de migração de base de dados do Azure para efetuar uma migração de base de dados?
Durante uma migração típica, simples da base de dados, pode:
1.  Crie um bases de dados de destino.
2.  Migrar o esquema de bases de dados utilizando o [Assistente de migração de base de dados](https://www.microsoft.com/en-us/download/details.aspx?id=53595).
3.  Crie uma instância do serviço de migração de base de dados do Azure.
4.  Crie um projeto de migração, especificar as bases de dados de origem, bases de dados de destino e tabelas para migrar.
5.  Inicie a carga completa.
6.  Escolha a validação subsequente.
7.  Efetue uma switchover manual do seu ambiente de produção para a nova base de dados baseado na nuvem. 

### <a name="q-what-are-the-prerequisites-for-using-the-azure-database-migration-service"></a>Q. Quais são os pré-requisitos para utilizar o serviço de migração de base de dados do Azure?
Existem vários pré-requisitos necessários para garantir que o serviço de migração de base de dados do Azure é executada de facilmente quando efetuar as migrações de base de dados. Alguns dos pré-requisitos aplicam a todos os cenários (pares de destino de origem) suportados pelo serviço, enquanto outros pré-requisitos são exclusivos para um cenário específico.
Os pré-requisitos de serviço de migração de base de dados do Azure que são comuns em todos os cenários de migração suportados incluem a necessidade de:
- Criar uma VNET para o serviço de migração de base de dados do Azure utilizando o modelo de implementação Azure Resource Manager, que fornece a conectividade de site a site para os seus servidores de origem no local através de um [ExpressRoute](https://docs.microsoft.com/en-us/azure/expressroute/expressroute-introduction) ou [VPN](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Certifique-se de que a sua escolha de regras do Azure (VNET) rede segurança grupo de rede Virtual bloquear a comunicação seguinte portas 443, 53, 9354, 445, 12000. Para mais detalhes sobre a filtragem de tráfego do Azure VNET NSG, consulte o artigo [filtrar o tráfego de rede com grupos de segurança de rede](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-nsg).
- Quando utilizar uma aplicação de firewall à frente da sua bases de dados de origem, poderá ter de adicionar regras de firewall para permitir que o serviço de migração de base de dados do Azure para aceder a bases de dados de origem para migração.
 
Para obter uma lista de todos os pré-requisitos necessários para competem ter cenários de migração específico utilizando o serviço de migração de base de dados do Azure, consulte os tutoriais relacionados no serviço de migração de base de dados do Azure [documentação](https://docs.microsoft.com/en-us/azure/dms/dms-overview) no Docs.microsoft.com.

### <a name="q-how-do-i-find-the-ip-address-for-the-azure-database-migration-service-so-that-i-can-create-an-allow-list-for-the-firewall-rules-used-to-access-my-source-database-for-migration"></a>Q. Como encontrar o endereço IP para o serviço de migração de base de dados do Azure para que pode criar uma lista de permissões para as regras de firewall utilizada para aceder à minha base de dados de origem para migração?
Terá de adicionar regras de firewall, permitindo que o serviço de migração de base de dados do Azure aceder à sua base de dados de origem para migração. O endereço IP para o serviço for dinâmico, mas se estiver a utilizar o Expressroute, este endereço é em privado atribuído pela sua rede empresarial. A forma mais fácil para identificar o endereço IP adequado é parecer no mesmo grupo de recursos como o recurso do serviço de migração de base de dados do Azure aprovisionado para encontrar a Interface de rede associado. Normalmente, o nome do recurso de Interface de rede começa com o prefixo NIC e seguido por um caráter único e a sequência de número, exemplo NIC jj6tnztnmarpsskr82rbndyp. Ao selecionar este recurso de interface de rede, pode ver o endereço IP que tem de ser incluído na lista de permissões na página do portal do Azure descrição geral do recurso.

Também poderá ter de incluir a origem de porta do SQL Server escuta na lista de permissões. Por predefinição, é a porta 1433, mas a origem de SQL Server pode ser configurada para escutar, bem como outras portas. Neste caso, terá de incluir as portas na lista de permissões. Pode determinar a porta do SQL Server escuta na utilizando uma consulta de vista de gestão dinâmica:

```sql
    SELECT DISTINCT 
        local_tcp_port 
    FROM sys.dm_exec_connections 
    WHERE local_tcp_port IS NOT NULL
```
Também pode determinar a porta do SQL Server está a escutar consultando o registo de erros do SQL Server:

```sql
    USE master
    GO
    xp_readerrorlog 0, 1, N'Server is listening on' 
    GO
```

### <a name="q-are-there-any-recommendations-for-optimizing-the-performance-of-the-azure-database-migration-service"></a>Q. Existem recomendações para otimizar o desempenho do serviço de migração de base de dados do Azure?
Pode efetuar algumas coisas para acelerar a migração de base de dados utilizando o serviço:
- Utilize a várias CPU geral objetivo escalão de preço quando criar a instância de serviço para permitir que o serviço tirar partido das várias vCPUs para parallelization e transferência de dados mais rápida.
- Temporariamente escala de instância de destino a SQL Database do Azure para o escalão Premium SKU durante a operação de migração de dados para minimizar a base de dados do SQL Azure limitação que poderá ter impacto em atividades de transferência de dados ao utilizar os SKUs de nível inferior.

### <a name="q-how-do-i-set-up-an-azure-virtual-network"></a>Q. Como configurar uma rede Virtual do Azure?
Ao tutoriais Microsoft vários podem orientá-lo durante o processo de configuração de uma VNET do Azure, a documentação oficial aparece no artigo [Azure Virtual Network](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview).

### <a name="q-where-can-i-leave-feedback-about-the-azure-database-migration-service"></a>Q. Onde pode deixar os comentários sobre o serviço de migração de base de dados do Azure?
Queremos ouvi do utilizador. Envie quaisquer comentários e / ideias tem sobre o serviço de migração de base de dados do Azure através de voz do utilizador, [aqui](https://feedback.azure.com/forums/906100-azure-database-migration-service).

## <a name="next-steps"></a>Passos Seguintes
Para obter uma descrição geral do serviço de migração de base de dados do Azure e disponibilidade regional durante a pré-visualização pública, consulte o artigo [o que é a pré-visualização de serviço de migração de base de dados do Azure](dms-overview.md). 