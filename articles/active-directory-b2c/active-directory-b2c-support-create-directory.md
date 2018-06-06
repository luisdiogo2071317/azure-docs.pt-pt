---
title: Resolver problemas de inquilinos a criar no Azure Active Directory B2C | Microsoft Docs
description: Problemas e as resoluções para criar um inquilino do Azure Active Directory ou do Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 90d9d2fb80dfbd094754850b7d1270a5fafcdd96
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712509"
---
# <a name="troubleshoot-creating-an-azure-active-directory-or-azure-active-directory-b2c-tenant"></a>Resolver problemas de criação de um inquilino do Azure Active Directory ou do Azure Active Directory B2C 

## <a name="create-an-azure-ad-tenant"></a>Criar um inquilino do Azure AD
Se não é possível criar um inquilino do Azure Active Directory (Azure AD) na primeira tentativa, tente novamente. Se o problema persistir, contacte o suporte do Azure.

## <a name="create-an-azure-ad-b2c-tenant"></a>Criar um inquilino do Azure AD B2C
Se ocorrerem problemas quando é [criar um Azure Active Directory B2C inquilino (Azure AD B2C)](active-directory-b2c-get-started.md), experimente as seguintes opções:

* Se o inquilino do Azure AD B2C não aparecer na lista de inquilinos, tente novamente criar o inquilino.
* Se o inquilino do Azure AD B2C aparecer na lista de inquilinos e verá a seguinte mensagem de erro, elimine o inquilino e criá-la novamente:

    "Não foi possível concluir a criação do inquilino B2C 'contosob2c'. Visite este [ligação](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) para obter mais orientações. "
* Não existe são problemas conhecidos quando eliminar um inquilino do Azure AD B2C existente e recrie-a utilizando o mesmo nome de domínio. Quando cria um novo inquilino do Azure AD B2C, tem de utilizar um nome de domínio diferente.
* Se estes resoluções não funcionam, contacte o suporte do Azure. Para obter mais informações, consulte [pedidos de suporte de ficheiro para o Azure AD B2C](active-directory-b2c-support.md).

