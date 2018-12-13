---
title: Compreender o bloqueio de recursos
description: Saiba mais sobre as opções de bloqueios para proteger recursos, ao atribuir um plano gráfico.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 0e272f7137967b545269a408b6e83552de532682
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53309438"
---
# <a name="understand-resource-locking-in-azure-blueprints"></a>Compreender o recurso de bloqueio em esquemas do Azure

A criação de ambientes de consistentes e à escala só é realmente importante se existe um mecanismo para manter a consistência. Este artigo explica como bloqueio de recurso funciona em esquemas do Azure.

## <a name="locking-modes-and-states"></a>Modos de bloqueios e de Estados

Modo de bloqueio aplica-se para a atribuição do esquema e só tem duas opções: **NONE** ou **todos os recursos**. O modo de bloqueio é configurado durante a atribuição do esquema e não pode ser alterado depois da atribuição é aplicada com êxito para a subscrição.

Recursos criados por artefactos de uma atribuição do esquema tem três Estados: **Não bloqueado**, **só de leitura**, ou **não é possível editar / eliminar**. Cada artefato pode estar a **bloqueado não** estado. No entanto, os artefactos de grupo de recursos não têm **só de leitura** e tem de grupos de recursos **não é possível editar / eliminar** Estados. Essa diferença é uma distinção importante na forma como estes recursos são geridos.

O **só de leitura** exatamente conforme definido o estado é: o recurso não pode ser alterado de forma alguma – sem alterações e ele não podem ser eliminadas. O **não é possível editar / eliminar** é mais variada devido à natureza "contentor" de grupos de recursos. O objeto de grupo de recursos é só de leitura, mas é possível fazer alterações em recursos não bloqueado no grupo de recursos.

## <a name="overriding-locking-states"></a>Substituir os Estados de bloqueios

É normalmente possível que alguém com apropriado [controlo de acesso baseado em funções](../../../role-based-access-control/overview.md) (RBAC) na subscrição, como a função "Proprietário", para ser permissão para alterar ou eliminar qualquer recurso. Este acesso não é o caso quando esquemas aplica-se de bloqueio como parte de uma atribuição de implementado. Se a atribuição foi definida com o **bloqueio** opção, nem mesmo a subscrição proprietário pode alterar os recursos incluídos.

Esta medida de segurança protege a consistência do esquema definido e o ambiente, que ele foi projetado para criar a partir de eliminação acidental ou programática ou alteração.

## <a name="removing-locking-states"></a>Remover o bloqueios de Estados

Se for necessário eliminar os recursos criados por uma atribuição, é a forma de eliminá-los primeiro de remover a atribuição. Quando a atribuição ser removida, os bloqueios criados por esquemas são removidos. No entanto, o recurso seja deixado e tem de ser eliminada através de meios normais.

## <a name="how-blueprint-locks-work"></a>Como o esquema bloqueia a trabalho

Uma função RBAC `denyAssignments` é aplicada a recursos de artefacto durante a atribuição de um plano gráfico se a atribuição selecionado da **bloqueio** opção. A função é adicionada pela identidade gerida de atribuição do esquema e só pode ser removida dos recursos de artefacto pela mesma identidade gerida. Esta medida de segurança impõe o mecanismo de bloqueio e impede a remover o bloqueio de esquema fora de esquemas. Remoção da função e o bloqueio só é possível ao remover a atribuição do esquema, que só pode ser executada por indivíduos com direitos adequados.

> [!IMPORTANT]
> O Azure Resource Manager coloca em cache os detalhes de atribuição de função durante até 30 minutos. Como resultado, `denyAssignments` no esquema recursos talvez não seja em efeito total. Durante este período de tempo, é possível eliminar um recurso que se destina a ser protegidos por bloqueios de esquema.

## <a name="next-steps"></a>Passos Seguintes

- Saber mais sobre o [ciclo de vida do esquema](lifecycle.md)
- Compreender como utilizar [parâmetros estáticos e dinâmicos](parameters.md)
- Aprender a personalizar a [ordem de sequenciação do esquema](sequencing-order.md)
- Saber como [atualizar as atribuições existentes](../how-to/update-existing-assignments.md)
- Resolver problemas durante a atribuição de um esquema com [resolução de problemas gerais](../troubleshoot/general.md)