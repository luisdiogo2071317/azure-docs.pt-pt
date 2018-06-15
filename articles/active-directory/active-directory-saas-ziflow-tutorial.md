---
title: 'Tutorial: Integração do Azure Active Directory com Ziflow | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Ziflow.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 84e60fa4-36fb-49c4-a642-95538c78f926
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2018
ms.author: jeedes
ms.openlocfilehash: 4f667edbf6b0fab8c8dac4442d50d14252c4c4cd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34354246"
---
# <a name="tutorial-azure-active-directory-integration-with-ziflow"></a>Tutorial: Integração do Azure Active Directory com Ziflow

Neste tutorial, irá aprender a integrar Ziflow com o Azure Active Directory (Azure AD).

Integrar Ziflow com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Ziflow.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Ziflow (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Ziflow, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Ziflow-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Ziflow a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-ziflow-from-the-gallery"></a>Adicionar Ziflow a partir da Galeria
Para configurar a integração de Ziflow com o Azure AD, terá de adicionar Ziflow a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Ziflow a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Ziflow**, selecione **Ziflow** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Ziflow na lista de resultados](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Ziflow com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Ziflow é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Ziflow tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Ziflow, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Ziflow](#create-a-ziflow-test-user)**  - para ter um homólogo de Britta Simon Ziflow que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Ziflow.

**Para configurar o Azure AD-início de sessão único com Ziflow, execute os seguintes passos:**

1. No portal do Azure, no **Ziflow** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_samlbase.png)

3. No **Ziflow domínio e os URLs** secção, execute os seguintes passos:

    ![Domínio Ziflow e os URLs únicos de informações de início de sessão](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_url.png)

    a. No **iniciar sessão no URL** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.ziflow.io/#/login-sso/<Unique ID>`

    b. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `urn:auth0:ziflow-production:<Unique ID>`

    > [!NOTE] 
    > Os valores anteriores não são reais. Atualizar o valor de ID exclusivo no identificador e URL de início de sessão com o valor real, que é explicada mais tarde no tutorial. Contacte [equipa de suporte de Ziflow](mailto:support@ziflow.com) para o valor de subdomínios no URL do início de sessão.
    
4. No **certificado de assinatura de SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-ziflow-tutorial/tutorial_general_400.png)

6. No **Ziflow configuração** secção, clique em **configurar Ziflow** para abrir **configurar início de sessão** janela. Copiar o **Sign-Out URL e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configuração de Ziflow](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_configure.png) 

7. Numa janela do browser web diferente, no início de sessão Ziflow como um administrador de segurança.


8. Clique em Avatar no canto superior direito e, em seguida, clique em **gerir conta**.

    ![Gerir a configuração de Ziflow](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_manage.png)

9. Na parte superior esquerda, clique em **Single Sign-On**.

    ![Início de sessão de configuração de Ziflow](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_signon.png)

10. No **Single Sign-On** página, execute os seguintes passos:

    ![Ziflow configuração único](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_page.png)

    a. Selecione **tipo** como **SAML2.0**.

    b.In o **início de sessão no URL** caixa de texto, cole o valor de **único início de sessão no URL do serviço SAML**, que copiou do portal do Azure.

    c. Carregue o certificado codificado base-64 que transferiu do portal do Azure, para o **X509 certificado de assinatura**.

    d. No **URL de fim de sessão** caixa de texto, cole o valor de **Sign-Out URL**, que copiou do portal do Azure.

    e. Do **definições de configuração do seu fornecedor de identificador** secção, copie o valor de ID exclusivo realçado e o acrescentem com o identificador e o URL no início de sessão a **secção Ziflow domínio e os URLs** no Portal do Azure.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-ziflow-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-ziflow-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-ziflow-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-ziflow-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-a-ziflow-test-user"></a>Criar um utilizador de teste Ziflow

Para permitir que os utilizadores do Azure AD iniciem sessão nos Ziflow, têm de ser aprovisionados para Ziflow. No Ziflow, o aprovisionamento é uma tarefa manual.

Para Aprovisionar uma conta de utilizador, execute os seguintes passos:

1. Inicie sessão no Ziflow como um administrador de segurança.

2. Navegue para **pessoas** na parte superior.

    ![Pessoas Ziflow configuração](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_people.png)

3. Clique em **adicionar** e, em seguida, clique em **adicionar utilizador**.

    ![Configuração de Ziflow de adição de utilizador](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_add.png)

4. No **adicionar um utilizador** pop-up, execute os seguintes passos:

    ![Configuração de Ziflow de adição de utilizador](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_adduser.png)

    a. No **E-Mail** texto, introduza o e-mail do utilizador, como brittasimon@contoso.com.

    b. No **nome próprio** texto, introduza o nome de utilizador como Britta.

    c. No **Apelido** texto, introduza o apelido do utilizador como Simon.

    d. Selecione a sua função de Ziflow.

    e. Clique em **1 adicionar utilizador**.

    > [!NOTE]
    > Titular da conta do Azure Active Directory recebe uma mensagem de e-mail e segue-se de uma ligação para confirmar a respetiva conta para ficar ativa.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Ziflow.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a Ziflow, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Ziflow**.

    ![A ligação de Ziflow na lista de aplicações](./media/active-directory-saas-ziflow-tutorial/tutorial_ziflow_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Ziflow no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de Ziflow.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ziflow-tutorial/tutorial_general_203.png

