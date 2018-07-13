---
title: Descrição geral da migração da Contoso para o Azure | Documentos da Microsoft
description: Fornece uma descrição geral da estratégia de migração e cenários usados pela Contoso para migrar o seu datacenter no local para o Azure.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: 4cad84d1cabd140f4846cb823e8c4b7ce11bc1f6
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008251"
---
# <a name="contoso-migration-overview"></a>Migração de Contoso: Descrição geral


Este artigo demonstra como a organização fictícia Contoso migra uma infraestrutura no local para o [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) na cloud. 

Este documento é o primeiro de uma série de artigos que mostram como a empresa fictícia Contoso migra para o Azure. A série contém informações e cenários que ilustram como configurar uma infraestrutura de migração e executar diferentes tipos de migrações. Cenários de crescem em complexidade e vamos adicionar artigos adicionais ao longo do tempo. Artigos mostram como a empresa Contoso conclui sua missão de migração, mas ponteiros para leitura geral e instruções específicas são fornecidos ao longo.

## <a name="introduction"></a>Introdução

O Azure fornece acesso a um conjunto abrangente de serviços cloud. Como os programadores e profissionais de TI, pode utilizar estes serviços para criar, implementar e gerir aplicações numa variedade de ferramentas e arquiteturas, através de uma rede global de datacenters. À medida que a sua empresa enfrenta os desafios colocados pela transformação digital, a cloud do Azure ajuda-o a perceber como pode otimizar recursos e operações, interagir com os seus clientes e funcionários e transformar os seus produtos.

Contudo, o Azure reconhece que, mesmo com todas as vantagens que a cloud oferece em termos de velocidade e flexibilidade, minimização de custos, desempenho e fiabilidade, muitas organização terão de continuar a executar datacenters no local ainda durante algum tempo. Em resposta às barreiras à adoção da cloud, o Azure oferece uma estratégia de cloud híbrida que cria pontes entre os seus datacenters no local e a cloud pública do Azure. Por exemplo, pode utilizar os recursos na cloud do Azure como a cópia de segurança para proteger recursos no local ou utilizar a análise do Azure para obter informações sobre as cargas de trabalho no local. 

Como parte da estratégia de cloud híbrida, o Azure fornece cada vez mais soluções para migrar aplicações no local e cargas de trabalho para a cloud. Com alguns passos simples, pode fazer uma avaliação abrangente dos seus recursos no local para descobrir como serão executados na cloud do Azure. Em seguida, com uma avaliação detalhada à sua disposição, pode migrar recursos para o Azure com toda a confiança. Quando os recursos estiverem operacionais no Azure, pode otimizá-los para manter e melhorar o acesso, a flexibilidade, a segurança e a fiabilidade.

## <a name="migration-strategies"></a>Estratégias de migração

Estratégias de migração para a cloud enquadram-se em quatro amplas categorias: realojar, refatorar, rearquitete ou de reconstrução. A estratégia que Adote depende da sua fatores comerciais e os objetivos de migração. É possível adotar várias estratégias. Por exemplo, poderia optar por realojar aplicações simples de (migração lift-and-shift) ou as aplicações que não são críticas para o seu negócio, mas rearquitete aqueles que são mais complexas e críticas para a empresa. Vamos examinar as estratégias.


