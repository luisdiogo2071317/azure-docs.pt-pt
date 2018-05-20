---
title: Grupo de máquinas para avaliação com o Azure migrar | Microsoft Docs
description: Descreve como agrupar máquinas antes de executar uma avaliação com o serviço Azure migrar.
author: rayne-wiselman
ms.service: azure-migrate
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: raynew
ms.openlocfilehash: 5c5b5dc97c0faf43c9543422406c4fa1c30ed679
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="group-machines-for-assessment"></a>Grupo de máquinas para avaliação

Este artigo descreve como criar um grupo de computadores para avaliação por [Azure migrar](migrate-overview.md). Migrar do Azure avalia máquinas do grupo para verificar se serem adequados para a migração para o Azure e fornece estimativas de dimensionamento e custos de execução da máquina no Azure.


## <a name="create-a-group"></a>Criar um grupo

1. No **descrição geral** do projeto migrar do Azure, em gerir, clique em **grupos** > **+ grupo**e especifique um nome de grupo.
2. Adicione uma ou mais máquinas para o grupo e clique em **criar**. 
3. Opcionalmente, pode optar por executar uma nova avaliação para o grupo. 

    ![Criar um grupo](./media/how-to-create-a-group/create-group.png)

Depois do grupo for criado, pode modificá-la ao selecionar o grupo no **grupos** página e adição ou remoção de máquinas.

## <a name="next-steps"></a>Passos Seguintes

- Saiba como utilizar [mapeamento de dependência de máquina](how-to-create-group-machine-dependencies.md) para criar grupos de confiança elevada.
- [Saiba mais](concepts-assessment-calculation.md) sobre como são calculadas as avaliações.
