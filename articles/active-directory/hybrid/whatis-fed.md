---
title: O que é o federação com o Azure AD? | Microsoft Docs
description: Descreve o federação com o Azure AD.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/28/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: ac58992d81be8d3e275dc89a26ed4cb8b6a3242c
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54475973"
---
# <a name="what-is-federation-with-azure-ad"></a>O que é o federação com o Azure AD?

Federação é uma coleção de domínios que tenham estabelecido confiança. O nível de confiança pode variar, mas normalmente inclui autenticação e quase sempre inclui autorização. Uma federação típica pode incluir um número de organizações que tenham estabelecido confiança para acesso partilhado a um conjunto de recursos.

Pode federar o seu ambiente no local com o Azure AD e utilizar esta federação para autenticação e autorização.  Este método de início de sessão assegura que todos os autenticação do usuário ocorre no local.  Este método permite aos administradores implementar níveis mais rigorosas de controlo de acesso. Federação com o AD FS e o PingFederate está disponível.

![Identidade federada](./media/whatis-hybrid-identity/federated-identity.png)


> [!TIP]
> Se optar por utilizar a Federação com o Azure Active Directory Federation Services (AD FS), pode, facultativamente, configurar a sincronização de hash de palavras-passe como método alternativo, para o caso de a infraestrutura do AD FS falhar.


## <a name="next-steps"></a>Próximos Passos

- [O que é a identidade híbrida?](whatis-phs.md)
- [O que é o Azure AD Connect e Connect Health?](whatis-azure-ad-connect.md)
- [O que é a sincronização de hash de palavra-passe?](whatis-phs.md)
- [O que é o Federação?](whatis-fed.md)
- [O que é o início de sessão único em?](how-to-connect-sso.md)
- [Como funciona a Federação](how-to-connect-fed-whatis.md)
- [Federação com PingFederate](how-to-connect-install-custom.md#configuring-federation-with-pingfederate)