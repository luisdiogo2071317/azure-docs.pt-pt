---
title: "Tutorial: Integração do Azure Active Directory com o Cisco Webex | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e Cisco Webex."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory7
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 42632dcf8997ec5e987ac8a6615aae24e903399a
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/16/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Tutorial: Integração do Azure Active Directory com Cisco Webex

Neste tutorial, irá aprender a integrar Cisco Webex com o Azure Active Directory (Azure AD).

Integrar Cisco Webex com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Cisco Webex.
- Pode permitir que os utilizadores automaticamente obter a sessão iniciada no Cisco Webex com as respetivas contas do Azure AD.
- Pode gerir as contas numa única localização central, o portal do Azure.

Para obter mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte [que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Cisco Webex, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma subscrição de início de sessão em ativado único Cisco Webex

> [!NOTE]
> Não recomendamos a utilização de um ambiente de produção para testar os passos neste tutorial.

Para testar os passos neste tutorial, siga estas recomendações:

- Não utilize o seu ambiente de produção, exceto se for necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação gratuita de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. A adição de Cisco Webex de galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="add-cisco-webex-from-the-gallery"></a>Adicionar Cisco Webex a partir da Galeria
Para configurar a integração da Cisco Webex com o Azure AD, tem de adicionar Cisco Webex na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Cisco Webex na galeria do, siga os passos seguintes:**

1. No [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione o **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Aceda a **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar uma nova aplicação, selecione o **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Cisco Webex**. 

5. Selecione **Cisco Webex** partir do painel de resultados. Em seguida, selecione o **adicionar** botão para adicionar a aplicação.

    ![Cisco Webex na lista de resultados](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Webex Cisco, com base num utilizador de teste chamado "Britta Simon."

Para início de sessão trabalhar, do Azure AD tem de saber que o utilizador homólogo no Cisco Webex for para um utilizador no Azure AD. Por outras palavras, tem de estabelecer uma ligação entre um utilizador do Azure AD e um utilizador relacionado no Cisco Webex.

No Cisco Webex, atribua o valor **Username** o mesmo valor como **nome de utilizador** no Azure AD. Agora tem de estabelecer a ligação entre dois utilizadores. 

Para configurar e testar o Azure AD-início de sessão único com o Cisco Webex, conclua os blocos modulares seguintes:

1. [Configurar o Azure AD-início de sessão único](#configure-azure-ad-single-sign-on) para permitir aos utilizadores utilizar esta funcionalidade.
2. [Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user) para testar do Azure AD-início de sessão único com Britta Simon.
3. [Criar um utilizador de teste de Cisco Webex](#create-a-cisco-webex-test-user) para ter um homólogo de Britta Simon no Webex Cisco que está ligada a representação do Azure AD do utilizador.
4. [Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user) para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. [Teste o início de sessão único](#test-single-sign-on) para verificar que a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Cisco Webex.

**Para configurar o Azure AD-início de sessão único com o Cisco Webex, siga os passos seguintes:**

1. No portal do Azure, no **Cisco Webex** página de integração de aplicações, selecione **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. Para ativar o início de sessão, no **de sessão único-** caixa de diálogo a **modo** na lista pendente, selecione **baseados em SAML início de sessão**.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_samlbase.png)

3. No **Cisco Webex domínio e os URLs** secção, siga os passos seguintes:

    ![Cisco Webex domínio e os URLs únicos de informações de início de sessão](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_url.png)

    a. No **URL de início de sessão** caixa, escreva um URL com o padrão do seguinte:`https://<subdomain>.webex.com`

    b. No **identificador** caixa, escreva o URL `http://www.webex.com`.

    c. No **URL de resposta** caixa, escreva um URL com o padrão do seguinte:`https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company`
     
    > [!NOTE] 
    > Estes valores não estiverem reais. Atualizar estes valores com o URL de resposta real e início de sessão no URL. Contacte [equipa de suporte de cliente de Webex Cisco](https://www.webex.co.in/support/support-overview.html) para obter estes valores. 

5. No **certificado de assinatura de SAML** secção, selecione **XML de metadados**e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_certificate.png) 

6. Selecione **Guardar**.

    ![Configurar o início de sessão de gravação botão](./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_400.png)
    
6. No **Cisco Webex configuração** secção, selecione **configurar Webex de Cisco** para abrir o **configurar início de sessão** janela. Copiar o **Sign-Out URL**, **ID de entidade de SAML**, e **único início de sessão no URL do serviço SAML** do **referência rápida** secção.

    ![Configurar o início de sessão único](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_configure.png) 

7. Numa janela do browser web diferente, inicie sessão no site da sua empresa Cisco Webex como administrador.

8. No menu na parte superior, selecione **administração de sites**.

    ![Administração de sites](./media/active-directory-saas-cisco-webex-tutorial/ic777621.png "administração de sites")

9. No **Gerir Site** secção, selecione **SSO configuração**.
   
    ![Configuração de SSO](./media/active-directory-saas-cisco-webex-tutorial/ic777622.png "SSO configuração")

10. No **configuração de SSO Web Federado** secção, siga os passos seguintes:
   
    ![Federado SSO configuração](./media/active-directory-saas-cisco-webex-tutorial/ic777623.png "federado SSO configuração")  

    a. No **protocolo Federation** lista, selecione **SAML 2.0**.

    b. Para **perfil SSO**, selecione **SP iniciada**.

    c. Abra o seu certificado transferido no bloco de notas e, em seguida, copie o conteúdo.

    d. Selecione **importar metadados de SAML**e, em seguida, cole o conteúdo copiado do certificado.

    e. No **emissor para SAML (ID de IdP)** caixa, cole o valor da **ID de entidade de SAML** que copiou do portal do Azure.

    f. No **URL de início de sessão do serviço do cliente SSO** caixa, cole **único início de sessão no URL do serviço SAML**, que copiou do portal do Azure.

    g. Do **NameID formato** lista, selecione **endereço de correio eletrónico**.

    h. No **AuthnContextClassRef** caixa, escreva **urn: oasis: os nomes: tc: SAML:2.0:ac:classes:Password**.

    posso. No **URL de fim de sessão do serviço do cliente SSO** caixa, cole **Sign-Out URL**, que copiou do portal do Azure.
   
    j. Selecione **atualização**.

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com) enquanto estiver a configurar a aplicação. Depois de adicionar esta aplicação a partir de **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador e, em seguida, aceder a incorporados documentação através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados em [do Azure AD incorporado documentação](https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, selecione o **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, selecione **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, selecione **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, siga os passos seguintes:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-cisco-webex-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Selecione **Criar**.
 
### <a name="create-a-cisco-webex-test-user"></a>Criar um utilizador de teste Cisco Webex

Para ativar o Azure AD aos utilizadores iniciar sessão Cisco Webex, têm de ser aprovisionados na Cisco Webex. No caso de Cisco Webex, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, siga os passos seguintes:**

1. Inicie sessão no seu **Cisco Webex** inquilino.

2. Aceda a **gerir utilizadores** > **adicionar utilizador**.
   
    ![Adicionar utilizadores](./media/active-directory-saas-cisco-webex-tutorial/ic777625.png "adicionar utilizadores")

3. No **adicionar utilizador** secção, siga os passos seguintes:
   
    ![Adicionar utilizador](./media/active-directory-saas-cisco-webex-tutorial/ic777626.png "adicionar utilizador")   

    a. Para **tipo de conta**, selecione **anfitrião**.

    b. No **nome próprio** caixa, escreva o nome próprio do utilizador (neste caso, **Britta**).

    c. No **Apelido** caixa, digite o apelido do utilizador (neste caso, **Simon**).

    d. No **Username** caixa, escreva a mensagem de e-mail do utilizador (neste caso,  **Brittasimon@contoso.com** ).

    e. No **E-Mail** caixa, escreva o endereço de e-mail do utilizador (neste caso,  **Brittasimon@contoso.com** ).

    f. No **palavra-passe** caixa, escreva a palavra-passe do utilizador.

    g. No **confirmar** palavra-passe caixa, reintroduza a palavra-passe do utilizador.

    h. Selecione **Adicionar**.

>[!NOTE]
>Pode utilizar quaisquer outras ferramentas de criação de conta de utilizador de Cisco Webex ou APIs fornecidas pelo Cisco Webex aprovisionar contas de utilizador do Azure AD. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, permitir que o utilizador Britta Simon para utilizar o Azure-início de sessão único, concedendo-lhes acesso a Cisco Webex.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a Cisco Webex, siga os passos seguintes:**

1. No portal do Azure, abra a vista de aplicações. Em seguida, aceda à vista de diretório e, em seguida, para **aplicações empresariais**.  

2. Selecione **todas as aplicações**.

    ![Atribua o utilizador][201] 

3. Na lista de aplicações, selecione **Cisco Webex**.

    ![A ligação de Cisco Webex na lista de aplicações](./media/active-directory-saas-cisco-webex-tutorial/tutorial_ciscowebex_app.png)  

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Selecione o **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** no **utilizadores** lista.

6. No **utilizadores e grupos** caixa de diálogo, clique em de **selecione** botão.

7. Selecione o **atribuir** clique no botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, teste configuração do Azure AD único início de sessão utilizando o painel de acesso.

Quando seleciona o mosaico Cisco Webex no painel de acesso, que automaticamente obter sessão na sua aplicação Cisco Webex.

Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS no Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cisco-webex-tutorial/tutorial_general_203.png

