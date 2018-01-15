---
title: "Gerir as interfaces de rede no Azure Site Recovery para no local para cenários do Azure | Microsoft Docs"
description: "Descreve como gerir interfaces de rede no local para cenários do Azure com o Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/22/2017
ms.author: manayar
ms.openlocfilehash: 036d5c2945bd6730d65f88f72c9377047fefcde6
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/13/2018
---
# <a name="manage-virtual-machine-network-interfaces-for-on-premises-to-azure-scenarios"></a>Gerir as interfaces de rede de máquina virtual no local para cenários do Azure

Uma máquina virtual (VM) no Azure tem de ter, pelo menos, uma interface de rede ligada ao mesmo. Pode ter como muitas anexadas ao mesmo como o suporta de tamanho VM de interfaces de rede. 

Por predefinição, a primeira interface de rede ligada a uma máquina virtual do Azure está definida como a interface de rede principal. Todas as outras interfaces de rede na máquina virtual são interfaces de rede secundárias. Também por predefinição, todo o tráfego de saída da máquina virtual é enviado o endereço IP que está atribuído à configuração de IP primária da interface de rede principal.

Num ambiente no local, as máquinas virtuais ou servidores podem ter várias interfaces de rede para redes diferentes no ambiente. Redes diferentes, normalmente, são utilizados para efetuar operações específicas, tais como atualizações, a manutenção e o acesso à internet. Quando estiver a migrar ou entrar em ativação pós-falha para o Azure a partir de um ambiente no local, tenha em atenção que as interfaces de rede na mesma máquina virtual tem todos os de estar ligadas à mesma rede virtual.

Por predefinição, o Azure Site Recovery cria como interfaces numa máquina virtual do Azure de rede estão ligadas ao servidor no local. Pode evitar criar interfaces de rede redundantes durante a migração ou a ativação pós-falha ao editar as definições de interface de rede com as definições para a máquina virtual replicada.

## <a name="select-the-target-network"></a>Selecione a rede de destino

Para VMware e máquinas físicas e para máquinas virtuais Hyper-V (sem System Center Virtual Machine Manager), pode especificar a rede virtual de destino para máquinas virtuais individuais. Para máquinas virtuais do Hyper-V geridas com o Virtual Machine Manager, utilize [mapeamento da rede](site-recovery-network-mapping.md) mapear redes VM num servidor de origem do Virtual Machine Manager e redes do Azure de destino.

1. Em **replicado itens** num cofre dos serviços de recuperação, selecione qualquer item replicada para aceder às definições para que o item replicada.

2. Selecione o **computação e rede** separador para aceder as definições de rede para o item replicada.

3. Em **propriedades de rede**, escolha uma rede virtual da lista de interfaces de rede disponíveis.

    ![Definições de rede](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/compute-and-network.png)

Modificar a rede de destino afeta todas as interfaces de rede para que a máquina virtual específica.

Para nuvens do Virtual Machine Manager, modificar o mapeamento da rede afeta todas as máquinas virtuais e as respetivas interfaces de rede.

## <a name="select-the-target-interface-type"></a>Selecione o tipo de interface de destino

Sob o **interfaces de rede** secção o **computação e rede** painel, pode ver e editar as definições da interface de rede. Também pode especificar o tipo de interface de rede de destino.

- A **primário** interface de rede não é necessária para a ativação pós-falha.
- Interfaces de todos os outro rede selecionada, se qualquer um, são **secundário** interfaces de rede.
- Selecione **não utilize** para excluir uma interface de rede da criação na ativação pós-falha.

Por predefinição, quando estiver a ativar a replicação, a recuperação de sites seleciona todas as interfaces de rede detetados no servidor no local. Marca uma como **primário** e todos os outros como **secundário**. Qualquer interfaces subsequentes adicionados no servidor no local estão marcados **não utilize** por predefinição. Quando estiver a adicionar mais interfaces de rede, certifique-se de que o tamanho de destino correto máquina virtual do Azure está selecionado para acomodar todas as interfaces de rede necessários.

## <a name="modify-network-interface-settings"></a>Modificar as definições de interface de rede

Pode modificar a sub-rede e o endereço IP para interfaces de rede de um item replicadas. Se não for especificado um endereço IP, a recuperação de sites atribuir o endereço IP disponível seguinte da sub-rede para a interface de rede na ativação pós-falha.

1. Selecione qualquer interface de rede disponível para abrir as definições de interface de rede.

2. Escolha a sub-rede pretendida na lista de sub-redes disponíveis.

3. Introduza o endereço IP pretendido (conforme necessário).

    ![Definições da interface de rede](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/network-interface-settings.png)

4. Selecione **OK** para concluir a edição e voltar para o **computação e rede** painel.

5. Repita os passos 1 a 4 para outras interfaces de rede.

6. Selecione **guardar** para guardar todas as alterações.

## <a name="next-steps"></a>Passos Seguintes
  [Saiba mais](../virtual-network/virtual-network-network-interface-vm.md) sobre interfaces de rede para máquinas virtuais do Azure.
