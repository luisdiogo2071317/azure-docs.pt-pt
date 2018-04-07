---
title: Privileged Identity Management para recursos do Azure - MFA | Microsoft Docs
description: Este documento descreve como ativar a multi-factor authentication para recursos do PIM.
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
ms.openlocfilehash: 8d1c05e7f61ed76c47613bfab7bb8afd9b66cbe7
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="privileged-identity-management---resource-roles---mfa"></a>Privileged Identity Management - funções dos recursos - MFA

PIM para funções de recursos do Azure permite que os administradores de recursos e administradores de identidade proteger críticos de infraestrutura do Azure com a associação de vínculo de tempo e acesso just-in-time. Além disso, PIM oferece a implementação opcional do Azure multi-factor Authentication (MFA) para dois cenários distintos.

## <a name="require-mfa-to-activate"></a>Exigir a MFA ativar

Os administradores de recursos podem exigir elegíveis membros de uma função com êxito a MFA do Azure possa ativar. Este processo garante que a ativação de requerente do utilizador é quem diga a estão com certainty razoável. Impor esta opção protege a recursos críticos em situações quando a conta de utilizador poderá ter sido comprometida. 

Para impor a este requisito, selecione um recurso na lista de recursos geridos. Do [dashboard da descrição geral](pim-resource-roles-overview-dashboards.md), selecione uma função na lista de funções na parte inferior direita do ecrã.

Além disso, pode obter as definições de função das "funções" ou "Definições de função" separadores no menu de navegação esquerdo.

>[!Note]
>Se as opções no menu de navegação esquerdo estão desativadas e verá uma faixa na parte superior da página que indica "Tiver elegíveis funções que podem ser activadas" não é um administrador do Active Directory e tem [ativar](pim-resource-roles-activate-your-roles.md) antes de continuar.

![](media/azure-pim-resource-rbac/aadpim_rbac_manage_a_role_v2.png)

Se visualizar a associação de uma função, selecione "Definições de função" a partir da barra na parte superior do ecrã, para abrir o "Detalhes de definição de função".

Clique em de **editar** botão na parte superior para modificar as definições de função.

Na secção em **ativar**, clique na caixa de verificação para **requerem o multi-factor Authentication para ativar**, em seguida, clique em Guardar.

![](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa.png)

## <a name="require-mfa-on-assignment"></a>Exigir a MFA na atribuição

Em alguns casos, um administrador de recursos poderá ser útil atribuir um membro a uma função durante um curto período de tempo (um dia, por exemplo) e não precisa dos membro ou membros atribuídos para pedir a ativação. Neste cenário, PIM não é possível impor o MFA quando o membro utiliza a atribuição de função, uma vez que já estão ativas na função a partir do momento em que estão atribuídos.

Para garantir que o administrador de recursos satisfazer a atribuição é quem indicar são, pode impor o MFA na atribuição.

A partir do ecrã a mesmo função definição detalhes, selecione a caixa para "Requer autenticação Multifator atribuição".

![](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa_on_assignment.png)

## <a name="next-steps"></a>Passos Seguintes

[Exigir a aprovação para ativar](pim-resource-roles-approval-workflow.md)

[Utilizar o registo de auditoria](pim-resource-roles-use-the-audit-log.md)



