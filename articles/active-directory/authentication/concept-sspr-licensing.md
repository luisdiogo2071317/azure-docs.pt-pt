---
title: Reposição de palavra-passe self-service de licença - Azure Active Directory
description: Requisitos de licenciamento de redefinição de senha de autoatendimento de AD do Azure
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: 64c12177e5cf6c82018731b493c0da22e1895b7f
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855529"
---
# <a name="licensing-requirements-for-azure-ad-self-service-password-reset"></a>Os requisitos de palavra-passe self-service do Azure AD de licenciamento de reposição

Palavra-passe do Azure Active Directory (Azure AD) Repor para a função, para que *tem de ter pelo menos uma licença atribuída na sua organização* para esse utilizador. É necessária uma licença adequada se um utilizador beneficiar direta ou indiretamente de qualquer funcionalidade abrangida por essa licença.

* **Os utilizadores apenas na cloud**: do Office 365 quaisquer pagas do SKU ou do Azure AD básico
* **Cloud** ou **os utilizadores no local**: Azure AD Premium P1 ou P2, Enterprise Mobility + Security (EMS) ou Microsoft 365

## <a name="licensing-requirements-for-password-writeback"></a>Requisitos de licenciamento para a repetição de escrita de palavra-passe

**Self-Service palavra-passe reposição/alteração/desbloqueio com repetição de escrita no local é uma funcionalidade premium do Azure AD**. Para obter mais informações sobre o licenciamento, consulte a [do Azure Active Directory preços site](https://azure.microsoft.com/pricing/details/active-directory/).

Para utilizar a repetição de escrita de palavra-passe, tem de ter uma das seguintes licenças atribuídas no seu inquilino:

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3 ou A3
* Enterprise Mobility + Security E5 ou A5
* O Microsoft 365 E3 ou A3
* O Microsoft 365 E5 ou A5
* O Microsoft 365 F1

> [!WARNING]
> Autónomo Office 365, planos de licenciamento *não suportam a repetição de escrita de palavra-passe* e exige que possua um dos planos anteriores para esta funcionalidade funcione.
>

Informações de licenciamento adicionais, incluindo os custos, podem ser encontradas nas seguintes páginas:

* [O Azure Active Directory, preços do site](https://azure.microsoft.com/pricing/details/active-directory/)
* [Funcionalidades do Active Directory e capacidades do Azure](https://www.microsoft.com/cloud-platform/azure-active-directory-features)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Microsoft 365 Enterprise](https://www.microsoft.com/microsoft-365/enterprise)

## <a name="enable-group-or-user-based-licensing"></a>Ativar o grupo ou licenciamento baseado no utilizador

Agora o Azure AD suporta o licenciamento baseado em grupo. Os administradores podem atribuir licenças em massa a um grupo de utilizadores, em vez de atribuir um de cada vez. Para obter mais informações, consulte [atribuir, certifique-se e resolver problemas com licenças](../users-groups-roles/licensing-groups-assign.md#step-1-assign-the-required-licenses).

Alguns serviços da Microsoft não estão disponíveis em todas as localizações. Antes de uma licença pode ser atribuída a um utilizador, o administrador tem de especificar o **localização de utilização** propriedade no utilizador. Atribuição de licenças que pode ser feita sob o **usuário** > **perfil** > **definições** secção no portal do Azure. *Quando utilizar a atribuição de licenças de grupo, todos os utilizadores sem uma localização de utilização definida herdam a localização do diretório.*

## <a name="next-steps"></a>Passos Seguintes

* [Como posso concluir uma implementação com êxito da SSPR?](howto-sspr-deployment.md)
* [Repor ou alterar a palavra-passe](../active-directory-passwords-update-your-own-password.md)
* [Registar-se na reposição personalizada de palavra-passe](../active-directory-passwords-reset-register.md)
* [Que dados são utilizados pela SSPR e que dados devem ser preenchidos por si para os seus utilizadores?](howto-sspr-authenticationdata.md)
* [Que métodos de autenticação estão disponíveis para os utilizadores?](concept-sspr-howitworks.md#authentication-methods)
* [Quais são as opções de política da SSPR?](concept-sspr-policy.md)
* [O que é a repetição de escrita de palavras-passe e por que me deve interessar?](howto-sspr-writeback.md)
* [Como posso comunicar a atividade da SSPR?](howto-sspr-reporting.md)
* [Quais são todas as opções na SSPR e o que significam?](concept-sspr-howitworks.md)
* [Creio que algo está a funcionar incorretamente. Como posso resolver problemas da SSPR?](active-directory-passwords-troubleshoot.md)
* [Tenho uma pergunta que ainda não foi abordada](active-directory-passwords-faq.md)
