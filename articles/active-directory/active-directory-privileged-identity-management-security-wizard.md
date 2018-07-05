---
title: O Assistente de segurança do Azure AD Privileged Identity Management
description: Na primeira vez que utiliza a extensão do Azure Active Directory Privileged Identity Management, será apresentada com um Assistente de segurança. Este artigo descreve os passos para utilizar o assistente.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: protection
ms.date: 02/27/2017
ms.author: rolyon
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: d380c8076fbe1a6c9e06acb6f583d719f43aaba4
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448599"
---
# <a name="using-the-security-wizard-in-azure-ad-privileged-identity-management"></a>Utilizar o Assistente de segurança no Azure AD Privileged Identity Management 
Se for a primeira pessoa a executar o Azure Privileged Identity Management (PIM) para a sua organização, será apresentado um assistente. O assistente ajuda-o a compreender os riscos de segurança de identidades privilegiadas e como utilizar o PIM para reduzir esses riscos. Não precisa de fazer alterações às atribuições de funções existente no assistente, se prefira fazê-lo mais tarde.

## <a name="what-to-expect"></a>O que esperar
Antes de sua organização começa a utilizar o PIM, todas as atribuições de função são permanentes: os utilizadores são sempre nessas funções, mesmo que eles não precisam atualmente seus privilégios.  A primeira etapa do assistente mostra uma lista de funções de privilégios elevados e quantos utilizadores estão atualmente nessas funções. Pode explorar a uma função específica para saber mais sobre os utilizadores se um ou mais deles não estão familiarizados.

A segunda etapa do assistente fornece uma oportunidade para alterar as atribuições de funções do administrador.  

> [!WARNING]
> É importante que tem, pelo menos, um administrador global e mais do que um administrador com função privilegiada com uma conta institucional (não uma conta Microsoft). Se houver apenas um administrador com função privilegiada, a organização não será capaz de gerir PIM se essa conta é eliminada.
> Além disso, mantenha as atribuições de funções permanente se um utilizador tiver uma conta Microsoft (uma conta que utilizam para iniciar sessão para serviços Microsoft como o Skype e o Outlook.com). Se quiser exigir a MFA para ativação para essa função, que o utilizador será bloqueado.
> 
> 

Depois de efetuar alterações, o assistente já não será exibido. Da próxima vez que o utilizador ou outro administrador com função privilegiada utilizar o PIM, verá o dashboard do PIM.  

* Se desejar adicionar ou remover utilizadores de funções ou alterar as atribuições de permanente para elegíveis, leia mais sobre [como adicionar ou remover função de um utilizador](active-directory-privileged-identity-management-how-to-add-role-to-user.md).
* Se gostaria de dar mais usuários acesso para gerir o PIM, leia mais sobre [como conceder acesso para gerir no PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="next-steps"></a>Passos Seguintes
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

