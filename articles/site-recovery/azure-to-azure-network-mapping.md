---
title: Mapear as redes virtuais entre duas regiões do Azure no Azure Site Recovery | Documentos da Microsoft
description: O Azure Site Recovery coordena a replicação, ativação pós-falha e recuperação de máquinas virtuais e servidores físicos. Saiba mais sobre a ativação pós-falha para o Azure ou para um datacenter secundário.
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
editor: ''
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/06/2018
ms.author: manayar
ms.openlocfilehash: aed804a257376308c668ce0c2f3e8ce652ee9b3f
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "42054334"
---
# <a name="map-virtual-networks-in-different-azure-regions"></a>Mapear as redes virtuais em diferentes regiões do Azure


Este artigo descreve como mapear duas instâncias da rede Virtual do Azure localizados em diferentes regiões do Azure entre si. Mapeamento de rede assegura que quando uma máquina virtual replicada é criada na região do Azure de destino, a máquina virtual também é criada na rede virtual que está mapeada para a rede virtual da máquina virtual de origem.  

## <a name="prerequisites"></a>Pré-requisitos
Antes de mapear redes, certifique-se de que criou uma [rede virtual do Azure](../virtual-network/virtual-networks-overview.md) na região de origem e a região do Azure de destino.

## <a name="map-virtual-networks"></a>Mapear as redes virtuais

Para mapear uma rede virtual do Azure que está localizada numa região do Azure (rede de origem) para uma rede virtual que está localizada em outra região (rede de destino), para máquinas virtuais do Azure, aceda a **infraestrutura do Site Recovery**  >  **Mapeamento de rede**. Crie um mapeamento de rede.

