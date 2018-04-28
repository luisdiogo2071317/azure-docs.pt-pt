---
title: Conceda as contas de parceiro localmente geridos aceder a recursos na nuvem como utilizadores do Azure AD B2B | Microsoft Docs
description: Dê localmente geridos parceiros externos acesso a ambos os locais e de recursos de nuvem com as mesmas credenciais colaboração B2B do Azure AD.
services: active-directory
documentationcenter: ''
author: twooley
manager: mtillman
editor: ''
tags: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.date: 04/24/2018
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 9abbc5576708468abdda3efeeae4f28376257e32
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources-using-azure-ad-b2b-collaboration"></a>Conceder acesso a contas de parceiro gerido localmente a recursos de nuvem através de colaboração B2B do Azure AD

Antes do Azure Active Directory (Azure AD), as organizações com sistemas de identidade no local de ter contas de parceiro tradicionalmente gerido no respetivo diretório no local. Na organização, quando começar a mover as aplicações para o Azure AD, pretende assegurar-se de que os parceiros podem aceder os recursos de que precisam. Não deve é relevante se os recursos estão no local ou na nuvem. Além disso, pretende que os utilizadores do parceiro para poder utilizar as mesmas credenciais de início de sessão para recursos do Azure AD e no local. 

Se criar contas para os parceiros externos no seu diretório no local (por exemplo, criar uma conta com um nome de início de sessão de "wmoran" para um utilizador externo com o nome Wendy Moran no seu domínio partners.contoso.com), pode sincronizar agora a estas contas para o nuvem. Especificamente, pode utilizar o Azure AD Connect para sincronizar as contas de parceiro para a nuvem, como utilizadores do Azure AD B2B (ou seja, os utilizadores com UserType = convidado). Isto permite que os utilizadores do parceiro aceder a recursos da nuvem com as mesmas credenciais como as contas locais, sem conceder mais acesso que precisam. 

## <a name="identify-unique-attributes-for-usertype"></a>Identificar os atributos exclusivos para UserType

Antes de ativar a sincronização do atributo UserType, primeiro tem de decidir como derivar o atributo UserType do Active Directory no local. Por outras palavras, os parâmetros no seu ambiente no local são exclusivos para os colaboradores externos? Determine um parâmetro que distingue estes colaboradores externos de membros da sua própria organização.

Duas abordagens comuns para este são:

- Designe um atributo de Active Directory não utilizados no local (por exemplo, extensionAttribute1) para utilizar como o atributo de origem. 
- Em alternativa, deriva o valor para o atributo UserType outras propriedades. Por exemplo, pretende sincronizar todos os utilizadores como convidado se os respetivos atributos de UserPrincipalName do Active Directory no local termina com o domínio *@partners.contoso.com*.
 
Para requisitos de atributo detalhadas, consulte [ativar a sincronização de UserType](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype). 

## <a name="configure-azure-ad-connect-to-sync-users-to-the-cloud"></a>Configurar o Azure AD Connect, a sincronização de utilizadores para a nuvem

Depois de identificar o atributo exclusivo, pode configurar o Azure AD Connect para sincronizar estes utilizadores para a nuvem, como utilizadores do Azure AD B2B (ou seja, os utilizadores com UserType = convidado). Um autorização ponto de vista desses utilizadores são indistinguishable dos utilizadores B2B criados durante o processo de convite de colaboração B2B do Azure AD.

Para obter instruções de implementação, consulte [ativar a sincronização de UserType](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype).

## <a name="next-steps"></a>Passos Seguintes

- [Azure colaboração B2B do Active Directory para organizações híbrida](active-directory-b2b-hybrid-organizations.md)
- [Os utilizadores do Azure AD B2B conceder acedem às suas aplicações no local](active-directory-b2b-hybrid-cloud-to-on-premises.md)
- Para obter uma descrição geral do Azure AD Connect, consulte [integrar os diretórios no local ao Azure Active Directory](connect/active-directory-aadconnect.md).

