---
title: Compreender o recurso de bloqueio em esquemas do Azure
description: Saiba mais sobre as opções de bloqueios para proteger recursos, ao atribuir um plano gráfico.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 718c23b806da5058c042b961077e51ba0d4b6245
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973257"
---
# <a name="understand-resource-locking-in-azure-blueprints"></a>Compreender o recurso de bloqueio em esquemas do Azure

A criação de ambientes de consistentes e à escala só é realmente importante se existe um mecanismo para garantir que a consistência é mantida. Este artigo explica como bloqueio de recurso funciona em esquemas do Azure.

## <a name="locking-modes-and-states"></a>Modos de bloqueios e de Estados

Modo de bloqueio aplica-se para a atribuição do esquema e só tem duas opções: **None** ou **todos os recursos**. Isso é configurado durante a atribuição do esquema e não pode ser alterado depois da atribuição é aplicada com êxito para a subscrição.

Recursos criados por definições de artefacto no esquema atribuído para a subscrição tem três Estados: **não bloqueado**, **só de leitura**, ou **não é possível editar / eliminar**. Pode ter qualquer tipo de artefacto da **bloqueado não** estado. No entanto, os artefactos de grupo de recursos não são como **só de leitura** e grupos de recursos são como **não é possível editar / eliminar**. Esta é uma distinção importante na forma como estes recursos são geridos.

O **só de leitura** exatamente conforme definido o estado é: o recurso não pode ser alterado de forma alguma – sem alterações e ele não podem ser eliminadas. O **não é possível editar / eliminar** é mais variada devido à natureza "contentor" de grupos de recursos. O objeto de grupo de recursos é somente leitura, mas é possível criar, atualização e eliminação de recursos dentro do recurso grupos – desde que eles não fazem parte de qualquer atribuição do esquema com o **só de leitura** estado de bloqueio.

## <a name="overriding-locking-states"></a>Substituir os Estados de bloqueios

Embora seja normalmente possível que alguém com apropriado [controlo de acesso baseado em funções](../../../role-based-access-control/overview.md) (RBAC) na subscrição, como a função "Proprietário", para ser capaz de alterar ou eliminar qualquer recurso, esse não é o caso quando esquemas aplica-se de que o bloqueio como parte de uma atribuição de implementado. Se a atribuição foi definida com o **bloqueio** opção, nem mesmo a subscrição proprietário pode alterar os recursos incluídos.

Esta ação protege a consistência do esquema definido e o ambiente, que ele foi projetado para criar a partir de eliminação acidental ou programática ou alteração.

## <a name="removing-locking-states"></a>Remover o bloqueios de Estados

Se for necessário eliminar os recursos criados por uma atribuição, em seguida, a única forma de eliminá-los é primeiro de remover a atribuição. Quando a atribuição ser removida, os bloqueios criados por esquemas são removidos. No entanto, o recurso seja deixado e, em seguida, seria necessário ser eliminada através de meios normais por alguém com permissões adequadas.

## <a name="how-blueprint-locks-work"></a>Como o esquema bloqueia a trabalho

Uma função RBAC `denyAssignments` é aplicada a recursos de artefacto durante a atribuição de um plano gráfico se a atribuição selecionado da **bloqueio** opção. A função é adicionada pela identidade gerida de atribuição do esquema e só pode ser removida dos recursos de artefacto pela mesma identidade gerida. Isso impõe o mecanismo de bloqueio e impede tentativas ao remover o bloqueio de esquema fora de esquemas. Remoção da função e o bloqueio só é possível ao remover a atribuição do esquema, que só pode ser executada por indivíduos com direitos adequados.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre o [ciclo de vida de esquema](lifecycle.md)
- Compreender como utilizar [parâmetros estáticos e dinâmicos](parameters.md)
- Aprenda a personalizar o [plano gráfico de ordem de sequenciamento](sequencing-order.md)
- Saiba como [atualizar atribuições existentes](../how-to/update-existing-assignments.md)
- Resolver problemas durante a atribuição de um plano gráfico com [resolução de problemas gerais](../troubleshoot/general.md)