---
title: Descrição geral da migração de Contoso para o Azure | Microsoft Docs
description: Fornece uma descrição geral da estratégia de migração e cenários utilizados pelo Contoso para migrar os seus centros de dados no local para o Azure.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/05/2018
ms.author: raynew
ms.openlocfilehash: 6dd9e8a3bdd497b6bdc4a0a096bfddbd29d25069
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34805019"
---
# <a name="contoso-migration-overview"></a>Migração de Contoso: Descrição geral


Este é o primeiro artigo uma série que demonstra como a organização fictícia Contoso está a migrar a respetiva infraestrutura no local para o [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) na nuvem. 

A série de artigo fornece implementações de exemplo de exemplo que ajudam a compreender e experimentar as diferentes opções para a migração para o Azure e para trabalhar num ambiente híbrido. Artigos mostram a forma como a empresa Contoso conclui o missão de migração, mas ponteiros para leitura geral e instruções específicas são fornecidos ao longo.

## <a name="introduction"></a>Introdução

O Azure oferece acesso a um conjunto abrangente de serviços em nuvem. Como os programadores e profissionais de TI, pode utilizar estes serviços para criar, implementar e gerir aplicações numa gama de ferramentas e estruturas, através de uma rede global de datacenters. À medida que a sua empresa enfrenta os desafios colocados pela transformação digital, a cloud do Azure ajuda-o a perceber como pode otimizar recursos e operações, interagir com os seus clientes e funcionários e transformar os seus produtos.

Contudo, o Azure reconhece que, mesmo com todas as vantagens que a cloud oferece em termos de velocidade e flexibilidade, minimização de custos, desempenho e fiabilidade, muitas organização terão de continuar a executar datacenters no local ainda durante algum tempo. Em resposta às barreiras à adoção da cloud, o Azure oferece uma estratégia de cloud híbrida que cria pontes entre os seus datacenters no local e a cloud pública do Azure. Por exemplo, pode utilizar os recursos na cloud do Azure como a cópia de segurança para proteger recursos no local ou utilizar a análise do Azure para obter informações sobre as cargas de trabalho no local. 

Como parte da estratégia de nuvem híbrida, o Azure oferece soluções crescentes para migrar aplicações no local e cargas de trabalho para a nuvem. Com alguns passos simples, pode fazer uma avaliação abrangente dos seus recursos no local para descobrir como serão executados na cloud do Azure. Em seguida, com uma avaliação detalhada à sua disposição, pode migrar recursos para o Azure com toda a confiança. Quando os recursos estiverem operacionais no Azure, pode otimizá-los para manter e melhorar o acesso, a flexibilidade, a segurança e a fiabilidade.

## <a name="migration-strategies"></a>Estratégias de migração

Estratégias para migração para a nuvem que se enquadram quatro categorias amplas: realojamento, refatorar, rearchitect ou reconstrução. A estratégia que adotar depende da sua controladores de negócio e os objetivos de migração. Poderá adotar várias estratégias. Por exemplo, pode optar por realojamento aplicações simples (comparação de precisão e shift) ou aplicações que não são críticas para o seu negócio, mas rearchitect aqueles que são mais complexa e empresariais críticos. Vamos observar as estratégias.


