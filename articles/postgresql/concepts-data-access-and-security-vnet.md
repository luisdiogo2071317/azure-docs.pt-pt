---
title: Descrição geral de ponto final dos serviços de base de dados do Azure para o servidor de PostgreSQL vnet | Microsoft Docs
description: Descreve como funcionam os pontos finais do serviço de vnet para a base de dados do Azure para o servidor de PostgreSQL.
services: postgresql
author: mbolz
ms.author: mbolz
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/29/2018
ms.openlocfilehash: a832f45027fc5337d9e76ec9cc4898286121278c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655662"
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-azure-database-for-postgresql"></a>Utilizar pontos finais do serviço de rede Virtual e as regras da base de dados do Azure para PostgreSQL

*Regras de rede virtual* são uma funcionalidade de segurança de firewall que controla se a base de dados do Azure para o servidor de PostgreSQL aceita comunicações que são enviadas do sub-redes específicas em redes virtuais. Este artigo explica por que razão a funcionalidade de regra de rede virtual, por vezes, é a melhor opção em segurança permitir comunicações para a base de dados do Azure para o servidor de PostgreSQL.

Para criar uma regra de rede virtual, tem primeiro de existir um [rede virtual] [ vm-virtual-network-overview] (VNet) e um [ponto final do serviço de rede virtual] [ vm-virtual-network-service-endpoints-overview-649d] para o regra para referência. A imagem seguinte ilustra como um ponto final de serviço de rede Virtual funciona com a base de dados do Azure para PostgreSQL:

![Exemplo de como funciona um ponto final de serviço de VNet](media/concepts-data-access-and-security-vnet/vnet-concept.png)

> [!NOTE]
> Para a base de dados do Azure para PostgreSQL, esta funcionalidade está disponível em pré-visualização pública em todas as regiões de nuvem pública do Azure onde a base de dados do Azure para PostgreSQL é implementado.

<a name="anch-terminology-and-description-82f" />

## <a name="terminology-and-description"></a>Terminologia e descrição

**Rede virtual:** pode fazer com que as redes virtuais associadas à subscrição do Azure.

**Sub-rede:** uma rede virtual contém **sub-redes**. Quaisquer máquinas virtuais (VMs) do Azure que tiver são atribuídas a sub-redes. Uma sub-rede pode conter várias VMs ou outros nós de computação. Computação nós que estão fora da rede virtual não é possível aceder à sua rede virtual, exceto se configurar a segurança para permitir o acesso.

**Ponto final de serviço de rede virtual:** A [ponto final do serviço de rede Virtual] [ vm-virtual-network-service-endpoints-overview-649d] uma sub-rede cujos valores de propriedade incluem um ou mais nomes de tipo de serviço do Azure formal. Neste artigo está interessados no nome do tipo de **Microsoft.Sql**, que se refere ao serviço do Azure com o nome da base de dados do SQL Server. Esta etiqueta de serviço também se aplica à base de dados do Azure para serviços PostgreSQL e MySQL. É importante ter em conta ao aplicar o **Microsoft.Sql** etiquetas de serviço para um ponto final de serviço de VNet irá configurar o tráfego de ponto final de serviço para todos os SQL Database do Azure, base de dados do Azure para PostgreSQL e base de dados do Azure para servidores MySQL na sub-rede. 

**Regra de rede virtual:** uma regra de rede virtual para a base de dados do Azure para o servidor de PostgreSQL é uma sub-rede que está listada na lista de controlo de acesso (ACL) da base de dados do Azure para o servidor de PostgreSQL. Para ser na ACL para a base de dados do Azure para o servidor de PostgreSQL, a sub-rede tem de conter o **Microsoft.Sql** nome de tipo.

Uma regra de rede virtual indica que a base de dados do Azure para o servidor de PostgreSQL para aceitar comunicações de cada nó que está na sub-rede.







<a name="anch-benefits-of-a-vnet-rule-68b" />

## <a name="benefits-of-a-virtual-network-rule"></a>Vantagens de uma regra de rede virtual

Até que a ação, VMs as sub-redes não consegue comunicar com a base de dados do Azure para o servidor de PostgreSQL. Uma ação que estabelece a comunicação é a criação de uma regra de rede virtual. A lógica por detrás para escolher a abordagem de regra de VNet requer um debate comparar e contraste que envolve as opções de segurança concorrentes disponibilizadas pela firewall.

