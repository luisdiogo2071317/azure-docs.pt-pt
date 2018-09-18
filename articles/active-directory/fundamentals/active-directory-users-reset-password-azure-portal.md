---
title: Como repor a palavra-passe de um utilizador no Azure Active Directory | Documentos da Microsoft
description: Saiba como repor a palavra-passe de um utilizador com o Azure Active Directory.
services: active-directory
author: eross-msft
manager: mtillman
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: 3803808ab589c3d880111421f650141f0d829f0b
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736102"
---
# <a name="how-to-reset-a-users-password-using-azure-active-directory"></a>Como: repor a palavra-passe de um utilizador com o Azure Active Directory
Pode redefinir a senha de um usuário se a palavra-passe é esquecida, se o utilizador obtém impedido de aceder um dispositivo ou se o usuário nunca recebeu uma palavra-passe.

>[!Note]
>A menos que o inquilino do Azure AD é o diretório raiz de um utilizador, não será possível repor a palavra-passe. Isso significa que se o utilizador está a iniciar sessão na sua organização através de uma conta de outra organização, uma conta Microsoft ou uma conta do Google, não será possível repor a palavra-passe.<br><br>Se o utilizador tem uma origem de autoridade de como o Windows Server Active Directory, só poderá repor a palavra-passe, se ativou a repetição de escrita de palavra-passe.<br><br>Se o utilizador tem uma origem de autoridade como externo do Azure AD, não será possível repor a palavra-passe. Apenas o utilizador ou um administrador no Azure AD externo, pode repor a palavra-passe.

## <a name="to-reset-a-password"></a>Repor uma palavra-passe

1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) como um administrador global, administrador de utilizador ou administrador de palavras-passe. Para obter mais informações sobre as funções disponíveis, consulte [atribuir funções de administrador no Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md#available-roles)

2. Selecione **do Azure Active Directory**, selecione **utilizadores**, procure e selecione o utilizador que precisa a reposição e, em seguida, selecione **Repor palavra-passe**.

    O **Alain Charon - perfil** é apresentada a página com o **Repor palavra-passe** opção.

    ![Página de perfil do utilizador, com a opção de reposição da palavra-passe realçada](media/active-directory-users-reset-password-azure-portal/user-profile-reset-password-link.png)

3. Na **Repor palavra-passe** página, selecione **Repor palavra-passe**.

    Uma palavra-passe temporária é gerado automaticamente para o utilizador.

4. Copie a palavra-passe e conceda-lhe para o usuário. O utilizador será necessário para alterar a palavra-passe durante o processo de início de sessão seguinte.

    >[!Note]
    >Nunca expira a palavra-passe temporária. Da próxima vez que o utilizador inicia sessão, a palavra-passe continuarão a funcionar, seja como for quanto tempo passou, uma vez que a palavra-passe temporária gerada.

## <a name="next-steps"></a>Passos Seguintes
Depois de repor a palavra-passe do utilizador, pode executar os seguintes processos de basic:

- [Adicionar ou eliminar utilizadores](add-users-azure-active-directory.md)

- [Atribuir funções a utilizadores](active-directory-users-assign-role-azure-portal.md)

- [Adicionar ou alterar as informações de perfil](active-directory-users-profile-azure-portal.md)

- [Criar um grupo básico e adicionar membros](active-directory-groups-create-azure-portal.md)

Ou pode executar a cenários de usuário mais complexos, como a atribuição de delegados, através de políticas e partilhar contas de utilizador. Para obter mais informações sobre outras ações disponíveis, consulte [documentação de gestão de utilizador do Azure Active Directory](../users-groups-roles/index.yml).
