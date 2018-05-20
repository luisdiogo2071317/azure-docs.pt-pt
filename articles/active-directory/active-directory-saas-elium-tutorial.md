---
title: 'Tutorial: Integração do Azure Active Directory com Elium | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Elium.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: fae344b3-5bd9-40e2-9a1d-448dcd58155f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: jeedes
ms.openlocfilehash: b64f4153908668117ff07b20701c834ce66d3c46
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-elium"></a>Tutorial: Integração do Azure Active Directory com Elium

Neste tutorial, irá aprender a integrar Elium com o Azure Active Directory (Azure AD).

Integrar Elium com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Elium.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Elium (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Elium, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Elium-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Elium a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-elium-from-the-gallery"></a>Adicionar Elium a partir da Galeria
Para configurar a integração de Elium com o Azure AD, terá de adicionar Elium a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Elium a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Elium**, selecione **Elium** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Elium na lista de resultados](./media/active-directory-saas-elium-tutorial/tutorial_elium_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Elium com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Elium é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Elium tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Elium, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Elium](#create-an-elium-test-user)**  - para ter um homólogo de Britta Simon Elium que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Elium.

**Para configurar o Azure AD-início de sessão único com Elium, execute os seguintes passos:**

1. No portal do Azure, no **Elium** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-elium-tutorial/tutorial_elium_samlbase.png)

3. No **Elium domínio e os URLs** secção, execute os seguintes passos, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Domínio Elium e os URLs únicos de informações de início de sessão](./media/active-directory-saas-elium-tutorial/tutorial_elium_url.png)

    a. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<platform-domain>.elium.com/login/saml2/metadata`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<platform-domain>.elium.com/login/saml2/acs`

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![Domínio Elium e os URLs únicos de informações de início de sessão](./media/active-directory-saas-elium-tutorial/tutorial_elium_url1.png)

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: ` https://<platform-domain>.elium.com/login/saml2/login`
     
    > [!NOTE] 
    > Estes valores não estiverem reais. Irá obter estes valores no **ficheiro de metadados SP** transferível em `https://<platform-domain>.elium.com/login/saml2/metadata`, que é explicado posteriormente neste tutorial.

5. A aplicação de Elium espera as asserções de SAML num formato específico, que necessita para adicionar mapeamentos de atributos personalizado à sua configuração de atributos token SAML. Configure as seguintes afirmações para esta aplicação. Pode gerir os valores destes atributos a partir de "**atributos de utilizador**" secção na página de integração de aplicações.

    ![Configurar o início de sessão único](./media/active-directory-saas-elium-tutorial/tutorial_attribute.png)

6. No **atributos de utilizador** secção no **de sessão único-** caixa de diálogo, configurar atributos token SAML, conforme mostrado na imagem anterior e execute os seguintes passos:
           
    | Nome do Atributo | Valor do Atributo |   
    | ---------------| ----------------|
    | e-mail   |User.Mail |
    | first_name| User.givenName |
    | last_name| User.Surname|
    | job_title| user.jobtitle|
    | Empresa| user.companyname|
    
    > [!NOTE]
    > Estas são as afirmações de predefinição. **Apenas afirmações de e-mail é necessária**. Para JIT aprovisionamento também e-mail apenas a afirmação é obrigatória. Outras afirmações personalizadas podem diferir das plataforma de um cliente para noutra plataforma de cliente.

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/active-directory-saas-elium-tutorial/tutorial_attribute_04.png)

    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    ![Configurar o início de sessão único](./media/active-directory-saas-elium-tutorial/tutorial_attribute_05.png)

    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o espaço de nomes como em branco.
    
    e. Clique em **OK**. 

5. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-elium-tutorial/tutorial_elium_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-elium-tutorial/tutorial_general_400.png)
    
7. Numa janela do browser web diferente, inicie sessão no site da sua empresa Elium como administrador.

8. Clique em de **perfil de utilizador** do canto superior direito e selecione **administração**.

    ![Configurar o início de sessão único](./media/active-directory-saas-elium-tutorial/user1.png)

9. Selecione **segurança** separador.

    ![Configurar o início de sessão único](./media/active-directory-saas-elium-tutorial/user2.png)

10. Desloque para baixo até o **único início de sessão (SSO)** secção e execute os seguintes passos:

    ![Configurar o início de sessão único](./media/active-directory-saas-elium-tutorial/user3.png)

    a. Copie o valor da **Certifique-se de que a autenticação SAML2 funciona para a sua conta** e cole-a no **URL de início de sessão** caixa de texto no **Elium domínio e os URLs** secção do Azure Portal.

    > [!NOTE]
    > Depois de configurar o SSO, pode aceder sempre a página de início de sessão remoto predefinida no seguinte URL: `https://<platform_domain>/login/regular/login` 

    b. Selecione **SAML2 ativar a Federação** caixa de verificação.

    c. Selecione **JIT aprovisionamento** caixa de verificação.

    d. Abra o **SP metadados** ao clicar no **transferir** botão.

    e. Procure o **entityID** no **SP metadados** ficheiro, copie o **entityID** valor e cole-a no **identificador** caixa de texto no  **Domínio Elium e URLs** secção no portal do Azure. 

    ![Configurar o início de sessão único](./media/active-directory-saas-elium-tutorial/user4.png)

    f. Procure o **AssertionConsumerService** no **SP metadados** ficheiro, copie o **localização** valor e cole-a no **URL de resposta** caixa de texto no **Elium domínio e os URLs** secção no portal do Azure.

    ![Configurar o início de sessão único](./media/active-directory-saas-elium-tutorial/user5.png)

    g. Abrir o ficheiro de metadados transferido a partir do portal do Azure no bloco de notas, copie o conteúdo e cole-na **IdP metadados** caixa de texto.

    h. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-elium-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-elium-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-elium-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-elium-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-elium-test-user"></a>Criar um utilizador de teste Elium

O objetivo desta secção consiste em criar um utilizador chamado Britta Simon Elium. Elium suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não há nenhum item de ação para si nesta secção. Um novo utilizador é criado durante a tentativa de aceder Elium se não existir ainda.
>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de Elium](mailto:support@elium.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Elium.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a Elium, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Elium**.

    ![A ligação de Elium na lista de aplicações](./media/active-directory-saas-elium-tutorial/tutorial_elium_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Elium no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de Elium.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-elium-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-elium-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-elium-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-elium-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-elium-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-elium-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-elium-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-elium-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-elium-tutorial/tutorial_general_203.png

