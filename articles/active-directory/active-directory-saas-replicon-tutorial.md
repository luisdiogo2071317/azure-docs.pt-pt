---
title: "Tutorial: Integração do Azure Active Directory com Replicon | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e Replicon."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: jeedes
ms.openlocfilehash: 130e13b0bb801c498f6de6e4bfd61bd3d2c3bf00
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2017
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a>Tutorial: Integração do Azure Active Directory com Replicon

Neste tutorial, irá aprender a integrar Replicon com o Azure Active Directory (Azure AD).

Integrar Replicon com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Replicon.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Replicon (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Replicon, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Replicon-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Replicon a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-replicon-from-the-gallery"></a>Adicionar Replicon a partir da Galeria
Para configurar a integração de Replicon com o Azure AD, terá de adicionar Replicon a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Replicon a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Replicon**, selecione **Replicon** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Replicon na lista de resultados](./media/active-directory-saas-replicon-tutorial/tutorial_replicon_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Replicon com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Replicon é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Replicon tem de ser estabelecida.

No Replicon, atribua o valor do **nome de utilizador** no Azure AD como o valor a **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD-início de sessão único com Replicon, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Replicon](#create-a-replicon-test-user)**  - para ter um homólogo de Britta Simon Replicon que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Replicon.

**Para configurar o Azure AD-início de sessão único com Replicon, execute os seguintes passos:**

1. No portal do Azure, no **Replicon** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-replicon-tutorial/tutorial_replicon_samlbase.png)

3. No **Replicon domínio e os URLs** secção, execute os seguintes passos:

    ![Domínio replicon e os URLs únicos de informações de início de sessão](./media/active-directory-saas-replicon-tutorial/tutorial_replicon_url.png)

    a. No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://na2.replicon.com/<companyname>/saml2/sp-sso/post`

    b. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://global.replicon.com/<companyname>`

    c. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://global.replicon.com/!/saml2/<companyname>/sso/post`

    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o URL de início de sessão, identificador e o URL de resposta real. Contacte [equipa de suporte de cliente Replicon](https://www.replicon.com/customerzone/contact-support) para obter estes valores. 

4. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-replicon-tutorial/tutorial_replicon_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-replicon-tutorial/tutorial_general_400.png)

6. Numa janela do browser web diferente, inicie sessão no site da sua empresa Replicon como administrador.

7. Para configurar SAML 2.0, execute os seguintes passos:
   
    ![Ativar a autenticação SAML](./media/active-directory-saas-replicon-tutorial/ic777805.png "autenticação ativar SAML")
    
    a. Para apresentar o **EnableSAML Authentication2** caixa de diálogo, acrescentar o seguinte para o URL, após a sua chave de empresa:`/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`
    
    * O seguinte mostra o esquema do URL concluído:  
   `https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`
   
   b. Clique em de  **+**  para expandir o **v20Configuration** secção.

   c. Clique em de  **+**  para expandir o **metaDataConfiguration** secção.

   d. Clique em **Escolher ficheiro**, para selecionar o ficheiro XML de metadados de fornecedor de identidade e clique em **submeter**.

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-replicon-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-replicon-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-replicon-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-replicon-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-replicon-test-user"></a>Criar um utilizador de teste Replicon

Para permitir que os utilizadores do Azure AD sessão Replicon, têm de ser aprovisionados para Replicon. No caso de Replicon, o aprovisionamento é uma tarefa manual.

**Para configurar o aprovisionamento de utilizadores, execute os seguintes passos:**

1. Numa janela do browser web, inicie sessão no site da sua empresa Replicon como administrador.

2. Aceda a **administração \> utilizadores**.
   
    ![Os utilizadores](./media/active-directory-saas-replicon-tutorial/ic777806.png "utilizadores")

3. Clique em **+ adicionar utilizador**.
   
    ![Adicionar utilizador](./media/active-directory-saas-replicon-tutorial/ic777807.png "adicionar utilizador")

4. No **perfil de utilizador** secção, execute os seguintes passos:
   
    ![Perfil de utilizador](./media/active-directory-saas-replicon-tutorial/ic777808.png "perfil de utilizador")
    
    a. No **nome de início de sessão** caixa de texto, o Azure AD de tipo endereço de correio eletrónico do utilizador do Azure AD que pretende aprovisionar como  **BrittaSimon@contoso.com** .
    
    b. Como **tipo de autenticação**, selecione **SSO**.
    
    c. No **departamento** caixa de texto, escreva o departamento do utilizador.

    d. Como **empregado tipo**, selecione **administrador**.

    e. Clique em **guardar o perfil de utilizador**.

>[!NOTE]
>Pode utilizar quaisquer outras Replicon utilizador conta criação ferramentas ou APIs fornecidas pelo Replicon aprovisionar contas de utilizador do Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Replicon.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a Replicon, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Replicon**.

    ![A ligação de Replicon na lista de aplicações](./media/active-directory-saas-replicon-tutorial/tutorial_replicon_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Replicon no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de Replicon.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-replicon-tutorial/tutorial_general_203.png

