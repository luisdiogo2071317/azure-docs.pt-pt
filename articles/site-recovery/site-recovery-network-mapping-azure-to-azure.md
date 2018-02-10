---
title: "Mapear redes virtuais entre duas regiões do Azure no Azure Site Recovery | Microsoft Docs"
description: "O Azure Site Recovery coordena a replicação, ativação pós-falha e recuperação de máquinas virtuais e servidores físicos. Saiba mais sobre a ativação pós-falha para o Azure ou para um datacenter secundário."
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/15/2017
ms.author: manayar
ms.openlocfilehash: d7dd35a8382f4a99ababbe804c5c71b29148c44a
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="map-virtual-networks-in-different-azure-regions"></a>Mapear redes virtuais em diferentes regiões do Azure


Este artigo descreve como mapear duas instâncias do Azure Virtual Network localizadas em diferentes regiões Azure entre si. Mapeamento de rede assegura que quando é criada uma máquina virtual replicada no destino da região do Azure, a máquina virtual também é criada na rede virtual que está mapeada para a rede virtual da máquina virtual de origem.  

## <a name="prerequisites"></a>Pré-requisitos
Antes de mapear redes, certifique-se de que criou um [rede virtual do Azure](../virtual-network/virtual-networks-overview.md) na região de origem e a região do Azure de destino.

## <a name="map-virtual-networks"></a>Mapear redes virtuais

Para mapear uma Azure virtual network que está localizada numa região do Azure (rede de origem) a uma rede virtual que está localizada numa região de outro (rede de destino), máquinas virtuais do Azure, aceda a **infraestrutura de recuperação de Site**  >  **Mapeamento de rede**. Crie um mapeamento da rede.

![Janela de mapeamentos de rede - criar um mapeamento da rede](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


No exemplo seguinte, a máquina virtual está em execução na região Ásia Oriental. A máquina virtual está a ser replicada para a região Sudeste asiático.

Para criar um mapeamento de rede a partir da região de Oriental à região Sudeste asiático, selecione a localização de rede de origem e a localização de rede de destino. Em seguida, selecione **OK**.

![Adicionar a janela de mapeamento de rede - selecionar localizações de origem e destino para a rede de origem](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


Repetir o processo anterior para criar um mapeamento de rede a partir da região de Sudeste asiático à região Ásia Oriental.

![Adicionar o painel de mapeamento de rede - selecionar localizações de origem e destino para a rede de destino](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="map-a-network-when-you-enable-replication"></a>Mapear uma rede, ao ativar a replicação

Quando se replica uma máquina virtual de uma região do Azure noutra região pela primeira vez, não se existir nenhum mapeamento da rede, pode definir a rede de destino quando configurou a replicação. Com base nesta definição, a recuperação de sites do Azure cria mapeamentos de rede a região de origem para a região de destino e a região de destino para a região de origem.   

![Configurar o painel de definições – escolha a localização de destino](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

Por predefinição, a recuperação de sites cria uma rede na região de destino que é idêntica à rede de origem. Recuperação de sites cria uma rede adicionando **-asr** como um sufixo ao nome de rede de origem. Para escolher uma rede que já tiver sido criada, selecione **personalizar**.

![Personalizar o painel de definições de destino - nome do grupo de recursos do conjunto de destino e o nome de rede virtual de destino](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)

Se já tiver ocorrido mapeamento da rede, não é possível alterar a rede virtual de destino ao ativar a replicação. Neste caso, para alterar a rede virtual de destino, modifique o mapeamento de rede existente.  

![Destino de personalizar o painel de definições - definir o nome de grupo de recursos de destino](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Modificar o painel de mapeamento de rede - modificar um nome de rede virtual de destino existente](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> Se modificar um mapeamento de rede de região A região B, certifique-se de que também modificar o mapeamento da rede de região B região A.
>
>


## <a name="subnet-selection"></a>Seleção de sub-rede
A sub-rede da máquina virtual de destino está selecionada com base no nome de sub-rede da máquina virtual de origem. Se uma sub-rede que tem o mesmo nome que a máquina virtual de origem está disponível na rede de destino, essa sub-rede está definida para a máquina virtual de destino. Se uma sub-rede com o mesmo nome não existir na rede de destino, por ordem alfabética primeira sub-rede está definida como a sub-rede de destino. 

Para modificar a sub-rede, vá para o **computação e rede** definições para a máquina virtual.

![Janela de propriedades de computação de rede e de computação](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a>Endereço IP

O endereço IP para cada interface de rede da máquina virtual de destino está configurado conforme descrito nas secções seguintes.

### <a name="dhcp"></a>DHCP
Se a interface de rede da máquina virtual de origem utiliza DHCP, a interface de rede da máquina virtual de destino seja também definida para utilizar DHCP.

### <a name="static-ip-address"></a>Endereço IP estático
Se a interface de rede da máquina virtual de origem utilizar um endereço IP estático, a interface de rede da máquina virtual de destino também está configurada para utilizar um endereço IP estático. As secções seguintes descrevem como um endereço IP estático está definido.

#### <a name="same-address-space"></a>Mesmo espaço de endereços

Se a sub-rede de origem e a sub-rede de destino tem o mesmo espaço de endereços, o endereço IP da interface de rede da máquina virtual de origem está definido como o endereço IP de destino. Se o mesmo endereço IP não estiver disponível, o seguinte endereço IP disponível é definido como o endereço IP de destino.

#### <a name="different-address-spaces"></a>Espaços de endereços diferente

Se a sub-rede de origem e a sub-rede de destino tiverem espaços de endereços diferente, o seguinte endereço IP disponível na sub-rede de destino está definido como o endereço IP de destino.

Para modificar o IP de destino em cada interface de rede, vá para o **computação e rede** definições para a máquina virtual.

## <a name="next-steps"></a>Passos Seguintes

* Reveja [redes orientações para replicar máquinas virtuais do Azure](site-recovery-azure-to-azure-networking-guidance.md).
