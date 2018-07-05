---
title: Início rápido - necessitam de termos de utilização para ser aceite antes de aceder a aplicações na cloud que estão protegidas pelo acesso condicional do Azure Active Directory | Documentos da Microsoft
description: Este guia de introdução, saiba como pode exigir que os seus termos de utilização são aceites antes do acesso a aplicações na cloud selecionada é concedido pelo acesso condicional do Azure Active Directory.
services: active-directory
keywords: acesso condicional para aplicações, o acesso condicional com o Azure AD, o acesso seguro aos recursos da empresa, políticas de acesso condicional, termos de utilização
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
ms.date: 06/13/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: d5fad320c49691626c820429b07b6864137760b9
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37450034"
---
# <a name="quickstart-require-terms-of-use-to-be-accepted-before-accessing-cloud-apps"></a>Início rápido: Exigir termos de utilização para ser aceite antes de aceder a aplicações na cloud 

Antes de aceder a determinadas aplicações na cloud no seu ambiente, pode querer obter consentimento de utilizadores na forma de aceitar os termos de utilização (ToU). Acesso condicional do Azure Active Directory (Azure AD) fornece-lhe: 

- Um método simples para configurar os termos de utilização
- A opção para exigir a aceitar os termos de utilização por meio de uma política de acesso condicional  

Este início rápido mostra como configurar uma [política de acesso condicional do Azure AD](active-directory-conditional-access-azure-portal.md) que requer um termos de utilização sejam aceites para uma aplicação em nuvem selecionado no seu ambiente.

![Criar política](./media/active-directory-conditional-access-tou/5555.png)


Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.



## <a name="prerequisites"></a>Pré-requisitos 

Para concluir o cenário neste início rápido, precisa de:

- **Acesso a uma edição do Azure AD Premium** -acesso condicional do Azure AD é um recurso do Azure AD Premium. 

