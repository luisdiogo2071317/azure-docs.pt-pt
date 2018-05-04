---
title: 'Tutorial: Integração do Azure Active Directory com KnowledgeOwl | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e KnowledgeOwl.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2ae30996-864d-4872-90bc-f770e1ea159a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: 62dd5b4e213c3da5038e308f300685b1dad2b64d
ms.sourcegitcommit: 4bc2aa08a116a0acca0af3b02b5c1d144d3cf9b1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/30/2018
---
# <a name="tutorial-azure-active-directory-integration-with-knowledgeowl"></a>Tutorial: Integração do Azure Active Directory com KnowledgeOwl

Neste tutorial, irá aprender a integrar KnowledgeOwl com o Azure Active Directory (Azure AD).

Integrar KnowledgeOwl com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao KnowledgeOwl.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para KnowledgeOwl (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com KnowledgeOwl, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um KnowledgeOwl-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar KnowledgeOwl a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-knowledgeowl-from-the-gallery"></a>Adicionar KnowledgeOwl a partir da Galeria
Para configurar a integração de KnowledgeOwl com o Azure AD, terá de adicionar KnowledgeOwl a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar KnowledgeOwl a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **KnowledgeOwl**, selecione **KnowledgeOwl** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![KnowledgeOwl na lista de resultados](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_knowledgeowl_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com KnowledgeOwl com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no KnowledgeOwl é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no KnowledgeOwl tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com KnowledgeOwl, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste KnowledgeOwl](#create-a-knowledgeowl-test-user)**  - para ter um homólogo de Britta Simon KnowledgeOwl que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação KnowledgeOwl.

**Para configurar o Azure AD-início de sessão único com KnowledgeOwl, execute os seguintes passos:**

1. No portal do Azure, no **KnowledgeOwl** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_knowledgeowl_samlbase.png)

3. No **KnowledgeOwl domínio e os URLs** secção, execute os seguintes passos, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Domínio KnowledgeOwl e os URLs únicos de informações de início de sessão](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_knowledgeowl_url.png)

    a. No **identificador (ID de entidade)** caixa de texto, escreva um URL a utilizar o padrão do seguinte:
    |||
    |-|-|
    | `https://app.knowledgeowl.com/sp`|
    | `https://app.knowledgeowl.com/sp/id/<unique ID>`|
    |||

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte:
    |||
    |-|-|
    | `https://subdomain.knowledgeowl.com/help/saml-login`|
    | `https://subdomain.knowledgeowl.com/docs/saml-login`|
    | `https://subdomain.knowledgeowl.com/home/saml-login`|
    | `https://privatedomain.com/help/saml-login`|
    | `https://privatedomain.com/docs/saml-login`|
    | `https://privatedomain.com/home/saml-login`|
    |||

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![Domínio KnowledgeOwl e os URLs únicos de informações de início de sessão](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_knowledgeowl_url1.png)

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte:
    |||
    |-|-|
    | `https://subdomain.knowledgeowl.com/help/saml-login`|
    | `https://subdomain.knowledgeowl.com/docs/saml-login`|
    | `https://subdomain.knowledgeowl.com/home/saml-login`|
    | `https://privatedomain.com/help/saml-login`|
    | `https://privatedomain.com/docs/saml-login`|
    | `https://privatedomain.com/home/saml-login`|
    |||
     
    > [!NOTE]
    > Estes valores não estiverem reais. Terá de atualizar estes valor real identificador, o URL de resposta e URL de início de sessão que é explicada mais tarde no tutorial.

5. A aplicação de KnowledgeOwl espera as asserções de SAML num formato específico, que necessita para adicionar mapeamentos de atributos personalizado à sua configuração de atributos token SAML. Configure as seguintes afirmações para esta aplicação. Pode gerir os valores destes atributos a partir de "**atributos de utilizador**" secção na página de integração de aplicações.

    ![Configurar o início de sessão único](./media/active-directory-saas-knowledgeowl-tutorial/attribute.png)

