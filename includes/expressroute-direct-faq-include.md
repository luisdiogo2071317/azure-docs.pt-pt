---
title: incluir ficheiro
description: incluir ficheiro
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 09/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: f4a1937062f7e59cb3ef3f38610e077e8d36a3ac
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47005558"
---
### <a name="what-is-expressroute-direct"></a>O que é direta do ExpressRoute?

ExpressRoute Direct fornece aos clientes com a capacidade de ligar diretamente para a rede global da Microsoft em localizações de peering estrategicamente distribuídas em todo o mundo. ExpressRoute Direct fornece conectividade 100Gbps dupla, que suporta a conetividade de ativo/ativo em escala. 

### <a name="how-do-customers-connect-to-expressroute-direct"></a>Como os clientes ligar a Direct do ExpressRoute? 

Clientes precisam trabalhar com as respetivas operadoras locais e fornecedores de localização conjunta para obter conectividade ao ExpressRoute routers para tirar partido do ExpressRoute Direct.

### <a name="what-locations-will-the-100gbps-expressroute-direct-be-available-for-public-preview"></a>As localizações direto 100Gbps ExpressRoute estará disponível para pré-visualização pública? 

Um número seleto de localizações de peering de ExpressRoute irá suportar isso em pré-visualização pública. As portas disponíveis serão dinâmicas e estarão disponíveis através do PowerShell para ver a capacidade. Locais incluem e *estão sujeitos a alterações com base na disponibilidade*:

* Amesterdão
* Camberra
* Chicago
* Washington DC
* Dallas 
* RAE de Hong Kong
* Los Angeles
* Cidade de nova York
* Paris
* San Antonio
* Silicon Valley
* Singapura 

### <a name="what-is-the-sla-for-expressroute-direct"></a>O que é o SLA para o Direct do ExpressRoute?

ExpressRoute Direct passarão a utilizar o mesmo [nível empresarial do ExpressRoute](https://azure.microsoft.com/support/legal/sla/expressroute/v1_3/). 

### <a name="what-scenarios-should-customers-consider-with-expressroute-direct"></a>Quais cenários devem considerar os clientes com Direct do ExpressRoute?  

ExpressRoute Direct fornece aos clientes com pares de portas de 100Gbps direto na estrutura principal global da Microsoft. Os cenários que irão proporcionar aos clientes com maiores benefícios incluem: ingestão de dados em massa, isolamento físico para mercados regulamentados e capacidade dedicada para o cenário de expansão, como a composição. 

### <a name="what-is-the-billing-model-for-expressroute-direct"></a>O que é o modelo de faturação direta do ExpressRoute? 

Será faturada direta do ExpressRoute para o par de porta numa quantidade fixa. Circuitos padrão serão incluídos em nenhuma horas adicionais e premium terão um custo de complemento pequeno. Saída é cobrada de forma por circuito, com base na zona da localização do peering.