---
title: Reativação pós-falha no Azure Site Recovery | Microsoft Docs
description: Este artigo fornece uma descrição geral dos vários tipos de reativação pós-falha e avisos para ser considerado ao falhar no local com o serviço do Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: guaravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: rajanki
ms.openlocfilehash: 372a7867b47960338d7a1bf7e646fb9fffbe72e1
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
ms.locfileid: "29803557"
---
# <a name="overview-of-failback"></a>Descrição geral da reativação pós-falha

Depois de ter a pós-falha para o Azure, pode falhar novamente para o site no local. Existem dois tipos diferentes de reativação pós-falha que são possíveis com o Azure Site Recovery: 

- A localização original 
- Uma localização alternativa

Se tiver falhado através de uma máquina virtual VMware, pode falhar ao mesmo local máquina virtual de origem, caso ainda exista. Neste cenário, apenas as alterações são replicadas novamente. Este cenário é conhecido como **recuperação da localização original**. Se a máquina virtual no local não existe, o cenário é um **alternativa recuperação numa localização**.

> [!NOTE]
> Pode apenas a reativação pós-falha para o original vCenter e o servidor de configuração. Não é possível implementar um novo servidor de configuração e falhar novamente utilizá-la. Além disso, não é possível adicionar um novo vCenter para o servidor de configuração existente e a reativação pós-falha para o novo vCenter.

## <a name="original-location-recovery-olr"></a>Recuperação da localização original (OLR)
Se escolher para a reativação pós-falha da máquina virtual original, tem de ser satisfeitas as seguintes condições:

* Se a máquina virtual é gerida por um vCenter server, o anfitrião do ESX o destino principal deve ter acesso ao arquivo de dados da máquina virtual.
* Se a máquina virtual está num anfitrião ESX, mas não é gerida pelo vCenter, em seguida, o disco rígido da máquina virtual deve estar no arquivo de dados que o anfitrião de destino principal pode aceder.
* Se a máquina virtual está num anfitrião do ESX e não utilize o vCenter, deve efetuar a deteção do anfitrião ESX de destino principal antes de a voltar a proteger. Isto aplica-se se está a reativar novamente físicas, demasiado.
* Pode efetuar para uma rede de área de armazenamento virtual (vSAN) ou um disco com base no dispositivo sem formato mapeamento (RDM) se os discos já existem e estão ligados à máquina virtual no local.

> [!IMPORTANT]
> É importante ativar disk.enableUUID= TRUE os durante a reativação pós-falha, o serviço do Azure Site Recovery é identificar o VMDK original na máquina virtual para que as alterações pendentes serão escritas. Se este valor não está definido ser verdadeiro, em seguida, o serviço tenta identificar o VMDK no local correspondente na base de melhor esforço. Se o direito VMDK não for encontrado, cria um disco adicional e os dados obtém escrito na sessão que.

## <a name="alternate-location-recovery-alr"></a>Recuperação numa localização alternativa (ALR)
Se a máquina virtual no local não existe antes de proteger novamente a máquina virtual, o cenário seja designado uma recuperação numa localização alternativa. O fluxo de trabalho de reproteção cria novamente a máquina virtual no local. Isto também fará com que uma transferência de dados completa.

* Quando falhar novamente para uma localização alternativa, a máquina virtual é recuperada para o mesmo anfitrião ESX no qual o servidor de destino principal está implementado. O arquivo de dados que é utilizado para criar o disco será o mesmo arquivo de dados que foi selecionado quando proteger novamente a máquina virtual.
* Pode efetuar a cópia apenas a um sistema de ficheiros de máquina virtual (VMFS) ou o arquivo de dados de vSAN. Se tiver um RDM, reproteção e a reativação pós-falha não irão funcionar.
* Reproteção envolve uma transferência de dados inicial grande que é seguida das alterações. Este processo existe porque a máquina virtual não existe no local. Os dados completos tem de ser replicadas de volta. Este reproteção também irá demorar mais tempo do que uma recuperação da localização original.
* Não pode falhar novamente para discos com base em RDM. Apenas novos discos da máquina virtual (VMDKs) podem ser criados num arquivo de dados VMFS/vSAN.

> [!NOTE]
> Um computador físico, quando a ativação pós-falha para o Azure, pode ser não conseguiu fazer uma cópia apenas como uma máquina virtual VMware. Segue-se mesmo fluxo de trabalho, como a recuperação para localização alternativa. Certifique-se de que o se detetar pelo menos um servidor de destino principal e os anfitriões ESX/ESXi necessários para o qual terá de voltar a falhar.

## <a name="next-steps"></a>Passos Seguintes

Siga os passos para efetuar o [operação de reativação pós-falha](vmware-azure-failback.md).