**Estratégia** | **Definição** | **Quando utilizar** 
--- | --- | --- 
**Realojar** | Muitas vezes referido como uma migração "lift-and-shift". Esta opção não requer alterações ao código, e vamos migrar as suas aplicações existentes para o Azure rapidamente. Cada aplicação é migrada como está, para obter os benefícios da cloud, sem o risco e custo associado a alterações de código. | Quando precisar de mover aplicações rapidamente para a cloud.<br/><br/> Quando pretender mover uma aplicação sem modificá-la.<br/><br/> Quando as suas aplicações são arquitetadas, de modo a que podem tirar partido [IaaS do Azure](https://azure.microsoft.com/overview/what-is-iaas/) escalabilidade após a migração.<br/><br/> Quando as aplicações são importantes para o seu negócio, mas não precisa imediatas alterações nas funcionalidades de aplicação.
**Refactor** | Muitas vezes referido como "reempacotamento", refatoração requer alterações mínimas para aplicações, para que se possam ligar a [PaaS do Azure](https://azure.microsoft.com/overview/what-is-paas/)e utilizar as ofertas de nuvem.<br/><br/> Por exemplo, poderá migrar aplicações existentes para o serviço de aplicações do Azure ou Azure Kubernetes Service (AKS).<br/><br/> Em alternativa, poderia refatorar bancos de dados relacionais e não relacionais em Opções, como a instância gerida da base de dados SQL do Azure, base de dados do Azure para MySQL, base de dados do Azure para PostgreSQL e o Azure Cosmos DB. | Se a sua aplicação pode facilmente ser reempacotada para funcionar no Azure.<br/><br/> Se deseja aplicar práticas de DevOps inovadoras fornecido pelo Azure ou estiver pensando em usar uma estratégia de contentor para cargas de trabalho de DevOps.<br/><br/> Para refatoração, precisa pensar sobre a portabilidade da sua base de código existente e habilidades de desenvolvimento disponíveis.
**Rearquitete** | Rearquitetar para a migração se concentra em modificar e estender a funcionalidade de aplicação e base de códigos para otimizar a arquitetura de aplicação para a escalabilidade da cloud.<br/><br/> Por exemplo, pode dividir uma aplicação monolítica num grupo de microsserviços que funcionam em conjunto e dimensione com facilidade.<br/><br/> Ou, poderia rearquitete relacional e bases de dados não relacionais para totalmente geridos soluções DBaaS, por exemplo, a instância gerida da base de dados SQL do Azure, base de dados do Azure para MySQL, base de dados do Azure para PostgreSQL e o Azure Cosmos DB. | Quando as suas aplicações tem grandes revisões para incorporar novos recursos ou para trabalhar eficazmente numa plataforma na cloud.<br/><br/> Quando quiser utilizar os investimentos existentes do aplicativo, atender aos requisitos de escalabilidade, aplicar práticas de DevOps do Azure inovadoras e minimizar a utilização de máquinas virtuais.
**Reconstruir** | Reconstrua leva as coisas um passo ainda mais ao recriar uma aplicação do zero usando tecnologias de cloud do Azure.<br/><br/> Por exemplo, pode criar aplicações de campo verde com tecnologias nativas da cloud, como sem servidor, IA do Azure, instância gerida da base de dados SQL do Azure e Azure Cosmos DB. | Quando pretender que o desenvolvimento rápido e aplicações existentes têm funcionalidade limitada e tempo de vida.<br/><br/> Quando estiver pronto para acelerar a inovação de negócios (incluindo práticas de DevOps fornecidas pelo Azure), criar aplicações novas com tecnologias nativas da cloud e tirar partido dos avanços em IA, blockchain e IoT.

## <a name="migration-articles"></a>Artigos de migração

Os artigos da série estão resumidos na tabela abaixo.  

- Cada cenário de migração é orientado por objetivos de negócio ligeiramente diferente que determinam a estratégia de migração.
- Para cada cenário de implantação, fornecemos informações sobre controladores de negócios e objetivos, uma arquitetura proposta, passos para efetuar a migração e as recomendações para a limpeza e os passos seguintes após a conclusão da migração.

**Artigo** | **Detalhes** | **Estado**
--- | --- | ---
Artigo 1: Descrição geral | Fornece uma descrição geral da estratégia de migração da Contoso, a série de artigos e as aplicações de exemplo que usamos. | Este artigo.
[Artigo 2: Implementar uma infraestrutura do Azure](contoso-migration-infrastructure.md) | Descreve como o Contoso prepara a sua infraestrutura do Azure e no local, para a migração. A mesma infra-estrutura é utilizada para todos os artigos de migração. | Disponível
[Artigo 3: Avaliar a recursos no local para migração para o Azure](contoso-migration-assessment.md)  | Mostra como Contoso é executado uma avaliação de uma aplicação de SmartHotel de duas camadas no local em execução no VMware. Contoso avalia as VMs de aplicação com o [do Azure Migrate](migrate-overview.md) serviço e a base de dados do SQL Server do aplicativo com o [Assistente de migração de base de dados](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponível
[Artigo 4: Realojar a uma aplicação em VMs do Azure e uma instância gerida de SQL](contoso-migration-rehost-vm-sql-managed-instance.md) | Demonstra como Contoso é executado uma migração lift-and-shift para o Azure para a aplicação de SmartHotel no local. Contoso migra o front-end de aplicação VM através de [do Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)e a base de dados de aplicação para a instância gerida de SQL, utilizando o [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Disponível
[Artigo 5: Realojar a uma aplicação em VMs do Azure](contoso-migration-rehost-vm.md) | Mostra como o Contoso migrar as VMs da aplicação SmartHotel para VMs do Azure, com o serviço Site Recovery. | Disponível
[Artigo 6: Realojar a uma aplicação em VMs do Azure e SQL Server sempre no grupo de disponibilidade](contoso-migration-rehost-vm-sql-ag.md) | Mostra como a Contoso migra a aplicação de SmartHotel. A Contoso utiliza o Site Recovery para migrar as VMs da aplicação e o serviço de migração de base de dados para migrar a base de dados de aplicação para um cluster do SQL Server protegido por um grupo de Disponibilidade AlwaysOn. | Disponível
[Artigo 7: Realojar uma aplicação do Linux em VMs do Azure](contoso-migration-rehost-linux-vm.md) | Mostra como Contoso faz uma migração lift-and-shift da aplicação de osTicket do Linux para VMs do Azure, com o Site Recovery | Disponível
[Artigo 8: Realojar uma aplicação do Linux em VMs do Azure e MySQL do Azure](contoso-migration-rehost-linux-vm-mysql.md) | Demonstra como a Contoso migra a aplicação de osTicket do Linux para as VMs do Azure com o Site Recovery e migra a base de dados de aplicação para uma instância do servidor MySQL do Azure com o MySQL Workbench. | Disponível
[Artigo 9: Refatorizar uma aplicação na base de dados de aplicações Web do Azure e SQL do Azure](contoso-migration-refactor-web-app-sql.md) | Demonstra como Contoso migra a aplicação de SmartHotel para uma aplicação Web do Azure e migra a base de dados de aplicação para a instância de servidor SQL do Azure | Disponível
[Artigo 10: Refatorizar uma aplicação do Linux em aplicações Web do Azure e MySQL do Azure](contoso-migration-refactor-linux-app-service-mysql.md) | Mostra como a Contoso migra a aplicação de osTicket do Linux para aplicações Web do Azure em vários sites, integrados com o GitHub para a entrega contínua. Estes migrar a base de dados de aplicação para uma instância do MySQL do Azure. | Disponível
[Artigo 11: Refatorar o TFS no VSTS](contoso-migration-tfs-vsts.md) | Mostra como a Contoso migra a implantação do Team Foundation Server (TFS) no local ao migrá-lo para Visual Studio Team Services (VSTS) no Azure. | Disponível
[Artigo 12: Rearquitetar uma aplicação em contentores do Azure e base de dados do Azure SQL](contoso-migration-rearchitect-container-sql.md) | Mostra como Contoso migra e rearchitects a respetiva aplicação SmartHotel para o Azure. Eles rearquitetar a camada de web de aplicação como um contentor do Windows e a base de dados de aplicação numa base de dados SQL do Azure. | Disponível
[Artigo 13: Recriar uma aplicação no Azure](contoso-migration-rebuild.md) | Mostra como Contoso reconstruir a sua aplicação de SmartHotel com uma gama de capacidades do Azure e serviços, incluindo serviços de aplicações, Kubernetes do Azure, as funções do Azure, serviços cognitivos e Cosmos DB. | Disponível






### <a name="demo-apps"></a>Aplicações de demonstração

Os artigos utilizam duas aplicações de demonstração - SmartHotel e osTicket.

- **SmartHotel360**: esta aplicação foi desenvolvida pela Microsoft como uma aplicação de teste que pode utilizar ao trabalhar com o Azure. São fornecidas como código-fonte aberto e pode baixá-lo partir [GitHub](https://github.com/Microsoft/SmartHotel360). É uma aplicação ASP.NET ligada a uma base de dados do SQL Server. Atualmente, a aplicação está em duas VMs de VMware que executam o Windows Server 2008 R2 e o SQL Server 2008 R2. A aplicação, as VMs estão alojados no local e gerido pelo vCenter Server.
- **osTicket**: uma central de serviços de código-fonte aberto emissão de permissões de aplicação que é executada no Linux. Pode baixá-lo partir [GitHub](https://github.com/osTicket/osTicket). A aplicação atual está em duas VMs de VMware com o Ubuntu 16.04LTS, através do Apache 2, o PHP 7.0 e o MySQL 5.7
    

## <a name="next-steps"></a>Passos Seguintes

[Saiba como](contoso-migration-infrastructure.md) Contoso configura no local e a infraestrutura do Azure para se preparar para migração.



