---
title: 'Tutorial: Integração do Azure Active Directory com Fluxx Labs | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Fluxx laboratórios.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d8fac770-bb57-4e1f-b50b-9ffeae239d07
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2018
ms.author: jeedes
ms.openlocfilehash: 2d22720e71788493d3663524f2b70783ba26b84d
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36218170"
---
# <a name="tutorial-azure-active-directory-integration-with-fluxx-labs"></a>Tutorial: Integração do Azure Active Directory com Fluxx laboratórios

Neste tutorial, irá aprender a integrar Fluxx laboratórios com o Azure Active Directory (Azure AD).

Integrar Fluxx laboratórios com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso a laboratórios Fluxx.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada para laboratórios Fluxx (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Fluxx laboratórios, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Fluxx laboratórios-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. A adição de Fluxx laboratórios de galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-fluxx-labs-from-the-gallery"></a>A adição de Fluxx laboratórios de galeria
Para configurar a integração de Fluxx laboratórios com o Azure AD, tem de adicionar Fluxx laboratórios na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Fluxx laboratórios na galeria do, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Fluxx laboratórios**, selecione **Fluxx laboratórios** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Laboratórios Fluxx na lista de resultados](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com laboratórios Fluxx com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo em laboratórios de Fluxx é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado em laboratórios de Fluxx tem de ser estabelecida.

Em laboratórios de Fluxx, atribua o valor do **nome de utilizador** no Azure AD como o valor a **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD-início de sessão único com Fluxx laboratórios, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Fluxx laboratórios](#create-a-fluxx-labs-test-user)**  - para ter um homólogo de Britta Simon Fluxx laboratórios, que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Fluxx laboratórios.

**Para configurar o Azure AD-início de sessão único com Fluxx laboratórios, execute os seguintes passos:**

1. No portal do Azure, no **Fluxx laboratórios** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_samlbase.png)

3. No **Fluxx laboratórios de domínio e os URLs** secção, execute os seguintes passos:

    ![Fluxx laboratórios de domínio e os URLs únicos de informações de início de sessão](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_url.png)

    a. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte:

    | Ambiente | Padrão de URL|
    |-------------|------------|
    | Produção | `https://<subdomain>.fluxx.io` |
    | Pré-produção | `https://<subdomain>.preprod.fluxxlabs.com`|
        
    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte:

    | Ambiente | Padrão de URL|
    |-------------|------------|
    | Produção | `https://<subdomain>.fluxx.io/auth/saml/callback` |
    | Pré-produção | `https://<subdomain>.preprod.fluxxlabs.com/auth/saml/callback`|
        
    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o identificador e o URL de resposta real. Contacte [Fluxx laboratórios suporta equipa](mailto:travis@fluxxlabs.com) para obter estes valores.

4. No **certificado de assinatura de SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/fluxxlabs-tutorial/tutorial_general_400.png)

6. No **Fluxx laboratórios configuração** secção, clique em **configurar laboratórios de Fluxx** para abrir **configurar início de sessão** janela. Copiar o **ID de entidade de SAML e único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configuração de laboratórios Fluxx](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_configure.png)

7. Numa janela do browser web diferente, inicie sessão site da sua empresa Fluxx laboratórios como administrador.

8. Selecione **Admin** abaixo o **definições** secção.

    ![Configuração de laboratórios Fluxx](./media/fluxxlabs-tutorial/config1.png)

9. No painel administração, selecione **Plug-ins** > **integrações** e, em seguida, selecione **SAML SSO-(Disabled)**

    ![Configuração de laboratórios Fluxx](./media/fluxxlabs-tutorial/config2.png)
    
10. Na secção de atributo, execute os seguintes passos:
    
    ![Configuração de laboratórios Fluxx](./media/fluxxlabs-tutorial/config3.png)

    a. Selecione o **SAML SSO** caixa de verificação.

    b. No **pedido caminho** caixa de texto, tipo **/auth/saml**.

    c. No **caminho de chamada de retorno** caixa de texto, tipo **/auth/saml/callback**.

    d. No **asserção consumidor serviço Url(Single Sign-On URL)** caixa de texto, cole o valor de **único início de sessão no URL do serviço SAML**, que copiou do portal do Azure.

    e. No **público-alvo (ID de entidade SP)** caixa de texto, cole o valor de **ID de entidade de SAML**, que copiou do portal do Azure.

    f. Abra o certificado codificado base-64 no bloco de notas, copie o conteúdo do mesmo para a sua área de transferência e, em seguida, cole-os para o **certificado do fornecedor de identidade** caixa de texto.

    g. No **identificador de nome de formato** caixa de texto, introduza o valor `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.

    h. Clique em **Guardar**.

    > [!NOTE]
    > Depois do conteúdo guardado, o campo irá aparecer em branco para a segurança, mas foi guardado o valor na configuração.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/fluxxlabs-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/fluxxlabs-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/fluxxlabs-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/fluxxlabs-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-a-fluxx-labs-test-user"></a>Criar um utilizador Fluxx laboratórios de teste

Para permitir que os utilizadores do Azure AD iniciem sessão nos laboratórios de Fluxx, têm de ser aprovisionados para Fluxx laboratórios. No caso de Fluxx laboratórios, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa Fluxx laboratórios como administrador.

2. Clique em de apresentada abaixo **ícone**.

    ![Configuração de laboratórios Fluxx](./media/fluxxlabs-tutorial/config6.png)

3. No dashboard, clique em de abaixo o ícone apresentado para abrir o **pessoas novo** cartão.

    ![Configuração de laboratórios Fluxx](./media/fluxxlabs-tutorial/config4.png)

4. No **pessoas novo** secção, execute os seguintes passos:
    
    ![Configuração de laboratórios Fluxx](./media/fluxxlabs-tutorial/config5.png)

    a. Laboratórios Fluxx utilizar e-mail como o identificador exclusivo para inícios de sessão do SSO. Preencher o **SSO UID** campo com o endereço de e-mail do utilizador, que corresponde ao endereço de e-mail, que estão a utilizar como início de sessão com o SSO.

    b. Clique em **Guardar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso a laboratórios Fluxx.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a Fluxx laboratórios, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Fluxx laboratórios**.

    ![A ligação de Fluxx laboratórios na lista de aplicações](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Fluxx laboratórios no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de Fluxx laboratórios.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/fluxxlabs-tutorial/tutorial_general_01.png
[2]: ./media/fluxxlabs-tutorial/tutorial_general_02.png
[3]: ./media/fluxxlabs-tutorial/tutorial_general_03.png
[4]: ./media/fluxxlabs-tutorial/tutorial_general_04.png

[100]: ./media/fluxxlabs-tutorial/tutorial_general_100.png

[200]: ./media/fluxxlabs-tutorial/tutorial_general_200.png
[201]: ./media/fluxxlabs-tutorial/tutorial_general_201.png
[202]: ./media/fluxxlabs-tutorial/tutorial_general_202.png
[203]: ./media/fluxxlabs-tutorial/tutorial_general_203.png
