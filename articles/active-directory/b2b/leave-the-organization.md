---
title: Sair de uma organização como um utilizador convidado - Azure Active Directory | Microsoft Docs
description: Mostra como um utilizador de convidado do Azure AD B2B pode sair de uma organização utilizando o painel de acesso.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/11/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 3415989cb6cec68f95e7d317671c31ba9bd231e9
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34267503"
---
# <a name="leave-an-organization-as-a-guest-user"></a>Sair de uma organização como um utilizador convidado

Um utilizador de convidado do B2B Azure Active Directory (Azure AD) pode optar por sair de uma organização em qualquer altura se já não têm de utilizar as aplicações a partir da organização ou manter quaisquer associação. Um utilizador pode sair de uma organização por si próprios, sem ter de contactar um administrador.

## <a name="leave-an-organization"></a>Sair de uma organização

Para sair de uma organização, como um utilizador com sessão iniciada para o [painel de acesso](https://myapps.microsoft.com), efetue o seguinte procedimento:

1. Se ainda não tem sessão iniciada organização que pretende manter, selecione o nome no canto superior direito e clique a organização que pretende manter.
2. No canto superior direito, selecione o seu nome.
3. Junto a **organizações**, selecione o ícone de definições (engrenagem).
 
   ![Captura de ecrã que mostra as definições de utilizador no painel de acesso](media/leave-the-organization/UserSettings.png) 

3. Em **organizações**, localizar a organização que pretende sair e selecione **deixar a organização**.

   ![Captura de ecrã com a opção de organização deixe na interface de utilizador](media/leave-the-organization/LeaveOrg.png)

4. Quando lhe for pedido para confirmar, selecione **deixe**. 

## <a name="account-removal"></a>Remoção da conta

Quando um utilizador deixa uma organização, a conta de utilizador é "soft apagada" no diretório. Por predefinição, o objeto de utilizador movido para o **eliminar utilizadores** área no Azure AD, mas não é permanentemente eliminado durante 30 dias. Esta eliminação de forma recuperável permite ao administrador restaurar a conta de utilizador (incluindo permissões e grupos), se o utilizador faz um pedido para restaurar a conta dentro do período de 30 dias.

Se assim o desejar, um administrador inquilino pode eliminar permanentemente a conta em qualquer altura durante o período de 30 dias. Para efetuar este procedimento:

1. No [portal do Azure](https://portal.azure.com), selecione **do Azure Active Directory**.
2. Em **gerir**, selecione **utilizadores**.
3. Selecione **eliminar utilizadores**.
4. Selecione a caixa de verificação junto a um utilizador eliminado e, em seguida, selecione **eliminar de forma permanente**.

Se eliminar permanentemente um utilizador, esta ação é irrevogável.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Passos Seguintes

- Para uma descrição geral do Azure AD B2B, consulte [o que é a colaboração B2B do Azure AD?](what-is-b2b.md)



