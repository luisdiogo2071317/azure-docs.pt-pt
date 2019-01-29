---
title: Configurar palavras-passe de aplicação utilizando as informações de segurança - Azure Active Directory | Documentos da Microsoft
description: Configurar gerado automaticamente palavras-passe (palavras-passe de aplicação) para utilizar com cada aplicação não baseada no browser, separar a partir de uma palavra-passe normal, com informações de segurança.
services: active-directory
author: eross-msft
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: lizross
ms.openlocfilehash: 185c6e18a84369702de3bd0c398a9cc0c64b8217
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55191385"
---
# <a name="manage-app-passwords-using-security-info-preview"></a>Gerir palavras-passe de aplicação utilizando as informações de segurança (pré-visualização)

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

Determinadas aplicações não baseadas no browser, tais como o Outlook 2010, não suporta a verificação de dois passos. Essa falta de suporte significa que, se estiver usando a verificação de dois passos, a aplicação não funcionará. Para resolver este problema, pode criar uma palavra-passe gerada automaticamente para utilizar com cada aplicação de fora do browser separada da sua palavra-passe normal.

Ao utilizar as palavras-passe de aplicação, é importante lembrar-se:

- As palavras-passe de aplicação são gerados automaticamente e apenas introduziu uma vez por aplicação.

- Existe um limite de 40 palavras-passe por utilizador. Se tentar criar uma após esse limite, será solicitado para eliminar uma palavra-passe existente antes de poder criar novo.

- Utilize uma palavra-passe aplicação por dispositivo, não por aplicação. Por exemplo, crie uma única palavra-passe para todas as aplicações no computador portátil e, em seguida, outra palavra-passe única para todas as aplicações no ambiente de trabalho.

    >[!Note]
    >Novos protocolos de autenticação de suporte de clientes do Office 2013 (incluindo Outlook) e pode ser utilizado com verificação de dois passos. Esse suporte significa que após a verificação de dois passos está ativada, já não terá de palavras-passe de aplicação para clientes do Office 2013. Para obter mais informações, consulte a [como a autenticação moderna funciona para aplicações de cliente do Office 2013 e Office 2016](https://support.office.com/article/how-modern-authentication-works-for-office-2013-and-office-2016-client-apps-e4c45989-4b1a-462e-a81b-2a13191cf517) artigo.

## <a name="create-and-delete-app-passwords-using-security-info"></a>Criar e eliminar as palavras-passe de aplicação utilizando as informações de segurança

Se usar a verificação de dois passos com o seu trabalho ou a conta da instituição de ensino e o administrador tiver ativado a experiência de informações de segurança, pode criar e eliminar as palavras-passe de aplicação com o portal as minhas aplicações.

Se o administrador não tenha ativado a experiência de informações de segurança, tem de seguir as instruções e informações da [gerir palavras-passe de aplicação para a verificação de](multi-factor-authentication-end-user-app-passwords.md) secção.

### <a name="to-create-app-passwords-using-the-my-apps-portal"></a>Para criar palavras-passe de aplicação com o portal as minhas aplicações

1. Inicie sessão na sua conta escolar ou profissional.

2. Vá para myapps.microsoft.com, selecione o seu nome no canto superior direito da página e, em seguida, selecione **perfil**.

3. Na **gerir a conta** área, selecione **editar as informações de segurança**.

    ![Ecrã de perfil com a ligação de informações de segurança de editar realçada](media/security-info/security-info-profile.png)

4. Na **manter a sua conta segura** ecrã, selecione **adicione informações de segurança**.

    ![Ecrã de informações de segurança com informações existentes, editável](media/security-info/security-info-edit-add-info.png)

5. Na **adicionar informações de segurança** ecrã, selecione **palavra-passe de aplicação**.

6. Na **criar a sua palavra-passe de aplicação** ecrã, escreva um nome para a sua palavra-passe de aplicação e, em seguida, selecione **próxima**.

    ![Ecrã onde nomeia a palavra-passe de aplicação](media/security-info/security-info-name-app-password.png)

7. Selecione **cópia** para copiar a palavra-passe para a área de transferência e, em seguida, selecione **próxima**.

    ![Ecrã com a palavra-passe de aplicação para copiar](media/security-info/security-info-create-app-password.png)
    
8. Certifique-se de que a palavra-passe de aplicação aparece no **manter a sua conta segura** ecrã.

    ![Manter o ecrã seguro, com a palavra-passe de aplicação](media/security-info/security-info-keep-secure-app-password.png)

### <a name="to-delete-app-passwords-using-the-my-apps-portal"></a>Para eliminar as palavras-passe de aplicação com o portal as minhas aplicações

1. Sobre o **manter a sua conta segura** ecrã, selecione a **X** junto a palavra-passe de aplicação para eliminar.

    ![Manter a tela segura, elimine a palavra-passe de aplicação](media/security-info/security-info-keep-secure-delete-app-password.png)

2. Na **palavra-passe de aplicação de eliminação** ecrã, selecione **eliminar**.

    ![Eliminar ecrã de palavra-passe de aplicação](media/security-info/security-info-keep-secure-delete-app-password2.png)

## <a name="next-steps"></a>Passos Seguintes

- Se precisar de atualizar as suas informações de segurança, siga as instruções no [gerir as suas informações de segurança](security-info-manage-settings.md) artigo.

- Para obter informações mais gerais sobre informações de segurança e o que pode fazer, consulte [descrição geral de informações de segurança](user-help-security-info-overview.md) 
