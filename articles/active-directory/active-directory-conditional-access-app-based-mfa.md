---
title: Guia de introdução - configurar por aplicação de nuvem MFA com acesso condicional do Azure Active Directory | Microsoft Docs
description: Saiba como pode juntar os requisitos de autenticação para o tipo de aplicação de nuvem acedidos através de acesso condicional do Azure Active Directory (Azure AD).
services: active-directory
keywords: acesso condicional para aplicações, o acesso condicional com o Azure AD, o acesso seguro aos recursos da empresa, as políticas de acesso condicional
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: eba67e6fe7d836d7b36d2bcd576eeaf4c70df970
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-configure-per-cloud-app-mfa-with-azure-active-directory-conditional-access"></a>Início rápido: Configurar por aplicação de nuvem MFA com acesso condicional do Azure Active Directory 


Para simplificar a experiência de início de sessão dos utilizadores, pode querer Permitir início de sessão às suas aplicações na nuvem utilizando um nome de utilizador e uma palavra-passe. No entanto, muitos ambientes tem, pelo menos, algumas aplicações para o qual é recomendado para exigir um formulário de verificação de conta, tais como a autenticação multifator mais forte. Isto pode ser, para VERDADEIRO de exemplo, para acesso ao sistema de e-mail da sua organização ou as aplicações de RH.  

Este guia de introdução mostra como pode exigir a autenticação multifator apenas para um conjunto de aplicações na nuvem selecionada no seu ambiente utilizando um [política de acesso condicional do Azure AD](active-directory-conditional-access-azure-portal.md).

Para concluir o cenário deste guia de introdução, poderá:


> [!div class="checklist"]
> * Criar a sua política de acesso condicional
> * Avaliar a política de acesso condicional
> * Testar a sua política de acesso condicional  


## <a name="scenario-description"></a>Descrição do cenário

O cenário neste artigo utiliza o portal do Azure como marcador de posição para uma aplicação de nuvem que requer autenticação multifator para um utilizador específico. Britta Simon é um utilizador na sua organização. Quando iniciar sessão para o portal do Azure, pretende-lhe para verificar ainda mais a sua conta multi-factor Authentication.

![Multi-Factor Authentication](./media/active-directory-conditional-access-app-based-mfa/01.png)



## <a name="before-you-begin"></a>Antes de começar 

Para concluir o cenário deste guia de introdução, precisa de:

