---
title: 'Tutorial: Integração do Azure Active Directory com os laboratórios Fluxx | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Fluxx laboratórios.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d8fac770-bb57-4e1f-b50b-9ffeae239d07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: 367310527619d4bdb5f84a80c567a9d83698846e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39433777"
---
# <a name="tutorial-azure-active-directory-integration-with-fluxx-labs"></a>Tutorial: Integração do Azure Active Directory com os laboratórios Fluxx

Neste tutorial, saiba como integrar os laboratórios de Fluxx com o Azure Active Directory (Azure AD).

Integrar os laboratórios de Fluxx no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso aos laboratórios Fluxx.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para laboratórios de Fluxx (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Fluxx laboratórios, precisa do seguinte:

- Uma subscrição do Azure AD
- Um Fluxx laboratórios logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Fluxx laboratórios da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-fluxx-labs-from-the-gallery"></a>Adicionando Fluxx laboratórios da Galeria
Para configurar a integração de Fluxx laboratórios com o Azure AD, terá de adicionar Fluxx laboratórios a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Fluxx laboratórios a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Fluxx laboratórios**, selecione **Fluxx laboratórios** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Laboratórios de Fluxx na lista de resultados](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com laboratórios Fluxx com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no Fluxx Labs a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado nos laboratórios de Fluxx deve ser estabelecido.

Nos laboratórios de Fluxx, atribuir o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Fluxx laboratórios, precisa concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de laboratórios de Fluxx](#create-a-fluxx-labs-test-user)**  - para ter um equivalente da Eduarda Almeida nos laboratórios de Fluxx que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Fluxx laboratórios.

**Para configurar o Azure AD início de sessão único com laboratórios Fluxx, execute os seguintes passos:**

1. No portal do Azure, sobre o **Fluxx laboratórios** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_samlbase.png)

1. Sobre o **Fluxx laboratórios de domínio e URLs** secção, execute os seguintes passos:

    ![Fluxx laboratórios de domínio e URLs únicas início de sessão em informações](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão:

    | Ambiente | Padrão de URL|
    |-------------|------------|
    | Produção | `https://<subdomain>.fluxx.io` |
    | Pré-produção | `https://<subdomain>.preprod.fluxxlabs.com`|
        
    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão:

    | Ambiente | Padrão de URL|
    |-------------|------------|
    | Produção | `https://<subdomain>.fluxx.io/auth/saml/callback` |
    | Pré-produção | `https://<subdomain>.preprod.fluxxlabs.com/auth/saml/callback`|

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador real e o URL de resposta. Contacte [equipa de suporte de laboratórios de Fluxx](mailto:travis@fluxxlabs.com) obter esses valores.

1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/fluxxlabs-tutorial/tutorial_general_400.png)

1. Na **Fluxx laboratórios Configuration** secção, clique em **configurar laboratórios de Fluxx** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de laboratórios Fluxx](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_configure.png)

1. Numa janela do browser web diferente, inicie sessão site da sua empresa Fluxx laboratórios como administrador.

1. Selecione **administrador** abaixo a **definições** secção.

    ![Configuração de laboratórios Fluxx](./media/fluxxlabs-tutorial/config1.png)

1. No painel administração, selecione **Plug-ins** > **integrações** e, em seguida, selecione **SSO-(Disabled) de SAML**

    ![Configuração de laboratórios Fluxx](./media/fluxxlabs-tutorial/config2.png)

1. Na secção de atributo, execute os seguintes passos:

    ![Configuração de laboratórios Fluxx](./media/fluxxlabs-tutorial/config3.png)

    a. Selecione o **SAML SSO** caixa de verificação.

    b. Na **caminho do pedido** caixa de texto, tipo **saml/auth/**.

    c. Na **caminho de retorno de chamada** caixa de texto, tipo **/auth/saml/callback**.

    d. Na **asserção consumidor serviço Url(Single Sign-On URL)** caixa de texto, introduza o **URL de resposta** valor, que introduziu no portal do Azure.

    e. Na **público-alvo (ID de entidade de SP)** caixa de texto, introduza o **identificador** valor, que introduziu no portal do Azure.

    f. Na **URL de destino de SSO de fornecedor de identidade** caixa de texto, colar a **SAML único início de sessão no URL do serviço** valor, que copiou do portal do Azure.

    g. Abra o certificado com codificação base 64 no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado do fornecedor de identidade** caixa de texto.

    h. Na **identificador de nome de formato** caixa de texto, introduza o valor `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.

    i. Clique em **Guardar**.

    > [!NOTE]
    > Assim que o conteúdo salvo, o campo será apresentado em branco para segurança, mas o valor foi guardado na configuração.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/fluxxlabs-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/fluxxlabs-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/fluxxlabs-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/fluxxlabs-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-a-fluxx-labs-test-user"></a>Criar um utilizador de teste Fluxx laboratórios

Para ativar a utilizadores do Azure AD iniciar sessão no Fluxx laboratórios, tem de ser aprovisionados como Fluxx laboratórios. No caso de Fluxx laboratórios, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa Fluxx laboratórios como administrador.

1. Clique no apresentado a seguir **ícone**.

    ![Configuração de laboratórios Fluxx](./media/fluxxlabs-tutorial/config6.png)

1. No dashboard, clique no abaixo apresentado ícone para abrir o **novas pessoas** cartão.

    ![Configuração de laboratórios Fluxx](./media/fluxxlabs-tutorial/config4.png)

1. Sobre o **novas pessoas** secção, execute os seguintes passos:

    ![Configuração de laboratórios Fluxx](./media/fluxxlabs-tutorial/config5.png)

    a. Laboratórios de Fluxx utilizam o e-mail como o identificador exclusivo para inícios de sessão do SSO. Preencher o **UID de SSO** campo com o endereço de e-mail do utilizador, que corresponde ao endereço de e-mail, que estão a utilizar como início de sessão com o SSO.

    b. Clique em **Guardar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso aos laboratórios Fluxx.

![Atribuir a função de utilizador][200]

**Para atribuir a Eduarda Almeida a Fluxx laboratórios, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Fluxx laboratórios**.

    ![A ligação de Fluxx laboratórios na lista de aplicações](./media/fluxxlabs-tutorial/tutorial_fluxxlabs_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de laboratórios de Fluxx no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação Fluxx laboratórios.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

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
