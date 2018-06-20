---
title: 'Tutorial: Integração do Azure Active Directory com o GitHub | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e do GitHub.
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
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 9263be24f883d8a02735e757f3ac02b54960f61f
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36227399"
---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Tutorial: Integração do Azure Active Directory com o GitHub

Neste tutorial, irá aprender a integrar o GitHub com o Azure Active Directory (Azure AD).

Integrar o GitHub com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao GitHub.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada no GitHub (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o GitHub, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um GitHub-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar GitHub a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-github-from-the-gallery"></a>Adicionar GitHub a partir da Galeria
Para configurar a integração do GitHub com o Azure AD, terá de adicionar GitHub a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar GitHub a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **GitHub**, selecione **GitHub** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![GitHub na lista de resultados](./media/github-tutorial/tutorial_github_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com o GitHub com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no GitHub é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no GitHub tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com o GitHub, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste do GitHub](#create-a-github-test-user)**  - para ter um homólogo de Britta Simon no GitHub que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação do GitHub.

**Para configurar o Azure AD-início de sessão único com o GitHub, execute os seguintes passos:**

1. No portal do Azure, no **GitHub** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/github-tutorial/tutorial_github_samlbase.png)

3. No **domínio GitHub e URLs** secção, execute os seguintes passos:

    ![Domínio de GitHub e os URLs únicos de informações de início de sessão](./media/github-tutorial/tutorial_github_url.png)

    a. No **iniciar sessão no URL** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://github.com/orgs/<entity-id>/sso`

    b. No **identificador (ID de entidade)** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://github.com/orgs/<entity-id>`

    > [!NOTE]
    > Tenha em atenção que estas não são os valores reais. Tem de atualizar estes valores com o início de sessão real no URL e o identificador. Aqui sugerimos que utilize o valor de cadeia exclusivo no identificador de. Aceda à secção de administração do GitHub para obter estes valores.

4. No **atributos de utilizador** secção, selecione **identificador de utilizador** como user.mail.

    ![Configurar o início de sessão único](./media/github-tutorial/tutorial_github_attribute_new01.png)

5. No **certificado de assinatura de SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/github-tutorial/tutorial_github_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/github-tutorial/tutorial_general_400.png)

7. No **configuração de GitHub** secção, clique em **configurar GitHub** para abrir **configurar início de sessão** janela. Copiar o **Sign-Out URL, o ID de entidade de SAML e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configuração de GitHub](./media/github-tutorial/tutorial_github_configure.png) 

8. Numa janela do browser web diferente, inicie sessão no seu site do GitHub organização como um administrador.

9. Navegue para **definições** e clique em **segurança**

    ![Definições](./media/github-tutorial/tutorial_github_config_github_03.png)

10. Verifique o **autenticação ativar SAML** caixa, revelando os campos de configuração-início de sessão único. Em seguida, utilize o início de sessão URL valor único para atualizar o URL Single sign-on na configuração do Azure AD.

    ![Definições](./media/github-tutorial/tutorial_github_config_github_13.png)

11. Configure os seguintes campos:

    a. No **iniciar sessão no URL** caixa de texto, colar **SAML URL Single sign-on serviço** valor que copiou do portal do Azure.

    b. No **emissor** caixa de texto, colar **ID de entidade de SAML** valor que copiou do portal do Azure.

    c. Abra o certificado transferido a partir do portal do Azure no bloco de notas, cole o conteúdo para o **certificado público** caixa de texto.

    ![Definições](./media/github-tutorial/tutorial_github_config_github_051.png)

12. Clique em **configuração SAML do teste** para confirmar que não existem falhas de validação ou erros durante a SSO.

    ![Definições](./media/github-tutorial/tutorial_github_config_github_06.png)

13. Clicar em **Guardar**

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/github-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/github-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/github-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/github-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-github-test-user"></a>Criar um utilizador de teste do GitHub

O objetivo desta secção consiste em criar um utilizador chamado Britta Simon no GitHub. GitHub suporta o aprovisionamento de utilizadores automática, que é por predefinição ativada. Pode encontrar mais detalhes [aqui](github-provisioning-tutorial.md) sobre como configurar o aprovisionamento de utilizadores automática.

**Se precisar de criar manualmente o utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa GitHub como administrador.

2. Clique em **pessoas**.

    ![As pessoas](./media/github-tutorial/tutorial_github_config_github_08.png "pessoas")

3. Clique em **convite membro**.

    ![Convidar utilizadores](./media/github-tutorial/tutorial_github_config_github_09.png "convidar utilizadores")

4. No **convite membro** diálogo página, execute os seguintes passos:

    a. No **E-Mail** caixa de texto, escreva o endereço de correio eletrónico da conta de Britta Simon.

    ![Convidar pessoas](./media/github-tutorial/tutorial_github_config_github_10.png "convidar pessoas")

    b. Clique em **enviar o convite**.

    ![Convidar pessoas](./media/github-tutorial/tutorial_github_config_github_11.png "convidar pessoas")

    > [!NOTE]
    > O marcador de posição de conta do Azure Active Directory irá receber um e-mail e siga uma ligação para confirmar a respetiva conta para ficar ativa.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar Azure-início de sessão único, concedendo acesso no GitHub.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon ao GitHub, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201]

2. Na lista de aplicações, selecione **GitHub**.

    ![A ligação do GitHub na lista de aplicações](./media/github-tutorial/tutorial_github_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico do GitHub no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação do GitHub.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



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

