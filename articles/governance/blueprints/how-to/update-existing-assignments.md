---
title: Como atualizar uma atribuição de esquema do Azure existente
description: Saiba mais sobre o mecanismo para a atualização de uma atribuição existente no Azure esquemas.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: ecac0fb21a6691874d5e8db49eadd7114d41845f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956205"
---
# <a name="how-to-update-an-existing-blueprint-assignment"></a>Como atualizar uma existente da atribuição do esquema

Quando lhe for atribuído um plano gráfico, a atribuição pode ser atualizada. Há vários motivos para atualizar uma atribuição existente, incluindo:

- Adicionar ou remover [bloqueio do recurso](../concepts/resource-locking.md)
- Alterar o valor de [parâmetros dinâmicos](../concepts/parameters.md#dynamic-parameters)
- Atualizar a atribuição para uma mais recente **publicado** versão do esquema

## <a name="updating-assignments"></a>A atualizar as atribuições

1. Inicie o serviço de esquemas do Azure no portal do Azure ao clicar no **todos os serviços** e a procurar e selecionando **política** no painel esquerdo. Sobre o **política** página, clique em **esquemas**.

1. Selecione **esquemas atribuídos** partir da página à esquerda.

1. Na lista de esquemas, left-click a atribuição do esquema e, em seguida, clique nas **a atribuição de atualizações** ou a atribuição do esquema com o botão direito e selecione **a atribuição de atualizações**.

   ![Atribuição de atualizações](../media/update-existing-assignments/update-assignment.png)

1. O **atribuir esquema** página será carregada previamente preenchido com todos os valores da atribuição original. Pode alterar o **versão da definição do esquema**, o **atribuição de bloqueio** estado e qualquer um dos parâmetros dinâmicos que existem na definição do esquema. Clique em **atribuir** quando terminar de efetuar alterações.

1. Na página de detalhes de atribuição atualizada, ver o estado de novo. Neste exemplo, adicionamos **bloqueio** para a atribuição.

   ![Atribuição atualizada - bloqueada](../media/update-existing-assignments/updated-assignment.png)

1. Para explorar detalhes sobre outras **operações de atribuição** usando a menu pendente. A tabela da **recursos geridos** atualizações por operação de atribuição selecionado.

   ![Operações de atribuição](../media/update-existing-assignments/assignment-operations.png)

## <a name="rules-for-updating-assignments"></a>Regras para atualizar as atribuições

A implementação das atribuições atualizadas segue algumas regras importantes. Estas regras determinam o que acontece com um recurso existente, dependendo da alteração pedida e o tipo de recurso de artefacto a ser implementado ou atualizado.

- Atribuições de Funções
  - Se a função ou o detentor de função (utilizador, grupo ou aplicação) for alterado, é criada uma nova atribuição de função. A atribuição de função anteriormente implementada é deixada no lugar.
- Atribuições de Política
  - Se os parâmetros da atribuição de política são alterados, a atribuição existente é atualizada.
  - Se a definição de atribuição de política é alterada, é criada uma nova atribuição de política. A atribuição de política implementada anteriormente é deixada no lugar.
  - Se o artefacto de atribuição de política é removido do plano gráfico, a atribuição de política implementada anteriormente é deixada no lugar.
- Modelos do Azure Resource Manager
  - O modelo é processado através do Resource Manager como um **colocar**. Cada tipo de recurso lida com isso forma diferente, reveja a documentação para cada recurso incluído determinar o impacto desta ação quando executada pelo esquemas.

## <a name="possible-errors-on-updating-assignments"></a>Possíveis erros sobre como atualizar atribuições

Ao atualizar as atribuições, é possível fazer alterações que não funcionam quando executado. Um exemplo disso é alterar a localização de um grupo de recursos, após já ter sido implementado. Qualquer alteração que são suportadas pelo [do Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md) podem ser feitas, mas qualquer alteração que seria resultarem num erro através do Gestor de recursos do Azure também irá resultar numa falha da atribuição.

Não existe nenhum limite no número de vezes que uma atribuição pode ser atualizado. Assim, se ocorrer um erro, devido a um parâmetro incorreto, um objeto já existente ou uma alteração não permitida pelo Azure Resource Manager, determinar o erro e fazer outra atualização para a atribuição.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre o [ciclo de vida de esquema](../concepts/lifecycle.md)
- Compreender como utilizar [parâmetros estáticos e dinâmicos](../concepts/parameters.md)
- Aprenda a personalizar o [plano gráfico de ordem de sequenciamento](../concepts/sequencing-order.md)
- Descubra como tornar a utilização de [plano gráfico de bloqueio do recurso](../concepts/resource-locking.md)
- Resolver problemas durante a atribuição de um plano gráfico com [resolução de problemas gerais](../troubleshoot/general.md)