- **Acesso a uma edição do Azure AD Premium** -acesso condicional do Azure AD é uma capacidade do Azure AD Premium. Se não tiver acesso para uma edição do Azure AD Premium, pode [inscrição para uma versão de avaliação](https://azure.microsoft.com/trial/get-started-active-directory/).

- **Uma conta de teste chamado Britta Simon** - se não souber como criar uma conta de teste, leia [estas instruções](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).



## <a name="create-your-conditional-access-policy"></a>Criar a sua política de acesso condicional 

Esta secção mostra como criar a política de acesso condicional necessária.  
Na sua política, definir:

|Definição |Valor|
|---     | --- |
|Utilizadores e grupos | Britta Simon |
|Aplicações na cloud | Gestão do Microsoft Azure |
|Conceder | Exigir autenticação multifator |
 

![Criar política](./media/active-directory-conditional-access-app-based-mfa/12.png)




**Para configurar a política de acesso condicional:**

1. Inicie sessão no seu [portal do Azure](https://portal.azure.com) como administrador global.

2. No portal do Azure, no navbar esquerdo, clique em **do Azure Active Directory**. 

    ![Azure Active Directory](./media/active-directory-conditional-access-app-based-mfa/02.png)

3. No **do Azure Active Directory** na página de **gerir** secção, clique em **acesso condicional**.

    ![Acesso condicional](./media/active-directory-conditional-access-app-based-mfa/03.png)
 
4. No **acesso condicional** página, na barra de ferramentas na parte superior, clique em **adicionar**.

    ![Adicionar](./media/active-directory-conditional-access-app-based-mfa/04.png)

5. No **novo** na página de **nome** caixa de texto, tipo **exigir a MFA para Britta**.

    ![Nome](./media/active-directory-conditional-access-app-based-mfa/05.png)

6. No **atribuição** secção, clique em **utilizadores e grupos**.

    ![Utilizadores e grupos](./media/active-directory-conditional-access-app-based-mfa/06.png)

7. No **utilizadores e grupos** página, execute os seguintes passos:

    ![Utilizadores e grupos](./media/active-directory-conditional-access-app-based-mfa/07.png)

    a. Clique em **selecionar utilizadores e grupos**.

    b. Clique em **Selecionar**.

    c. No **selecione** página, selecione o utilizador de teste e, em seguida, clique em **selecione**.

    d. No **utilizadores e grupos** página, clique em **feito**.

8. Clique em **aplicações em nuvem**.

    ![Aplicações na cloud](./media/active-directory-conditional-access-app-based-mfa/08.png)

9. No **aplicações em nuvem** página, execute os seguintes passos:

    ![Selecione as aplicações em nuvem](./media/active-directory-conditional-access-app-based-mfa/09.png)

    a. Clique em **selecionar aplicações**.

    b. Clique em **Selecionar**.

    c. No **selecione** página, selecione **Microsoft Azure Management**e, em seguida, clique em **selecione**.

    d. No **aplicações em nuvem** página, clique em **feito**.


10. No **controlos de acesso** secção, clique em **conceder**.

    ![Controlos de acesso](./media/active-directory-conditional-access-app-based-mfa/10.png)

11. No **conceder** página, execute os seguintes passos:

    ![Conceder](./media/active-directory-conditional-access-app-based-mfa/11.png)

    a. Selecione **conceder acesso**.

    a. Selecione **requer autenticação multifator**.

    b. Clique em **Selecionar**.

12. No **ativar política de** secção, clique em **no**.

    ![Activar política](./media/active-directory-conditional-access-app-based-mfa/18.png)

13. Clique em **Criar**.


## <a name="evaluate-your-conditional-access-policy"></a>Avaliar a política de acesso condicional

Para compreender o impacto das políticas de acesso condicional no seu ambiente, pode utilizar o [condicional aceder a que se ferramenta política](active-directory-conditional-access-whatif.md). Utilizar esta ferramenta, pode avaliar um simulada início de sessão de um utilizador.

Ao configurar a ferramenta com **Britta Simon** como utilizador e **Microsoft Azure Management** como aplicação na nuvem, a ferramenta mostra **exigir a MFA para Britta** em  **As políticas que se apliquem**.

![E se a ferramenta de política](./media/active-directory-conditional-access-app-based-mfa/17.png)



**Para avaliar a política de acesso condicional:**

1. No [acesso condicional - políticas](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) página, no menu na parte superior, clique em **e se**.  
 
    ![Hipótese](./media/active-directory-conditional-access-app-based-mfa/14.png)

2. Clique em **utilizadores**, selecione **Britta Simon**e, em seguida, clique em **selecione**.

    ![Utilizador](./media/active-directory-conditional-access-app-based-mfa/15.png)

2. Para selecionar uma aplicação de nuvem, execute os seguintes passos:

    ![Aplicações na cloud](./media/active-directory-conditional-access-app-based-mfa/16.png)

    a. Clique em **aplicações em nuvem**.

    b. No **página de aplicações em nuvem**, clique em **selecionar aplicações**.

    c. Clique em **Selecionar**.

    d. No **selecione** página, selecione o Microsoft Azure Management * * e, em seguida, clique em **selecione**.

    e. Na página de aplicações em nuvem, clique em **feito**.

3. Clique em **e se**.


## <a name="test-your-conditional-access-policy"></a>Testar a sua política de acesso condicional

Para testar a política, experimente iniciar sessão no seu [portal do Azure](https://portal.azure.com) utilizando o **Britta Simon** testar conta. Deverá ver uma caixa de diálogo que requer que defina a sua conta para verificação de segurança adicional.

![Multi-Factor Authentication](./media/active-directory-conditional-access-app-based-mfa/01.png)


## <a name="next-steps"></a>Passos Seguintes

Se quiser saber mais sobre o acesso condicional, consulte o artigo [acesso condicional do Azure Active Directory](active-directory-conditional-access-azure-portal.md).

