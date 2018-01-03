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
ms.openlocfilehash: b60c746ae6c243d2b448056f8768df3baaed4cc1
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/23/2017
---
# <a name="manage-virtual-machine-network-interfaces-for-on-premises-to-azure-scenarios"></a>Gerir as interfaces de rede de máquina virtual no local para cenários do Azure

Uma máquina virtual no Azure tem de ter, pelo menos, uma interface de rede ligada ao mesmo e podem ter tantos interfaces de rede ligados ao mesmo como o suporta de tamanho VM. Por predefinição, a primeira interface de rede ligada a uma máquina virtual do Azure está definida como a interface de rede principal. Todas as outras interfaces de rede na máquina virtual são interfaces de rede secundárias. Por predefinição, todo o tráfego de saída da máquina virtual é enviado o endereço IP atribuído à configuração de IP primária da interface de rede principal.

Num ambiente no local, as máquinas virtuais/servidores pode ter várias interfaces de rede para redes diferentes no ambiente. Redes diferentes, normalmente, são utilizados para efetuar operações específicas, tais como atualizações, a manutenção, a acesso à internet, etc. Quando a migração ou a efetuar ativação pós-falha do Azure a partir de um ambiente no local, tenha em atenção que as interfaces de rede na mesma máquina virtual tem todos os de estar ligadas à mesma rede virtual.

Por predefinição, a recuperação de sites cria como interfaces numa máquina virtual do Azure de rede estão ligadas ao servidor no local. Pode evitar criar interfaces de rede redundantes durante a migração ou a ativação pós-falha ao editar as definições de interface de rede com as definições para a máquina virtual replicada.

## <a name="select-the-target-network"></a>Selecione a rede de destino

Para VMware e máquinas físicas e para máquinas virtuais Hyper-V (sem VMM), pode especificar a rede virtual de destino para máquinas virtuais individuais. Para máquinas virtuais de Hyper-V geridas com o VMM, [mapeamento da rede](site-recovery-network-mapping.md) é utilizado para mapear redes VM no servidor VMM de origem e redes do Azure de destino.

1. Em 'Itens replicados' num cofre dos serviços de recuperação, clique em qualquer item replicada para aceder às definições para que o item replicada.

2. Clique no separador "Computação e rede" para aceder as definições de rede para o item replicada.

3. Em 'Propriedades da rede', escolha uma rede virtual da lista de interfaces de rede disponíveis.

    ![Computação e rede](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/compute-and-network.png)

Modificar a rede de destino afeta todas as interfaces de rede para que a máquina virtual específica.

Para as nuvens do VMM, modificar o mapeamento da rede afeta todas as máquinas virtuais e as respetivas interfaces de rede.

## <a name="select-the-target-interface-type"></a>Selecione o tipo de interface de destino

Na secção 'Interfaces de rede' do painel "Computação e rede", pode ver e editar as definições da interface de rede e especificar o tipo de interface de rede de destino.

- A **primário** interface de rede não é necessária para a ativação pós-falha.
- Interfaces de todos os outro rede selecionada, se qualquer um, são **secundário** interfaces de rede.
- Selecione **não utilize** para excluir uma interface de rede da criação na ativação pós-falha.

Por predefinição, quando ativar a replicação, a recuperação de sites seleciona todos os detetada interfaces de rede no servidor no local, marcar um como 'Primary' e todos os outros como 'Secundário' interfaces de rede. Qualquer interfaces subsequentes adicionados no servidor no local são marcados 'Não utilizam' por predefinição. Ao adicionar mais interfaces de rede, certifique-se de que o tamanho de destino correto máquina virtual do Azure está selecionado para acomodar todas as interfaces de rede necessários.

## <a name="modifying-network-interface-settings"></a>Modificar as definições da interface de rede

Pode modificar a sub-rede e IP para interfaces de rede de um item replicadas. Se não for especificado um IP, a recuperação de sites atribuir o IP disponível seguinte da sub-rede para a interface de rede na ativação pós-falha.

1. Clique em qualquer interface de rede disponível para abrir o painel de definições de interface de rede.

2. Escolha a sub-rede pretendida na lista de sub-redes disponíveis.

3. Introduza o IP pretendido (conforme necessário).

    ![Definições da interface de rede](./media/site-recovery-manage-network-interfaces-on-premises-to-azure/network-interface-settings.png)

4. Clique em OK para terminar a edição e voltar ao painel de 'Computação e rede'.

5. Repita os passos 1 a 4 para outras interfaces de rede.

6. Clique em 'Guardar' para guardar todas as alterações.

## <a name="next-steps"></a>Passos Seguintes
  [Saiba mais](../virtual-network/virtual-network-network-interface-vm.md) sobre interfaces de rede para máquinas virtuais do Azure.
