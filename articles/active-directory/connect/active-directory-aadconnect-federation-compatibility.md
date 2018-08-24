---
title: Lista de compatibilidades de federação do Azure AD
description: Esta página tem fornecedores de identidade de terceiros que podem ser utilizados para implementar o início de sessão único.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: curtand
ms.assetid: 22c8693e-8915-446d-b383-27e9587988ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 3b2313b79c57a95af40d29bca3d7522c83e10e4c
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818719"
---
# <a name="azure-ad-federation-compatibility-list"></a>Lista de compatibilidades de federação do Azure AD
O Azure Active Directory fornece início de sessão único em e avançada de segurança de acesso de aplicativo para Office 365 e outros serviços Online da Microsoft para híbridos e implementações apenas na cloud sem exigir qualquer solução de terceiros. Office 365, como a maioria dos Serviços Online da Microsoft, está integrado com o Azure Active Directory para serviços de diretório, autenticação e autorização. O Azure Active Directory também fornece início de sessão único a milhares de aplicações SaaS e aplicativos da web no local. Veja o Azure Active Directory [Galeria de aplicações](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps) para aplicações SaaS suportadas. 

## <a name="idp-validation"></a>Validação de IDP
Se a sua organização utiliza uma solução de Federação de terceiros, pode configurar o início de sessão único para os seus utilizadores do Active Directory no local com os Serviços Online da Microsoft, como o Office 365, desde que a solução de Federação de terceiros é compatível com o Azure Do Active Directory.  Para perguntas sobre a compatibilidade, contacte o fornecedor de identidade.  Se gostaria de ver uma lista de fornecedores de identidade que anteriormente foram testados para compatibilidade com o Azure AD, pela Microsoft, clique em [aqui](https://www.microsoft.com/download/details.aspx?id=56843). 

>[!NOTE]
>A Microsoft já não fornece testes de validação para fornecedores de identidade independente para compatibilidade com o Azure Active Directory. Se quiser testar o seu produto para interoperabilidade, consulte estes [diretrizes](https://www.microsoft.com/download/details.aspx?id=56843). 

## <a name="next-steps"></a>Próximos Passos

- [Integrar os diretórios no local com o Azure Active Directory](active-directory-aadconnect.md)
- [Azure AD Connect e a federação](active-directory-aadconnectfed-whatis.md)