**Estratégia de** | **Definição** | **Quando utilizar** 
--- | --- | --- 
**Realojamento** | Muitas vezes, referida como uma migração "comparação de precisão e shift". Esta opção não necessita de alterações de código e permite-lhe migra as suas aplicações existentes para o Azure rapidamente. Cada aplicação é migrada como é, reap as vantagens da nuvem, sem o risco e os custos associados com alterações de código. | Quando tiver de mover as aplicações rapidamente para a nuvem.<br/><br/> Quando pretender mover uma aplicação sem modificá-lo.<br/><br/> Quando as suas aplicações são criadas para que estes possam tirar partido de [IaaS do Azure](https://azure.microsoft.com/overview/what-is-iaas/) escalabilidade após a migração.<br/><br/> Quando as aplicações são importantes ao seu negócio, mas não precisa de imediato alterações nas funcionalidades de aplicação.
**Refatorar** | Muitas vezes, referida como "incluí-las num", refactoring necessita de alterações mínimas para aplicações, para que liguem a [Azure PaaS](https://azure.microsoft.com/overview/what-is-paas/)e utilizar as ofertas de nuvem.<br/><br/> Por exemplo, foi possível migrar as aplicações existentes para o App Service do Azure ou do Azure Kubernetes serviço (AKS). Em alternativa, pode refatorar bases de dados relacionais e não relacionais para opções como a base de dados geridas por instância de SQL do Azure, base de dados do Azure para MySQL, base de dados do Azure para PostgreSQL e base de dados do Azure Cosmos. | Se a sua aplicação pode facilmente ser empacotadas novamente para funcionar no Azure.<br/><br/> Refactoring também pode ser utilizado se pretende aplicar inovadoras práticas de DevOps fornecidas pelo Azure ou estiver a pensar sobre a utilização de uma estratégia de contentor para cargas de trabalho do DevOps.<br/><br/> Para refactoring, tem de pensar sobre a portabilidade da sua base de código existente e competências de desenvolvimento disponíveis.
**Rearchitect** | Reformulação da migração está prestes a modificação e expandir a funcionalidade da aplicação e o código base para otimizar a arquitetura de aplicação para escalabilidade de cloud.<br/><br/> Por exemplo, pode dividir uma aplicação monolithic num grupo de micro-serviços que funcionam em conjunto e procure de escala. Ou, foi rearchitect relacional e bases de dados não relacionais completamente geridos soluções DBaaS, tais como base de dados geridas por instância de SQL do Azure, base de dados do Azure para MySQL, base de dados do Azure para PostgreSQL e base de dados do Azure Cosmos. | Quando as suas aplicações precisam revisões principais para incorporar as novas capacidades de ou para trabalhar de forma eficaz numa plataforma de nuvem.<br/><br/> Rearchitecture é uma opção quando pretender que a utilização de investimentos existentes de aplicação, cumpre os requisitos de escalabilidade de forma económica, aplicar inovadoras práticas de DevOps fornecidas pelo Azure e minimizar a utilização de máquinas virtuais.
**Reconstrução** | Reconstrua demora coisas um passo ainda mais ao reconstruir uma aplicação a partir do zero utilizando tecnologias de nuvem do Azure. Por exemplo, pode criar aplicações de campo verde com tecnologias de nuvem nativo como sem servidor, AI do Azure, base de dados geridas por instância de SQL do Azure e a base de dados do Azure Cosmos. | Quando pretender que um desenvolvimento rápido e as aplicações existentes limitada a funcionalidade e tempo de vida. Rearchitect quando pronto para agilizar a inovação empresariais (incluindo práticas de DevOps fornecidas pelo Azure), criar novas aplicações utilizando tecnologias de nuvem nativo e tirar partido de avanços AI, blockchain e IoT.

## <a name="migration-articles"></a>Artigos de migração

Os artigos que compõem a série de migração de Contoso estão resumidos na tabela abaixo.  

- Estes são os cenários de aumentar a complexidade e estamos a adicionar aos mesmos ao longo do tempo.
- Cada cenário de migração é conduzido pelos objetivos de negócio ligeiramente diferentes determinam a estratégia de migração.
- Para cada cenário de implementação, podemos fornecer informações sobre controladores de negócio e objetivos, uma arquitetura de propostas, os passos para efetuar a migração e a recomendação para limpeza e próximos passos depois de concluída a migração.

**Artigo** | **Detalhes** | **Estado**
--- | --- | ---
Artigo 1: Descrição geral (Este artigo) | Fornece uma descrição geral da estratégia de migração da Contoso, a série de artigo e as aplicações de exemplo utilizadas. | Disponível
Artigo 2: Implementar uma infraestrutura do Azure | Descreve como Contoso prepara a infraestrutura do Azure e no local para a migração. A mesma infraestrutura é utilizada para todos os cenários de migração de Contoso. | Disponível
Artigo 3: Avaliar a recursos no local | Mostra como Contoso executa uma avaliação da respetiva aplicação de SmartHotel de duas camadas no local em execução no VMware. Avaliar a VMs de aplicação com o [Azure migrar](migrate-overview.md) serviço e a base de dados de SQL Server de aplicação com o [Assistente de migração de base de dados do Azure](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponível
Artigo 4: Refatorar (comparação de precisão e shift) para as VMs do Azure e uma instância do SQL Server geridos | Demonstra como Contoso migra a aplicação de SmartHotel para o Azure. São migradas o front-end de aplicação VM utilizando [do Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)e a base de dados de aplicação utilizando o [migração de base de dados do Azure](https://docs.microsoft.com/azure/dms/dms-overview) serviço, para migrar para uma instância do SQL Server geridos. | Disponível
Artigo 5: Refatorar (comparação de precisão e shift) para as VMs do Azure | Mostra como Contoso migrar a respetiva aplicação SmartHotel VMs utilizando apenas a recuperação de sites.
Artigo 6: Refatorar (comparação de precisão e shift) para as VMs do Azure e grupos de disponibilidade do SQL Server | Mostra como Contoso migra a aplicação de SmartHotel. Podem utilizar a recuperação de sites para migrar a aplicação VMs e o serviço de base de dados de migração para migrar a base de dados de aplicação para um grupo de disponibilidade do SQL Server. | Disponível
Artigo 7: Refatorar (comparação de precisão e shift) para as VMs do Azure e o servidor de MySQL do Azure | Demonstra como Contoso migra SmartHotel aplicação VMs com a recuperação de sites e o MySQL Workbench para migrar (cópia de segurança e restauro) para uma instância de servidor de MySQL do Azure. | Disponível



### <a name="demo-apps"></a>Aplicações de demonstração

Os artigos utilizam duas aplicações de demonstração - SmartHotel e osTicket.

- SmartHotel360: Esta aplicação foi desenvolvida pela Microsoft como uma aplicação de teste que pode utilizar ao trabalhar com o Azure. É fornecido como código aberto e poderá transferi-lo de [GitHub](https://github.com/Microsoft/SmartHotel360). É uma aplicação ASP.NET ligada a uma base de dados do SQL Server. A aplicação está atualmente em duas VMs de VMware com o Windows Server 2008 R2 e o SQL Server 2008 R2. A aplicação VMs estão alojados no local e gerido pelo vCenter Server.
- osTicket é um suporte de serviço de open source emissão de permissões de aplicação que é executado no Linux. Poderá transferi-lo de [GitHub](https://github.com/osTicket/osTicket). A aplicação atual está em duas VMs de VMware com Ubuntu 16.04LTS, utilizando o Apache 2, o PHP 7.0 e o MySQL 5.7
    

## <a name="next-steps"></a>Passos Seguintes

[Saiba como](contoso-migration-infrastructure.md) Contoso configura os respetivos no local e a infraestrutura do Azure como se preparar para a migração.



