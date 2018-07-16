---
title: 'Tutorial: Integração do Azure Active Directory com LaunchDarkly | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e LaunchDarkly.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0e9cb37e-16bf-493b-bfc8-8d9840545a1e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: 6d0024adfde82480d46f876021a25bb8f42ba6ff
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041301"
---
# <a name="tutorial-azure-active-directory-integration-with-launchdarkly"></a>Tutorial: Integração do Azure Active Directory com LaunchDarkly

Neste tutorial, saiba como integrar LaunchDarkly com o Azure Active Directory (Azure AD).

Integrar LaunchDarkly no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao LaunchDarkly.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para LaunchDarkly (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com LaunchDarkly, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um LaunchDarkly logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando LaunchDarkly da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-launchdarkly-from-the-gallery"></a>Adicionando LaunchDarkly da Galeria
Para configurar a integração do LaunchDarkly com o Azure AD, terá de adicionar LaunchDarkly a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar LaunchDarkly a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **LaunchDarkly**, selecione **LaunchDarkly** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![LaunchDarkly na lista de resultados](./media/launchdarkly-tutorial/tutorial_launchdarkly_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com LaunchDarkly com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no LaunchDarkly a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no LaunchDarkly deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com LaunchDarkly, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste LaunchDarkly](#create-a-launchdarkly-test-user)**  - para ter um equivalente da Eduarda Almeida na LaunchDarkly que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo LaunchDarkly.

**Para configurar o Azure AD início de sessão único com LaunchDarkly, execute os seguintes passos:**

1. No portal do Azure, sobre o **LaunchDarkly** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/launchdarkly-tutorial/tutorial_launchdarkly_samlbase.png)

3. Sobre o **LaunchDarkly domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![LaunchDarkly domínio e URLs únicas início de sessão em informações](./media/launchdarkly-tutorial/tutorial_launchdarkly_url.png)

    a. Na **identificador (ID de entidade)** caixa de texto, escreva o URL: `app.launchdarkly.com`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://app.launchdarkly.com/trust/saml2/acs/<customers-unique-id>`
    
    > [!NOTE]
    > O valor de URL de resposta não é real. Atualizar o valor com o URL de resposta real, o que é explicado mais tarde no tutorial.

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![LaunchDarkly domínio e URLs únicas início de sessão em informações](./media/launchdarkly-tutorial/tutorial_launchdarkly_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva o URL: `https://app.launchdarkly.com`

5. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/launchdarkly-tutorial/tutorial_launchdarkly_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/launchdarkly-tutorial/tutorial_general_400.png)
    
7. Sobre o **LaunchDarkly configuração** secção, clique em **configurar LaunchDarkly** para abrir **configurar início de sessão** janela. Cópia a **único URL de início de sessão no serviço** partir o **secção de referência rápida.**

    ![Configuração de LaunchDarkly](./media/launchdarkly-tutorial/tutorial_launchdarkly_configure.png)

8. Numa janela do browser web diferente, inicie sessão no site da sua empresa LaunchDarkly como um administrador.

9. Selecione **definições de conta** do painel de navegação à esquerda.

    ![Configuração de LaunchDarkly](./media/launchdarkly-tutorial/configure1.png)

10. Clique em **segurança** separador.

    ![Configuração de LaunchDarkly](./media/launchdarkly-tutorial/configure2.png)

11. Clique em **HABILITAR SSO** e, em seguida **Editar configuração de SAML**.

    ![Configuração de LaunchDarkly](./media/launchdarkly-tutorial/configure3.png)

12. Sobre o **editar sua configuração SAML** secção, execute os seguintes passos:

    ![Configuração de LaunchDarkly](./media/launchdarkly-tutorial/configure4.png)

    a. Copiar o **URL do serviço de consumidor SAML** para a sua instância e cole-a na caixa de texto do URL de resposta na **LaunchDarkly domínio e URLs** secção no portal do Azure.

    b. Na **URL de início de sessão** caixa de texto, colar a **único URL de início de sessão no serviço** valor, que copiou do portal do Azure.

    c. Abrir o certificado transferido a partir do portal do Azure no bloco de notas, copie o conteúdo e, em seguida, cole-o para o **certificado X.509** caixa ou pode carregar diretamente o certificado ao clicar no **carregar uma**.

    d. Clicar em **Guardar**

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/launchdarkly-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/launchdarkly-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/launchdarkly-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/launchdarkly-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-launchdarkly-test-user"></a>Criar um utilizador de teste LaunchDarkly

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no LaunchDarkly. LaunchDarkly suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder LaunchDarkly se não existir ainda.
>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de cliente LaunchDarkly](mailto:support@launchdarkly.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para LaunchDarkly.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a LaunchDarkly, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **LaunchDarkly**.

    ![A ligação de LaunchDarkly na lista de aplicações](./media/launchdarkly-tutorial/tutorial_launchdarkly_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico LaunchDarkly no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo LaunchDarkly.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/launchdarkly-tutorial/tutorial_general_01.png
[2]: ./media/launchdarkly-tutorial/tutorial_general_02.png
[3]: ./media/launchdarkly-tutorial/tutorial_general_03.png
[4]: ./media/launchdarkly-tutorial/tutorial_general_04.png

[100]: ./media/launchdarkly-tutorial/tutorial_general_100.png

[200]: ./media/launchdarkly-tutorial/tutorial_general_200.png
[201]: ./media/launchdarkly-tutorial/tutorial_general_201.png
[202]: ./media/launchdarkly-tutorial/tutorial_general_202.png
[203]: ./media/launchdarkly-tutorial/tutorial_general_203.png

