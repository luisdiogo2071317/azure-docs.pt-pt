---
title: Partilha externa do Office 365 e de colaboração B2B do Azure Active Directory do | Microsoft Docs
description: Explica como partilhar recursos com parceiros externos através de colaboração do O365 e do Azure Active Directory B2B.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/24/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 4421e6ca2bdce721c360f2701b3693024a694eb6
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a>Partilha externa do Office 365 e de colaboração B2B do Azure Active Directory do

Externo a partilha do Office 365 (OneDrive, SharePoint Online, Unified grupos, etc.) e de colaboração B2B do Azure Active Directory (Azure AD) é tecnicamente a mesma coisa. Partilha externa todas as (exceto o OneDrive/SharePoint Online), incluindo os convidados em grupos do Office 365, já utiliza o convite de colaboração B2B do Azure AD APIs para a partilha.

OneDrive/SharePoint Online, tem um Gestor de convite separado. Suporte para a partilha externa no OneDrive/SharePoint Online foi iniciada antes do Azure AD desenvolveu o respetivo suporte. Ao longo do tempo, partilha externa OneDrive/SharePoint Online tem acumulados várias funcionalidades e muitos milhões de utilizadores que utilizam o produto da-criada partilha padrão. No entanto, existem algumas ligeiras diferenças entre como funciona a partilha externa OneDrive/SharePoint Online e como funciona a colaboração B2B do Azure AD:

- OneDrive/SharePoint Online adiciona utilizadores ao diretório depois dos utilizadores têm resgatadas os respetivos convites. Por isso, antes de resgate, não verá o utilizador no portal do Azure AD. Se o outro site invites entretanto um utilizador, é gerado um novo convite. No entanto, quando utilizar a colaboração B2B do Azure AD, são adicionados utilizadores imediatamente no convite para que possam apareçam everywhere.

- A experiência de resgate no OneDrive/SharePoint Online procura diferente da experiência na colaboração B2B do Azure AD. Depois de um utilizador redeems um convite, as experiências parecer igual.

- Os utilizadores de convidado de colaboração do Azure AD B2B podem ser selecionados a partir do OneDrive/SharePoint Online partilha caixas de diálogo. Os utilizadores do OneDrive/SharePoint Online convidou apareçam também no Azure AD depois de resgatar a sua convites.

- Para gerir a partilha externa no OneDrive/SharePoint Online com a colaboração B2B do Azure AD, defina o OneDrive/SharePoint Online externo partilha definição para **permitir apenas a partilhar com utilizadores externos já no diretório**. Os utilizadores podem aceder a sites partilhados externamente e escolha colaboradores externos que o administrador tenha adicionado. O administrador pode adicionar os colaboradores externos através de convite de colaboração B2B APIs.

![O OneDrive/SharePoint Online definição de partilha externa](media/active-directory-b2b-o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a>Passos Seguintes

* [O que é a colaboração B2B do Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [A adição de um utilizador de colaboração B2B a uma função](active-directory-b2b-add-guest-to-role.md)
* [Delegar convites para colaboração do B2B](active-directory-b2b-delegate-invitations.md)
* [Grupos dinâmicos e de colaboração B2B](active-directory-b2b-dynamic-groups.md)