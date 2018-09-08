---
title: 'Tutorial: Integração do Azure Active Directory com Clarizen | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Clarizen.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: jeedes
ms.openlocfilehash: 855f147b0622ecc0831f2bc464e83d245af9e574
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158676"
---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Tutorial: Integração do Azure Active Directory com Clarizen

Neste tutorial, saiba como integrar o Azure Active Directory (Azure AD) com Clarizen. Esta integração dá-lhe as seguintes vantagens:

- Pode controlar, no Azure AD, quem tem acesso a Clarizen.
- Pode permitir que os utilizadores ter automaticamente sessão iniciada no Clarizen (início de sessão único) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central, o portal do Azure.

O cenário neste tutorial consiste em duas tarefas principais:

1. Adicione Clarizen a partir da galeria.
1. Configurar e testar o Azure AD início de sessão único.

Se pretender obter mais detalhes sobre o software como uma integração de aplicação de serviço (SaaS) com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos
Para configurar a integração do Azure AD com Clarizen, terá dos seguintes itens:

- Uma subscrição do Azure
- Uma subscrição de Clarizen que está ativada para início de sessão único

Para testar os passos neste tutorial, siga as seguintes recomendações:

- Teste do Azure AD início de sessão único num ambiente de teste. Não utilize o seu ambiente de produção, a menos que isso é necessário.
- Se não tiver um ambiente de teste do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="add-clarizen-from-the-gallery"></a>Adicionar Clarizen a partir da Galeria
Para configurar a integração do Clarizen com o Azure AD, adicione Clarizen a partir da Galeria à sua lista de aplicações de SaaS geridas.

