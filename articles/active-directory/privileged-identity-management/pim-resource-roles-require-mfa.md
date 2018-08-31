---
title: Exigir autenticação multifator para funções de recursos do Azure no PIM | Documentos da Microsoft
description: Saiba como exigir autenticação multifator (MFA) para funções de recursos do Azure no Azure AD Privileged Identity Management (PIM).
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
ms.openlocfilehash: 171d79856cf67dae9573dd1076c2ae4617cf86d1
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190573"
---
# <a name="require-multi-factor-authentication-for-azure-resource-roles-in-pim"></a>Exigir autenticação multifator para funções de recursos do Azure no PIM

Privileged Identity Management (PIM) para funções de recursos do Azure permite que os administradores de recursos e os administradores de identidade para proteger críticas de infraestrutura do Azure com associação de limite de tempo e o acesso just-in-time. Além disso, o PIM fornece opcional imposição de multi-factor Authentication para dois cenários distintos.

## <a name="require-multi-factor-authentication-to-activate"></a>Exigir autenticação Multifator ativar

Os administradores de recursos podem exigir elegíveis membros de uma função executar o Azure multi-factor Authentication para que poderem ativar. Este processo garante que o utilizador que está a pedir a ativação é realmente quem afirma são com certeza razoável. Impor esta opção protege os recursos críticos em situações, quando a conta de utilizador pode ter sido comprometida. 

Para impor a este requisito, selecione um recurso na lista de recursos gerenciados. Partir do [dashboard de descrição geral](pim-resource-roles-overview-dashboards.md), selecionar uma função a partir da lista de funções na parte inferior direito da tela.

Além disso, pode obter para as definições de função em qualquer um de **funções** ou **as definições de função** separadores no painel esquerdo.

>[!Note]
>Se as opções no painel esquerdo são bloqueadas e verá uma faixa na parte superior da página que indica "Tem funções elegíveis que podem ser ativadas", não for um administrador do Active Directory. Isso significa que deve [ativar](pim-resource-roles-activate-your-roles.md) antes de continuar.

!["Funções" e "Definições de função" guias ](media/azure-pim-resource-rbac/aadpim_rbac_manage_a_role_v2.png)

Para ver a associação de uma função, selecione **definições de função** a partir da barra na parte superior do ecrã para abrir o **detalhes da definição de função**.

Para modificar as definições de função, selecione o **editar** botão na parte superior.

Na secção sob **Activate**, selecione a caixa de verificação para **requerer o multi-factor Authentication na ativação**. Em seguida, selecione **Guardar**.

![Exigir Multi-Factor Authentication](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa.png)

## <a name="require-multi-factor-authentication-on-assignment"></a>Exigir autenticação Multifator aquando da atribuição

Em alguns casos, um administrador de recursos pode querer atribuir um membro a uma função durante um breve período (um dia, por exemplo). Neste caso, não precisam o membro ou membros atribuído para pedir a ativação. Neste cenário, PIM não é possível impor o multi-factor Authentication quando o membro utiliza a respetiva atribuição de função, uma vez que eles já estão ativos na função desde o momento que são atribuídas.

Para garantir que o administrador de recursos cumprindo a atribuição é que eles dizem que eles são, pode impor o multi-factor Authentication na atribuição.

No ecrã a mesma função definição detalhes, selecione a caixa para **requerer o multi-factor Authentication na atribuição direta**.

![Exigir autenticação Multifator aquando da atribuição direta](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa_on_assignment.png)

## <a name="next-steps"></a>Passos Seguintes

- [Configurar as definições de função de recursos do Azure no PIM](pim-resource-roles-configure-role-settings.md)
- [Configurar alertas de segurança para funções de recursos do Azure no PIM](pim-resource-roles-configure-alerts.md)


