---
title: Colaboração B2B para organizações híbrida - Azure Active Directory | Microsoft Docs
description: Conceder acesso de parceiros no local e na nuvem a recursos com colaboração B2B do Azure AD.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/26/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: b9a74a4e44fefd389a309b776e50d76d362ee9d8
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="azure-active-directory-b2b-collaboration-for-hybrid-organizations"></a>Azure colaboração B2B do Active Directory para organizações híbrida

Colaboração do Azure Active Directory (Azure AD) B2B torna mais fácil para conceder os parceiros externos acesso a aplicações e recursos da sua organização. Isto acontece mesmo numa configuração híbrida onde pode contar recursos baseados na nuvem e no local. É irrelevante se atualmente gerir contas de parceiros externos localmente no seu sistema de identidade no local, ou se gerir as contas externas na nuvem como utilizadores do Azure AD B2B. Pode agora conceder estes utilizadores acesso a recursos em qualquer localização, utilizando as mesmas credenciais de início de sessão para ambos os ambientes.

## <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-apps"></a>Os utilizadores do Azure AD B2B conceder acedem às suas aplicações no local

Se a sua organização utiliza as capacidades de colaboração B2B do Azure AD para convidar utilizadores convidados de organizações de parceiros ao seu Azure AD, agora pode fornecer estas B2B os utilizadores acedam a aplicações no local.

Para aplicações que utilizam a autenticação baseada em SAML, que pode efetuar estas aplicações disponíveis para utilizadores de B2B através do portal do Azure, a utilização de autenticação do Proxy de aplicações do Azure AD.

Para aplicações que utilizam a autenticação integrada de Windows (IWA) com a delegação restringida de Kerberos (KCD), também é utilizar Proxy do Azure AD para autenticação. No entanto, para autorização funcione, é necessário um objeto de utilizador no local no Windows Server Active Directory. Existem dois métodos que pode utilizar para criar objetos de utilizador local que representam os utilizadores de convidado B2B.

- Pode utilizar o Microsoft Identity Manager (MIM) 2016 SP1 e o agente de gestão de MIM para o Microsoft Graph.
- Pode utilizar um script do PowerShell. (Esta solução não necessita de MIM.)

Para obter mais informações sobre como implementar estas soluções, consulte [conceder B2B utilizadores no Azure AD acedem às suas aplicações no local](hybrid-cloud-to-on-premises.md).

## <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources"></a>Conceda as contas de parceiro localmente geridos aceder a recursos na nuvem

Antes do Azure AD, as organizações com sistemas de identidade no local têm tradicionalmente parceiro contas geridas no seu diretório no local. Se a essa uma organização, pretende assegurar-se de que os parceiros continuam a ter acesso como mover as suas aplicações e outros recursos para a nuvem. Idealmente, pretende que estes utilizadores utilizem o mesmo conjunto de credenciais para aceder aos recursos na nuvem e no local. 

Iremos agora os métodos de oferta onde pode utilizar o Azure AD Connect para sincronizar estas contas locais para a nuvem como "os utilizadores convidados", onde as contas comportar-se apenas como utilizadores do Azure AD B2B.

Para ajudar a proteger os dados da empresa, pode controlar o acesso aos recursos à direita e configurar políticas de autorização que processe estes utilizadores convidados diferente dos seus empregados.

Para detalhes de implementação, consulte [as contas de parceiro localmente geridos de concessão de aceder a recursos de nuvem através de colaboração B2B do Azure AD](hybrid-on-premises-to-cloud.md).
 
## <a name="next-steps"></a>Passos Seguintes

- [Os utilizadores do Azure AD B2B conceder acedem às suas aplicações no local](hybrid-cloud-to-on-premises.md)
- [Conceder acesso a contas de parceiro gerido localmente a recursos de nuvem através de colaboração B2B do Azure AD](hybrid-on-premises-to-cloud.md)


