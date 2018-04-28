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
ms.openlocfilehash: 53e09498324f802ce1839d262999657d37ee973b
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="high-availability-ports-overview"></a>Descrição geral de portas de elevada disponibilidade

Balanceador de carga de padrão do Azure ajuda-o a fluxos TCP e UDP de balanceamento de carga em todas as portas em simultâneo quando estiver a utilizar um balanceador de carga interno. 

Uma regra de portas de elevada disponibilidade (HA) é uma variante de uma regra de balanceamento de carga configurada num interno Balanceador de carga padrão. Pode simplificar a utilização de um balanceador de carga, fornecendo uma regra única para balancear a carga de todos os fluxos TCP e UDP que chegam em todas as portas de um balanceador de carga de padrão interno. A decisão de balanceamento de carga é efetuada por fluxo. Esta ação é com base na seguinte ligação de cinco cadeias de identificação: endereço IP, porta de origem, endereço IP de destino, porta de destino e protocolo de origem.

A funcionalidade de portas do HA ajuda-o com cenários importantes, tais como de elevada disponibilidade e dimensionamento de aplicações virtuais de rede (NVAs) dentro de redes virtuais. Também pode ajudar a funcionalidade quando um grande número de portas tem de ser com balanceamento de carga. 

A funcionalidade de portas do HA está configurada quando definir as portas front-end e back-end **0** e o protocolo para **todos os**. O recurso de Balanceador de carga interno, em seguida, equilibra a todos os fluxos de TCP e UDP, independentemente do número de porta.

## <a name="why-use-ha-ports"></a>Porquê utilizar portas HA?

### <a name="nva"></a>Dispositivos de rede virtuais

Pode utilizar NVAs para ajudar a proteger a carga de trabalho do Azure a partir de vários tipos de ameaças de segurança. Quando utilizar NVAs nestes cenários, têm de ser fiável e altamente disponível e tem ampliar para a pedido.

Pode alcançar estes objetivos, simplesmente ao adicionar instâncias NVA para o conjunto de back-end de Balanceador de carga interno e configurar um HA portas a regra de Balanceador de carga.

Para NVA HA cenários, as portas HA oferecem as seguintes vantagens:
- Fornecer ativação pós-falha rápida para bom estado de funcionamento instâncias, com por instância de sondas de estado de funcionamento
- Certifique-se de um desempenho superior com escalável para *n*-instâncias ativas
- Fornecer *n*-Active Directory e ativa-passiva cenários
- Eliminar a necessidade de soluções complexas, tais como nós de Apache ZooKeeper para monitorização de aplicações

O diagrama seguinte apresenta uma implementação de concentrador hub-and-spoke de rede virtual. A spokes realizar imposição de túnel tráfego para a rede virtual do hub e através de NVA, antes de abandonar o fileparser o espaço fidedigno. Os NVAs estejam atrás de um balanceador de carga interno padrão com uma configuração de portas do HA. Todo o tráfego pode ser processado e reencaminhado em conformidade.

![Diagrama de rede virtual de concentrador hub-and-spoke, com NVAs implementada no modo de HA](./media/load-balancer-ha-ports-overview/nvaha.png)

>[!NOTE]
> Se estiver a utilizar NVAs, certifique-se com os seus fornecedores de como utilizar melhor HA portas e para saber quais os cenários que são suportados.

### <a name="load-balancing-large-numbers-of-ports"></a>Balanceamento de carga um grande número de portas

Também pode utilizar as portas HA para aplicações que requerem balanceamento de carga de grandes quantidades de portas. Pode simplificar a estes cenários, utilizando um interno [padrão Balanceador de carga](load-balancer-standard-overview.md) com portas HA. Uma única regra de balanceamento de carga substitui vários balanceamento de carga regras individuais, um para cada porta.

## <a name="region-availability"></a>Disponibilidade de região

A funcionalidade de portas HA está disponível em todas as regiões do Azure global.

