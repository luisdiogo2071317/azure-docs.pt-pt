---
title: 'Tutorial: Integração do Active Directory do Azure com Vidyard | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Vidyard.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bed7df23-6e13-4e7c-b4cc-53ed4804664d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2018
ms.author: jeedes
ms.openlocfilehash: 683cdc43c8d671c14142d0770cd2594ad862b7ac
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55196247"
---
# <a name="tutorial-azure-active-directory-integration-with-vidyard"></a>Tutorial: Integração do Active Directory do Azure com Vidyard

Neste tutorial, saiba como integrar Vidyard com o Azure Active Directory (Azure AD).

Integrar Vidyard no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Vidyard.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Vidyard (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Vidyard, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Vidyard logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Vidyard da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-vidyard-from-the-gallery"></a>Adicionando Vidyard da Galeria
Para configurar a integração do Vidyard com o Azure AD, terá de adicionar Vidyard a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Vidyard a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Vidyard**, selecione **Vidyard** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Vidyard na lista de resultados](./media/vidyard-tutorial/tutorial_vidyard_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Vidyard com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Vidyard a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Vidyard deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Vidyard, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Vidyard](#create-a-vidyard-test-user)**  - para ter um equivalente da Eduarda Almeida na Vidyard que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Vidyard.

**Para configurar o Azure AD início de sessão único com Vidyard, execute os seguintes passos:**

1. No portal do Azure, sobre o **Vidyard** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/vidyard-tutorial/tutorial_vidyard_samlbase.png)

1. Sobre o **Vidyard domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Vidyard domínio e URLs únicas início de sessão em informações](./media/vidyard-tutorial/tutorial_vidyard_url2.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://secure.vidyard.com/sso/saml/<unique id>/metadata`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://secure.vidyard.com/sso/saml/<unique id>/consume`

1. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Vidyard domínio e URLs únicas início de sessão em informações](./media/vidyard-tutorial/tutorial_vidyard_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://secure.vidyard.com/sso/saml/<unique id>/login`

    > [!NOTE]
    > Estes valores não são reais. Atualizar estes valores com o identificador de real, a URL de resposta e o início de sessão no URL, que é explicado mais tarde no tutorial

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/vidyard-tutorial/tutorial_vidyard_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/vidyard-tutorial/tutorial_general_400.png)

1. Sobre o **Vidyard configuração** secção, clique em **configurar Vidyard** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de Vidyard](./media/vidyard-tutorial/tutorial_vidyard_configure.png)

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Vidyard Software como um administrador.

1. A partir do Vidyard dashboard, selecione **grupo** > **segurança**

    ![Configuração de Vidyard](./media/vidyard-tutorial/configure1.png)

1. Clique em **novo perfil** separador.

    ![Configuração de Vidyard](./media/vidyard-tutorial/configure2.png)

1. Na **configuração SAML do** secção, execute os seguintes passos:

    ![Configuração de Vidyard](./media/vidyard-tutorial/configure3.png)

    a. Introduza o nome do perfil geral na **nome do perfil** caixa de texto.

    b. Cópia **página de início de sessão de utilizador de SSO** valor e cole-a na **iniciar sessão no URL** caixa de texto no **secção Vidyard domínio e URLs** no portal do Azure.

    c. Cópia **URL de ACS** valor e cole-a na **URL de resposta** caixa de texto no **secção Vidyard domínio e URLs** no portal do Azure.

    d. Cópia **URL de emissor/metadados** valor e cole-a na **identificador** caixa de texto no **secção Vidyard domínio e URLs** no portal do Azure.

    e. Abra o ficheiro de certificado transferido a partir do portal do Azure no bloco de notas e, em seguida, cole-o para o **certificado X.509** caixa de texto.

    f. Na **URL de ponto final de SAML** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** copiados a partir do portal do Azure.

    g. Clique em **Confirmar**.

1. No separador de início de sessão único, selecione **atribuir** junto a um perfil existente

    ![Configuração de Vidyard](./media/vidyard-tutorial/configure4.png)

    > [!NOTE]
    > Depois de criar um perfil SSO, atribuí-la para qualquer grupo (s) para o qual os utilizadores precisarão de acesso através do Azure. Se o utilizador não existir dentro do grupo ao qual foram atribuídos, Vidyard irá automaticamente criar uma conta de utilizador e atribuir a respetiva função em tempo real.

1. Selecione o seu grupo de organização, o que é visível no **grupos disponíveis para atribuir**.

    ![Configuração de Vidyard](./media/vidyard-tutorial/configure5.png)

1. Pode ver os grupos atribuídos sob o **grupos atribuídos**. Selecione uma função para o grupo de acordo com a sua organização e clique em **confirmar**.

    ![Configuração de Vidyard](./media/vidyard-tutorial/configure6.png)

    > [!NOTE]
    > Para obter mais informações, consulte [este documento](https://knowledge.vidyard.com/saml-single-sign-on-authentication/saml-based-single-sign-on-sso-in-vidyard).

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/vidyard-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/vidyard-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/vidyard-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/vidyard-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-vidyard-test-user"></a>Criar um utilizador de teste Vidyard

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Vidyard. Vidyard suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder Vidyard se não existir ainda.
>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de Vidyard](mailto:support@vidyard.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Vidyard.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Vidyard, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Vidyard**.

    ![A ligação de Vidyard na lista de aplicações](./media/vidyard-tutorial/tutorial_vidyard_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Vidyard no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Vidyard.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/vidyard-tutorial/tutorial_general_01.png
[2]: ./media/vidyard-tutorial/tutorial_general_02.png
[3]: ./media/vidyard-tutorial/tutorial_general_03.png
[4]: ./media/vidyard-tutorial/tutorial_general_04.png

[100]: ./media/vidyard-tutorial/tutorial_general_100.png

[200]: ./media/vidyard-tutorial/tutorial_general_200.png
[201]: ./media/vidyard-tutorial/tutorial_general_201.png
[202]: ./media/vidyard-tutorial/tutorial_general_202.png
[203]: ./media/vidyard-tutorial/tutorial_general_203.png

