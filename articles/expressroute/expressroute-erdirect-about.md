---
title: Sobre o Direct do ExpressRoute do Azure | Documentos da Microsoft
description: Esta página fornece uma visão geral do ExpressRoute Direct (pré-visualização)
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: mialdrid
ms.openlocfilehash: 3b46f24edf10fee7001b35ffdd9fa218aa438e19
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50959686"
---
# <a name="about-expressroute-direct-preview"></a>Sobre o ExpressRoute direcionar (pré-visualização)

O ExpressRoute Direct oferece aos clientes a possibilidade de se ligarem diretamente à rede global da Microsoft em localizações de peering estrategicamente distribuídas em todo o mundo. ExpressRoute Direct fornece conectividade de 100-Gbps dupla, que suporta a conetividade de ativo/ativo em escala.

Recursos-chave que ExpressRoute Direct fornece incluem, mas não estão limitados a:

* Ingestão de Dados em Massa em serviços como o Armazenamento e o Cosmos DB
* Isolamento físico para setores que estejam reguladas e exigir dedicado e isolado a conectividade, como: bancários, governamentais e varejo
* Controlo granular de distribuição do circuito baseado na unidade de negócios

> [!IMPORTANT]
> Direct do ExpressRoute está atualmente em pré-visualização.
>
> Esta Pré-visualização Pública é disponibilizada sem um contrato de nível de serviço e não deve ser utilizada para cargas de trabalho de produção. Algumas funcionalidades podem não ser suportadas, podem ter capacidades restringidas ou podem não estar disponíveis em todas as localizações do Azure. Veja os [Termos Suplementares de Utilização para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obter mais informações.

## <a name="enroll-in-the-preview"></a>Inscrever-se na Pré-visualização

Antes de utilizar o ExpressRoute Direct, primeiro tem de inscrever a subscrição na pré-visualização. Para se inscrever, enviar um E-Mail para <ExpressRouteDirect@microsoft.com> com o seu ID de subscrição, incluindo os seguintes detalhes:

* Cenários que pretende para realizar com **direta do ExpressRoute**
* Preferências de localização - consulte [parceiros e localizações de peering](expressroute-locations-providers.md) para obter uma lista completa de todas as localizações
* Linha cronológica para implementação
* Outras perguntas

## <a name="expressroute-using-a-service-provider-and-expressroute-direct"></a>ExpressRoute através de um fornecedor de serviços e direta do ExpressRoute

| **ExpressRoute com um fornecedor de serviços** | **Direct do ExpressRoute** | 
| --- | --- |
| Utiliza fornecedores de serviços para ativar a integração rápida e conectividade na infraestrutura existente | Requer a infraestrutura de 100 Gbps e o gerenciamento completo de todas as camadas
| Integra-se com centenas de fornecedores, incluindo Ethernet e MPLS | Capacidade de Direct/dedicado para indústrias reguladas e ingestão de dados em massa |
| SKUs de circuitos de 50 Mbps para 10 Gbps | Cliente pode selecionar uma combinação do circuito SKUs seguinte: 5 Gbps, 10 Gbps, 40 Gbps, 100 Gbps - limitados a um total de 200 Gbps
| Otimizado para o inquilino único | Otimizado para fornecedores de serviços de inquilino único/Cloud / várias unidades de negócio

## <a name="expressroute-direct-circuits"></a>Circuitos do ExpressRoute Direct

O Microsoft Azure ExpressRoute permite-lhe expandir as redes no local para a cloud da Microsoft através de uma ligação privada facilitada por um fornecedor de conectividade. Com o ExpressRoute, pode ligar aos serviços cloud da Microsoft, tais como o Microsoft Azure, o Office 365 e o Dynamics 365.  

Cada localização de peering tem acesso à rede global da Microsoft e pode aceder a qualquer região numa zona geopolítica por padrão e pode aceder a todas as regiões globais com um circuito de premium.  

A funcionalidade na maioria dos cenários é equivalente a circuitos que utilizam um fornecedor de serviços de ExpressRoute para operar. Para suportar mais granularidade e novos recursos oferecidos através do ExpressRoute Direct, há determinadas capacidades principais que existe em direto de circuitos do ExpressRoute.

## <a name="circuit-skus"></a>SKUs do circuito

ExpressRoute Direct suporta cenários de ingestão de grandes quantidades de dados no armazenamento do Azure e outros serviços de grandes volumes de dados. ExpressRoute circuits no ExpressRoute Direct agora também suporte **40 Gbps** e **100 Gbps** circuito SKUs.

## <a name="vlan-tagging"></a>Marcação de VLAN

ExpressRoute Direct suporta QinQ e Dot1Q marcação de VLAN.

* **A marcação de VLAN QinQ** permite para domínios de encaminhamento isolados num por base de circuito do ExpressRoute. Azure aloca uma marca de S durante a criação do circuito e não pode ser alterado dinamicamente. Cada peering no circuito (privado e Microsoft), utilizará uma etiqueta de C exclusiva como a VLAN. A etiqueta de C não é necessário para ser exclusivo em circuitos nas portas Direct do ExpressRoute.

* **A marcação de VLAN Dot1Q** permite para um único etiquetados VLAN num por base de par de porta Direct do ExpressRoute. Uma etiqueta de C, usada num modo de peering tem de ser exclusiva em todos os circuitos e peerings no par de porta Direct do ExpressRoute.

## <a name="workflow"></a>Fluxo de trabalho

![fluxo de trabalho](./media/expressroute-erdirect-about/workflow1.png)

## <a name="sla"></a>SLA

ExpressRoute Direct fornece o mesmo SLA de nível empresarial com ligações redundantes do ativo/ativo na rede Global da Microsoft. Infraestrutura de ExpressRoute é redundante e conectividade à rede Global da Microsoft é redundante e diversificado e dimensiona-se em conformidade com os requisitos dos clientes. Durante a pré-visualização, não haverá nenhum SLA e devem ser consideradas apenas para cargas de trabalho de não produção.

## <a name="next-steps"></a>Passos Seguintes

[Configurar o ExpressRoute Direct](expressroute-howto-erdirect.md)
