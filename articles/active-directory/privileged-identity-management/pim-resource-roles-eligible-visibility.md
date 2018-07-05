---
title: Atribuições elegíveis e visibilidade de recursos do Azure no Privileged Identity Management | Documentos da Microsoft
description: Descreve como atribuir membros como elegíveis para funções de recursos, ao utilizar o PIM.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 8089591708676073bcef84ad13b3690b39bdc653
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448208"
---
# <a name="eligible-assignments-and-resource-visibility-with-privileged-identity-management"></a>Atribuições elegíveis e visibilidade de recursos com o Privileged Identity Management

Privileged Identity Management (PIM) para funções de recursos do Azure fornece mais segurança para as organizações que tenham recursos do Azure críticos. Os administradores de recursos podem utilizar o PIM para atribuir membros como elegíveis para funções de recursos. Saiba mais sobre os tipos de atribuição de diferentes e Estados de atribuição de funções de recursos do Azure nas seções a seguir. 

## <a name="assignment-types"></a>Tipos de atribuição

PIM para recursos do Azure fornece dois tipos de atribuição distintos:

- Elegível
- Ativa

Atribuições elegíveis requerem o membro da função de executar uma ação para utilizar a função. Ações podem incluir uma verificação de autenticação multifator que os sucedem, fornecer uma justificação de negócio ou o pedido de aprovação de aprovadores designados.

Atribuições ativas não necessitam de membro a executar qualquer ação para utilizar a função. Os membros atribuídos como ativo têm os privilégios atribuídos à função tempo todo.

## <a name="assignment-duration"></a>Duração de atribuição

Os administradores de recursos podem escolher duas opções para cada tipo de atribuição ao configurar as definições do PIM para uma função. Estas opções tornam-se a duração máxima do padrão quando um membro é atribuído à função no PIM. 

Um administrador pode escolher um dos seguintes tipos de atribuição:

- Permitir a atribuição elegível permanente
- Permitir a atribuição ativa permanente

Em alternativa, um administrador pode escolher um dos seguintes tipos de atribuição:

- Expirar as atribuições elegíveis após
- Expirar atribuições ativas após

Se um administrador de recursos escolhe **permitir a atribuição elegível permanente** ou **permitir que a atribuição ativa permanente**, todos os administradores que atribuir membros para o recurso podem atribuir permanente associações.

Se um administrador de recursos escolhe **expirar atribuições elegíveis após** ou **expirar atribuições ativas após**, o administrador do recurso controla o ciclo de vida de atribuição ao exigir que todos os atribuições de tem uma data de início e de fim especificada.

> [!NOTE] 
> Todas as atribuições de que têm uma data de fim especificada podem ser renovadas por administradores de recursos. Além disso, os membros podem iniciar pedidos de Self-serviços de mensagens em fila para [expandir ou renovar as atribuições](pim-resource-roles-renew-extend.md).


## <a name="assignment-states"></a>Estados de atribuição

PIM para recursos do Azure tem dois Estados de atribuição distintos que aparecem no **funções do Active Directory** separador a **minhas funções**, **funções**, e **membros**modos de exibição do PIM. Esses Estados são:

- Atribuído
- Ativada

Ao visualizar uma associação que está listada na **funções do Active Directory**, pode utilizar o valor a **estado** coluna para diferenciar entre os utilizadores que estão **atribuído** como ativo e os utilizadores que **Activated** uma atribuição elegível e está agora ativa.

## <a name="next-steps"></a>Passos Seguintes

[Atribuir funções no Privileged Identity Manager](pim-resource-roles-assign-roles.md)
