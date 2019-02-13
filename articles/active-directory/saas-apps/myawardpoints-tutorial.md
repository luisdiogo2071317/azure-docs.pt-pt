---
title: 'Tutorial: Integração do Active Directory do Azure com meus pontos principais Sub/superior a equipa do prémio | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e meus pontos principais Sub/superior a equipa do prémio.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: a7a08eed-7a6b-4a83-8f8e-0add6d2fb8cf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 19ed1c12768eed0265f65388984a5859b8f0630b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56210735"
---
# <a name="tutorial-azure-active-directory-integration-with-my-award-points-top-subtop-team"></a>Tutorial: Integração do Active Directory do Azure com meus pontos principais Sub/superior a equipa do prémio

Neste tutorial, saiba como integrar os meus pontos principais Sub/superior a equipa do prémio com o Azure Active Directory (Azure AD).

Meus pontos principais Sub/superior a equipa do prémio a integração com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao meu pontos principais Sub/superior a equipa do prémio.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para meus pontos superior Sub/superior a equipa do prémio (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com meus pontos principais Sub/superior a equipa do prémio, terá dos seguintes itens:

- Uma subscrição do Azure
- Um meus pontos principais Sub/superior a equipa do prémio logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando My pontos principais Sub/superior a equipa do prémio da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-my-award-points-top-subtop-team-from-the-gallery"></a>Adicionando My pontos principais Sub/superior a equipa do prémio da Galeria

Para configurar a integração de meus pontos principais Sub/superior a equipa do prémio para o Azure AD, terá de adicionar meus pontos principais Sub/superior a equipa do prémio a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar os meus pontos principais Sub/superior a equipa do prémio da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **meus pontos principais Sub/superior a equipa do prémio**, selecione **meus pontos principais Sub/superior a equipa do prémio** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Minha equipe do prémio pontos principais Sub/superior na lista de resultados](./media/myawardpoints-tutorial/tutorial_myawardpoints_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com meus pontos principais Sub/superior a equipa do prémio com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de equivalente no meu pontos principais Sub/superior a equipa do prémio para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado em meus pontos principais Sub/superior a equipa do prémio deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com meus pontos principais Sub/superior a equipa do prémio, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de meus pontos principais Sub/superior a equipa do prémio](#create-a-my-award-points-top-subtop-team-test-user)**  - para ter um equivalente da Eduarda Almeida em meus pontos principais Sub/superior a equipa do prémio que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de meus pontos principais Sub/superior a equipa do prémio.

**Para configurar o Azure AD início de sessão único com meus pontos principais Sub/superior a equipa do prémio, execute os seguintes passos:**

1. No portal do Azure, sobre o **meus pontos principais Sub/superior a equipa do prémio** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/myawardpoints-tutorial/tutorial_myawardpoints_samlbase.png)

3. Sobre o **My prémio pontos superior Sub/superior equipe domínio e URLs** secção, execute os seguintes passos:

    ![Minhas prémio pontos de início superior/Sub equipe domínio e URLs únicas início de sessão em informações](./media/myawardpoints-tutorial/tutorial_myawardpoints_url.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://microsoftrr.performnet.com/biwv1auth/Shibboleth.sso/Login?providerId=<SAMLENTITYID>`

    > [!NOTE]
    > Obterá o `<SAMLENTITYID>` valor nos passos mais adiante neste tutorial.

4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/myawardpoints-tutorial/tutorial_myawardpoints_certificate.png)

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/myawardpoints-tutorial/tutorial_general_400.png)

6. Na **My prémio pontos superior Sub/superior equipe configuração** secção, selecione **configurar meus pontos principais Sub/superior a equipa do prémio** para abrir a janela de início de sessão em configurar. Copie o ID de entidade de SAML do **referência rápida** secção e acrescente o valor de ID de entidade de SAML com a URL em vez de início de sessão `<SAMLENTITYID>` no **My prémio pontos superior Sub/superior equipe domínio e URLs** secção no portal do Azure.

7. Para configurar o início de sessão único num **meus pontos principais Sub/superior a equipa do prémio** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte de meus pontos principais Sub/superior a equipa do prémio](mailto:myawardpoints@biworldwide.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/myawardpoints-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/myawardpoints-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/myawardpoints-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/myawardpoints-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-my-award-points-top-subtop-team-test-user"></a>Criar um utilizador de teste de meus pontos principais Sub/superior a equipa do prémio

Nesta secção, vai criar um usuário chamado Eduarda Almeida no meus pontos principais Sub/superior a equipa do prémio. Trabalhar com [equipa de suporte de meus pontos principais Sub/superior a equipa do prémio](mailto:myawardpoints@biworldwide.com) para adicionar os utilizadores na plataforma do meus pontos principais Sub/superior a equipa do prémio. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso a meus pontos principais Sub/superior a equipa do prémio.

![Atribuir a função de utilizador][200]

**Para atribuir a Eduarda Almeida aos meus pontos principais Sub/superior a equipa do prémio, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **meus pontos principais Sub/superior a equipa do prémio**.

    ![A ligação de meus pontos principais Sub/superior a equipa do prémio na lista de aplicações](./media/myawardpoints-tutorial/tutorial_myawardpoints_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de meus pontos principais Sub/superior a equipa do prémio no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo de meus pontos principais Sub/superior a equipa do prémio.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/myawardpoints-tutorial/tutorial_general_01.png
[2]: ./media/myawardpoints-tutorial/tutorial_general_02.png
[3]: ./media/myawardpoints-tutorial/tutorial_general_03.png
[4]: ./media/myawardpoints-tutorial/tutorial_general_04.png

[100]: ./media/myawardpoints-tutorial/tutorial_general_100.png

[200]: ./media/myawardpoints-tutorial/tutorial_general_200.png
[201]: ./media/myawardpoints-tutorial/tutorial_general_201.png
[202]: ./media/myawardpoints-tutorial/tutorial_general_202.png
[203]: ./media/myawardpoints-tutorial/tutorial_general_203.png
