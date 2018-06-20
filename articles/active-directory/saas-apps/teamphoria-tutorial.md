---
title: 'Tutorial: Integração do Azure Active Directory com Teamphoria | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Teamphoria.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2018
ms.author: jeedes
ms.openlocfilehash: 9a6b14be6b41aacf0cc2ccbc4cf1c8421bc8aa98
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231166"
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a>Tutorial: Integração do Azure Active Directory com Teamphoria

Neste tutorial, irá aprender a integrar Teamphoria com o Azure Active Directory (Azure AD).

Integrar Teamphoria com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Teamphoria
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Teamphoria (Single Sign-On) com as respetivas contas do Azure AD
- Pode gerir as contas numa localização central - portal do Azure

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Teamphoria, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Teamphoria início de sessão único subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Teamphoria a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-teamphoria-from-the-gallery"></a>Adicionar Teamphoria a partir da Galeria
Para configurar a integração de Teamphoria com o Azure AD, terá de adicionar Teamphoria a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Teamphoria a partir da galeria, execute os seguintes passos:**

1. No  **[Portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone.

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![Aplicações][2]
    
3. Clique em **adicionar** botão na parte superior da caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Teamphoria**.

    ![Criar um utilizador de teste do Azure AD](./media/teamphoria-tutorial/tutorial_teamphoria_search.png)

5. No painel de resultados, selecione **Teamphoria**e, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/teamphoria-tutorial/tutorial_teamphoria_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD de sessão único-
Nesta secção, configure e teste do Azure AD-início de sessão único com Teamphoria com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Teamphoria é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Teamphoria tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Teamphoria, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Teamphoria](#creating-a-teamphoria-test-user)**  - para ter um homólogo de Britta Simon Teamphoria que está ligada a representação do Azure AD da forma.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Teamphoria.

**Para configurar o Azure AD-início de sessão único com Teamphoria, execute os seguintes passos:**

1. No portal do Azure, no **Teamphoria** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar o início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

3. No **Teamphoria domínio e os URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/tutorial_teamphoria_url.png)

    No **URL de início de sessão** caixa de texto, escreva o URL a utilizar o padrão do seguinte: `https://<sub-domain>.teamphoria.com/login`   

    > [!NOTE] 
    > O valor do URL de início de sessão não é real. Tem de atualizar este valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente Teamphoria](https://www.teamphoria.com/) para obter o URL de início de sessão.

4. No **certificado de assinatura de SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o certificado no seu computador.

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/tutorial_teamphoria_certificate.png)

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/tutorial_general_400.png)

6. No **Teamphoria configuração** secção, clique em **configurar Teamphoria** para abrir **configurar início de sessão** janela. Copiar o **único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/tutorial_teamphoria_configure.png)

7. Para configurar o início de sessão único em **Teamphoria** lado, início de sessão para a aplicação de Teamphoria como administrador.

8. Aceda a **definições de administrador** opção na barra de ferramentas esquerda e, no separador configurar, clique em **ON de início de sessão único** para abrir a janela de configuração do SSO.

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/admin_sso_configure.png)

9. Clique em **adicionar novo fornecedor de identidade** opção no canto superior direito para abrir o formulário para adicionar as definições para SSO.

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/add_new_identity_provider.png)

10. Introduza os detalhes nos campos, conforme descrito abaixo-

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/Teamphoria_sso_save.png)

    a. **NOME a apresentar**: introduza o nome a apresentar o plug-in na página Administração.

    b. **NOME do botão**: O nome do separador que será apresentado na página de início de sessão para iniciar sessão através do SSO.

    c. **CERTIFICADO**: Abra o certificado anterior transferido a partir do portal do Azure no bloco de notas, copie o conteúdo do mesmo e cole-o aqui na caixa.

    d. **PONTO de entrada**: colar o **único início de sessão no URL do serviço SAML** copiou anteriormente do portal do Azure.

    e. Mudar a opção de **ON** e clique em **guardar**.

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No **portal do Azure**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/teamphoria-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    ![Criar um utilizador de teste do Azure AD](./media/teamphoria-tutorial/create_aaduser_02.png) 

3. Na parte superior da caixa de diálogo, clique em **adicionar** para abrir o **utilizador** caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/teamphoria-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/teamphoria-tutorial/create_aaduser_04.png) 

    a. No **nome** caixa de texto, tipo **BrittaSimon**.

    b. No **nome de utilizador** caixa de texto, tipo de **endereço de correio eletrónico** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e anote o valor da **palavra-passe**.

    d. Clique em **Criar**.

### <a name="creating-a-teamphoria-test-user"></a>Criar um utilizador de teste Teamphoria

Para permitir que os utilizadores do Azure AD sessão Teamphoria, têm de ser aprovisionados para Teamphoria. No caso de Teamphoria, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa Teamphoria como administrador.

2. Clique em **ADMIN** definições na barra de ferramentas esquerda e, no **GERIR** separador clique no **utilizadores** para abrir a página de administração para os utilizadores.

    ![Adicionar empregado](./media/teamphoria-tutorial/admin_manage_users.png)

3. Clique em de **MANUAL CONVIDAR** opção.

    ![Convidar pessoas](./media/teamphoria-tutorial/admin_manage_add_users.png)

4. Nesta página, execute os seguintes ação.
    
    ![Convidar pessoas](./media/teamphoria-tutorial/manual_user_invite.png)

    a. No **endereço de correio ELETRÓNICO** caixa de texto, o **endereço de correio eletrónico** de BrittaSimon.

    b. No **nome próprio** caixa de texto, tipo **Britta**.

    c. No **Apelido** caixa de texto, tipo **Simon**.

    d. Clique em **convite 1 utilizador**. Utilizador tem de aceitar o convite a ser criados no sistema.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Teamphoria.

![Atribua o utilizador][200]

**Para atribuir Britta Simon a Teamphoria, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201]

2. Na lista de aplicações, selecione **Teamphoria**.

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/tutorial_teamphoria_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribua o utilizador][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![Atribua o utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.

### <a name="testing-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Se pretender testar as definições de início de sessão único, abra o painel de acesso. Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/teamphoria-tutorial/tutorial_general_01.png
[2]: ./media/teamphoria-tutorial/tutorial_general_02.png
[3]: ./media/teamphoria-tutorial/tutorial_general_03.png
[4]: ./media/teamphoria-tutorial/tutorial_general_04.png

[100]: ./media/teamphoria-tutorial/tutorial_general_100.png

[200]: ./media/teamphoria-tutorial/tutorial_general_200.png
[201]: ./media/teamphoria-tutorial/tutorial_general_201.png
[202]: ./media/teamphoria-tutorial/tutorial_general_202.png
[203]: ./media/teamphoria-tutorial/tutorial_general_203.png
