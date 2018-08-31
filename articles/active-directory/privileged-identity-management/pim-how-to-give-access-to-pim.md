---
title: Conceder acesso a outros administradores para gerir o PIM - Azure | Documentos da Microsoft
description: Saiba como conceder acesso a outros as administrações para gerir o Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 06/06/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: d6b2d9f43ce9bb86f4557c92887689c83beb49fa
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43189560"
---
# <a name="grant-access-to-other-administrators-to-manage-pim"></a>Conceder acesso a outros administradores para gerir o PIM
O administrador global que ativa o Azure AD Privileged Identity Management (PIM) para uma organização automaticamente obter atribuições de funções e acesso ao PIM. Ninguém mais obtém acesso de escrita por predefinição, no entanto, incluindo outros administradores globais. Outros administradores globais, administradores de segurança e leitores de segurança têm acesso só de leitura ao Azure AD PIM. Para dar acesso ao PIM, o primeiro utilizador pode atribuir outras pessoas para o **administrador com função privilegiada** função.

> [!NOTE]
> Gerir PIM do Azure AD requer o MFA do Azure. Uma vez que as contas Microsoft não é possível registar para MFA do Azure, um utilizador que inicia sessão com uma conta Microsoft não é possível aceder PIM do Azure AD.
> 
> 

Certificar-se de que existem sempre pelo menos dois utilizadores numa função de administrador com função privilegiada, no caso de um utilizador está bloqueado ou a sua conta foi eliminada.

## <a name="give-another-user-access-to-manage-pim"></a>Dar outro acesso de utilizador para gerir o PIM
1. Entrar para o [portal do Azure](https://portal.azure.com/) e selecione o **Azure AD Privileged Identity Management** aplicação no dashboard.
2. Selecione **gerir funções com privilégios** > **administrador com função privilegiada** > **adicionar**.
   
    ![Adicionar administradores de função com privilégios - captura de ecrã](./media/pim-how-to-give-access-to-pim/PIM_add_PRA.png)
3. No painel de utilizadores geridos Add, o passo 1 já está concluído. Selecione passo 2, **selecionar utilizadores** e procure o utilizador que pretende adicionar.
   
    ![Selecione utilizadores - captura de ecrã](./media/pim-how-to-give-access-to-pim/PIM_select_users.png)
4. Selecione o utilizador os resultados da pesquisa e clique em **feito**.
5. Clique em **OK** para guardar a sua seleção. O usuário que tiver selecionado será apresentado na lista de administradores de função com privilégios.
   
   * Sempre que atribui uma nova função a alguém, eles são automaticamente configurados como elegíveis para ativar a função. Se quiser torná-los permanente na função, clique o utilizador na lista. Selecione **tornar permanente** no menu de informações do utilizador.
6. Enviar ao usuário uma ligação para [introdução ao Azure AD Privileged Identity Management](pim-getting-started.md).

## <a name="remove-another-users-access-rights-for-managing-pim"></a>Remover direitos de acesso de outro utilizador para gerir o PIM
Antes de remover alguém da função de administrador com função privilegiada, sempre Verifique se ainda haverá dois utilizadores atribuídos à mesma.

1. No dashboard do PIM, clique na função **administrador com função privilegiada**.  Será apresentada a lista de utilizadores atualmente da função.
2. Clique no utilizador na lista de utilizadores.
3. Clique em **remover**.  É apresentada uma mensagem de confirmação.
4. Clique em **Sim** para remover o utilizador da função.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Passos Seguintes

- [Ativar a gestão de subscrição no seu inquilino](pim-resource-roles-enable-subscription-management.md)