- **Uma conta de teste chamado Isabella Simonsen** – se não saiba como criar uma conta de teste, consulte [adicionar utilizadores com base na cloud](fundamentals/add-users-azure-active-directory.md#add-cloud-based-users).


## <a name="test-your-sign-in"></a>Testar o início de sessão

O objetivo deste passo é obter uma impressão da experiência de início de sessão sem uma política de acesso condicional.

**Para testar o início de sessão:**

1. Inicie sessão no seu [portal do Azure](https://portal.azure.com/) como Isabella Simonsen.

2. Termine a sessão.




## <a name="create-your-terms-of-use"></a>Criar os termos de utilização

Esta secção fornece os passos para criar um exemplo de termos de utilização. Quando cria um termos de utilização, selecione um valor para **impor com modelos de política de acesso condicional**. Selecionando **política personalizada do** abre a caixa de diálogo para criar uma nova política de acesso condicional, assim que tem sido criado os termos de utilização.


**Para criar os termos de utilização:**

1. No Microsoft Word, crie um novo documento.

2. Tipo **meu termos de utilização**e, em seguida, guarde o documento no seu computador como **mytou.pdf**.

3. Inicie sessão no seu [portal do Azure](https://portal.azure.com) como administrador global, administrador de segurança ou um administrador de acesso condicional.

4. No portal do Azure, na barra de navegação esquerda, clique em **do Azure Active Directory**. 

    ![Azure Active Directory](./media/active-directory-conditional-access-tou/02.png)

5. Na **do Azure Active Directory** página, além do **gerir** secção, clique em **acesso condicional**.

    ![Acesso condicional](./media/active-directory-conditional-access-tou/03.png) 

6. Na **Manage** secção, clique em **termos de utilização**.

    ![Termos de utilização](./media/active-directory-conditional-access-tou/04.png) 

7. No menu na parte superior, clique em **novos termos**.

    ![Termos de utilização](./media/active-directory-conditional-access-tou/05.png) 

8. Sobre o **novos termos de utilização** página:

    ![Termos de utilização](./media/active-directory-conditional-access-tou/112.png) 

    a. Na **nome** caixa de texto, tipo **My TOU**.

    b. Na **nome a apresentar** caixa de texto, tipo **My TOU**.

    c. Carregue os seus termos de utilize um ficheiro PDF.

    d. Como **linguagem**, selecione **inglês**.

    e. Como **exigir que os utilizadores expandam os termos de utilização**, selecione **no**.

    f. Como **impor com modelos de política de acesso condicional**, selecione **política personalizada do**.

    g. Clique em **Criar**.
 


## <a name="create-your-conditional-access-policy"></a>Criar a sua política de acesso condicional 

Esta secção mostra como criar a política de acesso condicional necessário. O cenário neste início rápido utiliza:

- Portal do Azure como marcador de posição para uma aplicação de cloud que requer o termos de utilização a ser aceites. 
- O utilizador de exemplo para testar a política de acesso condicional.  

Na sua política, defina:

|Definição |Valor|
|---     | --- |
|Utilizadores e grupos | Isabella Simonsen |
|Aplicações na cloud | Gestão do Microsoft Azure |
|Conceder acesso | Meu termos de utilização |
 

![Criar política](./media/active-directory-conditional-access-tou/1234.png)

 


**Para configurar a política de acesso condicional:**

1. No **New** na página a **nome** caixa de texto, tipo **necessitam de termos de utilização para Isabella**.

    ![Nome](./media/active-directory-conditional-access-tou/71.png)

2. Na **atribuição** secção, clique em **utilizadores e grupos**.

    ![Utilizadores e grupos](./media/active-directory-conditional-access-tou/06.png)

3. Sobre o **utilizadores e grupos** página:

    ![Utilizadores e grupos](./media/active-directory-conditional-access-tou/24.png)

    a. Clique em **selecionar utilizadores e grupos**e, em seguida, selecione **utilizadores e grupos**.

    b. Clique em **Selecionar**.

    c. Sobre o **selecionar** , selecione **Isabella Simonsen**e, em seguida, clique em **selecionar**.

    d. Sobre o **utilizadores e grupos** página, clique em **feito**.

4. Clique em **aplicações na Cloud**.

    ![Aplicações na cloud](./media/active-directory-conditional-access-tou/08.png)

5. Sobre o **aplicações na Cloud** página:

    ![Selecione aplicações na cloud](./media/active-directory-conditional-access-tou/26.png)

    a. Clique em **selecionar aplicações**.

    b. Clique em **Selecionar**.

    c. Sobre o **selecionar** , selecione **Microsoft Azure Management**e, em seguida, clique em **selecionar**.

    d. Sobre o **aplicações na Cloud** página, clique em **feito**.


6. Na **controlos de acesso** secção, clique em **concessão**.

    ![Controlos de acesso](./media/active-directory-conditional-access-tou/10.png)

7. Sobre o **concessão** página:

    ![Conceder](./media/active-directory-conditional-access-tou/111.png)

    a. Selecione **conceder acesso**.

    a. Selecione **meu TOU**.

    b. Clique em **Selecionar**.

8. Na **ativar política** secção, clique em **no**.

    ![Activar política](./media/active-directory-conditional-access-tou/18.png)

9. Clique em **Criar**.


## <a name="evaluate-a-simulated-sign-in"></a>Avaliar um simulado início de sessão

Agora que configurou a política de acesso condicional, provavelmente quer saber se ele funciona conforme esperado. Como primeiro passo, utilize o acesso condicional e se a política de ferramenta para simular um início de sessão do seu utilizador de teste. A simulação estima o impacto neste início de sessão tem em suas diretivas e gera um relatório de simulação.  

Para inicializar o que se o conjunto de ferramentas de avaliação da política,:

- **Isabella Simonsen** como utilizador 
- **Microsoft Azure Management** como aplicação de cloud


Clicar **e se** cria um relatório de simulação, que mostra:

- **Exigir a termos de utilização para Isabella** em **políticas que serão aplicadas** 
- **Meu TOU** como **conceder controlos**.

![E se a ferramenta de política](./media/active-directory-conditional-access-tou/79.png)



**Para avaliar a sua política de acesso condicional:**

1. Sobre o [acesso condicional - políticas](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) página, no menu na parte superior, clique em **e se**.  
 
    ![What If](./media/active-directory-conditional-access-tou/14.png)

2. Clique em **usuários**, selecione **Isabella Simonsen**e, em seguida, clique em **selecionar**.

    ![Utilizador](./media/active-directory-conditional-access-tou/15.png)

2. Para selecionar uma aplicação na cloud:

    ![Aplicações na cloud](./media/active-directory-conditional-access-tou/16.png)

    a. Clique em **aplicações na Cloud**.

    b. Sobre o **página de aplicações na Cloud**, clique em **selecionar aplicações**.

    c. Clique em **Selecionar**.

    d. Sobre o **selecionar** , selecione **Microsoft Azure Management**e, em seguida, clique em **selecionar**.

    e. Na página de aplicações na cloud, clique em **feito**.

3. Clique em **e se**.


## <a name="test-your-conditional-access-policy"></a>Testar a sua política de acesso condicional

Na secção anterior, aprendeu como avaliar um simulado início de sessão. Além de uma simulação, também deve testar sua política de acesso condicional para garantir que funciona conforme esperado. 

Para testar a sua política, tente iniciar sessão no seu [portal do Azure](https://portal.azure.com) usando sua **Isabella Simonsen** conta de teste. Deverá ver uma caixa de diálogo que requer que aceite os termos de utilização.

![Termos de utilização](./media/active-directory-conditional-access-tou/57.png)


## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o utilizador de teste e a política de acesso condicional:

- Se não souber como eliminar um utilizador do Azure AD, veja [eliminar utilizadores do Azure AD](fundamentals/add-users-azure-active-directory.md#delete-users-from-azure-ad).

- Para eliminar a política, selecione a política e, em seguida, clique em **eliminar** na barra de ferramentas de acesso rápido.

    ![Multi-Factor Authentication](./media/active-directory-conditional-access-tou/33.png)

- Para eliminar os termos de utilização, selecioná-lo e, em seguida, clique em **eliminar termos** na barra de ferramentas na parte superior. 

    ![Multi-Factor Authentication](./media/active-directory-conditional-access-tou/29.png)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Exigir a MFA para aplicações específicas](./active-directory-conditional-access-app-based-mfa.md)
> [bloquear o acesso quando é detetado um risco de sessão](./active-directory-conditional-access-app-sign-in-risk.md)

