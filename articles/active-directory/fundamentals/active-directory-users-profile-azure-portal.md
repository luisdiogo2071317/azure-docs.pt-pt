---
title: Adicionar ou atualizar as informações de perfil do usuário - Azure Active Directory | Documentos da Microsoft
description: Instruções sobre como adicionar informações a um perfil de utilizador no Azure Active Directory, incluindo os detalhes de uma imagem e a tarefa.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: b11c71f7f5a329a836d379a16afe66c08572ccde
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56177993"
---
# <a name="add-or-update-a-users-profile-information-using-azure-active-directory"></a>Adicionar ou atualizar as informações de perfil de um utilizador com o Azure Active Directory
Adicione informações de perfil do usuário, incluindo uma imagem do perfil, informações específicas de tarefa e algumas definições com o Azure Active Directory (Azure AD). Para obter mais informações sobre como adicionar novos utilizadores, consulte [como adicionar ou eliminar utilizadores no Azure Active Directory](add-users-azure-active-directory.md).

## <a name="add-or-change-profile-information"></a>Adicionar ou alterar as informações de perfil
Como verá, há mais informações disponíveis no perfil de um utilizador que o que pode adicionar durante a criação do utilizador. Todas essas informações adicionais é opcionais e podem ser adicionadas conforme necessário pela sua organização.

## <a name="to-add-or-change-profile-information"></a>Para adicionar ou alterar as informações de perfil
1. Inicie sessão para o [portal do Azure](https://portal.azure.com/) como um Administrador Global ou administrador de utilizadores do diretório.

2. Selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione um utilizador. Por exemplo, _Alain Charon_.

    O **Alain Charon - perfil** é apresentada a página.

    ![Página de perfil do utilizador, incluindo informações editáveis](media/active-directory-users-profile-azure-portal/user-profile-all-blade.png)

3. Selecione **editar** para, opcionalmente, adicionar ou atualizar as informações incluídas em cada uma das seções disponíveis.

    ![Página de perfil do usuário, que mostra as áreas editáveis](media/active-directory-users-profile-azure-portal/user-profile-edit.png)

    - **Imagem do perfil.** Selecione uma imagem em miniatura para a conta de utilizador. Esta imagem é apresentada no Azure Active Directory e páginas pessoais do utilizador, tais como a página de myapps.microsoft.com.

    - **Identidade.** Adicione quaisquer informações relacionadas com a conta, como um apelido married ou um nome de utilizador foi alterado. 

    - **Informações da tarefa.** Adicione qualquer informação relacionados com tarefas, como cargo, departamento ou gestor do utilizador.

    - **Definições.** Decida se o utilizador pode iniciar sessão no inquilino do Azure Active Directory. Também pode especificar a localização do utilizador global.

    - **Informações de contacto.** Adicione informações de contacto relevantes para o utilizador. Por exemplo, um endereço de rua ou um número de telefone celular.

    - **Informações de contacto de autenticação.** Verifique se estas informações para se certificar de que existe um endereço de e-mail e número de telefone do Active Directory para o utilizador. Estas informações são utilizadas pelo Azure Active Directory para se certificar de que o usuário realmente é o utilizador durante o início de sessão. Informações de contacto de autenticação podem ser atualizada apenas por um administrador global.

4. Selecione **Guardar**.

    Todas as alterações são guardadas para o utilizador.

    >[!Note]
    >Tem de utilizar o Windows Server Active Directory para atualizar a identidade, informações de contacto ou informações da tarefa para os utilizadores cuja origem de autoridade é o Windows Server Active Directory. Depois de concluir a atualização, tem de aguardar o próximo ciclo de sincronização seja concluída antes de verá as alterações.

## <a name="next-steps"></a>Passos Seguintes
Depois de atualizou os perfis dos seus utilizadores, pode executar os seguintes processos de basic:

- [Adicionar ou eliminar utilizadores](add-users-azure-active-directory.md)

- [Atribuir funções a utilizadores](active-directory-users-assign-role-azure-portal.md)

- [Criar um grupo básico e adicionar membros](active-directory-groups-create-azure-portal.md)

Ou pode realizar outras tarefas de gestão de utilizador, como a atribuição de delegados, através de políticas e partilhar contas de utilizador. Para obter mais informações sobre outras ações disponíveis, consulte [documentação de gestão de utilizador do Azure Active Directory](../users-groups-roles/index.yml).
