---
title: O que é uma proteção de linha de base do acesso condicional do Azure Active Directory? -pré-visualização | Microsoft Docs
description: Saiba como a proteção da linha de base garante que tem, pelo menos, o nível de linha de base de segurança ativado no seu ambiente do Azure Active Directory.
services: active-directory
keywords: acesso condicional para aplicações, o acesso condicional com o Azure AD, o acesso seguro aos recursos da empresa, as políticas de acesso condicional
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/21/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 86b57a82573760ac73975e851b2bb4caf769845b
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36308565"
---
# <a name="what-is-baseline-protection---preview"></a>O que é a proteção da linha de base? -preview  

No último ano, ataques de identidade aumentou por 300%. Para proteger o seu ambiente contra ataques alguma vez aumentar, o Azure Active Directory (Azure AD) apresenta uma nova funcionalidade chamada proteção de linha de base. Proteção de linha de base é um conjunto de predefinidos [políticas de acesso condicional](active-directory-conditional-access-azure-portal.md). O objetivo destas políticas é Certifique-se de que tem, pelo menos, o nível de linha de base de segurança ativado em todas as edições do Azure AD. 

Este artigo fornece uma descrição geral da proteção de linha de base no Azure Active Directory.


 
## <a name="require-mfa-for-admins"></a>Exigir a MFA para administradores

Os utilizadores com acesso a contas com privilégios têm acesso irrestrito ao seu ambiente. Devido a potência que tem estas contas, deverá tratá-los com especial cuidado. É um método comum para melhorar a proteção de contas com privilégios requerer uma forma mais forte de verificação de conta quando são utilizados para início de sessão. No Azure Active Directory, pode obter uma verificação de conta mais forte ao exigir a autenticação multifator (MFA).  

**Exigir a MFA para os administradores** é uma política de linha de base que exige a MFA para as seguintes funções de diretório: 

- Administrador global  

- Administrador do SharePoint  

- Administrador do Exchange  

- Administrador de acesso condicional  

- Administrador de segurança  


![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/01.png)

Esta política de linha de base fornece-lhe a opção para excluir os utilizadores e grupos. Pode querer excluir um *[conta de administrador de acesso de emergência](active-directory-admin-manage-emergency-access-accounts.md)* para garantir que não é bloqueado ao inquilino.


## <a name="enable-a-baseline-policy"></a>Ativar uma política de linha de base 

Enquanto as políticas de linha de base estão na pré-visualização, estes são por predefinição não ativada. Tem de ativar manualmente uma política se pretende ativar. Assim que esta funcionalidade foi atingido o disponibilidade geral, as políticas são, por predefinição ativada. A alteração do comportamento planeada é o motivo por que motivo tiver além disso, para ativar e desativar uma terceira opção para definir o estado de uma política: **ativar automaticamente política**. Ao selecionar esta opção, pode permitir que a Microsoft decidir quando ativar uma política.      


**Para ativar uma política de linha de base:**  

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) como administrador global, o administrador de segurança ou o administrador de acesso condicional.

2. No **portal do Azure**, navbar esquerdo, clique em **do Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/02.png)

3. No **do Azure Active Directory** na página de **gerir** secção, clique em **acesso condicional**.

    ![Acesso condicional](./media/active-directory-conditional-access-baseline-protection/03.png)

4. Na lista de políticas, clique numa política que começa com **política de linha de base:**. 

5. Para ativar a política, clique em **utilizar a política de imediato**.

6. Clique em **Guardar**. 
 


  
 

## <a name="what-you-should-know"></a>O que deve conhecer 

Embora a gestão de políticas de acesso condicional personalizado requer uma licença do Azure AD Premium, políticas de linha de base estão disponíveis em todas as edições do Azure AD.     

As funções do diretório que estão incluídas na política de linha de base são as funções do Azure AD com mais privilégios. 

Se tiver privilegiados contas que são utilizadas nos scripts, deve substituí-los com [identidade de serviço geridas (MSI)](./managed-service-identity/overview.md) ou [princípios com certificados de serviço](../azure-resource-manager/resource-group-authenticate-service-principal.md). Como solução temporária, pode excluir contas de utilizador específico da política de linha de base. 

Políticas de linha de base aplicam aos fluxos de autenticação existente POP, IMAP, mais antigo cliente de ambiente de trabalho do Office. 




## <a name="next-steps"></a>Passos Seguintes

Se quiser saber como configurar uma política de acesso condicional, consulte [introdução ao acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

Se estiver pronto para configurar políticas de acesso condicional para o seu ambiente, consulte o [melhores práticas para acesso condicional no Azure Active Directory](active-directory-conditional-access-best-practices.md). 
