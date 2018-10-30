---
title: Preparar o ambiente de destino para replicação de VMware para o Azure | Documentos da Microsoft
description: Este artigo descreve como preparar o ambiente do Azure para replicação de VMS de VMware para o Azure de destino.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/29/2018
ms.author: ramamill
ms.openlocfilehash: a6f983b08415659b9a989ebed824cddd210396e1
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233434"
---
# <a name="prepare-the-target-environment-for-disaster-recovery-of-vmware-vms-or-physical-servers-to-azure"></a>Preparar o ambiente de destino para recuperação após desastre de VMs de VMware ou servidores físicos no Azure

Este artigo descreve como preparar o ambiente do Azure para iniciar a replicação de máquinas virtuais VMware ou servidores físicos para o Azure de destino.

## <a name="prerequisites"></a>Pré-requisitos

O artigo supõe:
- Criou um cofre dos serviços de recuperação [portal do Azure](http://portal.azure.com "portal do Azure") para proteger as suas máquinas de origem
- Tiver configurado seu ambiente no local para replicar a origem [máquinas virtuais VMware](vmware-azure-set-up-source.md) ou [servidores físicos](physical-azure-set-up-source.md) para o Azure.

## <a name="prepare-target"></a>Preparar o destino

Depois de concluir o **passo 1: objetivo de proteção de selecionar** e **passo 2: preparar origem**, é direcionado para **passo 3: destino**

![Preparar o destino](./media/vmware-azure-set-up-target/prepare-target-vmware-to-azure.png)

1. **Subscrição:** no menu pendente, selecione a subscrição que pretende replicar as máquinas virtuais ou servidores físicos para o.
2. **Modelo de implementação:** selecione o modelo de implementação (clássico ou do Resource Manager)

Com base no modelo de implementação escolhida, é executada uma validação para garantir que tem pelo menos uma conta de armazenamento compatíveis e a rede virtual na subscrição de destino para replicar e ativação pós-falha sua máquina virtual ou servidor físico para.

Assim que as validações concluir com êxito, clique em OK para ir para o passo seguinte.

Se não tiver uma conta de armazenamento do Resource Manager compatível ou a rede virtual, pode criar uma ao clicar o **+ conta de armazenamento** ou **+ rede** botões na parte superior da página.

## <a name="next-steps"></a>Passos Seguintes
[Configurar definições de replicação](vmware-azure-set-up-replication.md).
