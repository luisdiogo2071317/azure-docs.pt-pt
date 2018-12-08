---
title: O que é o federação com o Azure AD? | Microsoft Docs
description: Descreve o federação com o Azure AD.
services: active-directory
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/28/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 0556a01490786ec4101176bc0437661723ce9b4f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53109499"
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