![Janela de mapeamentos de rede - criar um mapeamento de rede](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


No exemplo seguinte, a máquina virtual está em execução na região Leste asiático. A máquina virtual está a ser replicada para a região do Sudeste asiático.

Para criar um mapeamento de rede a partir da região da Ásia Oriental para a região do Sudeste asiático, selecione a localização de rede de origem e a localização de rede de destino. Em seguida, selecione **OK**.

![Adicionar a janela de mapeamento de rede - Selecione as localizações de origem e destino para a rede de origem](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


Repita o processo anterior para criar um mapeamento de rede a partir da região do Sudeste asiático para a região da Ásia Oriental.

![Adicionar o painel de mapeamento de rede - Selecione as localizações de origem e destino para a rede de destino](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="map-a-network-when-you-enable-replication"></a>Mapear uma rede, quando ativa a replicação

Quando replicar uma máquina virtual de uma região do Azure para outra região pela primeira vez, não se existir nenhum mapeamento de rede, pode definir a rede de destino quando configurar a replicação. Com base nesta definição, do Azure Site Recovery cria mapeamentos de rede de região de origem para a região de destino e de região de destino para a região de origem.   

![Configurar o painel de definições – escolha a localização de destino](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

Por predefinição, o Site Recovery cria uma rede na região de destino que é idêntica à rede de origem. O site Recovery cria uma rede adicionando **-asr** como sufixo ao nome da rede de origem. Para escolher uma rede que já tenha sido criada, selecione **personalizar**.

![Personalizar o painel de definições de destino - nome do grupo de recursos do conjunto de destino e nome de rede virtual de destino](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)

Se já tiver ocorrido mapeamento da rede, não é possível alterar a rede virtual de destino, quando ativa a replicação. Neste caso, para alterar a rede virtual de destino, modifique o mapeamento de rede existente.  

![Destino de personalizar o painel de definições - definir o nome de grupo de recursos de destino](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Modificar o painel de mapeamento de rede - modificar um nome de rede virtual de destino existente](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> Se modificar um mapeamento de rede de região A região B, certifique-se de que também modificar o mapeamento de rede de região B para a região A.
>
>


## <a name="subnet-selection"></a>Seleção de sub-rede
A sub-rede da máquina virtual de destino é selecionada com base no nome da sub-rede da máquina virtual de origem. Se uma sub-rede que tem o mesmo nome que a máquina virtual de origem está disponível na rede de destino, que a sub-rede está definida para a máquina virtual de destino. Se uma sub-rede com o mesmo nome não existir na rede de destino, por ordem alfabética primeira sub-rede está definida como a sub-rede de destino.

Para modificar a sub-rede, vá para o **computação e rede** definições para a máquina virtual.

![Janela de propriedades de computação de computação e rede](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a>Endereço IP

O endereço IP para cada interface de rede da máquina virtual de destino está definido conforme descrito nas seções a seguir.

### <a name="dhcp"></a>DHCP
Se a interface de rede da máquina virtual de origem utilizar DHCP, a interface de rede da máquina virtual de destino também é definida para utilizar DHCP.

### <a name="static-ip-address"></a>Endereço IP estático
Se a interface de rede da máquina virtual de origem utilizar um endereço IP estático, a interface de rede da máquina virtual de destino também é definida para utilizar um endereço IP estático. As secções seguintes descrevem como um endereço IP estático está definido.

### <a name="ip-assignment-behavior-during-failover"></a>Comportamento de atribuição de IP durante a ativação pós-falha
#### <a name="1-same-address-space"></a>1. Mesmo espaço de endereços

Se a sub-rede de origem e a sub-rede de destino tiverem o mesmo espaço de endereços, o endereço IP da interface de rede da máquina virtual de origem está definido como o endereço IP de destino. Se o mesmo endereço IP não estiver disponível, o endereço IP disponível seguinte está definido como o endereço IP de destino.

#### <a name="2-different-address-spaces"></a>2. Espaços de endereços diferentes

Se a sub-rede de origem e a sub-rede de destino tiverem espaços de endereços diferentes, o endereço seguinte disponível de IP na sub-rede de destino está definido como o endereço IP de destino.


### <a name="ip-assignment-behavior-during-test-failover"></a>Comportamento de atribuição de IP durante a ativação pós-falha de teste
#### <a name="1-if-the-target-network-chosen-is-the-production-vnet"></a>1. Se a rede de destino escolhida é vNet em modo de produção
- O IP de recuperação (IP de destino) será um IP estático, mas ele **não será o mesmo endereço IP** como reservado para a ativação pós-falha.
- O endereço IP atribuído será o próximo IP disponível do fim do intervalo de endereços de sub-rede.
- Para por exemplo, se o IP estático da VM de origem está configurado para ser: 10.0.0.19 e ativação pós-falha de teste foi tentada com a rede de produção configurado: ***dr-PROD-nw***, com o intervalo de sub-rede que 10.0.0.0/24. </br>
A VM com ativação pós-falha seria atribuída com - próximo IP disponível do fim do intervalo de endereços de sub-rede é: 10.0.0.254 </br>

**Nota:** a terminologia **vNet de produção** é chamado de rede especiais de destino mapeada durante a configuração da recuperação após desastre.
####<a name="2-if-the-target-network-chosen-is-not-the-production-vnet-but-has-the-same-subnet-range-as-production-network"></a>2. Se a rede de destino selecionada não é a vNet de produção, mas tem o mesmo intervalo de sub-rede de rede de produção 

- O IP de recuperação (IP de destino) será um IP estático com o **mesmo endereço IP** (ou seja, configurado endereço IP estático) como reservado para a ativação pós-falha. Desde que o mesmo endereço IP está disponível.
- Se o IP estático configurado já está atribuído a algum outro VM/dispositivo, em seguida, o IP de recuperação será o próximo IP disponível do fim do intervalo de endereços de sub-rede.
- Para por exemplo, se o IP estático da VM de origem está configurado para ser: 10.0.0.19 e ativação pós-falha de teste foi tentada com uma rede de teste: ***dr-não-PROD-nw***, com o mesmo intervalo de sub-rede da rede de produção - 10.0.0.0/24. </br>
  A VM com ativação pós-falha seria atribuída com seguinte IP estático </br>
    - configurado o IP estático: 10.0.0.19 se o IP está disponível.
    - Próximo IP disponível: 10.0.0.254 se o endereço IP 10.0.0.19 já se encontra em usar.


Para modificar o IP de destino em cada interface de rede, vá para o **computação e rede** definições para a máquina virtual.</br>
Como prática recomendada é sempre aconselhável para escolher uma rede de teste para efetuar a ativação pós-falha de teste.
## <a name="next-steps"></a>Passos Seguintes

* Revisão [documentação de orientação para replicar máquinas virtuais do Azure de rede](site-recovery-azure-to-azure-networking-guidance.md).
