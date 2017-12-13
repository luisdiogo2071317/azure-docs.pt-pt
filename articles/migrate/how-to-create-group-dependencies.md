---
title: "Refinar um grupo de avaliação com mapeamento de dependência de grupo no Azure migrar | Microsoft Docs"
description: "Descreve como refinar uma avaliação utilizando o mapeamento de grupo de dependência no serviço Azure migrar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: c30d6546e7c2d471d4b262a8af1ce593b2c1c3fb
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2017
---
# <a name="refine-a-group-using-group-dependency-mapping"></a>Refinar um grupo utilizando o mapeamento de grupo de dependência

Este artigo descreve como configurar o mapeamento de dependência de grupo para [Azure migrar](migrate-overview.md) avaliação. Normalmente, utiliza este método quando pretender adicione refinar as definições para um grupo existente, a verificação entre dependências de grupo, antes de executar uma avaliação. Grupos para os quais pretende executar o mapeamento de grupo de dependência não devem conter mais de 10 máquinas.  

## <a name="modify-a-group"></a>Modificar um grupo

1. O Azure migrar em projeto, **gerir**, clique em **grupos**e selecione o grupo.
2. Na página de grupo, clique em **dependências de vista**, para abrir o mapa de dependência do grupo. 

     ![Grupo de vista](./media/how-to-create-group-dependencies/create-group.png)

3. Para ver mais granulares dependências, clique em modificar o intervalo de tempo. Por predefinição, o intervalo é uma hora. Pode modificar o intervalo de tempo, ou especificar o início e fim datas e duração.
4. Utilize Ctrl + clique para selecionar máquinas no mapa. Adicionar ou remover máquinas do mapa.
    - Só é possível adicionar máquinas que tenham sido detetadas.
    - Adição e remoção de um grupo de máquinas invalidam passado avaliações do mesmo.
    - Opcionalmente, pode criar uma nova avaliação quando modificar o grupo.
5. Clique em **OK** para guardar o grupo.

    ![Adicionar e remover](./media/how-to-create-group-dependencies/add-remove.png)

Se pretende verificar as dependências de um computador específico que aparece no mapa de dependência do grupo, [configurar o mapeamento de dependência de máquina](how-to-create-group-machine-dependencies.md).


## <a name="next-steps"></a>Passos seguintes

[Saiba mais](concepts-assessment-calculation.md) sobre como avaliações são calculadas.
