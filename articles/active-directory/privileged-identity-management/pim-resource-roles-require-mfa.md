---
title: Impor o multi-factor Authentication do Azure em recursos do Azure utilizando o Privileged Identity Management | Microsoft Docs
description: Este documento descreve como ativar a multi-factor authentication para recursos do PIM.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: mwahl
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: protection
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: b4f3fb0b25787711294d09faf65c2856d57709bc
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234418"
---
# <a name="enforce-azure-multi-factor-authentication-in-azure-resources-by-using-privileged-identity-management"></a>Impor o multi-factor Authentication do Azure em recursos do Azure utilizando o Privileged Identity Management

Privileged Identity Management (PIM) para funções de recursos do Azure permite que os administradores de recursos e administradores de identidade proteger críticos de infraestrutura do Azure com a associação de vínculo de tempo e acesso just-in-time. Além disso, PIM oferece a implementação opcional do Azure multi-factor Authentication para dois cenários distintos.

## <a name="require-multi-factor-authentication-to-activate"></a>Requer multi-factor Authentication ativar

Os administradores de recursos podem exigir elegíveis membros de uma função executar o Azure multi-factor Authentication possa ativar. Este processo garante que o utilizador que está a pedir a ativação é que possam indicar estão com certainty razoável. Impor esta opção protege a recursos críticos em situações quando a conta de utilizador poderá ter sido comprometida. 

Para impor a este requisito, selecione um recurso na lista de recursos geridos. Do [dashboard da descrição geral](pim-resource-roles-overview-dashboards.md), selecione uma função na lista de funções na parte inferior direita do ecrã.

Além disso, pode obter as definições de função partir o **funções** ou **definições da função** separadores no painel esquerdo.

>[!Note]
>Se as opções no painel esquerdo estão desativadas e verá uma faixa na parte superior da página que indica "Tiver elegíveis funções que podem ser activadas", que não é um administrador do Active Directory. Isto significa que tem [ativar](pim-resource-roles-activate-your-roles.md) antes de continuar.

!["Funções" e os separadores de "Definições de função" ](media/azure-pim-resource-rbac/aadpim_rbac_manage_a_role_v2.png)

Para ver a associação de uma função, selecione **definições da função** a partir da barra na parte superior do ecrã, para abrir o **detalhe de definição de função**.

Para modificar as definições da função, selecione o **editar** botão na parte superior.

Na secção em **ativar**, selecione a caixa de verificação para **requerem o multi-factor Authentication na ativação**. Em seguida, selecione **Guardar**.

![Exigir Multi-Factor Authentication](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa.png)

## <a name="require-multi-factor-authentication-on-assignment"></a>Requer multi-factor Authentication na atribuição

Em alguns casos, um administrador de recursos pode pretender atribuir um membro a uma função durante um curto período de tempo (um dia, por exemplo). Neste caso, não precisam de membro ou membros atribuídos para pedir a ativação. Neste cenário, PIM não é possível impor o multi-factor Authentication quando o membro utiliza a atribuição de função, uma vez que já estão ativas na função a partir do momento em que estão atribuídos.

Para garantir que o administrador de recursos satisfazer a atribuição que diga a estiverem, pode impor o multi-factor Authentication na atribuição.

A partir do ecrã a mesma função definição detalhes, selecione a caixa para **requerem o multi-factor Authentication na atribuição direta**.

![Requer multi-factor Authentication na atribuição direta](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa_on_assignment.png)

## <a name="next-steps"></a>Passos Seguintes

[Exigir a aprovação para ativar](pim-resource-roles-approval-workflow.md)

[Utilizar o registo de auditoria](pim-resource-roles-use-the-audit-log.md)



