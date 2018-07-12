---
title: Circuitos do ExpressRoute do Azure e domínios de encaminhamento | Documentos da Microsoft
description: Esta página fornece uma visão geral dos circuitos do ExpressRoute e domínios de encaminhamento.
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: ''
ms.assetid: 6f0c5d8e-cc60-4a04-8641-2c211bda93d9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: ganesr,cherylmc
ms.openlocfilehash: 563ee61b56af22ada662fcfff9f47ae58f3f32ba
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38969100"
---
# <a name="expressroute-circuits-and-routing-domains"></a>Circuitos do ExpressRoute e domínios de encaminhamento
 Deve ordenar uma *circuito do ExpressRoute* para ligar a sua infraestrutura no local para a Microsoft através de um fornecedor de conectividade. A figura seguinte mostra uma representação lógica de conectividade entre a sua WAN e o Microsoft.

![](./media/expressroute-circuit-peerings/expressroute-basic.png)

## <a name="expressroute-circuits"></a>Circuitos do ExpressRoute
Uma *circuito do ExpressRoute* representa uma conexão lógica entre a infraestrutura no local e serviços cloud da Microsoft através de um fornecedor de conectividade. É possível pedir vários circuitos do ExpressRoute. Cada circuito pode estar as mesmas ou em diferentes regiões e pode ser ligado a local através de fornecedores de conectividade diferentes. 

Circuitos do ExpressRoute não mapeiam para qualquer entidades físicas. Um circuito é exclusivamente identificado por um padrão chamado de GUID como uma chave de serviço (s-chave). A chave de serviço é a única parte das informações trocadas entre a Microsoft, o fornecedor de conectividade e o utilizador. A tecla s não é um segredo por motivos de segurança. Existe um mapeamento 1:1 entre um circuito do ExpressRoute e a tecla s.

Um circuito do ExpressRoute pode ter até três peerings independentes: Azure privado do Azure público e Microsoft. Cada peering é um par de BGP independente sessões, cada uma delas adquiria configurada para elevada disponibilidade. Existe um 1: n (1 < = N < = 3) de mapeamento entre um circuito do ExpressRoute e domínios de encaminhamento. Um circuito do ExpressRoute pode ter qualquer um, dois ou todos os três peerings ativados por circuito ExpressRoute.

Cada circuito tem uma largura de banda fixa (50 Mbps, 100 Mbps, 200 Mbps, 500 Mbps, 1 Gbps, 10 Gbps) e é mapeado para um fornecedor de conectividade e uma localização de peering. A largura de banda que selecionar é compartilhada entre todos os peerings para o circuito. 

### <a name="quotas-limits-and-limitations"></a>Quotas, limites e limitações
Limites e quotas predefinidas aplicam-se para cada circuito ExpressRoute. Consulte a [subscrição do Azure e limites do serviço, Quotas e restrições](../azure-subscription-service-limits.md) página para obter informações atualizadas sobre as quotas.

## <a name="expressroute-routing-domains"></a>Domínios de encaminhamento do ExpressRoute
Um circuito do ExpressRoute tem vários domínios de encaminhamento associados à mesma: Azure privado do Azure público e Microsoft. Cada um dos domínios de encaminhamento está configurada de forma idêntica num par de routers (no ativo-ativo ou a partilha de carga configuration) para elevada disponibilidade. Serviços do Azure são categorizados como *público do Azure* e *privado do Azure* para representar o esquemas de endereçamento de IP.

![](./media/expressroute-circuit-peerings/expressroute-peerings.png)

### <a name="azure-private-peering"></a>Peering privado do Azure
Serviços, ou seja, as máquinas virtuais (IaaS) de computação do Azure e serviços cloud (PaaS), que são implementados numa rede virtual podem ser ligados através do domínio de peering privado. O domínio de peering privado é considerado numa extensão fidedigna de sua rede principal para o Microsoft Azure. Pode configurar a conectividade bidirecional entre a rede principal e redes virtuais do Azure (VNets). Este peering permite-lhe ligar às máquinas virtuais e serviços diretamente nos respetivos endereços IP privados em nuvem.  

Pode ligar-se mais de uma rede virtual para o domínio de peering privado. Reveja os [página de perguntas frequentes](expressroute-faqs.md) para obter informações sobre limites e limitações. Pode visitar o [subscrição do Azure e limites do serviço, Quotas e restrições](../azure-subscription-service-limits.md) página para obter informações atualizadas sobre os limites.  Consulte a [encaminhamento](expressroute-routing.md) página para obter informações detalhadas sobre a configuração do encaminhamento.

### <a name="azure-public-peering"></a>Peering público do Azure

> [!IMPORTANT]
> Todos os serviços do Azure PaaS também estão acessíveis através do peering da Microsoft. Recomendamos a criação do peering da Microsoft e ligação aos serviços PaaS do Azure através do peering da Microsoft.  
>   


Serviços como Web sites, bases de dados SQL e armazenamento do Azure são oferecidos em endereços IP públicos. Em privado pode ligar a serviços alojados em endereços IP públicos, incluindo VIPs dos seus serviços cloud, através do domínio de encaminhamento de peering público. Pode ligar o domínio de peering público para sua rede de Perímetro e ligar a todos os serviços do Azure em seus endereços IP públicos a partir da WAN sem ter de se ligar através da internet. 

