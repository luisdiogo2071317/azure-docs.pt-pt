---
title: Grupos dinâmicos e de colaboração B2B do Azure Active Directory do | Microsoft Docs
description: Mostra como utilizar grupos dinâmicos do Azure AD com colaboração B2B do Azure Active Directory do
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 12/14/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 18057b71415bea5d5f029db6fe311f76c1a549a1
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a>Grupos dinâmicos e de colaboração B2B do Azure Active Directory do

## <a name="what-are-dynamic-groups"></a>O que são grupos dinâmicos?
Configuração dinâmica de associação ao grupo de segurança do Azure Active Directory (Azure AD) está disponível no [portal do Azure](https://portal.azure.com). Os administradores podem definir regras para preencher os grupos que são criados no Azure AD com base nos atributos de utilizador (tais como userType, departamento ou país). Os membros podem ser automaticamente adicionados ou removidos de um grupo de segurança com base nos respetivos atributos. Estes grupos podem fornecer acesso a aplicações ou recursos de nuvem (sites do SharePoint, documentos) e atribuir licenças aos membros. Leia mais informações sobre grupos dinâmicos no [dedicado grupos no Azure Active Directory](../active-directory-accessmanagement-dedicated-groups.md).

Apropriados [licenciamento do Azure AD Premium P1 ou P2](https://azure.microsoft.com/pricing/details/active-directory/) é necessário para criar e utilizar grupos dinâmicos. Saiba mais no artigo [criar regras baseadas em atributos para filiação dinâmica em grupos no Azure Active Directory](../active-directory-groups-dynamic-membership-azure-portal.md).

## <a name="what-are-the-built-in-dynamic-groups"></a>Quais são os grupos dinâmicos incorporados?
O **todos os utilizadores** grupo dinâmico permite aos administradores de inquilinos criar um grupo que contém todos os utilizadores no inquilino com um único clique. Por predefinição, o **todos os utilizadores** grupo inclui todos os utilizadores no diretório, incluindo membros e nos convidados.
Dentro do novo portal de administração do Azure Active Directory, pode optar por ativar o **todos os utilizadores** grupo na vista de definições do grupo.

![Permitir que mostra o grupo de todos os utilizadores definido como Sim](media/use-dynamic-groups/enable-all-users-group.png)

## <a name="hardening-the-all-users-dynamic-group"></a>Proteger a todos os utilizadores de grupo dinâmico
Por predefinição, o **todos os utilizadores** grupo contém, bem como os utilizadores de (convidado) de colaboração do B2B. Pode proteger ainda mais a **todos os utilizadores** grupo utilizando uma regra para remover os utilizadores convidados. A seguinte ilustração mostra a **todos os utilizadores** grupo modificado para excluir os convidados.

![Regra de mostra onde o tipo de utilizador não é igual a convidado](media/use-dynamic-groups/exclude-guest-users.png)

Poderá também achar úteis para criar um novo grupo dinâmico que contém apenas os utilizadores convidados, para que pode aplicar políticas (por exemplo, políticas de acesso condicional do Azure AD) aos mesmos.
Um grupo poderá aspeto:

![Regra de mostra onde o tipo de utilizador for igual a convidado](media/use-dynamic-groups/only-guest-users.png)

## <a name="next-steps"></a>Passos Seguintes

- [Propriedades de utilizador de colaboração B2B](user-properties.md)
- [A adição de um utilizador de colaboração B2B a uma função](add-guest-to-role.md)
- [Acesso condicional para os utilizadores de colaboração do B2B](conditional-access.md)