### <a name="a-allow-access-to-azure-services"></a>A. Permitir acesso aos serviços do Azure

O painel de segurança de ligação tem um **ON/OFF** botão assinalada como **permitir o acesso aos serviços do Azure**. O **ON** definição permite as comunicações de todos os endereços IP do Azure e todas as sub-redes do Azure. Estes Azure IPs sub-redes poderão não ser detidas por si. Isto **ON** definição é provavelmente mais aberta que pretende que a base de dados do Azure para a base de dados PostgreSQL ser. A funcionalidade de regra de rede virtual oferece muito controlo finer granular.

### <a name="b-ip-rules"></a>B. Regras IP

A base de dados do Azure para firewall de PostgreSQL permite-lhe especificar os intervalos de endereços IP do que as comunicações são considerados aceites para a base de dados do Azure para base de dados PostgreSQL. Esta abordagem é adequada para estáveis endereços IP que estão fora da rede privada do Azure. Mas muitos nós dentro da rede privada do Azure estão configurados com *dinâmica* endereços IP. Endereços IP dinâmicos podem ser alteradas, por exemplo, quando a VM é reiniciada. Seria folly para especificar um endereço IP dinâmico numa regra de firewall, num ambiente de produção.

Pode salvage a opção de IP por obter um *estático* endereço IP para a VM. Para obter mais informações, consulte [configurar endereços IP privados para uma máquina virtual utilizando o portal do Azure][vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w].

No entanto, a abordagem IP estática pode tornar-se difícil gerir, e é dispendioso quando terminar à escala. Regras de rede virtual são mais fáceis de estabelecer e gerir.

### <a name="c-cannot-yet-have-azure-database-for-postgresql-on-a-subnet-without-defining-a-service-endpoint"></a>C. Não é possível ainda tem base de dados do Azure para PostgreSQL numa sub-rede sem definir um ponto final de serviço

Se o **Microsoft.Sql** servidor era um nó numa sub-rede na sua rede virtual, todos os nós dentro da rede virtual conseguiu comunicar com a base de dados do Azure para o servidor de PostgreSQL. Neste caso, as suas VMs conseguiu comunicar com a base de dados do Azure para PostgreSQL sem necessidade de quaisquer regras de rede virtual ou das regras IP.

No entanto a partir de Maio de 2018, a base de dados do Azure para o serviço de PostgreSQL não é ainda entre os serviços que podem ser atribuídos diretamente a uma sub-rede.

<a name="anch-details-about-vnet-rules-38q" />

## <a name="details-about-virtual-network-rules"></a>Detalhes sobre regras de rede virtual

Esta secção descreve vários detalhes sobre regras de rede virtual.

### <a name="only-one-geographic-region"></a>Apenas uma região geográfica

Cada ponto final de serviço de rede Virtual aplica-se apenas uma região do Azure. O ponto final não ativar a outras regiões aceitar comunicações de sub-rede.

É limitada para a região que o seu ponto final subjacente aplica-se a qualquer regra de rede virtual.

### <a name="server-level-not-database-level"></a>Ao nível do servidor, não ao nível da base de dados

Cada regra de rede virtual aplica-se à base de dados do Azure completa para o servidor de PostgreSQL, não apenas para uma determinada base de dados no servidor. Por outras palavras, se aplica a regra de rede virtual ao nível do servidor, não ao nível da base de dados.

#### <a name="security-administration-roles"></a>Funções de administração de segurança

Há uma separação de funções de segurança na administração de pontos finais do serviço de rede Virtual. Tem de fazer nada cada uma das seguintes funções:

- **Administrador de rede:** &nbsp; ativar o ponto final.
- **Administrador de base de dados:** &nbsp; atualizar a lista de controlo de acesso (ACL) para adicionar a sub-rede especificada para a base de dados do Azure para o servidor de PostgreSQL.

*Alternativa RBAC:*

As funções de administrador de rede e administrador de base de dados têm mais capacidades que são necessárias para gerir as regras de rede virtual. É necessário apenas um subconjunto das respetivas capacidades.

Tem a opção de utilizar [controlo de acesso baseado em funções (RBAC)] [ rbac-what-is-813s] no Azure para criar uma função personalizada único que tenha apenas o necessário subconjunto das funcionalidades. A função personalizada pode ser utilizada em vez de envolver o administrador de rede ou o administrador da base de dados. A área de superfície da sua exposição de segurança é inferior se adicionar um utilizador para uma função personalizada, por oposição a adicionar o utilizador para as outras duas funções de administrador principais.

