---
title: Preparar o destino (físico para o Azure) | Microsoft Docs
description: Este artigo descreve como preparar o ambiente do Azure para iniciar a replicar servidores físicos que executem Windows ou Linux para o Azure.
services: site-recovery
documentationcenter: ''
author: bsiva
manager: abhemraj
editor: ''
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 02/22/2018
ms.author: bsiva
ms.openlocfilehash: 6704ddc24db8415051a6064bbde79a6fc527871a
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/07/2018
ms.locfileid: "29768418"
---
# <a name="prepare-target-physical-to-azure"></a>Preparar o destino (físico para o Azure)
> [!div class="op_single_selector"]
> * [VMware para o Azure](./site-recovery-prepare-target-vmware-to-azure.md)
> * [Físico para o Azure](./site-recovery-prepare-target-physical-to-azure.md)

Este artigo descreve como preparar o ambiente do Azure para iniciar a replicar servidores físicos (x64) que executem Windows ou Linux no Azure.

## <a name="prerequisites"></a>Pré-requisitos

O artigo assume:
- Criou um cofre dos serviços de recuperação para proteger os servidores físicos. Pode criar um cofre dos serviços de recuperação do [portal do Azure](http://portal.azure.com "portal do Azure").
- Tiver [configuração do ambiente no local](./site-recovery-set-up-physical-to-azure.md) replicar servidores físicos no Azure.

## <a name="prepare-target"></a>Preparar o destino

Depois de concluir o **objetivo de proteção do passo 1:Select** e **passo 2: preparar a origem**, é direcionado para **passo 3: destino**

![Preparar o destino](./media/site-recovery-prepare-target-physical-to-azure/prepare-target-physical-to-azure.png)

1. **Subscrição:** no menu pendente, selecione a subscrição que pretende replicar os servidores físicos para.
2. **Modelo de implementação:** selecione o modelo de implementação (clássico ou do Resource Manager)

Com base no modelo de implementação escolhida, é executada uma validação para se certificar de que tem pelo menos uma conta de armazenamento compatíveis e de rede virtual na subscrição de destino para replicar e ativação pós-falha os servidores físicos para.

Depois de validações concluir com êxito, clique em OK para ir para o passo seguinte.

Se não tiver uma conta de armazenamento do Resource Manager compatível ou de rede virtual, pode criar um clicando a **+ contas de armazenamento** ou **+ rede** botões na parte superior da página.

## <a name="next-steps"></a>Passos Seguintes
[Configurar as definições de replicação](./site-recovery-setup-replication-settings-vmware.md).
