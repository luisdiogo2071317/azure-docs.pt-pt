---
title: O que é uma proteção de linha de base no acesso condicional do Azure Active Directory? -pré-visualização | Documentos da Microsoft
description: Saiba como a proteção de linha de base garante que tem, pelo menos, o nível de linha de base de segurança ativada no seu ambiente do Azure Active Directory.
services: active-directory
keywords: acesso condicional para aplicações, o acesso condicional com o Azure AD, o acesso seguro aos recursos da empresa, políticas de acesso condicional
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/08/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 856673d2a5465f9646172a1436ed75c0d73692cb
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003177"
---
# <a name="what-is-baseline-protection-preview"></a>O que é a proteção de linha de base (pré-visualização)?  

No último ano, ataques de identidade tem aumentado em 300%. Para proteger o seu ambiente contra os ataques cada vez, o Azure Active Directory (Azure AD) apresenta um novo recurso chamado proteção de linha de base. Proteção de linha de base é um conjunto de predefinidos [políticas de acesso condicional](../active-directory-conditional-access-azure-portal.md). O objetivo dessas diretivas é para se certificar de que tem, pelo menos, o nível de linha de base de segurança ativada em todas as edições do Azure AD. 

Este artigo fornece uma descrição geral de proteção de linha de base no Azure Active Directory.


 
## <a name="require-mfa-for-admins"></a>Exigir a MFA para os administradores

Os utilizadores com acesso a contas com privilégios têm acesso irrestrito ao seu ambiente. Devido ao poder que essas contas têm, deve tratá-los com cuidado especial. Métodos comuns para melhorar a proteção de contas com privilégios é exigir um formulário mais forte de verificação de conta quando são utilizadas para início de sessão. No Azure Active Directory, pode obter uma verificação de conta mais forte ao exigir autenticação multifator (MFA).  

**Exigir a MFA para os administradores** é uma política de linha de base que exija a MFA para as seguintes funções de diretório: 

- Administrador global  

- Administrador do SharePoint  

- Administrador do Exchange  

- Administrador de acesso condicional  

- Administrador de segurança  


![Azure Active Directory](./media/baseline-protection/01.png)

Esta política de linha de base fornece-lhe a opção para excluir os utilizadores e grupos. Pode querer excluir uma * [conta de administrador de acesso de emergência](../users-groups-roles/directory-emergency-access.md) * para garantir que não é bloqueado ao inquilino.


## <a name="enable-a-baseline-policy"></a>Ativar uma política de linha de base 

Embora as políticas de linha de base estiverem em pré-visualização, são por predefinição não ativada. Tem de ativar manualmente uma política para ativá-lo. Assim que esta funcionalidade tem atingiram a disponibilidade geral, as políticas são, por predefinição ativada. A alteração de comportamento planeada é o motivo por que há além disso, para ativar e desativar uma terceira opção para definir o estado de uma política: **ativar política automaticamente no futuro**. Ao selecionar esta opção, deixar que Microsoft decida quando deve ativar uma política.      


**Para ativar uma política de linha de base:**  

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) como administrador global, administrador de segurança ou administrador de acesso condicional.

2. Na **portal do Azure**, na barra de navegação esquerda, clique em **Azure Active Directory**.

    ![Azure Active Directory](./media/baseline-protection/02.png)

3. Na **do Azure Active Directory** página, além do **segurança** secção, clique em **acesso condicional**.

    ![Acesso condicional](./media/baseline-protection/05.png)

4. Na lista de políticas, clique numa política que começa com **política de linha de base:**. 

5. Para ativar a política, clique em **utilizar a política imediatamente**.

6. Clique em **Guardar**. 
 
  
 

## <a name="what-you-should-know"></a>O que deve saber 

Embora a gestão de políticas de acesso condicional personalizada requer uma licença do Azure AD Premium, as políticas de linha de base estão disponíveis em todas as edições do Azure AD.     

As funções de diretório que estão incluídas na política de linha de base são as funções do Azure AD com mais privilégios. 

Se o ter privilegiado contas que são utilizadas nos seus scripts, deve substitui-los com [Managed Service Identity (MSI)](../managed-service-identity/overview.md) ou [principais com certificados de serviço](../../azure-resource-manager/resource-group-authenticate-service-principal.md). Como solução temporária, pode excluir contas de usuários específicos da política de linha de base. 

Aplicam políticas de linha de base para fluxos de autenticação legada como POP, IMAP, cliente de ambiente de trabalho do Office mais antiga. 




## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações, consulte:

- [Cinco etapas para proteger a sua infraestrutura de identidade](https://docs.microsoft.com/azure/security/azure-ad-secure-steps)

- [O que é o acesso condicional no Azure Active Directory?](overview.md) 

