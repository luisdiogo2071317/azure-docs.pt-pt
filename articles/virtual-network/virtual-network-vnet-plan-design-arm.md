---
title: Planear a redes virtuais do Azure | Microsoft Docs
description: Saiba mais sobre como planear as redes virtuais com base no seu isolamento, conetividade e requisitos de localização.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: 3a4a9aea-7608-4d2e-bb3c-40de2e537200
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/16/2018
ms.author: jdial
ms.openlocfilehash: 83558b9d8d47ac5e6bd15dd54db38125376d11bd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="plan-virtual-networks"></a>Planear a redes virtuais

Criar uma rede virtual para experimentar fácil suficiente, mas possibilidades são, irá implementar várias redes virtuais ao longo do tempo para suportar as necessidades de produção da sua organização. Com alguns um planeamento, poderá implementar redes virtuais e ligar os recursos que necessita de forma mais eficaz. As informações neste artigo são mais úteis se já estiver familiarizado com redes virtuais e tem alguma experiência trabalhar com eles. Se não estiver familiarizado com redes virtuais, é recomendado que leia [descrição geral de rede Virtual](virtual-networks-overview.md).

## <a name="naming"></a>Atribuição de nomes

Todos os recursos do Azure tem um nome. O nome tem de ser exclusivo dentro de um âmbito, que pode variar para cada tipo de recurso. Por exemplo, o nome de uma rede virtual tem de ser exclusivo dentro de um [grupo de recursos](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), mas podem ser duplicadas dentro de um [subscrição](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) ou do Azure [região](https://azure.microsoft.com/regions/#services). Definir uma convenção de nomenclatura que pode utilizar consistentemente quando os recursos de atribuição de nomes é útil quando gerir vários recursos de rede ao longo do tempo. Para sugestões, consulte [convenções de nomenclatura](/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="regions"></a>Regiões

Todos os recursos do Azure são criados numa região do Azure e a subscrição. Um recurso só pode ser criado numa rede virtual que existe na mesma região e subscrição como o recurso. No entanto, pode ligar redes virtuais que existam em diferentes subscrições e regiões. Para obter mais informações, consulte [conectividade](#connectivity). Ao decidir quais nas regiões implementar recursos, considere onde estão fisicamente localizados os consumidores de recursos:

- Os consumidores de recursos pretendem normalmente a menor latência de rede para os seus recursos. Para determinar as latências relativas entre uma localização especificada e regiões do Azure, consulte [ver latências relativas](../network-watcher/view-relative-latencies.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Tem requisitos de dados residency, soberania dos, compatibilidade ou resiliência? Se assim for, a região alinhar os requisitos é crítico. Para obter mais informações, consulte [localizações geográficas Azure](https://azure.microsoft.com/global-infrastructure/geographies/).
- Exige que resiliência através de zonas de disponibilidade do Azure na mesma região do Azure para os recursos que implementar? Pode implementar recursos, tais como máquinas virtuais (VM) zonas de disponibilidade diferente dentro da mesma rede virtual. Nem todas as regiões do Azure suportam contudo zonas de disponibilidade. Para saber mais sobre as regiões que suportam-los e zonas de disponibilidade, consulte o artigo [zonas de disponibilidade](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="subscriptions"></a>Subscrições

Pode implementar redes virtuais tantos conforme necessário dentro de cada subscrição, até o [limite](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits). Algumas organizações têm subscrições diferentes para diversos departamentos, por exemplo. Para obter mais informações e considerações em torno de subscrições, consulte [governação de subscrição](../azure-resource-manager/resource-manager-subscription-governance.md?toc=%2fazure%2fvirtual-network%2ftoc.json#define-your-hierarchy).

## <a name="segmentation"></a>Segmentação

Pode criar várias redes virtuais por subscrição e por região. Pode criar várias sub-redes dentro de cada rede virtual. As considerações que se seguem ajudam a determinar quantos redes virtuais e sub-redes necessita:

### <a name="virtual-networks"></a>Redes virtuais

Uma rede virtual é uma parte virtual, isolada da rede pública do Azure. Cada rede virtual está dedicado à sua subscrição. Aspetos a considerar quando decidir se pretende criar uma rede virtual ou várias redes virtuais numa subscrição:

- Existem quaisquer requisitos de segurança organizacional para isolar o tráfego para redes virtuais separados? Pode optar por ligar redes virtuais ou não. Se ligar redes virtuais, pode implementar uma aplicação virtual de rede, tais como uma firewall, para controlar o fluxo de tráfego entre as redes virtuais. Para obter mais informações, consulte [segurança](#security) e [conectividade](#connectivity).
- Existem de quaisquer requisitos organizacionais para isolamento de redes virtuais em separado [subscrições](#subscriptions) ou [regiões](#regions)?
- A [interface de rede](virtual-network-network-interface.md) permite que uma VM comunicar com outros recursos. Cada interface de rede tem um ou mais endereços IP privados atribuídos. O número de interfaces de rede e [endereços IP privados](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) exige que numa rede virtual? Existem [limites](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) para o número de interfaces de rede e endereços IP privados que podem ter dentro de uma rede virtual.
- Pretende ligar a rede virtual a outra rede virtual ou a rede no local? Pode optar por ligar algumas redes virtuais entre si ou redes no local, mas não a outras pessoas. Para obter mais informações, consulte [conectividade](#connectivity). Cada rede virtual que se ligar a outra rede virtual ou a rede no local, tem de ter um espaço de endereço exclusivo. Cada rede virtual tem um ou mais intervalos de endereços público ou privado atribuídos ao seu espaço de endereços. Um intervalo de endereços é especificado em classless internet domínio encaminhamento (CIDR) formato, tal como 10.0.0.0/16. Saiba mais sobre [intervalos de endereços](manage-virtual-network.md#add-or-remove-an-address-range) para redes virtuais.
- Tiver quaisquer requisitos organizacionais administração para os recursos na redes virtuais diferentes? Se Sim, pode separar recursos numa rede virtual separado para simplificar [atribuição de permissão](#permissions) para utilizadores na sua organização ou para atribuir diferentes [políticas](#policies) para diferentes virtual redes.
- Ao implementar alguns recursos do serviço do Azure para uma rede virtual, é criar a sua própria rede virtual. Para determinar se um serviço do Azure cria a sua própria rede virtual, consulte as informações para cada [serviço do Azure que pode ser implementado numa rede virtual](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network).

### <a name="subnets"></a>Sub-redes

Uma rede virtual pode ser segmentada para uma ou mais sub-redes até o [limites](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits). Aspetos a considerar quando decidir se pretende criar uma sub-rede ou várias redes virtuais numa subscrição:

- Cada sub-rede tem de ter um intervalo de endereço exclusivo, especificado no formato CIDR, dentro do espaço de endereços da rede virtual. O intervalo de endereços não pode sobrepor-se com outras sub-redes na rede virtual.
- Se planeia implementar alguns recursos do serviço do Azure para uma rede virtual, estes podem exigir, ou criar os seus próprios sub-rede, existe tem de ser suficientemente não alocado espaço para os mesmos para fazê-lo. Para determinar se um serviço do Azure cria a sua própria sub-rede, consulte as informações para cada [serviço do Azure que pode ser implementado numa rede virtual](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Por exemplo, se ligar uma rede virtual a uma rede no local utilizando um Gateway de VPN do Azure, a rede virtual tem de ter uma sub-rede dedicada para o gateway. Saiba mais sobre [sub-redes de gateway](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub).
- Azure rotas de rede tráfego entre todas as sub-redes na rede virtual, por predefinição. Pode substituir predefinição do Azure encaminhamento para impedir o Azure encaminhamento entre sub-redes ou para encaminhar o tráfego entre sub-redes através de uma aplicação virtual de rede, por exemplo. Se necessitar que o tráfego entre os recursos no mesmo fluxo de rede virtual através de uma aplicação virtual de rede (NVA), implemente os recursos em sub-redes diferentes. Saiba mais em [segurança](#security).
- Pode limitar o acesso aos recursos do Azure como uma conta de armazenamento do Azure ou a base de dados SQL do Azure, para sub-redes específicas com um ponto final de serviço de rede virtual. Além disso, pode negar o acesso aos recursos da internet. Pode criar várias sub-redes e ativar um ponto final de serviço para algumas sub-redes, mas não a outras pessoas. Saiba mais sobre [pontos finais de serviço](virtual-network-service-endpoints-overview.md), e os recursos do Azure pode ativá-los.
- Pode associar zero ou um grupo de segurança de rede para cada sub-rede numa rede virtual. Pode associar o mesmo ou outro, grupo de segurança para cada sub-rede de rede. Cada grupo de segurança de rede contém regras, que permitem ou negam o tráfego de e para as origens e destinos. Saiba mais sobre [grupos de segurança de rede](#traffic-filtering).

## <a name="security"></a>Segurança

Pode filtrar o tráfego de rede e de recursos numa rede virtual a utilizar grupos de segurança de rede e os dispositivos de rede virtual. Pode controlar a forma como o Azure encaminha o tráfego de sub-redes. Também pode limitar que, na sua organização, pode trabalhar com recursos na redes virtuais.

### <a name="traffic-filtering"></a>Filtragem do tráfego

- Pode filtrar o tráfego de rede entre os recursos numa rede virtual com um grupo de segurança de rede, uma NVA, que filtra o tráfego de rede, ou ambos. Para implementar uma NVA, tais como uma firewall, para filtrar o tráfego de rede, consulte o [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?subcategories=appliances&page=1). Quando utilizar uma NVA, também criar rotas personalizadas para encaminhar o tráfego de sub-redes para a NVA. Saiba mais sobre [o encaminhamento de tráfego](#traffic-routing).
- Um grupo de segurança de rede contém várias regras de segurança predefinidas que permitem ou negam o tráfego para ou a partir dos recursos. Um grupo de segurança de rede pode ser associado a uma interface de rede, a sub-rede que a interface de rede está a ser ou ambos. Para simplificar a gestão das regras de segurança, recomenda-se que associa um grupo de segurança de rede para sub-redes individuais, em vez de interfaces de rede individuais dentro da sub-rede, sempre que possível.
- Se precisam de diferentes VMs numa sub-rede regras de segurança diferentes aplicadas às mesmas, pode associar a interface de rede VM para um ou mais grupos de segurança de aplicações. Uma regra de segurança poderá especificar um grupo de segurança de aplicações da respetiva origem, destino ou ambos. Essa regra, em seguida, aplica-se apenas para as interfaces de rede que são membros do grupo de segurança de aplicações. Saiba mais sobre [grupos de segurança de rede](security-overview.md) e [grupos de segurança de aplicações](security-overview.md#application-security-groups).
- O Azure cria predefinido várias regras de segurança em cada grupo de segurança de rede. Uma regra predefinida permite que todo o tráfego flua entre todos os recursos numa rede virtual. Para substituir este comportamento, utilize a segurança de rede grupos, personalizada de encaminhamento para encaminhar o tráfego para uma NVA, ou ambos. Recomenda-se que a familiarizar-se com todos os do Azure [predefinido regras de segurança](security-overview.md#default-security-rules) e que compreende como as regras do grupo de segurança de rede são aplicadas a um recurso.

Pode ver as estruturas de exemplo para implementar uma rede de Perímetro entre o Azure e a internet utilizando um [NVA](/architecture/reference-architectures/dmz/secure-vnet-dmz?toc=%2Fazure%2Fvirtual-network%2Ftoc.json) ou [grupos de segurança de rede](virtual-networks-dmz-nsg.md).

### <a name="traffic-routing"></a>o encaminhamento de tráfego

O Azure cria várias rotas predefinidas para o tráfego de saída de sub-rede. Pode substituir predefinição do Azure encaminhamento através da criação de uma tabela de rota e associá-la a uma sub-rede. Os motivos mais comuns para substituir a predefinição do Azure encaminhamento são:
- Porque pretende que o tráfego entre sub-redes através de uma NVA. Para obter mais informações sobre como [configurar tabelas de rota para forçar o tráfego através de uma NVA](tutorial-create-route-table-portal.md)
- Porque pretender forçar todo o tráfego vinculado à internet através de uma NVA, ou no local, através de um gateway de VPN do Azure. Forçar internet tráfego no local para inspecção e o registo é frequentemente referido como imposição do túnel. Saiba mais sobre como configurar [imposição do túnel](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2Fazure%2Fvirtual-network%2Ftoc.json).

Se precisar de implementar o encaminhamento personalizado, recomenda-se que a familiarizar-se com [encaminhamento no Azure](virtual-networks-udr-overview.md).

## <a name="connectivity"></a>Conectividade

Pode ligar uma rede virtual a outras redes virtuais através de peering de rede virtual ou à sua rede no local, utilizando um gateway de VPN do Azure.

### <a name="peering"></a>Peering

Quando utilizar [peering de rede virtual](virtual-network-peering-overview.md), as redes virtuais podem estar no mesmo ou outro, suportada regiões do Azure. As redes virtuais podem estar nas subscrições do Azure idêntica ou diferentes, desde que ambas as subscrições são atribuídas ao mesmo inquilino do Azure Active Directory. Antes de criar um peering, recomenda-se que a familiarizar-se com tudo do peering [requisitos e restrições](virtual-network-manage-peering.md#requirements-and-constraints). Largura de banda entre os recursos na redes virtuais em modo de peering é o mesmo como se os recursos foram na mesma rede virtual.

### <a name="vpn-gateway"></a>Gateway de VPN

Pode utilizar um Azure [Gateway de VPN](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) para ligar uma rede virtual à sua rede no local ao utilizar um [VPN site a site](../vpn-gateway/vpn-gateway-tutorial-vpnconnection-powershell.md?toc=%2fazure%2fvirtual-network%2ftoc.json), ou uma ligação dedicada com o Azure [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Pode combinar peering e um gateway VPN para criar [redes hub- and -spoke](/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json), onde redes virtuais spoke ligarem a uma rede virtual do hub e o hub liga-se a uma rede no local, por exemplo.

### <a name="name-resolution"></a>Resolução de nomes

Recursos numa rede virtual não é possível resolver os nomes de recursos na rede virtual em modo de peering através do Azure [DNS incorporada](virtual-networks-name-resolution-for-vms-and-role-instances.md). Para resolver nomes numa rede virtual em modo de peering, [implementar o seu próprio servidor DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server), ou utilizar o DNS do Azure [domínios privados](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Resolver nomes entre os recursos numa rede virtual e redes no local também necessita de implementar o próprio servidor DNS.

## <a name="permissions"></a>Permissões

Utiliza o Azure [controlo de acesso baseado em funções](../role-based-access-control/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (RBAC) aos recursos. As permissões são atribuídas a um [âmbito](../role-based-access-control/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-hierarchy-and-access-inheritance) na hierarquia do seguinte: subscrição, o grupo de gestão, o grupo de recursos e o recurso individuais. Para saber mais sobre a hierarquia, consulte [organizar os recursos](../azure-resource-manager/management-groups-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Para trabalhar com redes virtuais do Azure e todas as respetivas capacidades relacionadas, tais como o peering, grupos de segurança de rede, pontos finais de serviço e tabelas de rota, pode atribuir membros da sua organização para o incorporado [proprietário](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#owner), [Contribuinte](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#contributor), ou [contribuinte de rede](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) funções e, em seguida, atribua a função para o âmbito adequado. Se pretender atribuir permissões específicas para um subconjunto das funcionalidades de rede virtual, crie um [função personalizada](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) e atribuir as permissões específicas necessárias para [redes virtuais](manage-virtual-network.md#permissions), [ pontos finais de serviço e as sub-redes](virtual-network-manage-subnet.md#permissions), [interfaces de rede](virtual-network-network-interface.md), [peering](virtual-network-manage-peering.md#permissions), [grupos de segurança de rede e de aplicação](manage-network-security-group.md#permissions), ou [tabelas de rotas](manage-route-table.md#permissions) à função.

## <a name="policy"></a>Política

Política do Azure permite-lhe criar, atribuir e gerir definições de política. Definições de política impor regras diferentes e efeitos sobre os recursos, pelo que os recursos se manter em conformidade com as normas organizacionais e contratos de nível de serviço. Política do Azure executa uma edição de avaliação dos seus recursos, análise de recursos que não são compatíveis com as definições de política que tem. Por exemplo, pode ter uma política que permite a criação de redes virtuais num grupo de recursos específico. Outra política pode requerer que todas as sub-redes tem um grupo de segurança de rede associado. As políticas são então avaliadas quando criar e atualizar recursos.

As políticas são aplicadas para a hierarquia seguinte: subscrição, o grupo de gestão e o grupo de recursos. Saiba mais sobre [política do Azure](../azure-policy/azure-policy-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou implementar algumas rede virtual [modelo de política](policy-samples.md) amostras.
