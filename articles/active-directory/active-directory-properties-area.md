---
title: Adicionar informações de privacidade da sua organização no Azure AD | Microsoft Docs
description: Explica como adicionar informações de privacidade da sua organização para a área de propriedades do Azure Active Directory (Azure AD).
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: lizross
ms.reviewer: bpham
ms.custom: it-pro
ms.openlocfilehash: 8cdf30ed09601a31529073eaedd4ab53780157d5
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
ms.locfileid: "34077551"
---
# <a name="how-to-add-your-organizations-privacy-info-in-azure-active-directory"></a>Procedimentos: Adicionar informações de privacidade da sua organização no Azure Active Directory
Este artigo explica como um administrador inquilino pode adicionar informações relacionadas com a privacidade no inquilino do Azure Active Directory (Azure AD) da organização, através do portal do Azure.

Recomendamos vivamente que adicione o seu contacto privacidade global e declaração de privacidade da sua organização, para que os empregados internos e externos convidados podem rever as políticas. Porque as declarações de privacidade exclusivamente são criadas e adaptadas para cada empresas, recomendamos vivamente que contacte um lawyer para obter assistência.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="access-the-properties-area-to-add-your-privacy-info"></a>Aceder à área de propriedades para adicionar as suas informações de privacidade

1.  Inicie sessão no portal do Azure como um administrador inquilino.

2.  No navbar esquerdo, selecione **do Azure Active Directory**e, em seguida, selecione **propriedades**.

    O **propriedades** área aparece.

    ![Área de propriedades do AD do Azure, realce a área de informações de privacidade](./media/active-directory-properties-area/properties-area.png)

3.  Adicione as suas informações de privacidade para os seus empregados:

    - **Contacte o técnico.** Escreva o endereço de correio eletrónico para a pessoa contactar o suporte técnico da sua organização.
    
    - **Contacte privacidade global.** Escreva o endereço de correio eletrónico para a pessoa contactar para compras sobre privacidade de dados pessoais. Esta pessoa também é quem Microsoft contacta se existir uma violação de dados. Se não houver nenhuma pessoa listada aqui, Microsoft entra em contacto com os administradores globais.

    - **URL da declaração de privacidade.** Tipo de ligação para o documento da sua organização que descreve a forma como a sua organização processa ambos privacidade dos dados internos e externos do convidado.

        >[!Important]
        >Se não incluir a sua declaração de privacidade ou para o seu contacto de privacidade, os convidados externos irão ver o texto a **permissões de revisão** caixa que indica,  **< _o nome da sua organização_> não forneceu as ligações para os seus termos de licenciamento para rever**. Por exemplo, um utilizador convidado verá esta mensagem quando recebem um convite para aceder a uma organização através de colaboração B2B.

        ![Caixa de permissões de revisão de colaboração B2B com a mensagem](./media/active-directory-properties-area/active-directory-no-privacy-statement-or-contact.png)

4.  Selecione **Guardar**.

## <a name="next-steps"></a>Passos Seguintes
- [Resgate de convite de colaboração B2B do Active Directory do Azure](https://aka.ms/b2bredemption)
- [Adicionar ou alterar informações de perfil para um utilizador no Azure Active Directory](/active-directory-users-profile-azure-portal.md)