---
title: 'Tutorial: Integração do Active Directory do Azure com as pessoas | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e as pessoas.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7c9b6202-11dd-4bb6-a679-8fb0a7a0ef4e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: jeedes
ms.openlocfilehash: c33f7fc23865169d50de73d27a0c45bb8a9aba0a
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55176833"
---
# <a name="tutorial-azure-active-directory-integration-with-people"></a>Tutorial: Integração do Active Directory do Azure com as pessoas

Neste tutorial, saiba como integrar as pessoas com o Azure Active Directory (Azure AD).

As pessoas a integração com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso a pessoas
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada a pessoas (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com as pessoas, precisa do seguinte:

- Uma subscrição do Azure
- Um pessoas logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando as pessoas da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-people-from-the-gallery"></a>Adicionando as pessoas da Galeria
Para configurar a integração de pessoas para o Azure AD, terá de adicionar as pessoas a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar as pessoas a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **pessoas**.

    ![Criar um utilizador de teste do Azure AD](./media/people-tutorial/tutorial_people_search.png)

1. No painel de resultados, selecione **pessoas**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/people-tutorial/tutorial_people_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com as pessoas com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte em pessoas para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado em pessoas deve ser estabelecido.

Em pessoas, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com pessoas, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de pessoas](#creating-a-people-test-user)**  - para ter um equivalente da Eduarda Almeida em pessoas que estão ligadas à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de pessoas.

**Para configurar o Azure AD início de sessão único com pessoas, execute os seguintes passos:**

1. No portal do Azure, sobre o **pessoas** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/people-tutorial/tutorial_people_samlbase.png)

1. Sobre o **pessoas domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/people-tutorial/tutorial_people_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão:  `https://<company name>.peoplehr.net`

    b. Na **identificador** caixa de texto, escreva o URL: `https://www.peoplehr.com`

    c. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão:  `https://<company name>.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx`
    
    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o URL de resposta e o URL de início de sessão real. Contacte [equipa de suporte de cliente de pessoas](mailto:customerservices@peoplehr.com) obter esses valores. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/people-tutorial/tutorial_people_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/people-tutorial/tutorial_general_400.png)
    
1. Para obter SSO configurado para a sua aplicação, terá de iniciar sessão com o seu inquilino de pessoas como administrador.
   
1. No menu no lado esquerdo, clique em **definições**.

    ![Configurar o início de sessão único](./media/people-tutorial/tutorial_people_001.png)

1. Clique em **empresa**.

    ![Configurar o início de sessão único](./media/people-tutorial/tutorial_people_002.png)

1. Sobre o **ficheiro de metadados de carregamento de "Início de sessão único" SAML**, clique em **procurar** para carregar o ficheiro de metadados baixado.

    ![Configurar o início de sessão único](./media/people-tutorial/tutorial_people_003.png)

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/people-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/people-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/people-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/people-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-people-test-user"></a>Criar um utilizador de teste de pessoas

Nesta secção, vai criar um usuário chamado Eduarda Almeida em pessoas. Trabalhar com [equipa de suporte de cliente de pessoas](mailto:customerservices@peoplehr.com) para adicionar os utilizadores da plataforma de pessoas. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso às pessoas.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a pessoas, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **pessoas**.

    ![Configurar o início de sessão único](./media/people-tutorial/tutorial_people_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção consiste em testar a configuração de SSO do Azure AD através do painel de acesso.

Quando clica no mosaico de pessoas no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo de pessoas.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/people-tutorial/tutorial_general_01.png
[2]: ./media/people-tutorial/tutorial_general_02.png
[3]: ./media/people-tutorial/tutorial_general_03.png
[4]: ./media/people-tutorial/tutorial_general_04.png

[100]: ./media/people-tutorial/tutorial_general_100.png

[200]: ./media/people-tutorial/tutorial_general_200.png
[201]: ./media/people-tutorial/tutorial_general_201.png
[202]: ./media/people-tutorial/tutorial_general_202.png
[203]: ./media/people-tutorial/tutorial_general_203.png

