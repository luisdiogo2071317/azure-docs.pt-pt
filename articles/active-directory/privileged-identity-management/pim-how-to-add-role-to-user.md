---
title: Como adicionar ou remover uma função de utilizador | Documentos da Microsoft
description: Saiba como adicionar funções para as identidades privilegiadas com a aplicação do Azure Active Directory Privileged Identity Management.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 01/03/2018
ms.author: rolyon
ms.openlocfilehash: eac0869c0f4a7dd780d6988ff9bc4362458a7e3d
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38590507"
---
# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a>Privileged Identity Management do Azure AD: como adicionar ou remover uma função de utilizador
Com o Azure Active Directory (AD), um administrador global (ou o administrador da empresa) pode atualizar que os utilizadores estiverem **permanentemente** atribuídas às funções no Azure AD. Isso é feito com os cmdlets do PowerShell, como `Add-MsolRoleMember` e `Remove-MsolRoleMember`. Ou podem utilizar o portal do Azure conforme descrito em [atribuir funções de administrador no Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md).

A aplicação Azure AD Privileged Identity Management permite aos administradores de função com privilégios fazer atribuições de função permanente, também. Além disso, os administradores de função com privilégios podem tornar os usuários **elegíveis** para funções de administrador. Um administrador elegível pode ativar a função quando surge a necessidade e, em seguida, as respetivas permissões expirarem assim que estiverem concluídos.

## <a name="manage-roles-with-pim-in-the-azure-portal"></a>Gerir funções com o PIM no portal do Azure
Na sua organização, pode atribuir utilizadores para diferentes funções administrativas no Azure AD, Office 365 e outros serviços e aplicações Microsoft.  Obter mais detalhes sobre as funções disponíveis que podem ser encontrados em [funções no Azure AD PIM](pim-roles.md).

Para adicionar ou remover um utilizador numa função com o Privileged Identity Management, abra o dashboard do PIM. Em seguida, clique a **utilizadores nas funções de administrador** botão ou selecione uma função específica (por exemplo, o Administrador Global) da tabela de funções.

> [!NOTE]
> Se ainda não tiver ativado PIM no portal do Azure, aceda a [introdução ao Azure AD PIM](pim-getting-started.md) para obter detalhes.

Se quiser permitir outro acesso de utilizador ao PIM em si, as funções que PIM exige que o utilizador ter são descritas mais detalhadamente no [como conceder acesso ao PIM](pim-how-to-give-access-to-pim.md).

## <a name="add-a-user-to-a-role"></a>Adicionar um utilizador a uma função
1. Na [portal do Azure](https://portal.azure.com/), selecione a **Azure AD Privileged Identity Management** mosaico no dashboard.
2. Selecione **gerir funções privilegiadas**.
3. Na **resumo de função** tabela, selecione a função que pretende gerir.
4. No painel da função, selecione **adicionar**.
5. Clique em **selecionar utilizadores** e procure o utilizador sobre o **selecionar utilizadores** painel.  
6. Selecione o utilizador a partir da lista de resultados de pesquisa e clique em **feito**.
7. Clique em **OK** para guardar a sua seleção. O usuário que tiver selecionado será apresentado na lista como elegíveis para a função.

> [!NOTE]
> Novos utilizadores numa função só são elegíveis para a função por predefinição. Se quiser tornar a função permanentes, clique o utilizador na lista. As informações do utilizador serão apresentado num novo painel. Selecione **permanente de tornar** no menu de informações do utilizador.  
> Se um utilizador não é possível registar para o Azure multi-factor Authentication (MFA), ou se está a utilizar uma conta Microsoft (normalmente @outlook.com), precisa para torná-los permanente em todas as suas funções. Administradores elegíveis são-lhe pedidos para se registar para MFA durante a ativação.

Agora que o utilizador é elegível para uma função, informá-los de que eles podem fazê-lo, de acordo com as instruções em [como ativar ou desativar uma função](pim-how-to-activate-role.md).

## <a name="remove-a-user-from-a-role"></a>Remover um utilizador de uma função
Pode remover os utilizadores de atribuições de funções elegíveis, mas certifique-se sempre existe pelo menos um utilizador que seja um administrador global permanente.

Siga estes passos para remover um utilizador específico de uma função:

1. Navegue para a função na lista de função ao selecionar uma função no dashboard do Azure AD PIM ou ao clicar na **utilizadores nas funções de administrador** botão.
2. Clique no utilizador na lista de utilizadores.
3. Clique em **remover**. Uma mensagem irá pedir-lhe para confirmar.
4. Clique em **Sim** para remover a função do usuário.

Se não tem a certeza de que os utilizadores ainda precisam de suas atribuições de funções, em seguida, pode [iniciar uma revisão de acesso para a função](pim-how-to-start-security-review.md).

## <a name="next-steps"></a>Passos Seguintes
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]

