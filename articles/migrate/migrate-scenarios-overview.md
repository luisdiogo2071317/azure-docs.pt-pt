---
title: Migrar o seu datacenter no local para o Azure | Microsoft Docs
description: Leia um documento técnico de descrição geral sobre como migrar os seus datacenters no local para o Azure.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/08/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: be322596da0c3e5ba18aa64285c437cdb823fc4b
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="migrating-your-on-premises-workloads-to-azure"></a>Migrar as suas cargas de trabalho no local para o Azure


O Microsoft Azure dá-lhe acesso a um conjunto abrangente de serviços cloud que, enquanto programador e profissional de TI, pode utilizar para criar, implementar e gerir aplicações numa variedade de ferramentas e arquiteturas, através de uma rede global de datacenters. À medida que a sua empresa enfrenta os desafios colocados pela transformação digital, a cloud do Azure ajuda-o a perceber como pode otimizar recursos e operações, interagir com os seus clientes e funcionários e transformar os seus produtos.

Contudo, o Azure reconhece que, mesmo com todas as vantagens que a cloud oferece em termos de velocidade e flexibilidade, minimização de custos, desempenho e fiabilidade, muitas organização terão de continuar a executar datacenters no local ainda durante algum tempo. Em resposta às barreiras à adoção da cloud, o Azure oferece uma estratégia de cloud híbrida que cria pontes entre os seus datacenters no local e a cloud pública do Azure. Por exemplo, pode utilizar os recursos na cloud do Azure como a cópia de segurança para proteger recursos no local ou utilizar a análise do Azure para obter informações sobre as cargas de trabalho no local. 

Como parte da nossa estratégia de cloud híbrida, o Azure fornece cada vez mais soluções para migrar aplicações e cargas de trabalho no local para a cloud. Com alguns passos simples, pode fazer uma avaliação abrangente dos seus recursos no local para descobrir como serão executados na cloud do Azure. Em seguida, com uma avaliação detalhada à sua disposição, pode migrar recursos para o Azure com toda a confiança. Quando os recursos estiverem operacionais no Azure, pode otimizá-los para manter e melhorar o acesso, a flexibilidade, a segurança e a fiabilidade.

Esta série de artigos de migração mostra-lhe como planear e criar uma estratégia de migração para a sua empresa. Os artigos mostram vários cenários de complexidade cada mais elevada, que iremos adicionar ao longo do tempo. Estes cenários estão resumidos na tabela abaixo. Para cada cenário, fornecemos uma descrição geral, uma arquitetura de migração e uma demonstração dos passos envolvidos no processo de migração. 

**Cenário** | **Solução** | **Serviços** | **Artigo** 
--- | --- | --- | ---
[Cenário 1: Deteção e avaliação](migrate-scenarios-assessment.md) | Descobrir e avaliar aplicações e dados no local para migração para o Azure | Assistente de Migração de Dados, serviço Azure Migrate  | Já disponível
**Cenário 2: Migração lift-and-shift** | Realojar aplicações internas no Azure. Otimizar no Azure após a migração. | Azure Site Recovery, Azure Database Migration Service, Instância Gerida SQL do Azure | Já disponível
**Cenário 3: Refatorizar e migrar** | Modernizar e refatorizar aplicações de cliente locais durante a migração para o Azure. | Em planeamento | Planeado
**Cenário 4: Rearquitetar e migrar** | Rearquitete e migre sites transacionais de cliente durante a migração para o Azure. | Em planeamento | Planeado
**Cenário 5: Recriar** |Recriar e migrar dados e aplicações de cliente para o Azure | Em planeamento | Planeado




