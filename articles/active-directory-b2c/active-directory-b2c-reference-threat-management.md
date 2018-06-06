---
title: Ameaça gestão no Azure Active Directory B2C | Microsoft Docs
description: Saiba mais sobre as técnicas de deteção e a respetiva mitigação de ataques denial of service e ataques de palavra-passe no Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 03/27/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: b2686b4bb2b98d3f79d8087f6857c149cfdeb553
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34711268"
---
# <a name="azure-active-directory-b2c-threat-management"></a>Do Azure Active Directory B2C: Gestão de ameaça

Gestão de ameaça inclui o planeamento para proteção contra ataques contra o seu sistema e redes. Ataques denial-of-service podem tornar os recursos disponíveis para utilizadores-alvo. Ataques de palavra-passe levar ao acesso não autorizado aos recursos. O Azure Active Directory B2C (Azure AD B2C) com funcionalidades incorporadas que podem ajudar a proteger os seus dados em relação a estas ameaças de várias formas.

## <a name="denial-of-service-attacks"></a>Ataques denial-of-service

Azure AD B2C utiliza a deteção e a respetiva mitigação técnicas, como cookies SIN e limites de taxa e ligação para proteger recursos subjacentes contra ataques denial-of-service.

## <a name="password-attacks"></a>Ataques de palavra-passe

O Azure AD B2C também tem técnicas de mitigação de ataques de palavra-passe. Inclui mitigação de ataques de palavra-passe de dicionário e ataques de palavra-passe de força bruta. Palavras-passe que são definidas pelos utilizadores têm de ser razoavelmente complexas. Ao utilizar vários sinais, o Azure AD B2C analisa a integridade dos pedidos. O Azure AD B2C foi concebido para diferenciar inteligentemente utilizadores-alvo de hackers e botnets. O Azure AD B2C fornece uma estratégia sofisticada para contas de bloqueio com base nas palavras-passe introduzidas na probabilidade de um ataque.

Para obter mais informações, visite o [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).