## <a name="supported-configurations"></a>Configurações suportadas

### <a name="a-single-non-floating-ip-non-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>Uma configuração de IP (não - devolução direta do servidor). o HA-portas único, de vírgula flutuante num Balanceador de carga interno padrão

Esta configuração é uma configuração de portas do HA básica. Pode configurar um HA regra de portas de balanceamento de carga num único endereço IP Front-end efetuando o seguinte procedimento:
1. Durante a configuração de Balanceador de carga padrão, selecione o **HA portas** caixa de verificação na configuração de regra de Balanceador de carga.
2. Para **IP flutuante**, selecione **desativado**.

Esta configuração não permite quaisquer outra balanceamento de carga na configuração da regra no recurso de Balanceador de carga atual. Também permite que não existem outra interno configuração Balanceador de carga recursos para o conjunto especificado de instâncias de back-end.

No entanto, pode configurar um balanceador de carga padrão público para as instâncias de back-end para além desta regra de portas do HA.

### <a name="a-single-floating-ip-direct-server-return-ha-ports-configuration-on-an-internal-standard-load-balancer"></a>Uma configuração de IP (devolução direta do servidor). o HA-portas único, vírgula flutuante num Balanceador de carga interno padrão

Da mesma forma pode configurar o Balanceador de carga a utilizar uma regra de balanceamento de carga com **HA porta** com um único front-end através da definição de **IP flutuante** para **ativado**. 

Ao utilizar esta configuração, pode adicionar mais flutuante IP balanceamento de carga regras e/ou de um balanceador de carga público. No entanto, não é possível utilizar um IP de-de vírgula flutuante, portas de HA balanceamento de carga na configuração sobre esta configuração.

### <a name="multiple-ha-ports-configurations-on-an-internal-standard-load-balancer"></a>Múltiplas configurações de HA portas num Balanceador de carga interno padrão

Se o seu cenário requer que configure a mais do que um HA porta front-end para o mesmo conjunto de back-end, pode efetuar o seguinte: 
- Configure mais do que um front-end endereço IP privado para um único recurso de padrão Balanceador de carga interno.
- Configure várias regras de balanceamento de carga, onde cada regra tem um endereço IP de front-end exclusivo único selecionado.
- Selecione o **HA portas** opção e, em seguida, defina **IP flutuante** para **ativado** para todas as regras de balanceamento de carga.

### <a name="an-internal-load-balancer-with-ha-ports-and-a-public-load-balancer-on-the-same-back-end-instance"></a>Um balanceador de carga interno com um balanceador de carga público na mesma instância de back-end e de portas HA

Pode configurar *um* recurso de Balanceador de carga padrão público para os recursos de back-end, juntamente com um único padrão Balanceador de carga interno com portas HA.

>[!NOTE]
>Esta capacidade está atualmente disponível através de modelos Azure Resource Manager, mas não está disponível através do portal do Azure.

## <a name="limitations"></a>Limitações

- HA configuração de portas está disponível apenas para balanceadores de carga internos. Não está disponível para balanceadores de carga públicos.

- Combinar de um HA portas um portas de não HA não é suportada a regra de balanceamento de carga e regra de balanceamento de carga.

- A funcionalidade de portas do HA não está disponível para IPv6.

- Fluxo symmetry para cenários NVA é suportado com apenas um único NIC. Consulte a descrição e diagrama para [aplicações virtuais de rede](#nva). No entanto, se um destino NAT pode funcionar para o seu cenário, pode utilizá-lo para se certificar de que o Balanceador de carga interno envia o tráfego de retorno para a NVA mesma.


## <a name="next-steps"></a>Passos Seguintes

- [Configurar HA portas num Balanceador de carga interno padrão](load-balancer-configure-ha-ports.md)
- [Saiba mais sobre o Balanceador de carga padrão](load-balancer-standard-overview.md)
