---
title: 'Tutorial: Integração do Active Directory do Azure com o BGS Online | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e BGS Online.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 4fd6b29b-1b46-4fd1-9f5e-16b1c9d892cd
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 043226f333649132cb33a64609a68a705851068e
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173433"
---
# <a name="tutorial-azure-active-directory-integration-with-bgs-online"></a>Tutorial: Integração do Active Directory do Azure com o BGS Online

Neste tutorial, saiba como integrar BGS Online com o Azure Active Directory (Azure AD).

Integrar BGS Online com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao BGS Online
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para BGS Online (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com BGS Online, terá dos seguintes itens:

- Uma subscrição do Azure
- Um BGS Online início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar BGS Online a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-bgs-online-from-the-gallery"></a>Adicionar BGS Online a partir da Galeria
Para configurar a integração do BGS Online com o Azure AD, terá de adicionar BGS Online na Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar BGS Online a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **BGS Online**.

    ![Criar um utilizador de teste do Azure AD](./media/bgsonline-tutorial/tutorial_bgsonline_search.png)

1. No painel de resultados, selecione **BGS Online**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/bgsonline-tutorial/tutorial_bgsonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com BGS Online com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no BGS Online a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no BGS Online tem de ser estabelecida.

No BGS Online, atribuir o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com BGS Online, terá de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Online de BGS](#creating-a-bgs-online-test-user)**  - para ter um equivalente da Eduarda Almeida BGS online que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo BGS Online.

**Para configurar o Azure AD início de sessão único com BGS Online, execute os seguintes passos:**

1. No portal do Azure, sobre o **BGS Online** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/bgsonline-tutorial/tutorial_bgsonline_samlbase.png)

1. Sobre o **BGS de domínio Online e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/bgsonline-tutorial/tutorial_bgsonline_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão:

    Para o ambiente de produção, utilize este padrão `https://<company name>.millwardbrown.report` 

    Para o ambiente de teste, utilize este padrão `https://millwardbrown.marketingtracker.nl/mt5/`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão:
    
    Para o ambiente de produção, utilize este padrão `https://<company name>.millwardbrown.report/sso/saml/AssertionConsumerService.aspx` 
      
    Para o ambiente de teste, utilize este padrão `https://millwardbrown.marketingtracker.nl/mt5/sso/saml/AssertionConsumerService.aspx`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador real e o URL de resposta. Contacte [equipa de Suporte Online de BGS](mailTo:bgsdashboardteam@millwardbrown.com) obter esses valores.
 

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/bgsonline-tutorial/tutorial_bgsonline_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/bgsonline-tutorial/tutorial_general_400.png)

1. Sobre o **Configuração Online de BGS** secção, clique em **configurar BGS Online** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/bgsonline-tutorial/tutorial_bgsonline_configure.png) 

1. Para configurar o início de sessão único num **BGS Online** lado, terá de enviar o transferido **XML de metadados** e **SAML único início de sessão no URL do serviço** para [BGS Online equipa de suporte](mailto:bgsdashboardteam@millwardbrown.com). 


> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/bgsonline-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/bgsonline-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/bgsonline-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/bgsonline-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-bgs-online-test-user"></a>Criar um utilizador de teste BGS Online

Nesta secção, vai criar um usuário chamado Eduarda Almeida no BGS Online. Trabalhar com [equipa de Suporte Online de BGS](mailto:bgsdashboardteam@millwardbrown.com) para adicionar os utilizadores na plataforma BGS Online.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao BGS Online.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a BGS Online, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **BGS Online**.

    ![Configurar o início de sessão único](./media/bgsonline-tutorial/tutorial_bgsonline_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar suas configurações de SSO do Azure AD utilizando o painel de acesso.

Quando clica no mosaico BGS Online no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo BGS Online.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/bgsonline-tutorial/tutorial_general_01.png
[2]: ./media/bgsonline-tutorial/tutorial_general_02.png
[3]: ./media/bgsonline-tutorial/tutorial_general_03.png
[4]: ./media/bgsonline-tutorial/tutorial_general_04.png

[100]: ./media/bgsonline-tutorial/tutorial_general_100.png

[200]: ./media/bgsonline-tutorial/tutorial_general_200.png
[201]: ./media/bgsonline-tutorial/tutorial_general_201.png
[202]: ./media/bgsonline-tutorial/tutorial_general_202.png
[203]: ./media/bgsonline-tutorial/tutorial_general_203.png