1. Na [portal do Azure](https://portal.azure.com), no painel esquerdo, clique nas **Azure Active Directory** ícone.

    ![Ícone do Active Directory do Azure][1]

1. Clique em **aplicações empresariais**. Em seguida, clique em **todos os aplicativos**.

    ![Clicar em "Aplicações empresariais" e "Todas as aplicações"][2]

1. Clique nas **adicionar** botão na parte superior da caixa de diálogo.

    ![O botão "Adicionar"][3]

1. Na caixa de pesquisa, escreva **Clarizen**.

    ![Escrever "Clarizen" na caixa de pesquisa](./media/clarizen-tutorial/tutorial_clarizen_000.png)

1. No painel de resultados, selecione **Clarizen**e, em seguida, clique em **Add** para adicionar a aplicação.

    ![Selecionar Clarizen no painel de resultados](./media/clarizen-tutorial/tutorial_clarizen_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único
Nas seções a seguir, configure e teste do Azure AD início de sessão único com Clarizen com base no utilizador de teste Eduarda Almeida.

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Clarizen a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Clarizen deve ser estabelecido. Estabelecer esta relação de ligação ao atribuir o valor de **nome de utilizador** no Azure AD como o valor de **Username** no Clarizen.

Para configurar e testar o Azure AD início de sessão único com Clarizen, conclua os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  para permitir aos utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Clarizen](#create-a-clarizen-test-user)**  ter um equivalente da Eduarda Almeida na Clarizen que está ligado à representação de ela do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único
Ativar o Azure AD início de sessão único no portal do Azure e configurar o início de sessão único em seu aplicativo Clarizen.

1. No portal do Azure, sobre o **Clarizen** página de integração de aplicação, clique em **início de sessão único**.

    ![Clicar em "Início de sessão único"][4]

1. Na **início de sessão único** caixa de diálogo, para **modo**, selecione **baseado em SAML logon** para ativar o início de sessão único.

    ![Selecionar "baseado em SAML Sign-on"](./media/clarizen-tutorial/tutorial_clarizen_01.png)

1. Na **Clarizen domínio e URLs** secção, execute os seguintes passos:

    ![Caixas para o URL de identificador e resposta](./media/clarizen-tutorial/tutorial_clarizen_02.png)

    a. Na **identificador** , escreva o valor como: **Clarizen**

    b. Na **URL de resposta** , escreva um URL ao utilizar o seguinte padrão: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**

    > [!NOTE]
    > Essas não são os valores reais. Tem de utilizar o identificador real e URL de resposta. Aqui sugerimos que utilize o valor exclusivo de uma cadeia de caracteres como o identificador. Para obter os valores reais, entre em contato com o [equipa de suporte de Clarizen](https://success.clarizen.com/hc/en-us/requests/new).

1. Sobre o **certificado de assinatura SAML** secção, clique em **criar novo certificado**.

    ![Clicar em "Criar novo certificado"](./media/clarizen-tutorial/tutorial_clarizen_03.png)    

1. Na **criar novo certificado** diálogo caixa, clique no ícone de calendário e selecione uma data de expiração. Em seguida, clique em **Guardar**.

    ![Selecionar e salvar uma data de expiração](./media/clarizen-tutorial/tutorial_general_300.png)

1. Na **certificado de assinatura SAML** secção, selecione **ativar o novo certificado**e, em seguida, clique em **guardar**.

    ![Selecionar a caixa de verificação para fazer com que o novo certificado ativo](./media/clarizen-tutorial/tutorial_clarizen_04.png)

1. Na **certificado de Rollover** caixa de diálogo, clique em **OK**.

    ![Clicar em "OK" para confirmar que pretende ativar o certificado](./media/clarizen-tutorial/tutorial_general_400.png)

1. Na **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Clicar em "Certificado (Base64)" para iniciar o download](./media/clarizen-tutorial/tutorial_clarizen_05.png)

1. Na **Clarizen Configuration** secção, clique em **configurar Clarizen** para abrir o **configurar início de sessão** janela.

    ![Clicar em "Configurar Clarizen"](./media/clarizen-tutorial/tutorial_clarizen_06.png)

    ![Janela de "Configurar o início de sessão", incluindo URLs e ficheiros](./media/clarizen-tutorial/tutorial_clarizen_07.png)

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Clarizen como um administrador.

1. Clique em seu nome de utilizador e, em seguida, clique em **definições**.

    ![Clicar em "Definições" em seu nome de utilizador](./media/clarizen-tutorial/tutorial_clarizen_001.png "definições")

1. Clique nas **definições globais** separador. Em seguida, junto a **autenticação federada**, clique em **editar**.

    ![Separador "Definições globais"](./media/clarizen-tutorial/tutorial_clarizen_002.png "definições globais")

1. Na **autenticação federada** diálogo caixa, execute os seguintes passos:

    !["Autenticação federada" caixa de diálogo](./media/clarizen-tutorial/tutorial_clarizen_003.png "autenticação federada")

    a. Selecione **Enable federado autenticação**.

    b. Clique em **carregar** para carregar o certificado transferido.

    c. Na **URL de início de sessão** , introduza o valor de **SAML único início de sessão no URL do serviço** partir da janela de configuração de aplicação do Azure AD.

    d. Na **URL de fim de sessão** , introduza o valor de **URL de fim de sessão** partir da janela de configuração de aplicação do Azure AD.

    e. Selecione **utilize POST**.

    f. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
No portal do Azure, crie um utilizador de teste Eduarda Almeida.

![Nome e endereço de e-mail do utilizador de teste do Azure AD][100]

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** ícone.

    ![Ícone do Active Directory do Azure](./media/clarizen-tutorial/create_aaduser_01.png)

1. Clique em **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores** para apresentar a lista de utilizadores.

    ![Clicar em "Utilizadores e grupos" e "Todos os utilizadores"](./media/clarizen-tutorial/create_aaduser_02.png)

1. Na parte superior da caixa de diálogo, clique em **Add** para abrir o **utilizador** caixa de diálogo.

    ![O botão "Adicionar"](./media/clarizen-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![Caixa de diálogo de "Usuário" com o nome, endereço de e-mail e palavra-passe preenchidos](./media/clarizen-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail da conta Eduarda Almeida.

    c. Selecione **mostrar palavra-passe** e indique o valor de **palavra-passe**.

    d. Clique em **Criar**.

### <a name="create-a-clarizen-test-user"></a>Criar um utilizador de teste Clarizen

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Clarizen.

**Se precisar de criar manualmente o utilizador, execute, os seguintes passos:**

Para ativar a utilizadores do Azure AD iniciar sessão no Clarizen, terá de aprovisionar contas de utilizador. No caso de Clarizen, o aprovisionamento é uma tarefa manual.

1. Inicie sessão no site da sua empresa Clarizen como um administrador.

2. Clique em **pessoas**.

    ![Clicar em "Quem"](./media/clarizen-tutorial/create_aaduser_001.png "pessoas")

3. Clique em **convidar utilizador**.

    ![Botão "Convidar o utilizador"](./media/clarizen-tutorial/create_aaduser_002.png "convidar utilizadores")

1. Na **convidar pessoas** diálogo caixa, execute os seguintes passos:

    !["Convidar pessoas" caixa de diálogo](./media/clarizen-tutorial/create_aaduser_003.png "convidar pessoas")

    a. Na **E-Mail** , escreva o endereço de e-mail da conta Eduarda Almeida.

    b. Clique em **convidar**.

    > [!NOTE]
    > O titular da conta do Azure Active Directory irá receber um e-mail e seguir uma ligação para confirmar a respetiva conta até se tornar Active Directory.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD
Ative a Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Clarizen.

![Utilizador de teste atribuídas][200]

1. No Azure portal, abra ver as aplicações, navegue para a vista de diretório, clique em **aplicações empresariais**e, em seguida, clique em **todos os aplicativos**.

    ![Clicar em "Aplicações empresariais" e "Todas as aplicações"][201]

1. Na lista de aplicações, selecione **Clarizen**.

    ![Selecionar Clarizen na lista](./media/clarizen-tutorial/tutorial_clarizen_50.png)

1. No painel esquerdo, clique em **utilizadores e grupos**.

    ![Clicar em "Utilizadores e grupos"][202]

1. Clique no botão **Adicionar**. Em seguida, na **adicionar atribuição** caixa de diálogo, selecione **utilizadores e grupos**.

    ![O botão "Adicionar" e a caixa de diálogo "Adicionar atribuição"][203]

1. Na **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista de utilizadores.

1. Na **utilizadores e grupos** caixa de diálogo, clique nas **selecione** botão.

1. Na **adicionar atribuição** caixa de diálogo, clique nas **atribuir** botão.

### <a name="test-single-sign-on"></a>Testar o início de sessão único
Teste a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Clarizen no painel de acesso, deve ser automaticamente conectado à sua aplicação Clarizen.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/clarizen-tutorial/tutorial_general_01.png
[2]: ./media/clarizen-tutorial/tutorial_general_02.png
[3]: ./media/clarizen-tutorial/tutorial_general_03.png
[4]: ./media/clarizen-tutorial/tutorial_general_04.png

[100]: ./media/clarizen-tutorial/tutorial_general_100.png

[200]: ./media/clarizen-tutorial/tutorial_general_200.png
[201]: ./media/clarizen-tutorial/tutorial_general_201.png
[202]: ./media/clarizen-tutorial/tutorial_general_202.png
[203]: ./media/clarizen-tutorial/tutorial_general_203.png