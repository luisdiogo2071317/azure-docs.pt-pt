---
title: 'Tutorial: Integração do Azure Active Directory com Uberflip | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Uberflip.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 754b1f5b-6694-4fd6-9e1e-9fad769c64db
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2018
ms.author: jeedes
ms.openlocfilehash: bb1f53895fcd91a9474302fcf8c9e0040fe91961
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42920537"
---
# <a name="tutorial-azure-active-directory-integration-with-uberflip"></a>Tutorial: Integração do Azure Active Directory com Uberflip

Neste tutorial, saiba como integrar Uberflip com o Azure Active Directory (Azure AD).

Integrar Uberflip no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Uberflip.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Uberflip (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Uberflip, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Uberflip logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Uberflip da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-uberflip-from-the-gallery"></a>Adicionando Uberflip da Galeria

Para configurar a integração do Uberflip com o Azure AD, terá de adicionar Uberflip a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Uberflip a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Uberflip**, selecione **Uberflip** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Uberflip na lista de resultados](./media/uberflip-tutorial/tutorial_uberflip_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Uberflip com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Uberflip a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Uberflip deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Uberflip, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Uberflip](#create-an-uberflip-test-user)**  - para ter um equivalente da Eduarda Almeida na Uberflip que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Uberflip.

**Para configurar o Azure AD início de sessão único com Uberflip, execute os seguintes passos:**

1. No portal do Azure, sobre o **Uberflip** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/uberflip-tutorial/tutorial_uberflip_samlbase.png)

3. Sobre o **Uberflip domínio e URLs** secção, executar o passo seguinte, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Uberflip domínio e URLs únicas início de sessão em informações](./media/uberflip-tutorial/tutorial_uberflip_url1.png)

    Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://app.uberflip.com/sso/saml2/<IDPID>/<ACCOUNTID>`

    > [!NOTE]
    > Este valor não é real. Atualize este valor com o URL de resposta real. Contacte [equipa de suporte de cliente Uberflip](mailto:support@uberflip.com) para obter este valor.

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Uberflip domínio e URLs únicas início de sessão em informações](./media/uberflip-tutorial/tutorial_uberflip_url2.png)

    Na **URL de início de sessão** caixa de texto, escreva o URL: `https://app.uberflip.com/users/login`

5. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/uberflip-tutorial/tutorial_uberflip_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/uberflip-tutorial/tutorial_general_400.png)

7. Para configurar o início de sessão único num **Uberflip** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte de Uberflip](mailto:support@uberflip.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/uberflip-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/uberflip-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/uberflip-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/uberflip-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-an-uberflip-test-user"></a>Criar um utilizador de teste Uberflip

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Uberflip. Uberflip suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder Uberflip se não existir ainda.

> [!Note]
> Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de Uberflip](mailto:support@uberflip.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Uberflip.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Uberflip, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Uberflip**.

    ![A ligação de Uberflip na lista de aplicações](./media/uberflip-tutorial/tutorial_uberflip_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Uberflip no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Uberflip.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/uberflip-tutorial/tutorial_general_01.png
[2]: ./media/uberflip-tutorial/tutorial_general_02.png
[3]: ./media/uberflip-tutorial/tutorial_general_03.png
[4]: ./media/uberflip-tutorial/tutorial_general_04.png

[100]: ./media/uberflip-tutorial/tutorial_general_100.png

[200]: ./media/uberflip-tutorial/tutorial_general_200.png
[201]: ./media/uberflip-tutorial/tutorial_general_201.png
[202]: ./media/uberflip-tutorial/tutorial_general_202.png
[203]: ./media/uberflip-tutorial/tutorial_general_203.png