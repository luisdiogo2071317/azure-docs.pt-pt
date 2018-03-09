---
title: "Preparar o destino (físico para o Azure) | Microsoft Docs"
description: "Este artigo descreve como preparar o ambiente do Azure para iniciar a replicar servidores físicos que executem Windows ou Linux para o Azure."
services: site-recovery
author: bsiva
manager: abhemraj
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: bsiva
ms.openlocfilehash: a2465bb3397a175b6ad8b8be0de933dfae1dee5b
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="prepare-target-vmware-to-azure"></a>Preparar o destino (VMware para o Azure)

Este artigo descreve como preparar o ambiente do Azure para iniciar a replicar servidores físicos (x64) que executem Windows ou Linux no Azure.

## <a name="prerequisites"></a>Pré-requisitos

O artigo assume:
- Criou um cofre dos serviços de recuperação para proteger os servidores físicos. Pode criar um cofre dos serviços de recuperação do [portal do Azure](http://portal.azure.com "portal do Azure").
- Tiver [configuração do ambiente no local](physical-azure-disaster-recovery.md) replicar servidores físicos no Azure.

## <a name="prepare-target"></a>Preparar o destino

Depois de concluir o **objetivo de proteção do passo 1:Select** e **passo 2: preparar a origem**, é direcionado para **passo 3: destino**

![Preparar o destino](./media/physical-azure-set-up-target/prepare-target-physical-to-azure.png)

1. **Subscrição:** no menu pendente, selecione a subscrição que pretende replicar os servidores físicos para.
2. **Modelo de implementação:** selecione o modelo de implementação (clássico ou do Resource Manager)

Com base no modelo de implementação escolhida, é executada uma validação para se certificar de que tem pelo menos uma conta de armazenamento compatíveis e de rede virtual na subscrição de destino para replicar e ativação pós-falha os servidores físicos para.

Depois de validações concluir com êxito, clique em OK para ir para o passo seguinte.

Se não tiver uma conta de armazenamento do Resource Manager compatível ou de rede virtual, pode criar um clicando a **+ contas de armazenamento** ou **+ rede** botões na parte superior da página.

## <a name="next-steps"></a>Passos Seguintes
[Configurar as definições de replicação](vmware-azure-set-up-replication.md).
