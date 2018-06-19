---
title: 'Tutorial: Integração do Azure Active Directory com o segredo de servidor (no local) | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e o segredo de servidor (no local).
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: be4ba84a-275d-4f71-afce-cb064edc713f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2018
ms.author: jeedes
ms.openlocfilehash: 552ba0ac9591f6c753e79ab5a1b406f2035457c0
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/18/2018
ms.locfileid: "35948082"
---
# <a name="tutorial-azure-active-directory-integration-with-secret-server-on-premises"></a>Tutorial: Integração do Azure Active Directory com o segredo de servidor (no local)

Neste tutorial, irá aprender a integrar o segredo de servidor (no local) com o Azure Active Directory (Azure AD).

Integrar o segredo de servidor (no local) com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao segredo de servidor (no local).
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada ao segredo de servidor (no local) (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o segredo de servidor (no local), terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um segredo de servidor (no local)-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar o segredo de servidor (no local) da galeria do
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-secret-server-on-premises-from-the-gallery"></a>Adicionar o segredo de servidor (no local) da galeria do
Para configurar a integração do segredo de servidor (no local) com o Azure AD, tem de adicionar segredo de servidor (no local) da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar o segredo de servidor (no local) da galeria do, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **segredo de servidor (no local)**, selecione **segredo de servidor (no local)** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Servidor secreta (no local), na lista de resultados](./media/secretserver-on-premises-tutorial/tutorial_secretserver_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com o segredo de servidor (no local), com base no chamado "Britta Simon" um utilizador de teste.

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no segredo Server (no local) é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no segredo do servidor (no local) tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com o segredo de servidor (no local), tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste do segredo de servidor (no local)](#create-a-secret-server-on-premises-test-user)**  - para ter um homólogo de Britta Simon no segredo do servidor (no local), que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação segredo de servidor (no local).

**Para configurar o Azure AD-início de sessão único com o segredo de servidor (no local), execute os seguintes passos:**

1. No portal do Azure, no **segredo de servidor (no local)** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/secretserver-on-premises-tutorial/tutorial_secretserver_samlbase.png)

3. No **domínio segredo de servidor (no local) e URLs** secção, execute os seguintes passos, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Segredo URLs de domínio do servidor (no local) e única informações de início de sessão](./media/secretserver-on-premises-tutorial/tutorial_secretserver_url.png)

    a. No **identificador** caixa de texto, introduza o utilizador escolhido o valor como um exemplo: `https://secretserveronpremises.azure`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<SecretServerURL>/SAML/AssertionConsumerService.aspx `

    > [!NOTE]
    > O ID de entidade mostrado acima é apenas um exemplo e estiver livre para escolher qualquer valor exclusivo que identifica a instância do servidor do segredo no Azure AD. É necessário enviar este ID de entidade para [equipa de suporte de cliente do segredo de servidor (no local)](https://thycotic.force.com/support/s/) e configurarem no seu lado. Para obter mais detalhes, leia [neste artigo](https://thycotic.force.com/support/s/article/Configuring-SAML-in-Secret-Server).

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![Segredo URLs de domínio do servidor (no local) e única informações de início de sessão](./media/secretserver-on-premises-tutorial/tutorial_secretserver_url1.png)

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<SecretServerURL>/login.aspx`
     
    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o URL de resposta real e o URL de início de sessão. Contacte [equipa de suporte de cliente do segredo de servidor (no local)](https://thycotic.force.com/support/s/) para obter estes valores.

5. No **certificado de assinatura de SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/secretserver-on-premises-tutorial/tutorial_secretserver_certificate.png)

6. Verifique **Mostrar avançadas definições de assinatura de certificado** e selecione **assinatura opção** como **resposta de início de sessão SAML e asserção**.

    ![Opções de assinatura](./media/secretserver-on-premises-tutorial/signing.png)

7. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/secretserver-on-premises-tutorial/tutorial_general_400.png)
    
8. No **configuração segredo de servidor (no local)** secção, clique em **configurar o segredo de servidor (no local)** para abrir **configurar início de sessão** janela. Copiar o **Sign-Out URL, o ID de entidade de SAML e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configuração do servidor secreta (no local)](./media/secretserver-on-premises-tutorial/tutorial_secretserver_configure.png)

9. Para configurar o início de sessão único em **segredo de servidor (no local)** lado, terá de enviar o transferido **Certificate(Base64), Sign-Out URL, único início de sessão no URL do serviço SAML**, e **SAML entidade ID** para [equipa de suporte do segredo de servidor (no local)](https://thycotic.force.com/support/s/). Se definir esta definição para que a ligação de SAML SSO corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/secretserver-on-premises-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/secretserver-on-premises-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/secretserver-on-premises-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/secretserver-on-premises-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-secret-server-on-premises-test-user"></a>Criar um utilizador de teste do segredo de servidor (no local)

Nesta secção, vai criar um utilizador chamado Britta Simon no segredo do servidor (no local). Trabalhar com [equipa de suporte do segredo de servidor (no local)](https://thycotic.force.com/support/s/) para adicionar os utilizadores na plataforma segredo de servidor (no local). Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso ao segredo de servidor (no local).

![Atribuir a função de utilizador][200]

**Para atribuir Britta Simon ao segredo de servidor (no local), execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201]

2. Na lista de aplicações, selecione **segredo de servidor (no local)**.

    ![A ligação do segredo de servidor (no local) na lista de aplicações](./media/secretserver-on-premises-tutorial/tutorial_secretserver_app.png)

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico do segredo de servidor (no local) no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de servidor segredo (no local).
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/secretserver-on-premises-tutorial/tutorial_general_01.png
[2]: ./media/secretserver-on-premises-tutorial/tutorial_general_02.png
[3]: ./media/secretserver-on-premises-tutorial/tutorial_general_03.png
[4]: ./media/secretserver-on-premises-tutorial/tutorial_general_04.png

[100]: ./media/secretserver-on-premises-tutorial/tutorial_general_100.png

[200]: ./media/secretserver-on-premises-tutorial/tutorial_general_200.png
[201]: ./media/secretserver-on-premises-tutorial/tutorial_general_201.png
[202]: ./media/secretserver-on-premises-tutorial/tutorial_general_202.png
[203]: ./media/secretserver-on-premises-tutorial/tutorial_general_203.png

