---
title: Cenários para zonas de privada de DNS do Azure | Microsoft Docs
description: Descrição geral dos cenários comuns para a utilização de zonas de DNS privado do Azure.
services: dns
documentationcenter: na
author: KumudD
manager: jennoc
editor: ''
ms.assetid: ''
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2018
ms.author: kumud
ms.openlocfilehash: fc6c871f89cd5f837eda741dfbadd64fd021bfbe
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="azure-dns-private-zones-scenarios"></a>Cenários de zonas de DNS privado do Azure
Zonas de DNS privado do Azure fornecem a resolução do nome dentro de uma rede virtual, bem como impedindo redes virtuais. Neste artigo, vamos ver alguns cenários comuns que podem ser realizados ao utilizar esta funcionalidade. 

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

## <a name="scenario-name-resolution-scoped-to-a-single-virtual-network"></a>Cenário: Resolução de nomes confinada a uma única rede virtual
Neste cenário, tem uma rede virtual no Azure que tem um número de recursos do Azure, incluindo máquinas virtuais (VMs). Pretende resolver os recursos a partir de dentro da rede virtual através de um nome de domínio específico (zona DNS), e terá a resolução do nome privadas e não está acessível a partir da internet. Além disso, para as VMs dentro da VNET, precisa do Azure para registar automaticamente para a zona DNS. 

Este cenário é descrito abaixo. Rede virtual denominada "A" contém duas VMs (VNETA VM1 e VNETA VM2). Cada um destes ter IPs privados associados. Depois de criar uma zona de privada com o nome contoso.com e ligar esta rede virtual como uma rede virtual do registo, DNS do Azure irá criar automaticamente dois registos na zona como descrito. Agora, consultas DNS de VNETA-VM1 resolver VNETA VM2.contoso.com irão receber uma resposta DNS que contém o IP privado VNETA VM2. Além disso, uma consulta DNS inverso (PTR) para o IP privado VNETA-VM1 (10.0.0.1) emitido a partir da VNETA VM2 irá receber uma resposta DNS que contém o nome do VNETA-VM1, conforme esperado. 

![Resolução de rede Virtual único](./media/private-dns-scenarios/single-vnet-resolution.png)

## <a name="scenario-name-resolution-across-virtual-networks"></a>Cenário: Resolução de nomes várias redes virtuais

Este cenário é o cenário mais comum onde precisa de associar uma zona privada várias redes virtuais. Este cenário pode ajustar arquiteturas, tais como o modelo de Concentrador Hub-and-Spoke onde existe uma rede virtual concentrador central ao qual outro vários Spoke estão ligadas a redes virtuais. A rede virtual de concentrador central pode ser ligada à rede virtual a uma zona privada de registo e as redes virtuais Spoke podem ser associadas como redes virtuais de resolução. 

O diagrama seguinte mostra uma versão simple deste cenário em que existem apenas duas redes virtuais - A e B. Um é designado como uma rede virtual do registo e B é designado como uma rede virtual de resolução. A intenção é que ambas as redes virtuais partilhar uma zona contoso.com comuns. Quando a zona é criada e as redes virtuais resolução e registo estão ligadas à zona, o Azure irá registar automaticamente registos DNS para as VMs (VNETA VM1 e VNETA VM2) da rede virtual. Pode também adicionar manualmente registos DNS na zona para as VMs na rede virtual resolução B. Com esta configuração, será observar o comportamento seguinte para consultas DNS direta e inversa:
* Uma consulta DNS de VNETB VM1 da rede virtual para VNETA-VM1.contoso.com, resolução B, irá receber uma resposta DNS que contém o IP privado VNETA VM1.
* Uma consulta DNS inverso (PTR) de VNETB VM2 a rede virtual para 10.1.0.1, resolução B, irá receber uma resposta DNS que contém o VNETB-VM1.contoso.com FQDN. O motivo é que as consultas DNS inverso estão confinadas a mesma rede virtual. 
* Uma consulta DNS inverso (PTR) de VNETB VM3 a rede virtual para 10.0.0.1, resolução B, irá receber NXDOMAIN. O motivo é que as consultas DNS inverso apenas passam para a mesma rede virtual. 


![Várias resoluções de rede Virtual](./media/private-dns-scenarios/multi-vnet-resolution.png)

## <a name="scenario-split-horizon-functionality"></a>Cenário: Funcionalidade de divisão Horizon

Neste cenário, tem um caso de utilização onde pretende tenha em consideração o comportamento de resolução DNS diferente, dependendo de onde o cliente se encontre (dentro do Azure ou horizontalmente na internet), para a mesma zona DNS. Por exemplo, pode ter uma versão pública e privada da sua aplicação com funcionalidades diferentes ou comportamento, mas pretende utilizar o mesmo nome de domínio para ambas as versões. Este cenário pode ser realizado com o DNS do Azure através da criação de uma zona DNS público, bem como uma zona de privada com o mesmo nome.

O diagrama seguinte ilustra este cenário. Tiver uma rede virtual A que tem duas VMs (VNETA VM1 e VNETA VM2) que tem ambos os IPs privados e IPs públicos atribuída. Pode criar uma zona DNS pública denominada contoso.com e registar os IPs públicos para estas VMs como registos DNS na zona. Também criar uma zona DNS de privada também denominada contoso.com especificando A que a rede virtual do registo. Azure regista automaticamente as VMs como registos para a zona privada apontar para as respetivas IPs privados.

Agora, quando um cliente de internet emite uma consulta DNS para procurar VNETA VM1.contoso.com, o Azure irá devolver o registo do IP público da zona pública. Se a mesma consulta DNS é emitida por outra VM (por exemplo: VNETA VM2) na mesma rede virtual A, Azure irá devolver o registo de IP privado da zona privada. 

![Resolução de divisão Brian](./media/private-dns-scenarios/split-brain-resolution.png)

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre zonas DNS privadas, veja [Utilizar o DNS do Azure para domínios privados](private-dns-overview.md).

Saiba como [criar uma zona DNS privada](./private-dns-getstarted-powershell.md) no DNS do Azure.

Saiba mais sobre registos e zonas DNS, visitando: [DNS zonas e as regista descrição geral](dns-zones-records.md).

Saiba mais sobre algumas das outras principais [capacidades de rede](../networking/networking-overview.md) do Azure.

