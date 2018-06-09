---
title: O Assistente de segurança do Azure AD Privileged Identity Management
description: Na primeira vez, utilizar a extensão do Azure Active Directory Privileged Identity Management, será apresentada com um Assistente de segurança. Este artigo descreve os passos para utilizar o assistente.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: protection
ms.date: 02/27/2017
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: cb08e5143814db29a7bec8c46226f07a26e51d25
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35233025"
---
# <a name="using-the-security-wizard-in-azure-ad-privileged-identity-management"></a>Utilizar o Assistente de segurança no Azure AD Privileged Identity Management 
Se for a primeira pessoa a execução do Azure Privileged Identity Management (PIM) para a sua organização, será apresentada com um assistente. O assistente ajuda-o a compreender os riscos de segurança das identidades privilegiadas e como utilizar o PIM para reduzir os riscos. Não precisa de fazer alterações ao atribuições de funções existente no assistente, se preferir fazê-lo mais tarde.

## <a name="what-to-expect"></a>O que esperar
Antes de começa a sua organização utilizar PIM, todas as atribuições de função são permanentes: os utilizadores estão sempre nestas funções, mesmo se é necessário atualmente os seus privilégios.  O primeiro passo do assistente mostra uma lista de funções de privilégios elevados e quantos utilizadores estão a ser dessas funções. Pode desagregar uma função específica para obter mais informações sobre os utilizadores se um ou mais destes não estão familiarizados.

O segundo passo do assistente fornece-lhe uma oportunidade para alterar as atribuições de funções de administrador.  

> [!WARNING]
> É importante que tenha, pelo menos, um administrador global e mais do que um administrador com função privilegiada com uma conta institucional (não uma conta Microsoft). Se houver apenas um administrador com função privilegiada, a organização não será capaz de gerir PIM se essa conta é eliminada.
> Além disso, tenha atribuições de funções permanente se um utilizador tiver uma conta Microsoft (uma conta que utilizam para iniciar sessão nos serviços Microsoft como o Outlook.com e o Skype). Se pretende exigir a MFA para ativação para essa função, esse utilizador será bloqueado.
> 
> 

Depois de efetuar alterações, o assistente irá deixar de mostrar cópias de segurança. Da próxima vez que o utilizador ou outro administrador com função privilegiada utilizar PIM, irá ver o dashboard do PIM.  

* Se quiser adicionar ou remover utilizadores de funções ou alterar atribuições permanente elegível para ler mais informações em [como adicionar ou remover a função de um utilizador](active-directory-privileged-identity-management-how-to-add-role-to-user.md).
* Se pretende conceder acesso para gerir o PIM mais utilizadores, leia mais em [como conceder acesso para gerir no PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="next-steps"></a>Passos Seguintes
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

