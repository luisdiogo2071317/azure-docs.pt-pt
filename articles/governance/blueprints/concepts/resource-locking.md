---
title: Compreender o bloqueio de recursos
description: Saiba mais sobre as opções de bloqueios para proteger recursos, ao atribuir um plano gráfico.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 01/23/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 2e281896d45ada8010f24a1f18265a8cdd523d31
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/04/2019
ms.locfileid: "55696998"
---
# <a name="understand-resource-locking-in-azure-blueprints"></a>Compreender o recurso de bloqueio em esquemas do Azure

A criação de ambientes de consistentes e à escala só é realmente importante se existe um mecanismo para manter a consistência. Este artigo explica como bloqueio de recurso funciona em esquemas do Azure.

## <a name="locking-modes-and-states"></a>Modos de bloqueios e de Estados

Modo de bloqueio aplica-se para a atribuição do esquema e tem três opções: **Não bloquearem**, **só de leitura**, ou **não elimine**. O modo de bloqueio é configurado durante a implementação de artefato durante uma atribuição do esquema. Um modo de bloqueio diferente pode ser definido através da atualização da atribuição do esquema.
Modos de bloqueio, no entanto, não pode ser alterado fora de esquemas.

Recursos criados por artefactos de uma atribuição do esquema têm quatro Estados: **Não bloqueado**, **só de leitura**, **não é possível editar / eliminar**, ou **não é possível eliminar**. Cada tipo de artefacto pode estar a **bloqueado não** estado. A tabela seguinte pode ser utilizada para determinar o estado de um recurso:

|Modo|Tipo de recurso de artefacto|Estado|Descrição|
|-|-|-|-|
|Não Bloquear|*|Não bloqueado|Recursos não estão protegidos por esquemas. Este estado também é utilizado para o recurso adicionado a um **só de leitura** ou **não elimine** artefacto de grupo de recursos de fora de uma atribuição do esquema.|
|Só de Leitura|Grupo de recursos|Não é possível editar / eliminar|O grupo de recursos é só de leitura e as etiquetas no grupo de recursos não podem ser modificadas. **Não bloqueado** recursos podem ser adicionados, movidos, alterados ou eliminados deste grupo de recursos.|
|Só de Leitura|Grupo de recursos não|Só de Leitura|--Sem alterações e ele não não possível eliminar o recurso não pode ser alterado de forma alguma.|
|Não elimine|*|Não é possível eliminar|Os recursos podem ser alterados, mas não podem ser eliminados. **Não bloqueado** recursos podem ser adicionados, movidos, alterados ou eliminados deste grupo de recursos.|

## <a name="overriding-locking-states"></a>Substituir os Estados de bloqueios

É normalmente possível que alguém com apropriado [controlo de acesso baseado em funções](../../../role-based-access-control/overview.md) (RBAC) na subscrição, como a função "Proprietário", para ser permissão para alterar ou eliminar qualquer recurso. Este acesso não é o caso quando esquemas aplica-se de bloqueio como parte de uma atribuição de implementado. Se a atribuição foi definida com o **só de leitura** ou **não elimine** opção, nem mesmo a subscrição proprietário pode realizar a ação bloqueada no recurso protegido.

Esta medida de segurança protege a consistência do esquema definido e o ambiente, que ele foi projetado para criar a partir de eliminação acidental ou programática ou alteração.

## <a name="removing-locking-states"></a>Remover o bloqueios de Estados

Se for necessário modificar ou eliminar um recurso protegido por uma atribuição, existem duas formas de fazê-lo.

- A atualizar a atribuição do esquema para um modo de bloqueio de **não bloqueio**
- Eliminar a atribuição do esquema

Quando a atribuição ser removida, os bloqueios criados por esquemas são removidos. No entanto, o recurso seja deixado e tem de ser eliminada através de meios normais.

## <a name="how-blueprint-locks-work"></a>Como o esquema bloqueia a trabalho

Um RBAC [negar atribuições](../../../role-based-access-control/deny-assignments.md) negar a ação é aplicada a recursos de artefacto durante a atribuição de um plano gráfico se a atribuição selecionado a **só de leitura** ou **não elimine** opção. A ação de negação é adicionada pela identidade gerida de atribuição do esquema e só pode ser removida dos recursos de artefacto pela mesma identidade gerida. Esta medida de segurança impõe o mecanismo de bloqueio e impede a remover o bloqueio de esquema fora de esquemas.

> [!IMPORTANT]
> O Azure Resource Manager coloca em cache os detalhes de atribuição de função durante até 30 minutos. Assim, negar atribuições negar a ação nos recursos de esquema pode não ser imediatamente em vigor completa. Durante este período de tempo, é possível eliminar um recurso que se destina a ser protegidos por bloqueios de esquema.

## <a name="next-steps"></a>Passos Seguintes

- Saber mais sobre o [ciclo de vida do esquema](lifecycle.md)
- Compreender como utilizar [parâmetros estáticos e dinâmicos](parameters.md)
- Aprender a personalizar a [ordem de sequenciação do esquema](sequencing-order.md)
- Saber como [atualizar as atribuições existentes](../how-to/update-existing-assignments.md)
- Resolver problemas durante a atribuição de um esquema com [resolução de problemas gerais](../troubleshoot/general.md)