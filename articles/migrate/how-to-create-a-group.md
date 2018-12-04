---
title: Agrupe máquinas para avaliação com o Azure Migrate | Documentos da Microsoft
description: Descreve como agrupar máquinas antes de executar uma avaliação com o serviço Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 11/28/2018
ms.author: raynew
ms.openlocfilehash: 3f90fbb4ae30f8cc7730385730c39321974a94c4
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52850708"
---
# <a name="group-machines-for-assessment"></a>Agrupe máquinas para avaliação

Este artigo descreve como criar um grupo de máquinas para avaliação por [do Azure Migrate](migrate-overview.md). O Azure Migrate avalia as máquinas do grupo para verificar se eles são adequados para migração para o Azure e oferece estimativas de dimensionamento e custos para executar a máquina no Azure. Se conhece as máquinas que tem de ser migradas em conjunto, pode criar manualmente o grupo no Azure Migrate usando o seguinte método. Se tiver não certeza muito sobre as máquinas que precisam ser agrupados em conjunto, pode utilizar a funcionalidade de visualização de dependência no Azure Migrate para criar grupos. [Saiba mais.](how-to-create-group-machine-dependencies.md)

## <a name="create-a-group"></a>Criar um grupo

1. Na **descrição geral** do projeto do Azure Migrate, em gerir, clique em **grupos** > **+ grupo**e especifique um nome de grupo.
2. Adicionar uma ou mais máquinas ao grupo e clique em **criar**.
3. Opcionalmente, pode selecionar para executar uma avaliação nova para o grupo.

    ![Criar um grupo](./media/how-to-create-a-group/create-group.png)

Depois de criar o grupo, pode modificá-la ao selecionar o grupo no **grupos** página e, em seguida, adicionar ou remover computadores.

## <a name="next-steps"></a>Passos Seguintes

- Aprenda a usar [mapeamento de dependências de máquina](how-to-create-group-machine-dependencies.md) para criar grupos de confiança elevada.
- [Saiba mais](concepts-assessment-calculation.md) sobre como são calculadas as avaliações.
