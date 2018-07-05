---
title: Informações de ameaças de gestão no Azure Active Directory B2C | Documentos da Microsoft
description: Saiba mais sobre técnicas de deteção e mitigação de ataques denial-of-service e ataques de palavra-passe no Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/27/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 7957fdf245090cbca3726cb1e4788ec34f63faca
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440424"
---
# <a name="azure-active-directory-b2c-threat-management"></a>Do Azure Active Directory B2C: Gestão de ameaças

Gestão de ameaças inclui o planeamento para proteção contra ataques contra o seu sistema e redes. Ataques denial-of-service podem tornar recursos indisponível para usuários. Ataques de palavra-passe levam ao acesso não autorizado aos recursos. O Azure Active Directory B2C (Azure AD B2C) tem recursos incorporados que podem ajudá-lo a proteger os seus dados contra essas ameaças de várias formas.

## <a name="denial-of-service-attacks"></a>Ataques denial-of-service

Azure AD B2C utiliza deteção e atenuação técnicas, como cookies SYN e limites de taxa e ligação de rede para proteger os recursos subjacentes contra ataques denial-of-service.

## <a name="password-attacks"></a>Ataques de palavra-passe

O Azure AD B2C também tem as técnicas de atenuação para ataques de palavra-passe. Atenuação inclui os ataques de palavra-passe de dicionário e ataques de força bruta a senhas. Palavras-passe que são definidas por utilizadores têm de ser razoavelmente complexo. Ao utilizar vários sinais, o Azure AD B2C analisa a integridade dos pedidos. O Azure AD B2C foi concebido para inteligência diferenciar usuários de hackers e botnets. O Azure AD B2C fornece uma estratégia sofisticada para bloquear as contas com base em palavras-passe introduzidas na probabilidade de um ataque.

Para obter mais informações, visite o [Microsoft Trust Center](https://www.microsoft.com/en-us/trustcenter/default.aspx).
