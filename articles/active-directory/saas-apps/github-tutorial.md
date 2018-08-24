---
title: 'Tutorial: Integração do Azure Active Directory com o GitHub | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e do GitHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8761f5ca-c57c-4a7e-bf14-ac0421bd3b5e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2018
ms.author: jeedes
ms.openlocfilehash: b2a90a4599e5d07baba721d5649b72422dc5cb4d
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818750"
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Tutorial: Integração do Azure Active Directory com o GitHub

Neste tutorial, saiba como integrar o GitHub com o Azure Active Directory (Azure AD).

Integração do GitHub com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao GitHub.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o GitHub (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o GitHub, terá dos seguintes itens:

- Uma subscrição do Azure
- Um GitHub logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar GitHub a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-github-from-the-gallery"></a>Adicionar GitHub a partir da Galeria

Para configurar a integração do GitHub para o Azure AD, terá de adicionar GitHub a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar GitHub a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **GitHub**, selecione **GitHub** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![GitHub na lista de resultados](./media/github-tutorial/tutorial_github_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o GitHub com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no GitHub para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no GitHub tem de ser estabelecida.

Para configurar e testar o Azure AD início de sessão único com o GitHub, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste do GitHub](#create-a-github-test-user)**  - para ter um equivalente da Eduarda Almeida no GitHub que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do GitHub.

**Para configurar o Azure AD início de sessão único com o GitHub, execute os seguintes passos:**

1. No portal do Azure, sobre o **GitHub** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/github-tutorial/tutorial_github_samlbase.png)

3. Sobre o **GitHub domínio e URLs** secção, execute os seguintes passos:

    ![URLs de domínio do GitHub e únicas início de sessão em informações](./media/github-tutorial/tutorial_github_url.png)

    a. Na **iniciar sessão no URL** caixa de texto, escreva um URL com o seguinte padrão: `https://github.com/orgs/<entity-id>/sso`

    b. Na **identificador (ID de entidade)** caixa de texto, escreva um URL com o seguinte padrão: `https://github.com/orgs/<entity-id>`

    > [!NOTE]
    > Tenha em atenção que estes não são os valores reais. Terá de atualizar estes valores com o início de sessão real URL e o identificador. Aqui iremos sugerir-lhe utilizar o valor único de cadeia de caracteres no identificador de. Aceda à secção de administrador do GitHub para obter estes valores.

4. Sobre o **atributos de utilizador** secção, selecione **identificador de utilizador** como user.mail.

    ![Configurar o início de sessão único](./media/github-tutorial/tutorial_github_attribute_new01.png)

5. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/github-tutorial/tutorial_github_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/github-tutorial/tutorial_general_400.png)

7. Sobre o **configuração do GitHub** secção, clique em **configurar o GitHub** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração do GitHub](./media/github-tutorial/tutorial_github_configure.png) 

8. Numa janela do browser web diferente, inicie sessão no seu site de organização do GitHub como um administrador.

9. Navegue para **configurações** e clique em **segurança**

    ![Definições](./media/github-tutorial/tutorial_github_config_github_03.png)

10. Verifique os **autenticação SAML ativar** caixa, revelando os campos de configuração início de sessão único. Em seguida, utilize o início de sessão no URL valor único para atualizar o URL único início de sessão na configuração do Azure AD.

    ![Definições](./media/github-tutorial/tutorial_github_config_github_13.png)

11. Configure os seguintes campos:

    a. Na **iniciar sessão no URL** caixa de texto, colar **SAML único URL de início de sessão no serviço** valor que copiou do portal do Azure.

    b. Na **emissor** caixa de texto, colar **ID de entidade de SAML** valor que copiou do portal do Azure.

    c. Abra o certificado transferido a partir do portal do Azure no bloco de notas, cole o conteúdo do **certificado público** caixa de texto.

    ![Definições](./media/github-tutorial/tutorial_github_config_github_051.png)

12. Clique em **configuração SAML do teste** para confirmar que não existem falhas de validação ou erros durante a SSO.

    ![Definições](./media/github-tutorial/tutorial_github_config_github_06.png)

13. Clicar em **Guardar**

> [!NOTE]
> Início de sessão único no GitHub efetua a autenticação para uma organização específica no GitHub e não substitui a autenticação do GitHub em si. Por conseguinte, se a sessão do utilizador GitHub.com tiver expirado, poderá ser-lhe pedido para autenticar com o ID de/palavra-passe do GitHub durante o processo de início de sessão único.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/github-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/github-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/github-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/github-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-github-test-user"></a>Criar um utilizador de teste do GitHub

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no GitHub. O GitHub suporta o aprovisionamento automático de utilizadores, que está por predefinição, ativada. Pode encontrar mais detalhes [aqui](github-provisioning-tutorial.md) sobre como configurar o aprovisionamento automático de utilizadores.

**Se precisar de criar manualmente o utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa GitHub como administrador.

2. Clique em **pessoas**.

    ![As pessoas](./media/github-tutorial/tutorial_github_config_github_08.png "pessoas")

3. Clique em **membro de convite**.

    ![Convidar utilizadores](./media/github-tutorial/tutorial_github_config_github_09.png "convidar utilizadores")

4. Sobre o **membro de convite** caixa de diálogo página, execute os seguintes passos:

    a. Na **E-Mail** caixa de texto, escreva o endereço de e-mail da conta da Eduarda Almeida.

    ![Convidar pessoas](./media/github-tutorial/tutorial_github_config_github_10.png "convidar pessoas")

    b. Clique em **Enviar convite**.

    ![Convidar pessoas](./media/github-tutorial/tutorial_github_config_github_11.png "convidar pessoas")

    > [!NOTE]
    > O titular da conta do Azure Active Directory irá receber um e-mail e seguir uma ligação para confirmar a respetiva conta até se tornar Active Directory.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso ao GitHub.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida para o GitHub, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **GitHub**.

    ![A ligação do GitHub na lista de aplicações](./media/github-tutorial/tutorial_github_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do GitHub no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do GitHub.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/github-tutorial/tutorial_general_01.png
[2]: ./media/github-tutorial/tutorial_general_02.png
[3]: ./media/github-tutorial/tutorial_general_03.png
[4]: ./media/github-tutorial/tutorial_general_04.png

[100]: ./media/github-tutorial/tutorial_general_100.png

[200]: ./media/github-tutorial/tutorial_general_200.png
[201]: ./media/github-tutorial/tutorial_general_201.png
[202]: ./media/github-tutorial/tutorial_general_202.png
[203]: ./media/github-tutorial/tutorial_general_203.png