Conectividade é sempre iniciada a partir da WAN aos serviços do Microsoft Azure. Serviços do Microsoft Azure não será capazes de iniciar as ligações na sua rede através deste domínio de encaminhamento. Depois de ativar o peering público, pode ligar a todos os serviços do Azure. Não podemos permitem-lhe escolher seletivamente os serviços para os quais podemos anunciar rotas.

Pode definir filtros de rota personalizada dentro da sua rede para consumir apenas as rotas que precisa. Consulte a [encaminhamento](expressroute-routing.md) página para obter informações detalhadas sobre a configuração do encaminhamento. 

Para obter mais informações sobre os serviços suportados através do domínio de encaminhamento de peering público, consulte a [FAQ](expressroute-faqs.md).

### <a name="microsoft-peering"></a>Peering da Microsoft
[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

A conectividade ao Microsoft online services (serviços do Office 365, Dynamics 365 e PaaS do Azure) é através do peering da Microsoft. Podemos ativar a conetividade bidirecional entre os serviços de cloud WAN e a Microsoft através do domínio de encaminhamento de peering de Microsoft. Tem de ligar a serviços cloud da Microsoft apenas através de endereços IP públicos, que são propriedade de utilizador ou o fornecedor de conectividade e terá de cumprir todas as regras definidas. Para obter mais informações, consulte a [pré-requisitos do ExpressRoute](expressroute-prerequisites.md) página.

Consulte a [página de perguntas frequentes](expressroute-faqs.md) para obter mais informações sobre serviços suportados, os custos e detalhes de configuração. Consulte a [localizações do ExpressRoute](expressroute-locations.md) página para obter informações sobre a lista de fornecedores de conectividade oferecem suporte peering da Microsoft.

## <a name="routing-domain-comparison"></a>Comparação de domínio de encaminhamento
A tabela seguinte compara os três domínios de encaminhamento:

|  | **Peering privado** | **Peering público** (preterido para criações nova) | **Peering da Microsoft** |
| --- | --- | --- | --- |
| **Máx. prefixos de # suportados por peering** |4000 por padrão, 10 000 com o ExpressRoute Premium |200 |200 |
| **Intervalos de endereços IP suportados** |Qualquer endereço IP válido na sua WAN. |Endereços IP públicos pertencentes ao utilizador ou o fornecedor de conectividade. |Endereços IP públicos pertencentes ao utilizador ou o fornecedor de conectividade. |
| **COMO os requisitos de número** |Privados e públicos como números. Tem de o proprietário público como número se optar por utilizar um. |Privados e públicos como números. No entanto, tem de provar a propriedade de endereços IP públicos. |Privados e públicos como números. No entanto, tem de provar a propriedade de endereços IP públicos. |
| **Protocolos IP suportados**| IPv4 | IPv4 | IPv4, IPv6 |
| **Encaminhamento de IP de Interface de endereços** |Endereços IP público e de RFC1918 |Endereços IP públicos registados nos registos de encaminhamento. |Endereços IP públicos registados nos registos de encaminhamento. |
| **Suporte de MD5 Hash** |Sim |Sim |Sim |



Pode optar por ativar uma ou mais domínios de encaminhamento como parte do seu circuito do ExpressRoute. Pode optar por ter todos os domínios de encaminhamento de colocar o VPN mesmo se desejar combiná-los num único domínio de encaminhamento. Também pode colocá-los com domínios de encaminhamento diferentes, semelhantes ao diagrama. A configuração recomendada é que o peering privado está ligado diretamente à rede principal, e o público e ligações de peering da Microsoft estão ligadas a sua rede de Perímetro.

Se optar por todas as três sessões de peering, tem de ter três pares de sessões de BGP (um par para cada tipo de peering). Os pares de sessão BGP fornecem uma ligação de elevada disponibilidade. Se estiver a ligar através de fornecedores de conectividade de 2 de camada, é responsável por configurar e gerir o encaminhamento. Pode saber mais, revendo os [fluxos de trabalho](expressroute-workflows.md) para configurar o ExpressRoute.

## <a name="expressroute-health"></a>Estado de funcionamento do ExpressRoute
Circuitos do ExpressRoute que podem ser monitorados para disponibilidade, conectividade a VNets e a utilização de utilização de largura de banda [Monitor de desempenho de rede](https://docs.microsoft.com/azure/networking/network-monitoring-overview) (NPM).

NPM monitoriza o estado de funcionamento de peering privado do Azure e peering da Microsoft.  Confira nosso [publicar](https://azure.microsoft.com/en-in/blog/monitoring-of-azure-expressroute-in-preview/) para obter mais informações.

## <a name="next-steps"></a>Passos Seguintes
* Encontre um fornecedor de serviços. Ver [fornecedores e localizações do ExpressRoute serviço](expressroute-locations.md).
* Confirme que todos os pré-requisitos são cumpridos. Veja os [Pré-requisitos do ExpressRoute](expressroute-prerequisites.md).
* Configurar a ligação do ExpressRoute.
  * [Criar e gerir circuitos do ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md)
  * [Configurar o encaminhamento (peering) dos circuitos do ExpressRoute](expressroute-howto-routing-portal-resource-manager.md)

