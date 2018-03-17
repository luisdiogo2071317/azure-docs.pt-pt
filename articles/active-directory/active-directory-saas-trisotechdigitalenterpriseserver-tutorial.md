---
title: "Tutorial: Integração do Azure Active Directory com Trisotech Digital Enterprise Server | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e Trisotech Digital Enterprise Server."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6d54d20c-eca1-4fa6-b56a-4c3ed0593db0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2018
ms.author: jeedes
ms.openlocfilehash: 82e88b0b2b7f04f2849bf5c3a780df3c8f1c9849
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-azure-active-directory-integration-with-trisotech-digital-enterprise-server"></a>Tutorial: Integração do Azure Active Directory com Trisotech Digital Enterprise Server

Neste tutorial, irá aprender a integrar Trisotech Digital Enterprise Server com o Azure Active Directory (Azure AD).

Integrar Trisotech Digital Enterprise Server com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Trisotech Digital Enterprise Server.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada para Trisotech Digital Enterprise Server (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Trisotech Digital Enterprise Server, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Trisotech Digital Enterprise Server-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Trisotech Digital Enterprise Server na galeria do
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-trisotech-digital-enterprise-server-from-the-gallery"></a>Adicionar Trisotech Digital Enterprise Server na galeria do
Para configurar a integração de Trisotech Digital Enterprise Server com o Azure AD, tem de adicionar Trisotech Digital Enterprise Server na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Trisotech Digital Enterprise Server a partir da galeria do, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Trisotech Digital Enterprise Server**, selecione **Trisotech Digital Enterprise Server** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar o aplicação.

    ![Trisotech Digital Enterprise Server na lista de resultados](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Trisotech Digital Enterprise Server com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Trisotech Digital Enterprise Server é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Trisotech Digital Enterprise Server tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Trisotech Digital Enterprise Server, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Trisotech Digital Enterprise Server](#create-a-trisotech-digital-enterprise-server-test-user)**  - para ter um homólogo de Britta Simon Trisotech Digital Enterprise Server que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Trisotech Digital Enterprise Server.

**Para configurar o Azure AD-início de sessão único com Trisotech Digital Enterprise Server, execute os seguintes passos:**

1. No portal do Azure, no **Trisotech Digital Enterprise Server** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_samlbase.png)

3. No **Trisotech Digital empresarial do domínio e os URLs** secção, execute os seguintes passos:

    ![Trisotech Digital empresarial do domínio e os URLs únicos de informações de início de sessão](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_url.png)

    a. No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<companyname>.trisotech.com`

    b. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<companyname>.trisotech.com`

    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente do servidor Enterprise Digital Trisotech](mailto:support@trisotech.com) para obter estes valores. 

4. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_400.png)

5. Para gerar o **metadados** url, execute os seguintes passos:

    a. Clique em **registos de aplicação**.
    
    ![Configurar o início de sessão único](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_appregistrations.png)
   
    b. Clique em **pontos finais** para abrir **pontos finais** caixa de diálogo.  
    
    ![Configurar o início de sessão único](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_endpointicon.png)

    c. Clique no botão Copiar para copiar **documento de METADADOS de Federação** url e cole-o bloco de notas.
    
    ![Configurar o início de sessão único](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_endpoint.png)
     
    d. Agora, a página de propriedades do **Trisotech Digital Enterprise Server** e copie o **Id da aplicação** utilizando **cópia** botão e cole-o bloco de notas.
 
    ![Configurar o início de sessão único](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_appid.png)

    e. Gerar o **URL de metadados** utilizando o padrão do seguinte: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`

6. Numa janela do browser web diferente, inicie sessão no site da sua empresa Trisotech Digital Enterprise a configuração do servidor como administrador.

7. Clique em de **ícone de Menu** e, em seguida, selecione **administração**.

    ![Configurar o início de sessão único](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/user1.png)

8. Selecione **utilizador fornecedor**.

    ![Configurar o início de sessão único](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/user2.png)

9. No **configurações de fornecedor de utilizador** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/user3.png)

    a. Selecione **protegida Assertion Markup Language 2 (SAML 2)** na lista pendente no **método de autenticação**.

    b. No **URL de metadados** caixa de texto, cole o **URL de metadados** valor que copiou formulário do portal do Azure.

    c. No **ID da aplicação** caixa de texto, introduza o URL a utilizar o padrão seguinte: `https://<companyname>.trisotech.com`.

    d. Clique em **guardar**

    e. Introduza o nome de domínio no **permitido domínios (vazio significa que todas as pessoas)** caixa de texto, automaticamente atribui licenças para utilizadores que correspondam a domínios permitido

    f. Clique em **guardar**

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-trisotech-digital-enterprise-server-test-user"></a>Criar um utilizador de teste Trisotech Digital Enterprise Server

O objetivo desta secção consiste em criar um utilizador chamado Britta Simon Trisotech Digital Enterprise Server. Trisotech Digital Enterprise Server suportam o aprovisionamento de just-in-time, que está por predefinição, ativada. Não há nenhum item de ação para si nesta secção. Um novo utilizador é criado durante a tentativa de aceder Trisotech Digital Enterprise Server se não existir ainda.
>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de Trisotech Digital Enterprise Server](mailto:support@trisotech.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Trisotech Digital Enterprise Server.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a Trisotech Digital Enterprise Server, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Trisotech Digital Enterprise Server**.

    ![A ligação de Trisotech Digital Enterprise Server na lista de aplicações](./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_trisotechdigitalenterpriseserver_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Trisotech Digital Enterprise Server no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de Trisotech Digital Enterprise Server.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-trisotechdigitalenterpriseserver-tutorial/tutorial_general_203.png

