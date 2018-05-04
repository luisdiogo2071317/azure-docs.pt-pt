---
title: Migrar o seu datacenter no local para o Azure | Microsoft Docs
description: Leia um documento técnico de descrição geral sobre como migrar os seus datacenters no local para o Azure.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/21/2018
ms.author: raynew
ms.openlocfilehash: 8ba490998ea5f20efca591327716a6e39e9c1ba8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="migrating-your-on-premises-workloads-to-azure"></a>Migrar as suas cargas de trabalho no local para o Azure


O Microsoft Azure dá-lhe acesso a um conjunto abrangente de serviços cloud que, enquanto programador e profissional de TI, pode utilizar para criar, implementar e gerir aplicações numa variedade de ferramentas e arquiteturas, através de uma rede global de datacenters. À medida que a sua empresa enfrenta os desafios colocados pela transformação digital, a cloud do Azure ajuda-o a perceber como pode otimizar recursos e operações, interagir com os seus clientes e funcionários e transformar os seus produtos.

Contudo, o Azure reconhece que, mesmo com todas as vantagens que a cloud oferece em termos de velocidade e flexibilidade, minimização de custos, desempenho e fiabilidade, muitas organização terão de continuar a executar datacenters no local ainda durante algum tempo. Em resposta às barreiras à adoção da cloud, o Azure oferece uma estratégia de cloud híbrida que cria pontes entre os seus datacenters no local e a cloud pública do Azure. Por exemplo, pode utilizar os recursos na cloud do Azure como a cópia de segurança para proteger recursos no local ou utilizar a análise do Azure para obter informações sobre as cargas de trabalho no local. 

Como parte da nossa estratégia de cloud híbrida, o Azure fornece cada vez mais soluções para migrar aplicações e cargas de trabalho no local para a cloud. Com alguns passos simples, pode fazer uma avaliação abrangente dos seus recursos no local para descobrir como serão executados na cloud do Azure. Em seguida, com uma avaliação detalhada à sua disposição, pode migrar recursos para o Azure com toda a confiança. Quando os recursos estiverem operacionais no Azure, pode otimizá-los para manter e melhorar o acesso, a flexibilidade, a segurança e a fiabilidade.

Esta série de artigos de migração mostra-lhe como planear e criar uma estratégia de migração para a sua empresa. Os artigos mostram vários cenários de complexidade cada mais elevada, que iremos adicionar ao longo do tempo. Estes cenários estão resumidos na tabela abaixo. Para cada cenário, fornecemos uma descrição geral, uma arquitetura de migração e uma demonstração dos passos envolvidos no processo de migração. 

**Cenário** | **Solução** | **Serviços** | **Artigo** 
--- | --- | --- | ---
[Cenário 1: Deteção e avaliação](migrate-scenarios-assessment.md) | Detetar e avaliar a infraestrutura para a migração para o Azure, dados e aplicações no local | Assistente de Migração de Dados, serviço Azure Migrate  | Já disponível
**[Cenário 2: Aplicação de realojamento](migrate-scenarios-lift-and-shift.md)** | Comparação de precisão e shift as aplicações do Azure. | Azure Site Recovery, Azure Database Migration Service, Instância Gerida SQL do Azure | Já disponível
**Cenário 3: Refactorize aplicação** | Refatorar aplicações durante a migração para o Azure. | Em planeamento | Planeado
**Cenário 4: Aplicação de Rearchitect** | Rearchitect aplicações durante a migração para o Azure. | Em planeamento | Planeado
**Cenário 5: Aplicação de reconstrução** |Reconstruir aplicações durante a migração para o Azure | Em planeamento | Planeado




