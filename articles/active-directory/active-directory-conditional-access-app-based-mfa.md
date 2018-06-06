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
ms.component: protection
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/11/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 1501ca1c036a8db1d53b9b27170d9ae05d41f797
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34724120"
---
# <a name="quickstart-require-mfa-for-specific-apps-with-azure-active-directory-conditional-access"></a>Início rápido: Exigir a MFA para aplicações específicas com acesso condicional do Azure Active Directory 

Para simplificar a experiência de início de sessão dos utilizadores, pode querer permitir-lhes autorização iniciar sessão suas aplicações em nuvem utilizando um nome de utilizador e uma palavra-passe. No entanto, muitos ambientes tem, pelo menos, algumas aplicações para o qual é recomendado para exigir um formulário de verificação de conta, tais como a autenticação multifator (MFA) mais forte. Isto pode ser, para VERDADEIRO de exemplo, para acesso ao sistema de e-mail da sua organização ou as aplicações de RH. No Azure Active Directory (Azure AD), pode realizar este objetivo com uma política de acesso condicional.    

Este guia de introdução mostra como configurar um [política de acesso condicional do Azure AD](active-directory-conditional-access-azure-portal.md) que requer autenticação multifator para uma aplicação de nuvem selecionada no seu ambiente.

![Criar política](./media/active-directory-conditional-access-app-based-mfa/32.png)


Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.



## <a name="prerequisites"></a>Pré-requisitos 

Para concluir o cenário deste guia de introdução, precisa de:

- **Acesso a uma edição do Azure AD Premium** -acesso condicional do Azure AD é uma capacidade do Azure AD Premium. 

- **Uma conta de teste chamado Isabella Simonsen** - se não souber como criar uma conta de teste, consulte [adicionar utilizadores baseados na nuvem](add-users-azure-active-directory.md#add-cloud-based-users).



## <a name="create-your-conditional-access-policy"></a>Criar a sua política de acesso condicional 

Esta secção mostra como criar a política de acesso condicional necessária. O cenário deste guia de introdução utiliza:

- O portal do Azure como marcador de posição para uma aplicação de nuvem que requeira o MFA. 
- O utilizador de exemplo para testar a política de acesso condicional.  

Na sua política, defina:

|Definição |Valor|
|---     | --- |
|Utilizadores e grupos | Isabella Simonsen |
|Aplicações na cloud | Gestão do Microsoft Azure |
|Conceder acesso | Exigir autenticação multifator |
 

![Criar política](./media/active-directory-conditional-access-app-based-mfa/31.png)

 


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


## <a name="evaluate-a-simulated-sign-in"></a>Avaliar um simulada início de sessão

Agora que configurou a política de acesso condicional, provavelmente pretende saber se funciona conforme esperado. Como primeiro passo, utilize o acesso condicional e se a política de ferramenta para simular um início de sessão de utilizador de teste. A simulação calcula o impacto neste início de sessão tem as suas políticas e gera um relatório de simulação.  

Para inicializar o que o se a ferramenta de avaliação de política, definida:

- **Isabella Simonsen** como utilizador 
- **Microsoft Azure Management** como aplicação na nuvem

 Ao clicar em **e se** cria um relatório de simulação mostra:

- **Exigir a MFA para acesso ao portal do Azure** em **políticas que serão aplicada** 
- **Exigir a autenticação multifator** como **conceder controlos**.

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

Na secção anterior, aprendeu a avaliar um simulada início de sessão. Para além de uma simulação, também deverá testar a política de acesso condicional para garantir que funciona conforme esperado. 

Para testar a política, experimente iniciar sessão no seu [portal do Azure](https://portal.azure.com) utilizando o **Isabella Simonsen** testar conta. Deverá ver uma caixa de diálogo que requer que defina a sua conta para verificação de segurança adicional.

![Multi-Factor Authentication](./media/active-directory-conditional-access-app-based-mfa/22.png)


## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, elimine o utilizador de teste e a política de acesso condicional:

- Se não souber como eliminar um utilizador do Azure AD, consulte [eliminar utilizadores do Azure AD](add-users-azure-active-directory.md#delete-users-from-azure-ad).

- Para eliminar a política, selecione a política e, em seguida, clique em **eliminar** na barra de ferramentas de acesso rápido.

    ![Multi-Factor Authentication](./media/active-directory-conditional-access-app-based-mfa/33.png)


## <a name="next-steps"></a>Passos Seguintes

Se quiser saber mais sobre o acesso condicional, consulte o artigo [acesso condicional do Azure Active Directory](active-directory-conditional-access-azure-portal.md).

