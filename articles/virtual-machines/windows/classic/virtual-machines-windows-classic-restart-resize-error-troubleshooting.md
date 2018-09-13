---
title: VM reiniciar ou redimensionar problemas | Documentos da Microsoft
description: Resolver problemas de implementação clássica ao reiniciar ou redimensionar uma Máquina Virtual de Windows existente no Azure
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: aa854fff-c057-4b8e-ad77-e4dbc39648cc
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: required
ms.date: 06/15/2018
ms.devlang: na
ms.author: delhan
ms.openlocfilehash: 400b20e474257650a22e04c89ddde581bf0552f4
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35970122"
---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-windows-virtual-machine-in-azure"></a>Resolver problemas de implementação clássica ao reiniciar ou redimensionar uma Máquina Virtual de Windows existente no Azure
> [!div class="op_single_selector"]
> * [Clássico](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
> * [Resource Manager](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
> 
> 

Ao tentar iniciar uma Máquina Virtual do Azure (VM) parada ou redimensionar uma VM do Azure existente, o erro comum que encontrar é uma falha de alocação. Este erro resulta quando o cluster ou a região não tem recursos disponíveis ou não suporta o tamanho da VM pedida.

> [!IMPORTANT]
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com os recursos: [Resource Manager e clássico](../../../azure-resource-manager/resource-manager-deployment-model.md).  Este artigo cobre a utilização do modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager.
> 
> 

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a>Registos de auditoria de recolha
Para iniciar a resolução de problemas, recolha os registos de auditoria para identificar o erro associado com o problema.

No portal do Azure, clique em **navegue** > **máquinas virtuais** > *sua máquina virtual do Windows*  >   **As definições** > **registos de auditoria**.

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problema: Erro ao iniciar uma VM parada
Tente iniciar uma VM parada, mas ocorre uma falha de alocação.

### <a name="cause"></a>Causa
O pedido para iniciar a VM parada deve ser tentada no cluster original que aloja o serviço em nuvem. No entanto, o cluster não tem espaço livre disponível para satisfazer o pedido.

### <a name="resolution"></a>Resolução
* Criar um novo serviço de nuvem e associá-lo com qualquer um, uma região ou uma rede virtual baseada em região, mas não um grupo de afinidades.
* Elimine a VM parada.
* Recrie a VM no novo serviço cloud utilizando os discos.
* Inicie a VM recriada.

Se obtiver um erro ao tentar criar um novo serviço cloud, tente novamente mais tarde ou alterar a região para o serviço cloud.

> [!IMPORTANT]
> O novo serviço em nuvem terão um novo nome e o VIP, por isso terá de alterar essas informações para todas as dependências que usará essas informações para o serviço cloud existente.
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problema: Ocorreu um erro ao redimensionar uma VM nova
Tente redimensionar uma VM existente, mas ocorre uma falha de alocação.

### <a name="cause"></a>Causa
O pedido para redimensionar a VM tem de ser tentada no cluster original que aloja o serviço em nuvem. No entanto, o cluster não suporta o tamanho da VM pedida.

### <a name="resolution"></a>Resolução
Reduzir o tamanho da VM pedida e repita o pedido de redimensionamento.

* Clique em **Procurar tudo** > **máquinas virtuais (clássicas)** > *sua máquina virtual* > **definições**  >  **Tamanho**. Para obter passos detalhados, consulte [redimensionar a máquina virtual](https://msdn.microsoft.com/library/dn168976.aspx).

Se não for possível reduzir o tamanho da VM, siga estes passos:

* Criar um novo serviço cloud, garantindo que não é associada a um grupo de afinidade e não associado a uma rede virtual que está ligada a um grupo de afinidades.
* Crie uma VM nova, maior porte no mesmo.

Pode consolidar todas as suas VMs no mesmo serviço cloud. Se o seu serviço cloud existente estiver associado uma rede virtual baseada em região, pode ligar o novo serviço cloud para a rede virtual existente.

Se o serviço em nuvem existente não estiver associado uma rede virtual baseada em região, em seguida, terá de eliminar as VMs no serviço cloud existente e recriá-las no novo serviço em nuvem dos seus discos. No entanto, é importante lembrar-se de que o novo serviço em nuvem terão um novo nome e o VIP, por isso terá de atualizá-los para todas as dependências que a utilizam atualmente estas informações para o serviço cloud existente.

## <a name="next-steps"></a>Passos Seguintes
Se tiver problemas ao criar uma VM do Windows no Azure, veja [resolver problemas de implementação com a criação de uma máquina virtual do Windows no Azure](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

