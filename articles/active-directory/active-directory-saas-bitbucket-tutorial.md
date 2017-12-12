---
title: "Tutorial: Azure Active Directory a integração com SAML SSO para Bitbucket pela resolução GmbH | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e SAML SSO para Bitbucket pela resolução GmbH."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: fc947df1-f24e-43ae-9a34-518293583d69
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: jeedes
ms.openlocfilehash: 52c741c66a796e53698a690c415cc60c814f74e8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-bitbucket-by-resolution-gmbh"></a>Tutorial: Azure Active Directory a integração com SAML SSO para Bitbucket pela resolução GmbH

Neste tutorial, irá aprender a integrar SAML SSO para Bitbucket pela resolução GmbH com o Azure Active Directory (Azure AD).

Integrar SAML SSO para Bitbucket pela resolução GmbH com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao SAML SSO para Bitbucket pela resolução GmbH.
- Pode permitir que os utilizadores automaticamente obter assinado no SAML SSO para Bitbucket pela resolução GmbH (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o SAML SSO para Bitbucket resolução GmbH, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um SAML SSO para Bitbucket pela resolução GmbH início de sessão único subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar SAML SSO para Bitbucket resolução GmbH na galeria do
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-saml-sso-for-bitbucket-by-resolution-gmbh-from-the-gallery"></a>Adicionar SAML SSO para Bitbucket resolução GmbH na galeria do
Para configurar a integração de SAML SSO para Bitbucket pela resolução GmbH com o Azure AD, tem de adicionar SAML SSO para Bitbucket pela resolução GmbH na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar SAML SSO para Bitbucket pela resolução GmbH na galeria do, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **SAML SSO para Bitbucket pela resolução GmbH**, selecione **SAML SSO para Bitbucket pela resolução GmbH** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar o aplicação.

    ![SAML SSO para Bitbucket pela resolução GmbH na lista de resultados](./media/active-directory-saas-bitbucket-tutorial/tutorial_bitbucket_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com o SAML SSO para Bitbucket pela resolução que GmbH com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber que homólogo utilizador no SAML SSO para Bitbucket pela resolução GmbH for para um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no SAML SSO para Bitbucket por resolução GmbH tem de ser estabelecida.

No SAML SSO para Bitbucket pela resolução GmbH, atribua o valor do **nome de utilizador** no Azure AD como o valor de **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD-início de sessão único com o SAML SSO para Bitbucket pela resolução GmbH, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um SAML SSO para Bitbucket pelo utilizador de teste de GmbH resolução](#create-a-saml-sso-for-bitbucket-by-resolution-gmbh-test-user)**  - para ter um homólogo de Britta Simon no SAML SSO para Bitbucket pela resolução GmbH que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, ativar o Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua SAML SSO para Bitbucket resolução GmbH aplicação.

**Para configurar do Azure AD-início de sessão único com o SAML SSO para Bitbucket resolução GmbH, execute os seguintes passos:**

1. No portal do Azure, no **SAML SSO para Bitbucket pela resolução GmbH** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-bitbucket-tutorial/tutorial_bitbucket_samlbase.png)

3. No **SAML SSO para URLs e Bitbucket pela resolução GmbH domínio** secção, execute os seguintes passos, se pretender configurar a aplicação no modo de IDP iniciado:

    ![URLs de SAML SSO para Bitbucket pela resolução GmbH domínio e única informações de início de sessão](./media/active-directory-saas-bitbucket-tutorial/tutorial_bitbucket_url.png)

    a. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://<server-base-url>/plugins/servlet/samlsso`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://<server-base-url>/plugins/servlet/samlsso`

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![URLs de SAML SSO para Bitbucket pela resolução GmbH domínio e única informações de início de sessão](./media/active-directory-saas-bitbucket-tutorial/tutorial_bitbucket_url1.png)

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [SAML SSO para Bitbucket pela resolução GmbH cliente suportar equipa](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bitbucket/server/support) para obter estes valores. 

5. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-bitbucket-tutorial/tutorial_bitbucket_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-bitbucket-tutorial/tutorial_general_400.png)
    
7. Início de sessão no seu SAML SSO para Bitbucket pelo site de empresa resolução GmbH como administrador.

8. No lado direito da barra de ferramentas principal, clique em **definições**.

9. Aceda à secção de contas, clique em **SAML SingleSignOn** no Menubar.

    ![O Samlsingle](./media/active-directory-saas-bitbucket-tutorial/tutorial_bitbucket_samlsingle.png)

10. No **página de configuração de plug-in SAML SIngleSignOn**, clique em **adicionar idp**. 

    ![A adicionar idp](./media/active-directory-saas-bitbucket-tutorial/tutorial_bitbucket_addidp.png)

11. No **escolha o fornecedor de identidade** página, execute os seguintes passos:

    ![O fornecedor de identidade](./media/active-directory-saas-bitbucket-tutorial/tutorial_bitbucket_identityprovider.png)

    a. Selecione **Idp tipo** como **do AZURE AD**.

    b. No **nome** caixa de texto, escreva o nome.

    c. No **Descrição** caixa de texto, digite a descrição.

    d. Clique em **Seguinte**.

12. No **página de configuração do fornecedor de identidade**, clique em **seguinte**.

    ![A configuração de identidade](./media/active-directory-saas-bitbucket-tutorial/tutorial_bitbucket_identityconfig.png)

13.  No **importação SAML Idp metadados** página, clique em **carga ficheiro** para carregar o **XML de METADADOS** ficheiro que transferiu a partir do portal do Azure.

    ![O idpmetadata](./media/active-directory-saas-bitbucket-tutorial/tutorial_bitbucket_idpmetadata.png)
    
14. Clique em **Seguinte**.

15. Clique em **guardar definições**.

    ![A guardar](./media/active-directory-saas-bitbucket-tutorial/tutorial_bitbucket_save.png)

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-bitbucket-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-bitbucket-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-bitbucket-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-bitbucket-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-saml-sso-for-bitbucket-by-resolution-gmbh-test-user"></a>Criar um SAML SSO para Bitbucket pelo utilizador de teste de GmbH de resolução

O objetivo desta secção consiste em criar um utilizador chamado Britta Simon no SAML SSO para Bitbucket pela resolução GmbH. SAML SSO para Bitbucket pela resolução GmbH suporta just-in-time o aprovisionamento e também os utilizadores podem ser criados manualmente, contacte [SAML SSO para Bitbucket pela resolução GmbH cliente suportar equipa](https://marketplace.atlassian.com/plugins/com.resolution.atlasplugins.samlsso-bitbucket/server/support) de acordo com os requisitos.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para SAML SSO para Bitbucket pela resolução GmbH.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a SAML SSO para Bitbucket pela resolução GmbH, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **SAML SSO para Bitbucket pela resolução GmbH**.

    ![O SAML SSO para Bitbucket pela ligação de GmbH resolução na lista de aplicações](./media/active-directory-saas-bitbucket-tutorial/tutorial_bitbucket_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Ao clicar o SAML SSO para Bitbucket pelo mosaico de GmbH resolução no painel de acesso, deve obter automaticamente com sessão iniciada para o SAML SSO para Bitbucket pela resolução GmbH aplicação.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bitbucket-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bitbucket-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bitbucket-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bitbucket-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bitbucket-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bitbucket-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bitbucket-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bitbucket-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bitbucket-tutorial/tutorial_general_203.png

