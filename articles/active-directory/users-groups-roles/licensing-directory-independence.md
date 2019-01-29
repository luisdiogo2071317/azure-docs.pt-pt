---
title: Características do Azure Active Directory inquilino interação | Documentos da Microsoft
description: Gerir os seus inquilinos de inquilino do Azure Active, compreendendo os seus inquilinos como recursos totalmente independentes
services: active-tenant
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 10/10/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: a1022bcc3c81ef22d1ba9f6c4905e1bb4c515fa5
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55150381"
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a>Compreender como vários inquilinos do Azure Active Directory interagem

No Azure Active Directory (Azure AD), cada inquilino é um recurso totalmente independente: um elemento de rede que é logicamente independente dos outros inquilinos que gere. Não existe nenhuma relação principal-subordinado entre inquilinos. Essa independência entre inquilinos inclui independência de recursos, independência administrativa e independência de sincronização.

## <a name="resource-independence"></a>Independência de recursos
* Se criar ou eliminar um recurso num inquilino, que não tem impacto em nenhum outro recurso no outro inquilino, com a exceção parcial dos utilizadores externos. 
* Se utilizar um dos seus nomes de domínio com um inquilino, não pode ser utilizado com nenhum outro inquilino.

## <a name="administrative-independence"></a>Independência administrativa
Se um usuário não administrativo do inquilino "Contoso" Criar um inquilino de teste 'Test', em seguida:

* Por predefinição, o utilizador que cria um inquilino é adicionado como um utilizador externo no inquilino novo e atribuído a função de administrador global desse inquilino.
* Os administradores do inquilino "Contoso" tem sem privilégios administrativos diretos para o inquilino "Teste", a menos que um administrador de "Teste" lhes concede esses privilégios. No entanto, os administradores de "Contoso" podem controlar o acesso ao inquilino "Teste" se elas controlam a conta de utilizador que criou "Teste".
* Se adicionar ou remover uma função de administrador para um usuário num inquilino, a alteração não afeta as funções de administrador que o utilizador tem noutro inquilino.

## <a name="synchronization-independence"></a>Independência de sincronização
Pode configurar cada inquilino do Azure AD independentemente para sincronização de uma única instância de qualquer um dos dados:

* A ferramenta do Azure AD Connect, para sincronizar dados com uma única floresta do AD.
* O conector para o Forefront Identity Manager, para sincronizar dados com um ou mais florestas no local e/ou origens de dados não pertencente ao Azure AD de inquilino do Active Directory do Azure.

## <a name="add-an-azure-ad-tenant"></a>Adicionar um inquilino do Azure AD
Para adicionar um inquilino do Azure AD no portal do Azure, inicie sessão no [do portal do Azure](https://portal.azure.com) com uma conta que seja administrador global do Azure AD e, no lado esquerdo, selecione **New**.

> [!NOTE]
> Ao contrário de outros recursos do Azure, os inquilinos não são recursos subordinados de uma subscrição do Azure. Se a sua subscrição do Azure é cancelada ou expirada, pode continuar a aceder os dados do inquilino com o Azure PowerShell, a Graph API do Azure ou o Centro de administração do Office 365. Também pode [associar outra subscrição ao inquilino de](../fundamentals/active-directory-how-subscriptions-associated-directory.md).
>

## <a name="next-steps"></a>Passos Seguintes
Para uma ampla visão geral de problemas de licenciamento do Azure AD e as melhores práticas, consulte [o que é inquilino do Azure ativa o licenciamento?](../fundamentals/active-directory-licensing-whatis-azure-portal.md).
