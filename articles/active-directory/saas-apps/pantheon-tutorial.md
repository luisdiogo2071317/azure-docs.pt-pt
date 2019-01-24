---
title: 'Tutorial: Integração do Active Directory do Azure com Pantheon | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Pantheon.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: d2c965d1-666f-44c2-b08f-b73163096374
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 0bfe96bb169e54b1ed6c79390b4aea714da8ee69
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54814584"
---
# <a name="tutorial-azure-active-directory-integration-with-pantheon"></a>Tutorial: Integração do Active Directory do Azure com Pantheon

Neste tutorial, saiba como integrar Pantheon com o Azure Active Directory (Azure AD).

Integrar Pantheon no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Pantheon
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Pantheon (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Pantheon, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Pantheon logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Pantheon da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-pantheon-from-the-gallery"></a>Adicionando Pantheon da Galeria
Para configurar a integração do Pantheon com o Azure AD, terá de adicionar Pantheon a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Pantheon a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Pantheon**.

    ![Criar um utilizador de teste do Azure AD](./media/pantheon-tutorial/tutorial_pantheon_search.png)

1. No painel de resultados, selecione **Pantheon**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/pantheon-tutorial/tutorial_pantheon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Pantheon com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Pantheon a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Pantheon deve ser estabelecido.

Pantheon, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Pantheon, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Pantheon](#creating-a-pantheon-test-user)**  - para ter um equivalente da Eduarda Almeida na Pantheon que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Pantheon.

**Para configurar o Azure AD início de sessão único com Pantheon, execute os seguintes passos:**

1. No portal do Azure, sobre o **Pantheon** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/pantheon-tutorial/tutorial_pantheon_samlbase.png)

1. Sobre o **Pantheon domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/pantheon-tutorial/tutorial_pantheon_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `urn:auth0:pantheon:<orgname>-SSO`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://pantheon.auth0.com/login/callback?connection=<orgname>-SSO`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador real e o URL de resposta. Contacte [equipa de suporte de Pantheon](https://pantheon.io/docs/getting-support/) obter esses valores.

1. Aplicação de pantheon espera a asserção de SAML em formato específico, o que requer que defina o valor do atributo UserIdentifier com endereço de e-mail do utilizador. Por predefinição do Azure AD utiliza o UserPrincipalName UserIdentifier atributo. Mas, para integração com êxito terá de ajustar este valor para corresponder com o endereço de e-mail do utilizador. A integração só funcionam depois de fazer o mapeamento correto.

    ![Configurar o início de sessão único](./media/pantheon-tutorial/tutorial_attribute.png)    


1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/pantheon-tutorial/tutorial_pantheon_certificate.png)

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/pantheon-tutorial/tutorial_general_400.png)

1. Sobre o **Pantheon configuração** secção, clique em **configurar Pantheon** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/pantheon-tutorial/tutorial_pantheon_configure.png) 

1. Para configurar o início de sessão único num **Pantheon** lado, terá de enviar o transferido **certificado** e **SAML único início de sessão no URL do serviço** para [Pantheon equipa de suporte](https://pantheon.io/docs/getting-support/).

     > [!Note]
     > Terá também de fornecer as informações de domínios de E-Mail e a hora da data quando pretender ativar esta ligação. Pode encontrar mais detalhes sobre este assunto na [aqui](https://pantheon.io/docs/sso-organizations/)

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/pantheon-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/pantheon-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/pantheon-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/pantheon-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-pantheon-test-user"></a>Criar um utilizador de teste Pantheon

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Pantheon. Siga os passos abaixo para adicionar o utilizador no Pantheon. 

>[!NOTE] 
>Para SSO para o utilizador de trabalho tem de ser criado pela primeira vez no Pantheon.

1. Inicie sessão no Pantheon com credenciais de administrador.

1. Navegue para **organização** a página do dashboard.
 
1. Clique em **pessoas**.

1. Clique em **Adicionar utilizador**.

1. Introduza o endereço de e-mail do utilizador.

1. Escolha a função do utilizador.

1. Clique em **Adicionar utilizador**.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Pantheon.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Pantheon, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Pantheon**.

    ![Configurar o início de sessão único](./media/pantheon-tutorial/tutorial_pantheon_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Pantheon no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Pantheon.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/pantheon-tutorial/tutorial_general_01.png
[2]: ./media/pantheon-tutorial/tutorial_general_02.png
[3]: ./media/pantheon-tutorial/tutorial_general_03.png
[4]: ./media/pantheon-tutorial/tutorial_general_04.png

[100]: ./media/pantheon-tutorial/tutorial_general_100.png

[200]: ./media/pantheon-tutorial/tutorial_general_200.png
[201]: ./media/pantheon-tutorial/tutorial_general_201.png
[202]: ./media/pantheon-tutorial/tutorial_general_202.png
[203]: ./media/pantheon-tutorial/tutorial_general_203.png

