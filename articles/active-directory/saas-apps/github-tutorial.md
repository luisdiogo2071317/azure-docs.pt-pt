---
title: 'Tutorial: Integração do Active Directory do Azure com o GitHub | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e do GitHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8761f5ca-c57c-4a7e-bf14-ac0421bd3b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: jeedes
ms.openlocfilehash: 41e8fb40f07a88cb6fa2108a38db7f973fccd2f9
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53607803"
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Tutorial: Integração do Active Directory do Azure com o GitHub

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
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar GitHub a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-github-from-the-gallery"></a>Adicionar GitHub a partir da Galeria
Para configurar a integração do GitHub para o Azure AD, terá de adicionar GitHub a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar GitHub a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![image](./media/github-tutorial/selectazuread.png)

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![image](./media/github-tutorial/a_select_app.png)

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![image](./media/github-tutorial/a_new_app.png)

4. Na caixa de pesquisa, escreva **GitHub**, selecione **GitHub** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![image](./media/github-tutorial/tutorial_github_addfromgallery.png)

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

1. Na [portal do Azure](https://portal.azure.com/), na **GitHub** página de integração de aplicações, selecione **início de sessão único**.

    ![image](./media/github-tutorial/b1_b2_select_sso.png)

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, clique em **selecione** para **SAML** modo para ativar o início de sessão único.

    ![image](./media/github-tutorial/b1_b2_saml_sso.png)

3. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **configuração básica de SAML** caixa de diálogo.

    ![image](./media/github-tutorial/b1-domains_and_urlsedit.png)

4. Sobre o **configuração básica de SAML** secção, execute os seguintes passos:

    ![image](./media/github-tutorial/tutorial_github_url.png)

    a. Na **iniciar sessão no URL** caixa de texto, escreva um URL com o seguinte padrão: `https://github.com/orgs/<entity-id>/sso`

    b. Na **identificador (ID de entidade)** caixa de texto, escreva um URL com o seguinte padrão: `https://github.com/orgs/<entity-id>`

    > [!NOTE]
    > Tenha em atenção que estes não são os valores reais. Terá de atualizar estes valores com o início de sessão real URL e o identificador. Aqui iremos sugerir-lhe utilizar o valor único de cadeia de caracteres no identificador de. Aceda à secção de administrador do GitHub para obter estes valores.

5. Aplicações GitHub espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. Clique em **edite** botão para abrir **atributos de utilizador** caixa de diálogo.

    ![image](./media/github-tutorial/i3-attribute.png)

6. No **afirmações de utilizador** secção sobre o **atributos de utilizador** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:

    a. Clique em **edite** botão para abrir o **afirmações de utilizador de gerir** caixa de diálogo.

    ![image](./media/github-tutorial/i2-attribute.png)

    ![image](./media/github-tutorial/i4-attribute.png)

    b. Partir do **atributo de origem** , selecione o valor do atributo.

    c. Clique em **Guardar**.

7. Na **certificado de assinatura SAML** secção, clique em **transferir** para transferir o **certificado (Base64)** e guarde-o no seu computador.

    ![image](./media/github-tutorial/tutorial_github_certficate.png)

8. Sobre o **configurar o GitHub** secção, copie o URL adequado, de acordo com seus requisitos.

    a. URL de início de sessão

    b. Identificador do Azure AD

    c. URL de fim de sessão

    ![image](./media/github-tutorial/d1_samlsonfigure.png)

9. Numa janela do browser web diferente, inicie sessão no seu site de organização do GitHub como um administrador.

10. Navegue para **configurações** e clique em **segurança**

    ![Definições](./media/github-tutorial/tutorial_github_config_github_03.png)

11. Verifique os **autenticação SAML ativar** caixa, revelando os campos de configuração início de sessão único. Em seguida, utilize o início de sessão no URL valor único para atualizar o URL único início de sessão na configuração do Azure AD.

    ![Definições](./media/github-tutorial/tutorial_github_config_github_13.png)

12. Configure os seguintes campos:

    ![Definições](./media/github-tutorial/tutorial_github_config_github_051.png)

    a. Na **iniciar sessão no URL** caixa de texto, colar **URL de início de sessão** valor que copiou do portal do Azure.

    b. Na **emissor** caixa de texto, colar **do Azure AD identificador** valor que copiou do portal do Azure.

    c. Abra o certificado transferido a partir do portal do Azure no bloco de notas, cole o conteúdo do **certificado público** caixa de texto.

    d. Clique em **editar** ícone para editar o **método de assinatura** e **Digest método** de **RSA SHA1** e **SHA1**para **RSA-SHA256** e **SHA256** conforme mostrado abaixo.

    ![image](./media/github-tutorial/tutorial_github_sha.png)

13. Clique em **configuração SAML do teste** para confirmar que não existem falhas de validação ou erros durante a SSO.

    ![Definições](./media/github-tutorial/tutorial_github_config_github_06.png)

14. Clicar em **Guardar**

> [!NOTE]
> Início de sessão único no GitHub efetua a autenticação para uma organização específica no GitHub e não substitui a autenticação do GitHub em si. Por conseguinte, se a sessão do utilizador github.com tiver expirado, poderá ser-lhe pedido para autenticar com o ID de/palavra-passe do GitHub durante o processo de início de sessão único.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![image](./media/github-tutorial/d_users_and_groups.png)

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![image](./media/github-tutorial/d_adduser.png)

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![image](./media/github-tutorial/d_userproperties.png)

    a. Na **Name** campo introduza **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.

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

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![image](./media/github-tutorial/d_all_applications.png)

2. Na lista de aplicações, selecione **GitHub**.

    ![image](./media/github-tutorial/tutorial_github_app.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![image](./media/github-tutorial/d_leftpaneusers.png)

4. Selecione o **Add** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![image](./media/github-tutorial/d_assign_user.png)

4. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

5. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.

### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do GitHub no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do GitHub.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)


