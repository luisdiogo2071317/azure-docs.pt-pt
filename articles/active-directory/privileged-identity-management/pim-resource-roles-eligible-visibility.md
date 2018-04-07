---
title: Privileged Identity Management para recursos do Azure - atribuições elegíveis e visibilidade do recurso | Microsoft Docs
description: Descreve como atribuir membros como elegível para funções de recursos.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2018
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 273b06c91d68a764fe814374c0eca6ed1698cc2e
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="eligible-assignments-and-resource-visibility"></a>Atribuições de elegíveis e visibilidade do recurso

PIM para funções de recursos do Azure fornece segurança melhorada para organizações com críticos resoruces do Azure. PIM fornece a capacidade de atribuir membros como elegíveis para funções dos recursos de administradores de recursos. Leia mais sobre os tipos de atribuição diferentes e Estados de funções de recursos do Azure abaixo. 

## <a name="assignment-types"></a>Tipos de atribuição

PIM para recursos do Azure fornece dois tipos de atribuição distintas:

- Elegível
- Ativa

Atribuições de elegíveis requerem o membro da função efetuar uma ação para utilizar a função. Estas ações podem incluir ter êxito uma verificação de multi-factor Authentication, fornecendo uma justificação e solicitar a aprovação de aprovadores designados.

Atribuições de Active Directory não necessitam de membro efetuar qualquer ação para utilizar a função. Os membros atribuídos como ativo tem os privilégios fornecidos pela função de todas as vezes.

## <a name="assignment-duration"></a>Duração de atribuição

Os administradores de recursos podem escolher uma das duas opções para cada tipo de atribuição ao configurar as definições do PIM numa função. Estas opções tornar-se a duração máxima de predefinição quando um membro é atribuído à função no PIM.

- Permitir que a atribuição de elegível permanente
- Permitir atribuição permanente de Active Directory

ou

- Expirar atribuições elegíveis após
- Atribuições de Active Directory depois de expirar

Se escolher um administrador de recursos para "Permitir permanente elegível atribuição" e/ou "Permitir Active Directory atribuição permanente", todos os administradores que atribuir membros para o recurso tem a capacidade de atribuir membros permanentes.

Escolher "Expirar elegíveis atribuições após" e/ou "Atribuições Active Directory para o expirar após" permite o controlo sobre o ciclo de vida de atribuição, exigindo que todas as atribuições de tem uma data de início e de fim especificada.

>[!NOTE] 
>Todas as atribuições com uma data de fim especificada podem ser renovadas por administradores de recursos e os membros podem iniciar pedidos de self-service para [expandir ou renovar atribuições](pim-resource-roles-renew-extend.md).


## <a name="assignment-states"></a>Estados de atribuição

PIM para recursos do Azure tem dois Estados distintos atribuição que aparecem no separador "Active Directory funções" os meus funções, funções e membros de vistas do PIM. Estes Estados permitidos são:

- Atribuído
- Ativado

Quando visualizar uma associação listado em "Active Directory funções" a coluna "STATE" permite-lhe diferenciar entre os utilizadores que são "Atribuído" como ativas por oposição aos utilizadores que "ativado" uma atribuição elegível e tem agora Active Directory.

## <a name="next-steps"></a>Passos Seguintes

[Atribuir funções no PIM](pim-resource-roles-assign-roles.md)

