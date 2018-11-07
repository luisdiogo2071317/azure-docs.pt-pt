---
title: Resolver problemas de criação de inquilinos no Azure Active Directory B2C | Documentos da Microsoft
description: Problemas e resoluções para a criação de um inquilino do Azure Active Directory ou do Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 1d510f51ed28c28c698437f905c4911a8c32e5ce
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51234991"
---
# <a name="troubleshoot-creating-an-azure-active-directory-or-azure-active-directory-b2c-tenant"></a>Resolver problemas de criação de um inquilino do Azure Active Directory ou do Azure Active Directory B2C 

## <a name="create-an-azure-ad-tenant"></a>Criar um inquilino do Azure AD
Se não é possível criar um inquilino do Azure Active Directory (Azure AD) na primeira tentativa, tente novamente. Se o problema persistir, contacte o suporte do Azure.

## <a name="create-an-azure-ad-b2c-tenant"></a>Criar um inquilino do Azure AD B2C
Se ocorrerem problemas quando [criar um Azure Active Directory B2C inquilino (Azure AD B2C)](active-directory-b2c-get-started.md), tente as seguintes opções:

* Se o inquilino do Azure AD B2C não aparecer na sua lista de inquilinos, tente novamente criar o inquilino.
* Se o inquilino do Azure AD B2C aparecem na sua lista de inquilinos e verá a seguinte mensagem de erro, eliminar o inquilino e criá-la novamente:

    "Não foi possível concluir a criação do inquilino B2C 'contosob2c'. Visite esta [link](https://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) para obter mais orientações. "
* Aqui estão os problemas conhecidos ao eliminar um inquilino do Azure AD B2C existente e voltar a criá-lo ao utilizar o mesmo nome de domínio. Quando cria um novo inquilino do Azure AD B2C, tem de utilizar um nome de domínio diferente.
* Se essas resoluções não funcionarem, contacte o suporte do Azure. Para obter mais informações, consulte [pedidos de suporte de ficheiro para o Azure AD B2C](active-directory-b2c-support.md).

