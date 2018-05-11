---
title: Guia de introdução - exigir autenticação multifator (MFA) para aplicações específicas com acesso condicional do Azure Active Directory | Microsoft Docs
description: Este guia de introdução, saiba como pode juntar os requisitos de autenticação para o tipo de aplicação de nuvem acedidos através de acesso condicional do Azure Active Directory (Azure AD).
services: active-directory
keywords: acesso condicional para aplicações, o acesso condicional com o Azure AD, o acesso seguro aos recursos da empresa, as políticas de acesso condicional
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/10/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: b451ede984d3baa8331ec87575557f845686c01f
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="quickstart-require-mfa-for-specific-apps-with-azure-active-directory-conditional-access"></a>Início rápido: Exigir a MFA para aplicações específicas com acesso condicional do Azure Active Directory 

Para simplificar a experiência de início de sessão dos utilizadores, pode querer permitir-lhes autorização iniciar sessão suas aplicações em nuvem utilizando um nome de utilizador e uma palavra-passe. No entanto, muitos ambientes tem, pelo menos, algumas aplicações para o qual é recomendado para exigir um formulário de verificação de conta, tais como a autenticação multifator mais forte. Isto pode ser, para VERDADEIRO de exemplo, para acesso ao sistema de e-mail da sua organização ou as aplicações de RH. No Azure Active Directory, pode realizar este objetivo com uma política de acesso condicional.    

Este guia de introdução mostra como configurar um [política de acesso condicional do Azure AD](active-directory-conditional-access-azure-portal.md) para exigir a autenticação multifator para um conjunto de aplicações em nuvem selecionado no seu ambiente.


## <a name="scenario-description"></a>Descrição do cenário

O cenário neste artigo utiliza o portal do Azure como marcador de posição para uma aplicação de nuvem que requer autenticação multifator para um utilizador específico. Isabella Simonsen é um utilizador na sua organização. Quando iniciar sessão para o portal do Azure, pretende-lhe para verificar ainda mais a sua conta multi-factor Authentication.

![Multi-Factor Authentication](./media/active-directory-conditional-access-app-based-mfa/22.png)



## <a name="prerequisites"></a>Pré-requisitos 

Para concluir o cenário deste guia de introdução, precisa de:

- **Acesso a uma edição do Azure AD Premium** -acesso condicional do Azure AD é uma capacidade do Azure AD Premium. Se não tiver acesso para uma edição do Azure AD Premium, pode [inscrição para uma versão de avaliação](https://azure.microsoft.com/trial/get-started-active-directory/).

- **Uma conta de teste chamado Isabella Simonsen** - se não souber como criar uma conta de teste, leia [estas instruções](https://docs.microsoft.com/azure/active-directory/add-users-azure-active-directory).



## <a name="create-your-conditional-access-policy"></a>Criar a sua política de acesso condicional 

Esta secção mostra como criar a política de acesso condicional necessária.  
Na sua política, definir:

|Definição |Valor|
|---     | --- |
|Utilizadores e grupos | Isabella Simonsen |
|Aplicações na cloud | Gestão do Microsoft Azure |
|Conceder | Exigir autenticação multifator |
 

![Criar política](./media/active-directory-conditional-access-app-based-mfa/21.png)




**Para configurar a política de acesso condicional:**

1. Inicie sessão no seu [portal do Azure](https://portal.azure.com) como administrador global.

2. No portal do Azure, no navbar esquerdo, clique em **do Azure Active Directory**. 

    ![Azure Active Directory](./media/active-directory-conditional-access-app-based-mfa/02.png)

3. No **do Azure Active Directory** na página de **gerir** secção, clique em **acesso condicional**.

    ![Acesso condicional](./media/active-directory-conditional-access-app-based-mfa/03.png)
 
4. No **acesso condicional** página, na barra de ferramentas na parte superior, clique em **adicionar**.

    ![Adicionar](./media/active-directory-conditional-access-app-based-mfa/04.png)

5. No **novo** na página de **nome** caixa de texto, tipo **exigir a MFA para acesso ao portal do Azure**.

    ![Nome](./media/active-directory-conditional-access-app-based-mfa/05.png)

6. No **atribuição** secção, clique em **utilizadores e grupos**.

    ![Utilizadores e grupos](./media/active-directory-conditional-access-app-based-mfa/06.png)

7. No **utilizadores e grupos** página, execute os seguintes passos:

    ![Utilizadores e grupos](./media/active-directory-conditional-access-app-based-mfa/24.png)

    a. Clique em **selecionar utilizadores e grupos**e, em seguida, selecione **utilizadores e grupos**.

    b. Clique em **Selecionar**.

    c. No **selecione** página, selecione **Isabella Simonsen**e, em seguida, clique em **selecione**.

    d. No **utilizadores e grupos** página, clique em **feito**.

8. Clique em **aplicações em nuvem**.

    ![Aplicações na cloud](./media/active-directory-conditional-access-app-based-mfa/08.png)

9. No **aplicações em nuvem** página, execute os seguintes passos:

    ![Selecione as aplicações em nuvem](./media/active-directory-conditional-access-app-based-mfa/26.png)

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

Agora que configurou a política de acesso condicional, provavelmente pretende saber se funciona conforme esperado. Como primeiro passo, pode utilizar o [condicional aceder a que se ferramenta política](active-directory-conditional-access-whatif.md) para simular um início de sessão de um utilizador. Ao configurar a ferramenta com **Isabella Simonsen** como utilizador e **Microsoft Azure Management** como aplicação na nuvem, a ferramenta mostra **exigir a MFA para acesso ao portal do Azure** em **Políticas que se apliquem** e **requer autenticação multifator** como **conceder controlos**.

![E se a ferramenta de política](./media/active-directory-conditional-access-app-based-mfa/23.png)



**Para avaliar a política de acesso condicional:**

1. No [acesso condicional - políticas](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) página, no menu na parte superior, clique em **e se**.  
 
    ![What If](./media/active-directory-conditional-access-app-based-mfa/14.png)

2. Clique em **utilizadores**, selecione **Isabella Simonsen**e, em seguida, clique em **selecione**.

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

A secção anterior, na secção anterior, 

Para testar a política, experimente iniciar sessão no seu [portal do Azure](https://portal.azure.com) utilizando o **Isabella Simonsen** testar conta. Deverá ver uma caixa de diálogo que requer que defina a sua conta para verificação de segurança adicional.

![Multi-Factor Authentication](./media/active-directory-conditional-access-app-based-mfa/22.png)


## <a name="next-steps"></a>Passos Seguintes

Se quiser saber mais sobre o acesso condicional, consulte o artigo [acesso condicional do Azure Active Directory](active-directory-conditional-access-azure-portal.md).

