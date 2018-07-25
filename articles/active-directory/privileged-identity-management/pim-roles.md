---
title: Funções no Azure AD Privileged Identity Management | Documentos da Microsoft
description: Saiba que funções são utilizadas para as identidades privilegiadas com a extensão Azure Privileged Identity Management.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 07/23/2018
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017;oldportal;it-pro;
ms.openlocfilehash: 6553fdba463144c6eda1e35c0967e92a3c44aff6
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225581"
---
# <a name="directory-roles-you-can-manage-using-azure-ad-pim"></a>Funções de diretório que pode gerir utilizando o Azure AD PIM
<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

Pode atribuir utilizadores na sua organização para diferentes funções administrativas no Azure AD. Estas atribuições de função controlam quais são as tarefas, como adicionar ou remover utilizadores ou alterar as definições de serviço, os utilizadores são capazes de efetuar no Azure AD, o Office 365 e outros serviços Online da Microsoft e aplicativos conectados.  

Um Administrador Global pode atualizar-se que os utilizadores estiverem **permanentemente** atribuídas às funções no Azure AD através do portal conforme descrito na [atribuir funções de administrador no Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md) ou usando [ Comandos do PowerShell](/powershell/module/azuread#directory_roles).

O Azure AD Privileged Identity Management (PIM) gere as políticas de acesso privilegiado para os utilizadores no Azure AD. PIM atribui aos usuários uma ou mais funções no Azure AD e pode atribuir alguém para ser permanentemente na função, ou elegíveis para a função. Quando um utilizador é permanentemente atribuído a uma função ou ativa uma atribuição de função elegível, em seguida, pode gerir do Azure Active Directory, do Office 365 e outras aplicações com as permissões atribuídas às respetivas funções.

Não existe nenhuma diferença no acesso concedido a qualquer pessoa com uma permanente em comparação com uma atribuição de função elegível. A única diferença é que, algumas pessoas não precisam que o acesso o tempo todo. Eles são feitos elegíveis para a função e podem ativá-la e desligar sempre que eles precisam.

## <a name="roles-managed-in-pim"></a>Funções gerenciadas no PIM
Privileged Identity Management permite-lhe atribuir utilizadores a funções de administrador comuns, incluindo:

* **Administrador global** (também conhecido como administrador de empresa) tem acesso a todas as funcionalidades administrativas. Pode ter mais do que um administrador global na sua organização. A pessoa que se inscreve para comprar o Office 365 automaticamente torna-se um administrador global.
* **Administrador com função privilegiada** gere o PIM do Azure AD e atualiza as atribuições de funções para outros utilizadores.  
* **Administrador de faturação** efetua compras, gere subscrições, gere pedidos de suporte e monitoriza o estado de funcionamento do serviço.
* **Administrador de palavras-passe** repõe palavras-passe, gere pedidos de serviço e monitoriza o estado de funcionamento do serviço. Os administradores de palavra-passe estão limitados à reposição de palavras-passe para os utilizadores.
* **Administrador de serviços** gere pedidos de serviço e monitoriza o estado de funcionamento do serviço.
  
  > [!NOTE]
  > Se estiver a utilizar o Office 365, em seguida, antes de atribuir a função de administrador de serviço para um utilizador, primeiro de atribuir o utilizador permissões administrativas para um serviço, como o Exchange Online.
  > 
  > 
* **Administrador de gestão de utilizadores** repõe palavras-passe, monitoriza o estado de funcionamento do serviço e gere contas de utilizador, grupos de utilizadores e pedidos de serviço. O administrador de gestão de utilizadores não é possível eliminar um administrador global, criar outras funções de administrador ou repor palavras-passe para faturação, globais e administradores de serviço.
* **Administrador do Exchange** tenha acesso administrativo para o Exchange Online através do Centro de administração do Exchange (EAC) e podem executar quase qualquer tarefa no Exchange Online.
* **Administrador do SharePoint (pré-visualização)** tenha acesso administrativo ao SharePoint Online através do Centro de administração do SharePoint Online e podem executar quase qualquer tarefa no SharePoint Online. Esta função está atualmente em pré-visualização. Os utilizadores elegíveis poderão ocorrem atrasos com esta função dentro do SharePoint após a ativação no PIM.
* **Skype para o administrador da empresa** tenha acesso administrativo ao Skype para empresas através do Skype para o Centro de administração de negócios e podem executar quase qualquer tarefa no Skype para empresas Online.

Leia os artigos seguintes para obter mais detalhes sobre [atribuir funções de administrador no Azure AD](../users-groups-roles/directory-assign-admin-roles.md) e [atribuir funções de administrador no Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504).

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


A partir do PIM, pode [atribuir essas funções a um usuário](pim-how-to-add-role-to-user.md) , para que o utilizador pode [ativar a função quando necessário](pim-how-to-activate-role.md).

Se quiser dar outro acesso de utilizador para gerir no PIM em si, as funções que PIM exige que o utilizador ter são descritas mais detalhadamente no [como conceder acesso ao PIM](pim-how-to-give-access-to-pim.md).

<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>Funções não são geridas no PIM
Funções no Exchange Online ou SharePoint Online, exceto aquelas mencionadas acima, não são representadas no Azure AD e, portanto, não estão visíveis no PIM. Para obter mais informações sobre como alterar as atribuições de funções detalhadas nestes serviços do Office 365, consulte [permissões no Office 365](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

As subscrições do Azure e grupos de recursos também não são representados no Azure AD. Para gerir as subscrições do Azure, consulte [como adicionar ou alterar funções de administrador do Azure](../../billing/billing-add-change-azure-subscription-administrator.md) e, para obter mais informações sobre o Azure RBAC, veja [controlo de acesso baseado em função do Azure](../../role-based-access-control/role-assignments-portal.md).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Funções de utilizador e iniciar sessão
Para algumas aplicações e serviços Microsoft, atribuir um utilizador a uma função pode não ser suficiente para permitir que o utilizador ser um administrador.

Acesso ao portal do Azure requer que o utilizador ser um administrador de serviços ou coadministrador numa subscrição do Azure, mesmo que o usuário não precisa de gerir as subscrições do Azure.  Por exemplo, para gerir as definições de configuração para o Azure AD, um utilizador tem de ser um administrador global no Azure AD e um coadministrador da subscrição numa subscrição do Azure.  Para saber como adicionar utilizadores às subscrições do Azure, veja [como adicionar ou alterar funções de administrador do Azure](../../billing/billing-add-change-azure-subscription-administrator.md).

Acesso a Serviços Online da Microsoft pode exigir o também ser atribuída uma licença do utilizador antes de poderem abrir o portal do serviço ou executar tarefas administrativas.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Atribuir uma licença a um utilizador no Azure AD
1. Inicie sessão para o [portal do Azure](http://portal.azure.com) com uma conta de administrador global ou uma conta de coadministrador.
3. Selecionar o Azure AD e o diretório que pretende trabalhar com e que tem licenças associadas ele.
4. Selecione **licenças** à esquerda. Será apresentada a lista de licenças disponíveis.
5. Selecione o plano de licença que contém as licenças que pretende distribuir.
6. Selecione **atribuir utilizadores**.
7. Selecione o utilizador que pretende atribuir uma licença.
8. Clique nas **atribuir** botão.  O utilizador pode agora iniciar sessão no Azure.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Passos Seguintes
[!INCLUDE [active-directory-privileged-identity-management-toc](../../../includes/active-directory-privileged-identity-management-toc.md)]

