---
title: Adicionar ou eliminar utilizadores no Azure Active Directory | Microsoft Docs
description: Explica como adicionar novos utilizadores ou eliminar os utilizadores existentes no Azure Active Directory
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 01/08/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: e6e21ea09909a3bd92a21e15428af347e3f20b93
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37767471"
---
# <a name="quickstart-add-new-users-to-azure-active-directory"></a>Início Rápido: Adicionar novos utilizadores ao Azure Active Directory
Este artigo explica como eliminar ou adicionar utilizadores na sua organização no inquilino do Azure Active Directory (Azure AD) da sua organização com o portal do Azure ou ao sincronizar os dados da conta de utilizador do AD do Windows Server no local. 

## <a name="add-cloud-based-users"></a>Adicionar utilizadores baseados na cloud
1. Inicie sessão no [Centro de administradores do Azure Active Directory](https://aad.portal.azure.com) com uma conta que seja administrador global do diretório.
2. Selecione **Azure Active Directory** e, em seguida, **Utilizadores e grupos**.
3. Em **Utilizadores e grupos**, selecione **Todos os utilizadores** e, em seguida, selecione **Novo utilizador**.
   ![Selecionar o comando Adicionar](./media/add-users-azure-active-directory/add-user.png)
4. Introduza os detalhes do utilizador, como o **Nome** e o **Nome de utilizador**. A parte do nome de utilizador do nome de domínio deve ser o nome de domínio predefinido inicial "[nome de domínio].onmicrosoft.com" ou um [nome de domínio personalizado](add-custom-domain.md) verificado e não federado, como "contoso.com."
5. Copie ou, caso contrário, tenha em atenção à palavra-passe de utilizador gerada, para que possa dá-la ao utilizador, após a conclusão deste processo.
6. Opcionalmente, pode abrir e preencher as informações em **Perfil**, **Grupos** ou **Função de diretório** para o utilizador. Para obter mais informações sobre as funções de utilizador e administrador, consulte [Atribuir funções de administrador no Azure AD](../users-groups-roles/directory-assign-admin-roles.md).
7. Em **Utilizador**, selecione **Criar**.
8. Dê a palavra-passe gerada com segurança ao novo utilizador, para que este possa iniciar sessão.

> [!TIP]
> Também pode sincronizar os dados da conta de utilizador do AD do Windows Server no local. As soluções de identidade da Microsoft abrangem capacidades baseadas na cloud e no local, ao criar uma única identidade de utilizador para autenticação e autorização a todos os recursos, independentemente da localização. Chamamos-lhe Identidade Híbrida. O [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) pode ser utilizado para integrar os diretórios no local ao Azure Active Directory, para cenários de identidade híbrida. Isto permite-lhe fornecer uma identidade comum para o utilizadores das aplicações do Office 365, do Azure e do SaaS integradas com o Azure AD. 

## <a name="delete-users-from-azure-ad"></a>Eliminar utilizadores do Azure AD
1. Inicie sessão no [Centro de administradores do Azure Active Directory](https://aad.portal.azure.com) com uma conta que seja administrador global do diretório.
2. Selecionar **Utilizadores e grupos**.
3. No painel **Utilizadores e grupos**, selecione um utilizador para eliminar da lista. 
4. No painel do utilizador selecionado, selecione **Descrição Geral**e, em seguida, na barra de comandos, selecione **Eliminar**.
   ![Selecionar o comando Adicionar](./media/add-users-azure-active-directory/delete-user.png)


### <a name="learn-more"></a>Saiba mais 
* [Adicionar utilizadores convidados a partir de outro diretório](../b2b/what-is-b2b.md) 
* [Atribuir um utilizador a uma função no seu Azure AD](active-directory-users-assign-role-azure-portal.md)
* [Gerir perfis de utilizador](active-directory-users-profile-azure-portal.md)
* [Restaurar um utilizador eliminado](active-directory-users-restore.md)



## <a name="next-steps"></a>Passos seguintes
Neste início rápido, aprendeu a adicionar novos utilizadores ao Azure AD Premium. 

Pode utilizar a ligação seguinte para criar um novo utilizador no Azure AD a partir do portal do Azure.

>[!div class="nextstepaction"]
>[Adicionar utilizadores ao Azure AD](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/)