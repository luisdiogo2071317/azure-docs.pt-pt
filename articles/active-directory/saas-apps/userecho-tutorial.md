---
title: 'Tutorial: Integração do Azure Active Directory com UserEcho | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e UserEcho.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: bedd916b-8f69-4b50-9b8d-56f4ee3bd3ed
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: ecdd37db662c6861e35f80bfbf4ac8ff7e0d08c9
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39443279"
---
# <a name="tutorial-azure-active-directory-integration-with-userecho"></a>Tutorial: Integração do Azure Active Directory com UserEcho

Neste tutorial, saiba como integrar UserEcho com o Azure Active Directory (Azure AD).

Integrar UserEcho no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao UserEcho
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para UserEcho (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com UserEcho, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um UserEcho logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma avaliação de um mês aqui: [oferta de avaliação](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando UserEcho da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-userecho-from-the-gallery"></a>Adicionando UserEcho da Galeria
Para configurar a integração do UserEcho com o Azure AD, terá de adicionar UserEcho a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar UserEcho a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **UserEcho**.

    ![Criar um utilizador de teste do Azure AD](./media/userecho-tutorial/tutorial_userecho_search.png)

1. No painel de resultados, selecione **UserEcho**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/userecho-tutorial/tutorial_userecho_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com UserEcho com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no UserEcho a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no UserEcho deve ser estabelecido.

UserEcho, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com UserEcho, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste UserEcho](#creating-a-userecho-test-user)**  - para ter um equivalente da Eduarda Almeida na UserEcho que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo UserEcho.

**Para configurar o Azure AD início de sessão único com UserEcho, execute os seguintes passos:**

1. No portal do Azure, sobre o **UserEcho** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/userecho-tutorial/tutorial_userecho_samlbase.png)

1. Sobre o **UserEcho domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/userecho-tutorial/tutorial_userecho_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.userecho.com/`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.userecho.com/saml/metadata/`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente UserEcho](https://feedback.userecho.com/) obter esses valores. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/userecho-tutorial/tutorial_userecho_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/userecho-tutorial/tutorial_general_400.png)

1. Sobre o **UserEcho configuração** secção, clique em **configurar UserEcho** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/userecho-tutorial/tutorial_userecho_configure.png) 

1. Na outra janela do browser, inicie sessão site da sua empresa UserEcho como um administrador.

1. Na barra de ferramentas na parte superior, clique em seu nome de utilizador para expandir o menu e, em seguida, clique em **configuração**.
   
    ![Configurar o início de sessão único](./media/userecho-tutorial/tutorial_userecho_06.png) 

1. Clique em **integrações**.
   
    ![Configurar o início de sessão único](./media/userecho-tutorial/tutorial_userecho_07.png) 

1. Clique em **Web site**e, em seguida, clique em **início de sessão único (SAML2)**.
   
    ![Configurar o início de sessão único](./media/userecho-tutorial/tutorial_userecho_08.png) 

1. Sobre o **início de sessão único (SAML)** página, execute os seguintes passos:
   
    ![Configurar o início de sessão único](./media/userecho-tutorial/tutorial_userecho_09.png)
    
    a. Como **habilitados no SAML**, selecione **Sim**.
    
    b. Colar **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure para o **URL de SSO SAML** caixa de texto.
    
    c. Colar **URL de fim de sessão**, que copiou do portal do Azure para o **URL remoto logoout** caixa de texto.
    
    d. Abra o seu certificado transferido no bloco de notas, copiar o conteúdo e, em seguida, cole-o para o **certificado X.509** caixa de texto.
    
    e. Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/userecho-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/userecho-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/userecho-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/userecho-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-userecho-test-user"></a>Criar um utilizador de teste UserEcho

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no UserEcho.

**Para criar um usuário chamado Eduarda Almeida no UserEcho, execute os seguintes passos:**

1. Início de sessão no site da sua empresa UserEcho como administrador.

1. Na barra de ferramentas na parte superior, clique em seu nome de utilizador para expandir o menu e, em seguida, clique em **configuração**.
   
    ![Configurar o início de sessão único](./media/userecho-tutorial/tutorial_userecho_06.png)

1. Clique em **usuários**, para expandir a **utilizadores** secção.
   
    ![Configurar o início de sessão único](./media/userecho-tutorial/tutorial_userecho_10.png)

1. Clique em **Utilizadores**.
   
    ![Configurar o início de sessão único](./media/userecho-tutorial/tutorial_userecho_11.png)

1. Clique em **convidar um utilizador novo**.
   
    ![Configurar o início de sessão único](./media/userecho-tutorial/tutorial_userecho_12.png)

1. Sobre o **convidar um utilizador novo** caixa de diálogo, execute os seguintes passos:
   
    ![Configurar o início de sessão único](./media/userecho-tutorial/tutorial_userecho_13.png)

    a. Na **nome** caixa de texto, nome do tipo de utilizador, como a Eduarda Almeida.
    
    b.  Na **E-Mail** caixa de texto, como o tipo de endereço de e-mail do utilizador Brittasimon@contoso.com.
    
    c. Clique em **convidar**.

É enviado um convite para Eduarda, que permite-lhe começar a utilizar UserEcho. 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para UserEcho.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a UserEcho, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **UserEcho**.

    ![Configurar o início de sessão único](./media/userecho-tutorial/tutorial_userecho_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção consiste em testar a configuração de SSO do Azure AD através do painel de acesso.  

Quando clica no mosaico UserEcho no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo UserEcho.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/userecho-tutorial/tutorial_general_01.png
[2]: ./media/userecho-tutorial/tutorial_general_02.png
[3]: ./media/userecho-tutorial/tutorial_general_03.png
[4]: ./media/userecho-tutorial/tutorial_general_04.png

[100]: ./media/userecho-tutorial/tutorial_general_100.png

[200]: ./media/userecho-tutorial/tutorial_general_200.png
[201]: ./media/userecho-tutorial/tutorial_general_201.png
[202]: ./media/userecho-tutorial/tutorial_general_202.png
[203]: ./media/userecho-tutorial/tutorial_general_203.png

