---
title: "Migrar uma política clássica que exija a autenticação multifator no portal do Azure | Microsoft Docs"
description: "Este artigo mostra como migrar uma política clássica que exija a autenticação multifator no portal do Azure."
services: active-directory
keywords: "acesso condicional para aplicações, o acesso condicional com o Azure AD, o acesso seguro aos recursos da empresa, as políticas de acesso condicional"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/11/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 77484dc3773736ea15c39ede5f9d49b6b694d960
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-a-classic-policy-that-requires-multi-factor-authentication-in-the-azure-portal"></a>Migrar uma política clássica que exija a autenticação multifator no portal do Azure 

Este artigo mostra como migrar uma política de clássica requer **autenticação multifator** para uma aplicação de nuvem. Embora não seja um pré-requisito, recomendamos que leia [migrar políticas clássicas no portal do Azure](active-directory-conditional-access-migration.md) antes de começar a migrar as suas políticas clássicas.


 
## <a name="overview"></a>Descrição geral 

O cenário neste artigo mostra como migrar uma política de clássica requer **autenticação multifator** para uma aplicação de nuvem. 

![Azure Active Directory](./media/active-directory-conditional-access-migration/33.png)


O processo de migração incluir os seguintes passos:

1. [Abrir a política clássica](#open-a-classic-policy) para obter as definições de configuração.
2. Criar uma nova política de acesso condicional do Azure AD para substituir a política de clássica. 
3. Desative a política clássica.



## <a name="open-a-classic-policy"></a>Abra uma política clássica

1. No [portal do Azure](https://portal.azure.com), navbar esquerdo, clique em **do Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration-mfa/01.png)

2. No **do Azure Active Directory** na página de **gerir** secção, clique em **acesso condicional**.

    ![Acesso condicional](./media/active-directory-conditional-access-migration-mfa/02.png)

3. No **gerir** secção, clique em **políticas clássico (pré-visualização)**.

    ![Políticas clássicas](./media/active-directory-conditional-access-migration-mfa/12.png)

4. Na lista de políticas clássicas, clique na política que requer **autenticação multifator** para uma aplicação de nuvem.

    ![Políticas clássicas](./media/active-directory-conditional-access-migration-mfa/13.png)


## <a name="create-a-new-conditional-access-policy"></a>Criar uma nova política de acesso condicional


1. No [portal do Azure](https://portal.azure.com), navbar esquerdo, clique em **do Azure Active Directory**.

    ![Azure Active Directory](./media/active-directory-conditional-access-migration/01.png)

2. No **do Azure Active Directory** na página de **gerir** secção, clique em **acesso condicional**.

    ![Acesso condicional](./media/active-directory-conditional-access-migration/02.png)



3. No **acesso condicional** página, para abrir o **novo** página, na barra de ferramentas na parte superior, clique em **adicionar**.

    ![Acesso condicional](./media/active-directory-conditional-access-migration/03.png)

4. No **novo** na página de **nome** caixa de texto, escreva um nome para a sua política.

    ![Acesso condicional](./media/active-directory-conditional-access-migration/29.png)

5. No **atribuições** secção, clique em **utilizadores e grupos**.

    ![Acesso condicional](./media/active-directory-conditional-access-migration/05.png)

    a. Se tiver de todos os utilizadores selecionados na sua política clássica, clique em **todos os utilizadores**. 

    ![Acesso condicional](./media/active-directory-conditional-access-migration/35.png)

    b. Se tiver selecionados na sua política clássica de grupos, clique em **selecionar utilizadores e grupos**e, em seguida, selecione os utilizadores necessários e grupos.

    ![Acesso condicional](./media/active-directory-conditional-access-migration/36.png)

    c. Se tiver os grupos excluídos, clique em de **excluir** separador e, em seguida, selecione os utilizadores necessários e grupos. 

    ![Acesso condicional](./media/active-directory-conditional-access-migration/37.png)

6. No **novo** página, para abrir o **aplicações em nuvem** na página de **atribuição** secção, clique em **aplicações em nuvem**.

    ![Acesso condicional](./media/active-directory-conditional-access-azure-portal-get-started/07.png)

8. No **aplicações em nuvem** página, execute os seguintes passos:

    ![Acesso condicional](./media/active-directory-conditional-access-migration/08.png)

    a. Clique em **selecionar aplicações**.

    b. Clique em **Selecionar**.

    c. No **selecione** página, selecione a sua aplicação de nuvem e, em seguida, clique em **selecione**.

    d. No **aplicações em nuvem** página, clique em **feito**.



9. Se tiver **requer autenticação multifator** selecionado:

    ![Acesso condicional](./media/active-directory-conditional-access-migration/26.png)

    a. No **controlos de acesso** secção, clique em **conceder**.

    ![Acesso condicional](./media/active-directory-conditional-access-migration/27.png)

    b. No **conceder** página, clique em **conceder acesso**e, em seguida, clique em **requer autenticação multifator**.

    c. Clique em **Selecionar**.


10. Clique em **no** para ativar a política.

    ![Acesso condicional](./media/active-directory-conditional-access-migration/30.png)



## <a name="disable-the-classic-policy"></a>Desativar a política clássica

Para desativar a política clássica, clique em **desativar** no **detalhes** vista.

![Políticas clássicas](./media/active-directory-conditional-access-migration-mfa/14.png)



## <a name="next-steps"></a>Passos seguintes

- Para obter mais informações sobre a migração de política clássico, consulte [migrar políticas clássicas no portal do Azure](active-directory-conditional-access-migration.md).


- Se quiser saber como configurar uma política de acesso condicional, consulte [introdução ao acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Se estiver pronto para configurar políticas de acesso condicional para o seu ambiente, consulte o [melhores práticas para acesso condicional no Azure Active Directory](active-directory-conditional-access-best-practices.md). 
