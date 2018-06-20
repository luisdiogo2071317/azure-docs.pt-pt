---
title: 'Tutorial: Integração do Azure Active Directory com um Zoho | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Zoho um.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bbc3038c-0d8b-45dd-9645-368bd3d01a0f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: jeedes
ms.openlocfilehash: 9168b58cda54fa5de497ba331e1e4995dbc4d4e7
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36220538"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho-one"></a>Tutorial: Integração do Azure Active Directory com um Zoho

Neste tutorial, irá aprender a integrar uma Zoho com o Azure Active Directory (Azure AD).

Integrar Zoho um com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Zoho um.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada a Zoho uma (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com um Zoho, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Zoho um início de sessão ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. A adição de um Zoho de galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-zoho-one-from-the-gallery"></a>A adição de um Zoho de galeria
Para configurar a integração de Zoho um com o Azure AD, terá de adicionar um Zoho a Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar um Zoho na galeria do, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Zoho um**, selecione **Zoho um** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Zoho um na lista de resultados](./media/zohoone-tutorial/tutorial_zohoone_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com um Zoho com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo Zoho um é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Zoho um tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Zoho um, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Zoho um](#create-a-zoho-one-test-user)**  - para ter um homólogo de Britta Simon Zoho uma que esteja ligado a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Zoho um.

**Para configurar o Azure AD-início de sessão único com um Zoho, execute os seguintes passos:**

1. No portal do Azure, no **Zoho um** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/zohoone-tutorial/tutorial_zohoone_samlbase.png)

3. No **Zoho um domínio e os URLs** secção, execute os seguintes passos, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Zoho um domínio e os URLs únicos de informações de início de sessão](./media/zohoone-tutorial/tutorial_zohoone_url.png)

    a. No **identificador (ID de entidade)** caixa de texto, escreva um URL: `one.zoho.com`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://accounts.zoho.com/samlresponse/<saml-identifier>`

    c. Verifique **Mostrar avançadas definições de URL**.

    d. No **reencaminhamento estado** caixa de texto, escreva um URL:`https://one.zoho.com`

4. Se pretender configurar a aplicação no **SP** modo iniciado executar o passo seguinte:

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://accounts.zoho.com/samlauthrequest/<domain_name>?serviceurl=https://one.zoho.com`
     
    > [!NOTE] 
    > Precedente **URL de resposta** e **URL de início de sessão** valor não é real. Atualizar o valor com o URL de resposta real e o URL de início de sessão que é explicada mais tarde no tutorial. 

5. No **certificado de assinatura de SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/zohoone-tutorial/tutorial_zohoone_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/zohoone-tutorial/tutorial_general_400.png)
    
7. No **Zoho uma configuração** secção, clique em **configurar um Zoho** para abrir **configurar início de sessão** janela. Copiar o **Sign-Out URL e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Zoho uma configuração](./media/zohoone-tutorial/tutorial_zohoone_configure.png) 

8. Numa janela do browser web diferente, inicie sessão no site da sua empresa Zoho um como um administrador.

9. No **organização** separador, clique em **configuração** em **autenticação SAML**.

    ![Um Zoho org](./media/zohoone-tutorial/tutorial_zohoone_setup.png)

10. Na página do pop-up, execute os seguintes passos:

    ![Sig Zoho um](./media/zohoone-tutorial/tutorial_zohoone_save.png)

    a. No **URL de início de sessão** caixa de texto, cole o valor de **único início de sessão no URL do serviço SAML**, que copiou do portal do Azure.

    b. No **Sign-out URL** caixa de texto, cole o valor de **Sign-Out URL**, que copiou do portal do Azure.

    c. Clique em **procurar** para carregar o **certificado (Base64)** que transferiu a partir do portal do Azure.

    d. Clique em **Guardar**.

11. Depois de guardar a configuração de autenticação SAML, copie o **SAML Identfier** valor e utilizar este valor no **URL de resposta** no portal do Azure, em **Zoho um domínio e os URLs** secção.

    ![Um Zoho saml](./media/zohoone-tutorial/tutorial_zohoone_samlidenti.png)

12. Vá para o **domínios** separador e, em seguida, clique em **Adicionar domínio**.

    ![Zoho um domínio](./media/zohoone-tutorial/tutorial_zohoone_domain.png)

13. No **Adicionar domínio** página, execute os seguintes passos:

    ![Adicionar Zoho um domínio](./media/zohoone-tutorial/tutorial_zohoone_adddomain.png)

    a. No **nome de domínio** caixa de texto, domínio do tipo como **contoso.com**.

    b. Clique em **Adicionar**.

    >[!Note]
    >Depois de adicionar a siga de domínio [estes](https://www.zoho.com/one/help/admin-guide/domain-verification.html) passos para verificar o seu domínio. Assim que o domínio é verfified, utilize o seu nome de domínio no **URL de início de sessão** no **Zoho um domínio e os URLs** secção no portal do Azure.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/zohoone-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/zohoone-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/zohoone-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/zohoone-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-zoho-one-test-user"></a>Criar um Zoho um utilizador de teste

Para ativar a utilizadores do Azure AD para início de sessão a uma Zoho, têm de ser aprovisionados para Zoho um. Um Zoho, aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no Zoho um como um administrador de segurança.

2. No **utilizadores** separador, clique em **logótipo de utilizador**.

    ![Zoho um utilizador](./media/zohoone-tutorial/tutorial_zohoone_users.png)

3. No **adicionar utilizador** página, execute os seguintes passos:

    ![Adicionar Zoho um utilizador](./media/zohoone-tutorial/tutorial_zohoone_adduser.png)
    
    a. No **nome** texto, introduza o nome de utilizador como **Britta simon**.
    
    b. No **endereço de correio eletrónico** texto, introduza o e-mail do utilizador, como **brittasimon@contoso.com**.

    >[!Note]
    >Selecione o seu domínio verificado na lista de domínio.

    c. Clique em **Adicionar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso a uma Zoho.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a Zoho uma, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Zoho um**.

    ![A ligação Zoho um na lista de aplicações](./media/zohoone-tutorial/tutorial_zohoone_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Ao clicar no Zoho um mosaico no painel de acesso, deve obter automaticamente com sessão iniciada Zoho uma aplicação.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/zohoone-tutorial/tutorial_general_01.png
[2]: ./media/zohoone-tutorial/tutorial_general_02.png
[3]: ./media/zohoone-tutorial/tutorial_general_03.png
[4]: ./media/zohoone-tutorial/tutorial_general_04.png

[100]: ./media/zohoone-tutorial/tutorial_general_100.png

[200]: ./media/zohoone-tutorial/tutorial_general_200.png
[201]: ./media/zohoone-tutorial/tutorial_general_201.png
[202]: ./media/zohoone-tutorial/tutorial_general_202.png
[203]: ./media/zohoone-tutorial/tutorial_general_203.png

