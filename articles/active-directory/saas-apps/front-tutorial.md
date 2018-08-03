---
title: 'Tutorial: Integração do Azure Active Directory com o início | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e frente.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 88270b6d-2571-434a-b139-b6ccc3a2b19f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: jeedes
ms.openlocfilehash: d0bdf3ff282152f92e1b661bf19768489d1a029b
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438618"
---
# <a name="tutorial-azure-active-directory-integration-with-front"></a>Tutorial: Integração do Azure Active Directory com frente

Neste tutorial, saiba como integrar frente com o Azure Active Directory (Azure AD).

Integração de frente com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso para a frente.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para a frente (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Front, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma parte frontal logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando frente da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-front-from-the-gallery"></a>Adicionando frente da Galeria
Para configurar a integração de frente com o Azure AD, terá de adicionar Front a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Front a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Front**, selecione **front-** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Frente na lista de resultados](./media/front-tutorial/tutorial_front_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com frente com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente à frente a um utilizador no Azure AD. Em outras palavras, uma relação entre um utilizador do Azure AD e o utilizador relacionado à frente de ligação tem de ser estabelecida.

Atribuir à frente, o valor do **nome de utilizador** no Azure AD como o valor da **nome de utilizador** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Front, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de frente](#create-a-front-test-user)**  - para ter um equivalente da Eduarda Almeida na frente que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação de Front.

**Para configurar o Azure AD início de sessão único com Front, execute os seguintes passos:**

1. No portal do Azure, sobre o **front-** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/front-tutorial/tutorial_front_samlbase.png)

1. Sobre o **front-domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/front-tutorial/tutorial_front_url1.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.frontapp.com`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.frontapp.com/sso/saml/callback`
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o real identificador e o URL de resposta que são explicadas mais tarde no tutorial ou contacte [equipa de suporte de cliente front-](mailto:support@frontapp.com) obter esses valores. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/front-tutorial/tutorial_front_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/front-tutorial/tutorial_general_400.png)
    
1. Sobre o **configuração de front-** secção, clique em **configurar front-** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/front-tutorial/tutorial_front_configure.png) 

1. Início de sessão no seu inquilino de frente como administrador.

1. Aceda a **(ícone de engrenagem na parte inferior da barra lateral esquerda) de definições > Preferências**.
   
    ![Configure o lado de início de sessão na aplicação única](./media/front-tutorial/tutorial_front_000.png)

1. Clique em **início de sessão único** ligação.
   
    ![Configure o lado de início de sessão na aplicação única](./media/front-tutorial/tutorial_front_001.png)

1. Selecione **SAML** na lista pendente de **início de sessão único**.
   
    ![Configure o lado de início de sessão na aplicação única](./media/front-tutorial/tutorial_front_002.png)

1. Na **ponto de entrada** caixa de texto coloca o valor de **único URL de início de sessão no serviço** do Assistente de configuração de aplicações do Azure AD.
    
    ![Configure o lado de início de sessão na aplicação única](./media/front-tutorial/tutorial_front_003.png)

1. Abra sua transferido **Certificate(Base64)** do ficheiro no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado de assinatura** caixa de texto.
    
    ![Configure o lado de início de sessão na aplicação única](./media/front-tutorial/tutorial_front_004.png)

1. Sobre o **definições do fornecedor do serviço** secção, execute os seguintes passos:

    ![Configure o lado de início de sessão na aplicação única](./media/front-tutorial/tutorial_front_005.png)

    a. Copie o valor da **ID de entidade** e cole-o no **identificador** caixa de texto no **front-domínio e URLs** secção no portal do Azure.

    b. Copie o valor da **URL de ACS** e cole-o no **URL de resposta** caixa de texto no **front-domínio e URLs** secção no portal do Azure.
    
1. Clique em **guardar** botão.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/front-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/front-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/front-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/front-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-front-test-user"></a>Criar um utilizador de teste de frente

Nesta secção, vai criar um usuário chamado Eduarda Almeida na frente. Trabalhar com [equipa de suporte de cliente front-](mailto:support@frontapp.com) para adicionar os utilizadores na plataforma da frente. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para a frente.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida para a frente, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **front-**.

    ![A ligação de frente na lista de aplicações](./media/front-tutorial/tutorial_front_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

O objetivo desta secção consiste em testar sua SSOconfiguration de AD do Azure com o painel de acesso.

Quando clica no mosaico de frente no painel de acesso, deve obter automaticamente com sessão iniciada para seu aplicativo de frente. 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/front-tutorial/tutorial_general_01.png
[2]: ./media/front-tutorial/tutorial_general_02.png
[3]: ./media/front-tutorial/tutorial_general_03.png
[4]: ./media/front-tutorial/tutorial_general_04.png

[100]: ./media/front-tutorial/tutorial_general_100.png

[200]: ./media/front-tutorial/tutorial_general_200.png
[201]: ./media/front-tutorial/tutorial_general_201.png
[202]: ./media/front-tutorial/tutorial_general_202.png
[203]: ./media/front-tutorial/tutorial_general_203.png

