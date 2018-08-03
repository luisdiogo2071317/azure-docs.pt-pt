---
title: 'Tutorial: Integração do Azure Active Directory com Heroku | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Heroku.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d7d72ec6-4a60-4524-8634-26d8fbbcc833
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: c78d1207c724622fe16fa8d0a0717d71fbb6d37c
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39443686"
---
# <a name="tutorial-azure-active-directory-integration-with-heroku"></a>Tutorial: Integração do Azure Active Directory com Heroku

Neste tutorial, saiba como integrar Heroku com o Azure Active Directory (Azure AD).

Integrar Heroku no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Heroku
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Heroku (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Heroku, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Heroku logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Heroku da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-heroku-from-the-gallery"></a>Adicionando Heroku da Galeria
Para configurar a integração do Heroku com o Azure AD, terá de adicionar Heroku a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Heroku a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Heroku**.

    ![Criar um utilizador de teste do Azure AD](./media/heroku-tutorial/tutorial_heroku_search.png)

1. No painel de resultados, selecione **Heroku**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/heroku-tutorial/tutorial_heroku_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Heroku com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Heroku a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Heroku deve ser estabelecido.

No Heroku, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Heroku, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Heroku](#creating-a-heroku-test-user)**  - para ter um equivalente da Eduarda Almeida no Heroku que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Heroku.

**Para configurar o Azure AD início de sessão único com Heroku, execute os seguintes passos:**

1. No portal do Azure, sobre o **Heroku** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/heroku-tutorial/tutorial_heroku_samlbase.png)

1. Sobre o **Heroku domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/heroku-tutorial/tutorial_heroku_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão:    
    `https://sso.heroku.com/saml/<company-name>/init`

    b. Na **URL de identificador** caixa de texto, escreva um URL com o seguinte padrão:            
    `https://sso.heroku.com/saml/<company-name>`

    > [!NOTE]
    >Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Obter estes valores da equipa de Heroku, que está descrita nas secções posteriores deste artigo. 
        
1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/heroku-tutorial/tutorial_heroku_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/heroku-tutorial/tutorial_general_400.png)

1. Para ativar o SSO no Heroku, execute os seguintes passos:
   
    a. Inicie sessão na conta Heroku como administrador.

    b. Clique no separador **Definições**.

    c. Sobre o **início de sessão único na página**, clique em **carregar metadados**.

    d. Carregue o ficheiro de metadados, que transferiu do portal do Azure.

    e. Quando a configuração for concluída com êxito, os administradores veem uma caixa de diálogo de confirmação e o URL de início de sessão do SSO para os utilizadores finais é apresentado. 

    f. Cópia a **URL de início de sessão de Heroku** e **ID de entidade Heroku** valores e volte ao **Heroku domínio e URLs** secção no portal do Azure e cole esses valores no  **Url de início de sessão** e **identificador** caixas de texto, respetivamente.

    ![Configurar o início de sessão único](./media/heroku-tutorial/tutorial_heroku_52.png) 
    
1. Clique em **Seguinte**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **aplicações da empresa do Active Directory** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/heroku-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/heroku-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/heroku-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/heroku-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-heroku-test-user"></a>Criar um utilizador de teste Heroku

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Heroku. Heroku suporta o aprovisionamento de just-in-time, que está ativado por predefinição.

Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado ao aceder ao Heroku, se o utilizador ainda não existe. Depois da conta é aprovisionada, o utilizador final recebe um e-mail de verificação e tem de clicar na ligação de confirmação.

>[!NOTE]
>Se precisar de criar manualmente um utilizador, terá de contactar o [equipa de suporte de cliente Heroku](https://www.heroku.com/support).
>  

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Heroku.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Heroku, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Heroku**.

    ![Configurar o início de sessão único](./media/heroku-tutorial/tutorial_heroku_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Heroku no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Heroku.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/heroku-tutorial/tutorial_general_01.png
[2]: ./media/heroku-tutorial/tutorial_general_02.png
[3]: ./media/heroku-tutorial/tutorial_general_03.png
[4]: ./media/heroku-tutorial/tutorial_general_04.png

[100]: ./media/heroku-tutorial/tutorial_general_100.png

[200]: ./media/heroku-tutorial/tutorial_general_200.png
[201]: ./media/heroku-tutorial/tutorial_general_201.png
[202]: ./media/heroku-tutorial/tutorial_general_202.png
[203]: ./media/heroku-tutorial/tutorial_general_203.png