> [!NOTE]
> Em alguns casos, a base de dados do Azure para PostgreSQL e a sub-rede da VNet estão em subscrições diferentes. Nestes casos, tem de garantir as seguintes configurações:
> - Ambas as subscrições tem de estar no mesmo inquilino do Azure Active Directory.
> - O utilizador tem as permissões necessárias para iniciar as operações, tais como pontos finais do serviço de ativação e adicionar uma sub-rede da VNet para o servidor especificado.

## <a name="limitations"></a>Limitações

Para a base de dados do Azure para PostgreSQL, a funcionalidade de regras de rede virtual tem as seguintes limitações:

- Na firewall para a base de dados do Azure para PostgreSQL, cada regra de rede virtual faz referência a uma sub-rede. Estas sub-redes referenciadas tem de estar alojadas na mesma região geográfica que aloja a base de dados do Azure para PostgreSQL.

- Cada base de dados do Azure para o servidor de PostgreSQL pode ter até 128 entradas ACL para qualquer determinada rede virtual.

- Regras de rede virtual são aplicadas apenas a redes virtuais do Azure Resource Manager; e não a [modelo de implementação clássica] [ arm-deployment-model-568f] redes.

- Ativar na rede virtual pontos finais de serviço à base de dados do Azure para PostgreSQL utilizando o **Microsoft.Sql** tag de serviço também permite que os pontos finais para todos os serviços de base de dados do Azure: base de dados do Azure para MySQL, base de dados do Azure para PostgreSQL , Base de dados SQL do azure e do armazém de dados SQL do Azure.

- No momento da pré-visualização pública, não são suportadas para operações de movimentação de VNet. Para mover uma regra de rede virtual, remova e recrie-lo.

- Suporte para pontos finais do serviço de VNet é apenas para fins gerais e otimização de memória de servidores.

- Na firewall, intervalos de endereços IP aplicam-se aos seguintes itens de rede, mas não as regras de rede virtual:
    - [Rede privada virtual (VPN) site a Site (S2S)][vpn-gateway-indexmd-608y]
    - No local através de [ExpressRoute][expressroute-indexmd-744v]

## <a name="expressroute"></a>ExpressRoute

Se a sua rede é ligado à rede do Azure através da utilização de [ExpressRoute][expressroute-indexmd-744v], cada circuito está configurado com dois endereços IP públicos no Microsoft Edge. Dois endereços IP são utilizados para ligar a Services da Microsoft, tal como para o Storage do Azure, utilizando o Peering público do Azure.

Para permitir comunicações do seu circuito à base de dados do Azure para PostgreSQL, tem de criar regras de rede IP para os endereços IP públicos do seu circuitos. Para localizar os endereços IP públicos do seu circuito do ExpressRoute, abra um pedido de suporte com o ExpressRoute, utilizando o portal do Azure.

## <a name="related-articles"></a>Artigos relacionados
- [Redes virtuais do Azure][vm-virtual-network-overview]
- [Pontos finais do serviço de rede virtual do Azure][vm-virtual-network-service-endpoints-overview-649d]

## <a name="next-steps"></a>Passos Seguintes
Para artigos sobre a criação de regras de VNet, consulte:
- [Criar e gerir a base de dados do Azure para as regras de PostgreSQL VNet no portal do Azure](howto-manage-vnet-using-portal.md)
- [Criar e gerir a base de dados do Azure para regras de PostgreSQL VNet utilizando a CLI do Azure](howto-manage-vnet-using-cli.md)


<!-- Link references, to text, Within this same Github repo. -->
[arm-deployment-model-568f]: ../azure-resource-manager/resource-manager-deployment-model.md

[vm-virtual-network-overview]: ../virtual-network/virtual-networks-overview.md

[vm-virtual-network-service-endpoints-overview-649d]: ../virtual-network/virtual-network-service-endpoints-overview.md

[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md

[rbac-what-is-813s]: ../active-directory/role-based-access-control-what-is.md

[vpn-gateway-indexmd-608y]: ../vpn-gateway/index.yml

[expressroute-indexmd-744v]: ../expressroute/index.yml