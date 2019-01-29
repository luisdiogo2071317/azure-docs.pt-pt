---
title: 'Tutorial: Integração do Active Directory do Azure com o UserVoice | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e UserVoice.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: a2b8faaf3ee7d8f5dfca35f860dbd003f9e6072b
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55194343"
---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Tutorial: Integração do Active Directory do Azure com o UserVoice

Neste tutorial, saiba como integrar o UserVoice com o Azure Active Directory (Azure AD).

Integrar o UserVoice com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao UserVoice.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada no UserVoice (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o UserVoice, terá dos seguintes itens:

- Uma subscrição do Azure
- Um UserVoice logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o UserVoice da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-uservoice-from-the-gallery"></a>Adicionando o UserVoice da Galeria
Para configurar a integração do UserVoice para o Azure AD, terá de adicionar UserVoice a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar UserVoice a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **UserVoice**, selecione **UserVoice** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![UserVoice na lista de resultados](./media/uservoice-tutorial/tutorial_uservoice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o UserVoice com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no UserVoice para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no UserVoice deve ser estabelecido.

No UserVoice, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o UserVoice, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste do UserVoice](#create-a-uservoice-test-user)**  - para ter um equivalente da Eduarda Almeida no UserVoice que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de UserVoice.

**Para configurar o Azure AD início de sessão único com o UserVoice, execute os seguintes passos:**

1. No portal do Azure, sobre o **UserVoice** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/uservoice-tutorial/tutorial_uservoice_samlbase.png)

1. Sobre o **UserVoice domínio e URLs** secção, execute os seguintes passos:

    ![UserVoice do domínio e URLs únicas início de sessão em informações](./media/uservoice-tutorial/tutorial_uservoice_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenantname>.UserVoice.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenantname>.UserVoice.com`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente de UserVoice](https://www.uservoice.com/) obter esses valores.

1. Sobre o **certificado de assinatura SAML** secção, copie a **THUMBPRINT** valor do certificado.

    ![O link de download de certificado](./media/uservoice-tutorial/tutorial_uservoice_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/uservoice-tutorial/tutorial_general_400.png)

1. Na **configuração do UserVoice** secção, clique em **configurar UserVoice** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de UserVoice](./media/uservoice-tutorial/tutorial_uservoice_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa UserVoice como administrador.

1. Na barra de ferramentas na parte superior, clique em **configurações**e, em seguida, selecione **Web portal** no menu.
   
    ![Secção de definições no lado de aplicação](./media/uservoice-tutorial/ic777519.png "definições")

1. Sobre o **Web portal** separador o **autenticação de utilizador** secção, clique em **editar** para abrir o **editar autenticação de utilizador** página de diálogo.
   
    ![Portal Web do guia](./media/uservoice-tutorial/ic777520.png "Web portal")

1. Sobre o **editar autenticação de utilizador** caixa de diálogo página, execute os seguintes passos:
   
    ![Editar autenticação de utilizador](./media/uservoice-tutorial/ic777521.png "editar autenticação de utilizador")
   
    a. Clique em **início de sessão único (SSO)**.
 
    b. Colar o **SAML único início de sessão no URL do serviço** valor, que copiou do portal do Azure para o **SSO remoto início de sessão** caixa de texto.

    c. Colar o **URL de fim de sessão** valor, que copiou do portal do Azure para o **caixa de texto de fim de sessão remoto SSO**.
 
    d. Colar o **Thumbprint** valor, que copiou do portal do Azure para o **impressão digital de certificado SHA1 atual** caixa de texto.
    
    e. Clique em **guardar as definições de autenticação**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/uservoice-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/uservoice-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/uservoice-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/uservoice-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-uservoice-test-user"></a>Criar um utilizador de teste de UserVoice

Para ativar a utilizadores do Azure AD iniciar sessão no UserVoice, tem de ser aprovisionados no UserVoice. No caso do UserVoice, aprovisionamento é uma tarefa manual.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Para Aprovisionar uma conta de utilizador, execute os seguintes passos:
1. Inicie sessão no seu **UserVoice** inquilino.

1. Aceda a **definições**.
   
    ![As definições](./media/uservoice-tutorial/ic777811.png "definições")

1. Clique em **gerais**.

1. Clique em **agentes e permissões**.
   
    ![Permissões e agentes](./media/uservoice-tutorial/ic777812.png "agentes e permissões")

1. Clique em **adicionar administradores**.
   
    ![Adicionar administradores](./media/uservoice-tutorial/ic777813.png "adicionar administradores")

1. Sobre o **convidar administradores** caixa de diálogo, execute os seguintes passos:
   
    ![Convidar admins](./media/uservoice-tutorial/ic777814.png "convidar os administradores")
   
    a. Na caixa de texto de mensagens de correio eletrónico, escreva o endereço de e-mail da conta que pretende aprovisionar e, em seguida, clique em **adicionar**.
   
    b. Clique em **convidar**.

> [!NOTE]
> Pode utilizar quaisquer outras UserVoice utilizador conta criação ferramentas ou APIs fornecidas pelo UserVoice para aprovisionar contas de utilizador do AAD.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao UserVoice.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a UserVoice, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **UserVoice**.

    ![A ligação de UserVoice na lista de aplicações](./media/uservoice-tutorial/tutorial_uservoice_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do UserVoice no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo de UserVoice.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/uservoice-tutorial/tutorial_general_01.png
[2]: ./media/uservoice-tutorial/tutorial_general_02.png
[3]: ./media/uservoice-tutorial/tutorial_general_03.png
[4]: ./media/uservoice-tutorial/tutorial_general_04.png

[100]: ./media/uservoice-tutorial/tutorial_general_100.png

[200]: ./media/uservoice-tutorial/tutorial_general_200.png
[201]: ./media/uservoice-tutorial/tutorial_general_201.png
[202]: ./media/uservoice-tutorial/tutorial_general_202.png
[203]: ./media/uservoice-tutorial/tutorial_general_203.png

