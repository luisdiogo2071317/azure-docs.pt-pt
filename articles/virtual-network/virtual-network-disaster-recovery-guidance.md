---
title: "Continuidade do negócio de rede virtual | Microsoft Docs"
description: "Saiba o que fazer em caso de uma interrupção do serviço do Azure com impacto na redes virtuais do Azure."
services: virtual-network
documentationcenter: 
author: NarayanAnnamalai
manager: jefco
editor: 
ms.assetid: ad260ab9-d873-43b3-8896-f9a1db9858a5
ms.service: virtual-network
ms.workload: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: narayan;aglick
ms.openlocfilehash: d993144006d1fb17d79ffee4f2da538264a309a4
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="virtual-network--business-continuity"></a>Rede virtual – continuidade do negócio

## <a name="overview"></a>Descrição geral
Uma rede Virtual (VNet) é uma representação lógica da rede na nuvem. Permite-lhe definir o seu próprio espaço de endereços IP privados e o segmento de rede em sub-redes. As VNets serve como um limite de fidedignidade para alojar os recursos de computação, tais como máquinas virtuais do Azure e serviços em nuvem (funções da web/trabalho). Uma VNet permite a comunicação de IP privada direta entre os recursos alojados no mesmo. Pode ligar uma rede virtual a uma rede no local através de uma VPN Gateway ou ExpressRoute.

É criada uma VNet dentro do âmbito de uma região. Pode criar as VNets com o mesmo espaço de endereços em duas regiões diferentes (por exemplo, EUA leste e dos EUA oeste), mas não é possível ligá-los em conjunto. 

## <a name="business-continuity"></a>Continuidade do Negócio

Podem existir várias maneiras diferentes que a aplicação foi interrompida. Uma região foi completamente cortar devido a um desastre natural ou um desastre parcial, devido a uma falha de vários dispositivos ou os serviços. O impacto no serviço de VNet é diferente em cada uma destas situações.

**P: se ocorrer uma falha de uma região de toda, o que devo fazer? Por exemplo, se uma região completamente cortar devido a um desastre natural? O que acontece para as redes virtuais alojadas na região?**

R: a rede virtual e os recursos na região afetado permanece inacessível durante o período de tempo de interrupção do serviço.

![Diagrama de rede Virtual simples](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**P: o que posso recriar a mesma rede virtual numa região diferente?**

R: redes virtuais são recursos bastante simples. Pode invocar as APIs do Azure para criar uma VNet com o mesmo espaço de endereços numa região diferente. Para recriar o ambiente mesmo que estava presente na região afetada, efetuar chamadas de API Reimplementar da web de serviços em nuvem e as funções de trabalho e as máquinas virtuais que tiver. Se tiver conectividade no local, tal como numa implementação híbrida, tem de implementar um novo Gateway de VPN e estabelecer ligação à sua rede no local.

Para criar uma rede virtual, consulte [criar uma rede virtual](manage-virtual-network.md#create-a-virtual-network).

**P: uma réplica de uma VNet uma determinada região ser recriada noutra região antecedência?**

R: Sim, pode criar duas VNets utilizando o mesmo espaço de endereços IP privados e recursos em duas regiões diferentes antecedência. Se alojar serviços para a internet na VNet, foi definiu cópias de segurança Gestor de tráfego para georreplicação-encaminhar o tráfego para a região que está ativa. No entanto, não é possível ligar duas VNets com o mesmo espaço de endereços à sua rede no local, como faria com que problemas de encaminhamento. O tempo de um desastre e perda de uma VNet na região de um, pode ligar a outra VNet na região disponível, com o espaço de endereços correspondentes à sua rede no local.

Para criar uma rede virtual, consulte [criar uma rede virtual](manage-virtual-network.md#create-a-virtual-network).

