---
title: Descrição geral de portas de elevada disponibilidade no Azure | Microsoft Docs
description: Saiba mais sobre num Balanceador de carga interno de balanceamento de carga de portas de elevada disponibilidade.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: kumud
ms.openlocfilehash: 09c51441d393de5d801e7a4c259b711a527349d8
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="high-availability-ports-overview"></a>Descrição geral de portas de elevada disponibilidade

Balanceador de carga de padrão do Azure ajuda-o a carregar os fluxos TCP e UDP de balanceamento em todas as portas em simultâneo, quando estiver a utilizar um balanceador de carga interno. 

Uma regra de portas do HA é uma variante de uma regra, configurada num interno Balanceador de carga padrão de balanceamento de carga. Pode simplificar a utilização do Balanceador de carga, fornecendo uma única regra de balanceamento de carga todos os TCP e UDP fluxos que chegam em todas as portas de um balanceador de carga de padrão interno. A decisão de balanceamento de carga é efetuada por fluxo. Isto baseia-se na seguinte ligação de cinco cadeias de identificação: endereço IP, porta de origem, endereço IP de destino, porta de destino e protocolo de origem.

A funcionalidade de portas do HA ajuda-o com cenários importantes, tais como de elevada disponibilidade e dimensionamento de aplicações virtuais de rede (NVA) dentro de redes virtuais. Também pode ajudar a quando um grande número de portas tem de ser com balanceamento de carga. 

A funcionalidade de portas do HA está configurada quando definir as portas front-end e back-end **0**e o protocolo para **todos os**. O recurso de Balanceador de carga interno, em seguida, equilibra a todos os fluxos de TCP e UDP, independentemente do número de porta.

## <a name="why-use-ha-ports"></a>Porquê utilizar portas HA?

### <a name="nva"></a>Dispositivos de rede virtuais

Pode utilizar NVAs para proteger a sua carga de trabalho do Azure a partir de vários tipos de ameaças de segurança. Quando são utilizadas NVAs nestes cenários, têm de ser fiável e altamente disponível e tem ampliar para a pedido.

Pode alcançar estes objetivos, simplesmente ao adicionar instâncias NVA para o conjunto de back-end de Balanceador de carga interno do Azure e configurar uma regra de Balanceador de carga de portas do HA.

HA portas oferecem várias vantagens para NVA HA cenários:
- Ativação pós-falha rápida para bom estado de funcionamento instâncias, com por instância de sondas de estado de funcionamento
- Desempenho superior com escalável para *n*-instâncias ativas
- *N*-Active Directory e ativa-passiva cenários
- Elimina a necessidade de soluções de complexas, como o Apache ZooKeeper nós para a monitorização de aplicações

O diagrama seguinte apresenta uma implementação de concentrador hub-and-spoke de rede virtual. A spokes realizar imposição de túnel tráfego para a rede virtual do hub e através de NVA, antes de abandonar o fileparser o espaço fidedigno. Os NVAs estejam atrás de um balanceador de carga interno padrão com uma configuração de portas do HA. Todo o tráfego pode ser processado e reencaminhado em conformidade.

![Diagrama de rede virtual de concentrador hub-and-spoke, com NVAs implementada no modo de HA](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> Se estiver a utilizar NVAs, confirme com o respetivo fornecedor como utilizar melhor HA portas e os cenários que são suportados.

### <a name="load-balancing-large-numbers-of-ports"></a>Grande número de portas de balanceamento de carga

Também pode utilizar as portas HA para aplicações que requerem balanceamento de carga de grandes quantidades de portas. Pode simplificar a estes cenários, utilizando um interno [padrão Balanceador de carga](load-balancer-standard-overview.md) com portas HA. Uma única regra de balanceamento de carga substitui várias regras, um para cada porta de balanceamento de carga individual.

## <a name="region-availability"></a>Disponibilidade de região

A funcionalidade de portas HA está disponível em todas as regiões do Azure global.

## <a name="supported-configurations"></a>Configurações suportadas

### <a name="one-single-non-floating-ip-non-direct-server-return-ha-ports-configuration-on-the-internal-standard-load-balancer"></a>Uma única-de vírgula flutuante IP (não - devolução direta do servidor) HA portas configuração no balanceador de carga padrão interno

Esta é uma configuração de portas HA básica. A seguinte configuração permite-lhe configurar a carga de portas HA balanceamento no front-end único endereço IP-
- Ao configurar o seu Balanceador de carga padrão, selecione **HA portas** caixa de verificação na configuração de regra de Balanceador de carga, 
- Certificar **IP flutuante** está definido como **desativado**.

Esta configuração não permite quaisquer outra balanceamento de carga na configuração da regra no recurso de Balanceador de carga atual, bem como não existem outra interno configuração Balanceador de carga recursos para o conjunto especificado de instâncias de back-end.

No entanto, pode configurar um balanceador de carga padrão público para as instâncias de back-end para além desta regra de porta HA.

## <a name="one-single-floating-ip-direct-server-return-ha-ports-configuration-on-the-internal-standard-load-balancer"></a>Uma configuração única de vírgula flutuante HA portas IP (devolução direta do servidor) no balanceador de carga padrão interno

Da mesma forma pode configurar o Balanceador de carga a utilizar uma regra com o balanceamento de carga **HA porta** com um único front-end e o **IP flutuante** definido como **ativado**. 

Esta configuração permite-lhe adicionar regras de e / ou um balanceador de carga público de balanceamento de carga IP flutuante mais. No entanto, não é possível utilizar uma configuração de balanceamento de boad sem de vírgula flutuante IP HA porta sobre esta configuração.

## <a name="multiple-ha-ports-configurations-on-the-internal-standard-load-balancer"></a>Configurações com várias portas HA no balanceador de carga padrão interno

Se o seu cenário requer que configure HA mais do que uma porta frontends, para o mesmo conjunto de back-end, pode conseguir isto por: 
- configurar mais do que um front-end privada os endereços IP para um único recurso de padrão Balanceador de carga interno.
- configurar várias regras, onde cada regra tem uma única de balanceamento de carga exclusivo front-end está selecionado endereço IP.
- Selecione **HA portas** opção e defina **IP flutuante** para **ativado** para todas a regras de balanceamento de carga.

## <a name="internal-load-balancer-with-ha-ports--public-load-balancer-on-the-same-backend-instances"></a>Balanceador de carga interno com portas HA & Balanceador de carga público nas mesmas instâncias de back-end

Pode configurar **um** recurso de Balanceador de carga padrão público para os recursos de back-end, juntamente com um único padrão Balanceador de carga interno com portas HA.

>[!NOTE]
>Esta capacidade está disponível através de modelos do Azure Resource Manager hoje em dia, mas não através do portal do Azure.

## <a name="limitations"></a>Limitações

- HA configuração de portas está disponível apenas para o Balanceador de carga interno, não está disponível para um balanceador de carga público.

- Não é suportada uma combinação de regra HA portas balanceamento de carga e regra não - HA portas balanceamento de carga.

- A funcionalidade de portas HA não está disponível para IPv6.

- Fluxo symmetry para cenários NVA é suportado com apenas um único NIC. Consulte a descrição e diagrama para [aplicações virtuais de rede](#nva). No entanto, se um NAT de destino pode funcionar para o seu cenário, pode utilizar que para se certificar de que o Balanceador de carga interno envia o tráfego de retorno para a NVA mesma.


## <a name="next-steps"></a>Passos Seguintes

- [Configurar HA portas num Balanceador de carga interno padrão](load-balancer-configure-ha-ports.md)
- [Saiba mais sobre o Balanceador de carga padrão](load-balancer-standard-overview.md)
