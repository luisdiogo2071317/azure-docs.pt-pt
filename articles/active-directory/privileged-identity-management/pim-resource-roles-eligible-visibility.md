---
title: Atribuições elegíveis e visibilidade de recursos no PIM - Azure | Documentos da Microsoft
description: Descreve como atribuir membros como elegíveis para funções de recursos do Azure no Azure AD Privileged Identity Management (PIM).
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
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 59d51ba8edadd1fd71255271623b144cab94fc97
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43344288"
---
# <a name="eligible-assignments-and-resource-visibility-in-pim"></a>Atribuições elegíveis e visibilidade de recursos no PIM

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

## <a name="azure-resource-role-approval-workflow"></a>Fluxo de trabalho de aprovação de função de recursos do Azure

Com o fluxo de trabalho aprovação no PIM para funções de recursos do Azure, os administradores ainda mais podem proteger ou restringir o acesso a recursos críticos. Ou seja, os administradores podem exigir aprovação para ativar as atribuições de funções.

O conceito de uma hierarquia de recurso é exclusivo para as funções de recursos do Azure. Esta hierarquia permite que a herança de atribuições de funções a partir de um objeto de recurso principal para baixo para todos os recursos subordinados dentro do contêiner pai. 

Por exemplo: João, um administrador de recursos, utiliza o PIM para atribuir a Alice como um membro elegível à função de proprietário na subscrição Contoso. Com esta atribuição, Alice é um proprietário elegível de todos os contentores do grupo de recursos dentro da subscrição da Contoso. Alice é também um proprietário elegível de todos os recursos (como máquinas virtuais) dentro de cada grupo de recursos da subscrição.

Vamos supor que existem três grupos de recursos na subscrição de Contoso: Fabrikam teste, desenvolvimento do Fabrikam e Fabrikam Prod. Cada um destes grupos de recursos contém uma única máquina virtual.

Definições do PIM são configuradas para cada função de um recurso. Ao contrário de atribuições, estas definições não são herdadas e aplicam-se estritamente a função de recursos.

Continuando com o exemplo: João utiliza o PIM para exigir que todos os membros na função de proprietário de aprovação de pedidos de subscrição da Contoso ser ativado. Para ajudar a proteger os recursos no grupo de recursos de Fabrikam Prod, Bob também requer aprovação para os membros da função de proprietário deste recurso. As funções de proprietário na Fabrikam teste e desenvolvimento do Fabrikam não necessita de aprovação para ativação.

Quando Alice solicita ativação da sua função de proprietário para a subscrição da Contoso, um aprovador deve aprovar ou negar o pedido antes de ela fica ativa na função de. Se decidir Alice [escopo de sua ativação](pim-resource-roles-activate-your-roles.md#apply-just-enough-administration-practices) para o grupo de recursos da Fabrikam Prod, um aprovador deve aprovar ou negar este pedido, demasiado. Mas, se decidir Alice definir o âmbito sua ativação para um ou ambos os Fabrikam teste ou desenvolvimento do Fabrikam, aprovação não é necessária.

O fluxo de trabalho de aprovação pode não ser necessário para todos os membros de uma função. Considere um cenário em que a sua organização contrata vários de contrato de associados, para ajudar com o desenvolvimento de um aplicativo que serão executados numa subscrição do Azure. Enquanto administrador de recursos, mesmo que os funcionários têm acesso elegível sem aprovação necessária, mas associa o contrato deve solicitar a aprovação. Para configurar o fluxo de trabalho de aprovação para apenas os associates de contrato, pode criar uma função personalizada com as mesmas permissões que a função atribuída aos funcionários. Pode exigir a aprovação para ativar essa função personalizada. [Saiba mais sobre as funções personalizadas](pim-resource-roles-custom-role-policy.md).

## <a name="next-steps"></a>Passos Seguintes

- [Atribuir funções de recursos do Azure no PIM](pim-resource-roles-assign-roles.md)