6. No **atributos de utilizador** secção no **de sessão único-** caixa de diálogo, configurar atributos token SAML, conforme mostrado na imagem anterior e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo | Espaço de Nomes|
    | ------------------- | -------------------- | -----|
    | ssoid | User.Mail | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.
    
    ![Configurar o início de sessão único](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_attribute_05.png)

    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d.From o **espaço de nomes** lista, introduza o valor de espaço de nomes mostrado para essa linha.
    
    e. Clique em **OK**.

7. No **certificado de assinatura de SAML** secção, clique em **certificado (bruto)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_knowledgeowl_certificate.png) 

8. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_400.png)
    
9. No **KnowledgeOwl configuração** secção, clique em **configurar KnowledgeOwl** para abrir **configurar início de sessão** janela. Copiar o **Sign-Out URL, o ID de entidade de SAML e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configuração de KnowledgeOwl](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_knowledgeowl_configure.png)

10. Numa janela do browser web diferente, inicie sessão no site da sua empresa KnowledgeOwl como administrador.

11. Clique em **definições** e, em seguida, selecione **segurança**.

    ![Configuração de KnowledgeOwl](./media/active-directory-saas-knowledgeowl-tutorial/configure1.png)

12. Desloque para baixo até o **SAML SSO integração** e execute os seguintes passos:
    
    ![Configuração de KnowledgeOwl](./media/active-directory-saas-knowledgeowl-tutorial/configure2.png)

    a. Selecione **ativar SAML SSO**.

    b. Copiar o **SP ID da entidade** valor e cole-o para o **identificador (ID de entidade)** no **KnowledgeOwl domínio e os URLs** secção no portal do Azure.

    c. Copiar o **SP URL de início de sessão** valor e cole-o para o **URL de início de sessão e o URL de resposta** caixas de texto no **KnowledgeOwl domínio e os URLs** secção no portal do Azure.

    d. No **IdP entityID** caixa de texto, cole o **ID de entidade de SAML** valor que copiou do portal do Azure.

    e. No **URL de início de sessão do IdP** caixa de texto, cole o **único início de sessão no URL do serviço SAML** valor que copiou do portal do Azure.

    f. No **URL de fim de sessão do IdP** caixa de texto, cole o **Sign-Out URL** valor que copiou do portal do Azure

    g. Carregar o formulário de certificado transferido o portal do Azure, clicando a **carregar certificado IdP**.

    h. Clique em **atributos de SAML do mapa** para mapear os atributos e execute os seguintes passos:
    
    ![Configuração de KnowledgeOwl](./media/active-directory-saas-knowledgeowl-tutorial/configure3.png)

    * Introduza `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/ssoid` para o **SSO ID** caixa de texto
    * Introduza `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` para o **nome de utilizador/E-Mail** caixa de texto.
    * Introduza `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` para o **nome próprio** caixa de texto.
    * Introduza `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` para o **Apelido** caixa de texto.
    * Clicar em **Guardar**

    i. Clique em **Guardar** na parte inferior da página.

    ![Configuração de KnowledgeOwl](./media/active-directory-saas-knowledgeowl-tutorial/configure4.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-knowledgeowl-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-knowledgeowl-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-knowledgeowl-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-knowledgeowl-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-knowledgeowl-test-user"></a>Criar um utilizador de teste KnowledgeOwl

O objetivo desta secção consiste em criar um utilizador chamado Britta Simon KnowledgeOwl. KnowledgeOwl suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não há nenhum item de ação para si nesta secção. Um novo utilizador é criado durante a tentativa de aceder KnowledgeOwl se não existir ainda.
>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de KnowledgeOwl](mailto:support@knowledgeowl.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para KnowledgeOwl.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a KnowledgeOwl, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **KnowledgeOwl**.

    ![A ligação de KnowledgeOwl na lista de aplicações](./media/active-directory-saas-knowledgeowl-tutorial/tutorial_knowledgeowl_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico KnowledgeOwl no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de KnowledgeOwl.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-knowledgeowl-tutorial/tutorial_general_203.png

