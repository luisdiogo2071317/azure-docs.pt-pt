---
title: 'Tutorial: Integração do Azure Active Directory com Ziflow | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Ziflow.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 84e60fa4-36fb-49c4-a642-95538c78f926
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/09/2018
ms.author: jeedes
ms.openlocfilehash: 6fe439daad78d6d660fd84be8a1e0263b231adbd
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39444379"
---
# <a name="tutorial-azure-active-directory-integration-with-ziflow"></a>Tutorial: Integração do Azure Active Directory com Ziflow

Neste tutorial, saiba como integrar Ziflow com o Azure Active Directory (Azure AD).

Integrar Ziflow no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Ziflow.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Ziflow (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Ziflow, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Ziflow logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Ziflow da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-ziflow-from-the-gallery"></a>Adicionando Ziflow da Galeria
Para configurar a integração do Ziflow com o Azure AD, terá de adicionar Ziflow a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Ziflow a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Ziflow**, selecione **Ziflow** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Ziflow na lista de resultados](./media/ziflow-tutorial/tutorial_ziflow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Ziflow com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Ziflow a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Ziflow deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Ziflow, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Ziflow](#create-a-ziflow-test-user)**  - para ter um equivalente da Eduarda Almeida na Ziflow que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Ziflow.

**Para configurar o Azure AD início de sessão único com Ziflow, execute os seguintes passos:**

1. No portal do Azure, sobre o **Ziflow** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/ziflow-tutorial/tutorial_ziflow_samlbase.png)

1. Sobre o **Ziflow domínio e URLs** secção, execute os seguintes passos:

    ![Ziflow domínio e URLs únicas início de sessão em informações](./media/ziflow-tutorial/tutorial_ziflow_url.png)

    a. Na **iniciar sessão no URL** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.ziflow.io/#/login-sso/<Unique ID>`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `urn:auth0:ziflow-production:<Unique ID>`

    > [!NOTE] 
    > Os valores anteriores não são reais. Atualizar o valor ID exclusivo do identificador e o URL de início de sessão com o valor real, o que é explicado mais tarde no tutorial. Contacte [equipa de suporte de Ziflow](mailto:support@ziflow.com) para o valor de subdomínio no URL de início de sessão.
    
1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/ziflow-tutorial/tutorial_ziflow_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/ziflow-tutorial/tutorial_general_400.png)

1. Sobre o **Ziflow configuração** secção, clique em **configurar Ziflow** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de Ziflow](./media/ziflow-tutorial/tutorial_ziflow_configure.png) 

1. Numa janela do browser web diferente, início de sessão para Ziflow como um administrador de segurança.


1. Clique em Avatar no canto superior direito e, em seguida, clique em **gerir conta**.

    ![Gerir a configuração de Ziflow](./media/ziflow-tutorial/tutorial_ziflow_manage.png)

1. No canto superior esquerdo, clique em **Single Sign-On**.

    ![Início de sessão de configuração de Ziflow](./media/ziflow-tutorial/tutorial_ziflow_signon.png)

1. Sobre o **Single Sign-On** página, execute os seguintes passos:

    ![Ziflow configuração única](./media/ziflow-tutorial/tutorial_ziflow_page.png)

    a. Selecione **tipo** como **SAML2.0**.

    b.In a **URL de início de sessão** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure.

    c. Carregar o certificado codificado de base 64 que transferiu do portal do Azure, para o **X509 certificado de assinatura**.

    d. Na **URL de fim de sessão** caixa de texto, cole o valor de **URL de fim de sessão**, que copiou do portal do Azure.

    e. Do **definições de configuração para o seu fornecedor de identificador** secção, copie o valor de ID exclusivo realçado e acrescentá-lo com o identificador e o URL de início de sessão a **seção Ziflow domínio e URLs** no Portal do Azure.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/ziflow-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/ziflow-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/ziflow-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/ziflow-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-a-ziflow-test-user"></a>Criar um utilizador de teste Ziflow

Para ativar a utilizadores do Azure AD iniciar sessão no Ziflow, tem de ser aprovisionados em Ziflow. Ziflow, aprovisionamento é uma tarefa manual.

Para Aprovisionar uma conta de utilizador, execute os seguintes passos:

1. Inicie sessão no Ziflow como um administrador de segurança.

1. Navegue para **pessoas** na parte superior.

    ![Pessoas de configuração de Ziflow](./media/ziflow-tutorial/tutorial_ziflow_people.png)

1. Clique em **Add** e, em seguida, clique em **adicionar utilizador**.

    ![Adicionar o utilizador Ziflow configuração](./media/ziflow-tutorial/tutorial_ziflow_add.png)

1. Sobre o **adicionar um utilizador** pop-up, execute os seguintes passos:

    ![Adicionar o utilizador Ziflow configuração](./media/ziflow-tutorial/tutorial_ziflow_adduser.png)

    a. Na **E-Mail** texto, introduza o e-mail do utilizador, como brittasimon@contoso.com.

    b. Na **nome próprio** texto, introduza o nome de utilizador, como a Eduarda.

    c. Na **Apelido** texto, digite o apelido do utilizador, como Simon.

    d. Selecione a sua função de Ziflow.

    e. Clique em **adicionar 1 utilizador**.

    > [!NOTE]
    > O titular da conta do Azure Active Directory recebe uma mensagem de e-mail e segue uma ligação para confirmar a respetiva conta até se tornar Active Directory.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Ziflow.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Ziflow, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Ziflow**.

    ![A ligação de Ziflow na lista de aplicações](./media/ziflow-tutorial/tutorial_ziflow_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Ziflow no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Ziflow.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/ziflow-tutorial/tutorial_general_01.png
[2]: ./media/ziflow-tutorial/tutorial_general_02.png
[3]: ./media/ziflow-tutorial/tutorial_general_03.png
[4]: ./media/ziflow-tutorial/tutorial_general_04.png

[100]: ./media/ziflow-tutorial/tutorial_general_100.png

[200]: ./media/ziflow-tutorial/tutorial_general_200.png
[201]: ./media/ziflow-tutorial/tutorial_general_201.png
[202]: ./media/ziflow-tutorial/tutorial_general_202.png
[203]: ./media/ziflow-tutorial/tutorial_general_203.png

