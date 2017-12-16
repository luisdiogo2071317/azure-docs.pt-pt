---
title: "Azure colaboração B2B do Active Directory para organizações híbrida | Microsoft Docs"
description: "Parceiros de conceder acesso a ambos os locais e de recursos da nuvem com colaboração B2B do Azure AD"
services: active-directory
documentationcenter: 
author: twooley
manager: mtillman
editor: 
tags: 
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.date: 12/15/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 2e690eeea6a9f7e1cc10830a913774daa3c66689
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/16/2017
---
# <a name="azure-active-directory-b2b-collaboration-for-hybrid-organizations"></a>Azure colaboração B2B do Active Directory para organizações híbrida

Uma versão híbrida organização, em que tem ambos no local e na nuvem a recursos, pode querer parceiros externos de conceder acesso a ambos os locais e de recursos na nuvem. Para aceder aos recursos locais, pode gerir contas de parceiro localmente no seu ambiente do Active Directory no local. Parceiros de iniciar sessão com as credenciais da conta de parceiro para aceder aos recursos da sua organização. Para conceder parceiros acesso a recursos da nuvem com estas mesmas credenciais, agora, pode utilizar Connect do Azure Active Directory (Azure AD) para sincronizar as contas de parceiro para a nuvem, como utilizadores do Azure AD B2B (ou seja, os utilizadores com UserType = convidado).

## <a name="identify-unique-attributes-for-usertype"></a>Identificar os atributos exclusivos para UserType

Antes de ativar a sincronização do atributo UserType, primeiro tem de decidir como derivar o atributo UserType do Active Directory no local. Por outras palavras, os parâmetros no seu ambiente no local são exclusivos para os colaboradores externos? Determine um parâmetro que distingue estes colaboradores externos de membros da sua própria organização.

Duas abordagens comuns para este são:

- Designe um atributo de Active Directory não utilizados no local (por exemplo, extensionAttribute1) para utilizar como o atributo de origem. 
- Em alternativa, deriva o valor para o atributo UserType outras propriedades. Por exemplo, pretende sincronizar todos os utilizadores como convidado se os respetivos atributos de UserPrincipalName do Active Directory no local termina com o domínio  *@partners.fabrikam123.org* .
 
Para requisitos de atributo detalhadas, consulte [ativar a sincronização de UserType](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype). 

## <a name="configure-azure-ad-connect-to-sync-users-to-the-cloud"></a>Configurar o Azure AD Connect, a sincronização de utilizadores para a nuvem

Depois de identificar o atributo exclusivo, pode configurar o Azure AD Connect para sincronizar estes utilizadores para a nuvem, como utilizadores do Azure AD B2B (ou seja, os utilizadores com UserType = convidado). Um autorização ponto de vista desses utilizadores são indistinguishable dos utilizadores B2B criados durante o processo de convite de colaboração B2B do Azure AD.

Para obter instruções de implementação, consulte [ativar a sincronização de UserType](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype).

## <a name="next-steps"></a>Passos seguintes

- Para obter uma descrição geral de colaboração B2B do Azure AD, consulte [o que é a colaboração B2B do Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
- Para obter uma descrição geral do Azure AD Connect, consulte [integrar os diretórios no local ao Azure Active Directory](connect/active-directory-aadconnect.md).

