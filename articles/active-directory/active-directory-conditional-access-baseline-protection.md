---
title: O que é uma proteção de linha de base do acesso condicional do Azure Active Directory | Microsoft Docs
description: Saiba como a proteção da linha de base garante que tem, pelo menos, o nível de linha de base de segurança ativado no seu ambiente.
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
ms.date: 06/08/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 25ae4db2cd4f2a2cea74c428a272c6868acaa5c5
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35249303"
---
# <a name="what-is-baseline-protection"></a>O que é a proteção da linha de base?  

No último ano, ataques de identidade aumentou por 300%. Para proteger o seu ambiente contra ataques alguma vez aumentar, o Azure Active Directory (Azure AD) apresenta uma nova funcionalidade chamada proteção de linha de base. Proteção de linha de base é um conjunto de políticas de acesso condicional predefinidas. O objetivo destas políticas é Certifique-se de que tem, pelo menos, o nível de linha de base de segurança ativado no seu ambiente. 

Durante a pré-visualização, tem de ativar políticas de linha de base se pretender ativá-los. Post disponibilidade geral, estas políticas são por predefinição ativada. 

A política de proteção de linha de base primeiro exige a MFA para contas com privilégios. Os atacantes que obter o controlo de contas com privilégios podem fazer imenso danos, pelo que é fundamental para proteger estas contas, primeiro. As seguintes funções com privilégios estão no âmbito para esta política: 

- Administrador global  

- Administrador do SharePoint  

- Administrador do Exchange  

- Administrador de acesso condicional  

- Administrador de segurança  


![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/01.png)

## <a name="how-to-get-started"></a>Como começar 

Para ativar a política de linha de base:  

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) como administrador global, o administrador de segurança ou o administrador de acesso condicional.

2. No **portal do Azure**, navbar esquerdo, clique em **do Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-baseline-protection/02.png)

3. No **do Azure Active Directory** na página de **gerir** secção, clique em **acesso condicional**.

    ![Acesso condicional](./media/active-directory-conditional-access-baseline-protection/03.png)

4. Na lista de políticas, clique em **política de linha de base: exigir a MFA para os administradores (pré-visualização)**. 

5. Para ativar a política, clique em **utilizar a política de imediato**.

6. Clique em **Guardar**. 
 

A política de linha de base fornece-lhe a opção para excluir os utilizadores e grupos. Pode querer excluir um *[conta de administrador de acesso de emergência](active-directory-admin-manage-emergency-access-accounts.md)* para garantir que não é bloqueado ao inquilino.
  
 

## <a name="what-you-should-know"></a>O que deve conhecer 

As funções do diretório que estão incluídas na política de linha de base são as funções do Azure AD com mais privilégios. Com base nos comentários, outras poderão ser incluídas no futuro. 

Se tiver contas com privilégios de administrador nos scripts, deve utilizar [identidade de serviço geridas (MSI)](managed-service-identity/overview.md) ou [principais (com certificados) do serviço](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal) em vez disso. Como solução temporária, pode excluir contas de utilizador específicas da sua política de linha de base. 

A política se aplica aos fluxos de autenticação existente POP, IMAP, mais antigo cliente de ambiente de trabalho do Office. 

## <a name="next-steps"></a>Passos Seguintes

Se quiser saber como configurar uma política de acesso condicional, consulte [introdução ao acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

Se estiver pronto para configurar políticas de acesso condicional para o seu ambiente, consulte o [melhores práticas para acesso condicional no Azure Active Directory](active-directory-conditional-access-best-practices.md). 
