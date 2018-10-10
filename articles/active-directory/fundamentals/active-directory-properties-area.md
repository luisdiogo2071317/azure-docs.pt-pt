---
title: Adicionar informações de privacidade da sua organização no Azure Active Directory | Documentos da Microsoft
description: Como adicionar informações de privacidade da sua organização para a área de propriedades do Azure Active Directory (Azure AD).
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: lizross
ms.reviewer: bpham
ms.custom: it-pro
ms.openlocfilehash: f32eeee8f02cee32eb05e14aa98387a20cb2e626
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48904295"
---
# <a name="how-to-add-your-privacy-info-using-azure-active-directory"></a>Procedimentos: Adicionar as suas informações de privacidade com o Azure Active Directory
Este artigo explica como um administrador de inquilinos pode adicionar informações relacionadas com a privacidade ao inquilino do Azure Active Directory (Azure AD) de uma organização, através do portal do Azure.

Recomendamos vivamente que adicione o seu contacto de privacidade global e declaração de privacidade da sua organização, para que os seus funcionários internos e convidados externos, podem rever as suas políticas. Como declarações de privacidade são exclusivamente criadas e desenvolvido sob medidas para cada empresa, é altamente recomendável que contate um advogado para obter assistência.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="add-your-privacy-info-on-azure-ad"></a>Adicionar as suas informações de privacidade no Azure AD
Adicionar informações de privacidade da sua organização no **propriedades** área do Azure AD.

### <a name="to-access-the-properties-area-and-add-your-privacy-information"></a>Para acessar a área de propriedades e adicionar as informações de privacidade

1.  Inicie sessão no portal do Azure como um administrador inquilino.

2.  Na barra de navegação esquerda, selecione **do Azure Active Directory**e, em seguida, selecione **propriedades**.

    O **propriedades** área é apresentada.

    ![Área de propriedades do AD do Azure, realce a área de informações de privacidade](media/active-directory-properties-area/properties-area.png)

3.  Adicione as suas informações de privacidade para os seus funcionários:

    - **Contacto técnico.** Escreva o endereço de e-mail da pessoa entrar em contacto para suporte técnico da sua organização.
    
    - **Contacto de privacidade global.** Escreva o endereço de e-mail da pessoa entrar em contacto para questões sobre a privacidade dos dados pessoais. Essa pessoa também se trata de quem entra em contacto com Microsoft se houver uma violação de dados. Se não houver nenhuma pessoa listada aqui, Microsoft entra em contacto com os administradores globais.

    - **URL da declaração de privacidade.** Escreva a ligação para documento da sua organização que descreve como a sua organização lida com ambos privacidade dos dados internos e externos do convidado.

        >[!Important]
        >Se não incluir a sua declaração de privacidade ou seu contacto de privacidade, os convidados externos irão ver o texto na **permissões de revisão** caixa que diz  **< _seu nome da org_> não forneceu ligações para os seus termos para rever**. Por exemplo, um utilizador convidado irá ver esta mensagem quando recebem um convite para aceder a uma organização por meio da colaboração B2B.

        ![Caixa de permissões de revisão de colaboração do B2B com a mensagem](media/active-directory-properties-area/active-directory-no-privacy-statement-or-contact.png)

4.  Selecione **Guardar**.

## <a name="next-steps"></a>Passos Seguintes
- [Resgate de convite de colaboração do Azure Active Directory B2B](https://aka.ms/b2bredemption)
- [Adicionar ou alterar as informações de perfil para um utilizador no Azure Active Directory](active-directory-users-profile-azure-portal.md)