---
title: 'Tutorial: Integração do Azure Active Directory com o Comm100 Live Chat | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Comm100, Chat ao vivo.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0340d7f3-ab54-49ef-b77c-62a0efd5d49c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2018
ms.author: jeedes
ms.openlocfilehash: b85162c8392e8ecdb08a3ed04ff5b9de835808a1
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "42061417"
---
# <a name="tutorial-azure-active-directory-integration-with-comm100-live-chat"></a>Tutorial: Integração do Azure Active Directory com Comm100, Chat ao vivo

Neste tutorial, saiba como integrar o Comm100, Chat ao vivo com o Azure Active Directory (Azure AD).

Integrar o Comm100, Chat ao vivo no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Comm100, Chat ao vivo.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Comm100 Chat ao vivo (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Comm100, Chat ao vivo, precisa do seguinte:

- Uma subscrição do Azure
- Um Comm100, Chat ao vivo logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Comm100, Chat ao vivo da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-comm100-live-chat-from-the-gallery"></a>Adicionando Comm100, Chat ao vivo da Galeria
Para configurar a integração do Comm100, Chat ao vivo com o Azure AD, terá de adicionar Comm100, Chat ao vivo da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar a Comm100, Chat ao vivo a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Comm100, Chat ao vivo**, selecione **Comm100, Chat ao vivo** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Comm100 Chat ao vivo na lista de resultados](./media/comm100livechat-tutorial/tutorial_comm100livechat_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Comm100 Chat ao vivo com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Comm100 Live Chat a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Comm100 Live Chat deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Comm100, Chat ao vivo, precisa concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Comm100, Chat ao vivo](#create-a-comm100-live-chat-test-user)**  - para ter um equivalente da Eduarda Almeida na Comm100 Chat ao vivo que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Comm100, Chat ao vivo.

**Para configurar o Azure AD início de sessão único com Comm100, Chat ao vivo, execute os seguintes passos:**

1. No portal do Azure, sobre o **Comm100, Chat ao vivo** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/comm100livechat-tutorial/tutorial_comm100livechat_samlbase.png)

3. Sobre o **Comm100 Live Chat domínio e URLs** secção, execute os seguintes passos:

    ![Domínio de Chat ao vivo da Comm100 e URLs únicas início de sessão em informações](./media/comm100livechat-tutorial/tutorial_comm100livechat_url.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<SUBDOMAIN>.comm100.com/AdminManage/LoginSSO.aspx?siteId=<SITEID>`
    
    > [!NOTE] 
    > O valor de URL de início de sessão não é real. Atualizar o valor de URL de início de sessão com o início de sessão no URL real, que é explicado mais tarde no tutorial.

4. Aplicação Comm100, Chat ao vivo espera que as asserções SAML para conter atributos específicos. Configure os seguintes atributos para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo disso.

    ![Configurar o início de sessão único](./media/comm100livechat-tutorial/tutorial_attribute_03.png)
    
5. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    |  Nome do Atributo  | Valor do Atributo |
    | --------------- | -------------------- |    
    |   e-mail    | User.Mail |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/comm100livechat-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/comm100livechat-tutorial/tutorial_attribute_05.png)
    
    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o **espaço de nomes** em branco.
    
    e. Clique em **OK**.

6. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/comm100livechat-tutorial/tutorial_comm100livechat_certificate.png) 

7. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/comm100livechat-tutorial/tutorial_general_400.png)

8. Sobre o **Comm100 Live Chat configuração** secção, clique em **configurar o Chat ao vivo de Comm100** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de Chat ao vivo Comm100](./media/comm100livechat-tutorial/tutorial_comm100livechat_configure.png)

9. Numa janela do browser web diferente, início de sessão para Comm100 Chat ao vivo como um administrador de segurança.

10. No canto superior direito da página, clique em **minha conta**.

    ![Myaccount Comm100, Chat ao vivo](./media/comm100livechat-tutorial/tutorial_comm100livechat_account.png)

11. Do lado esquerdo do menu, clique em **Security** e, em seguida, clique em **agente Single Sign-On**.

    ![Segurança Comm100, Chat ao vivo](./media/comm100livechat-tutorial/tutorial_comm100livechat_security.png)

12. Sobre o **agente Single Sign-On** página, execute os seguintes passos:

    ![Segurança Comm100, Chat ao vivo](./media/comm100livechat-tutorial/tutorial_comm100livechat_singlesignon.png)

    a. Copie a primeira ligação realçada e cole-a no **URL de início de sessão** na caixa de texto **Comm100 Live Chat domínio e URLs** secção no portal do Azure.

    b. Na **URL de SSO SAML** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure.

    c. Na **URL de fim de sessão remoto** caixa de texto, cole o valor de **URL de fim de sessão**, que copiou do portal do Azure.

    d. Clique em **escolher um ficheiro** carregar a base 64 codificada certificado que transferiu do portal do Azure, para o **certificado**.

    e. Clique em **guardar alterações**

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/comm100livechat-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/comm100livechat-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/comm100livechat-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/comm100livechat-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-comm100-live-chat-test-user"></a>Criar um utilizador de teste Comm100, Chat ao vivo

Para ativar a utilizadores do Azure AD iniciar sessão no Comm100 Live Chat, tem de ser aprovisionados no Comm100 Live Chat. No Comm100 Live Chat, aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no Chat ao vivo de Comm100 como um administrador de segurança.

2. No canto superior direito da página, clique em **minha conta**.

    ![Myaccount Comm100, Chat ao vivo](./media/comm100livechat-tutorial/tutorial_comm100livechat_account.png)

3. Do lado esquerdo do menu, clique em **agentes** e, em seguida, clique em **novo agente**.

    ![Agente Comm100, Chat ao vivo](./media/comm100livechat-tutorial/tutorial_comm100livechat_agent.png)

4. Sobre o **novo agente** página, execute os seguintes passos:

    ![Comm100, Chat ao vivo novo agente](./media/comm100livechat-tutorial/tutorial_comm100livechat_newagent.png)

    a. a. Na **E-Mail** texto, introduza o e-mail do utilizador, como **Brittasimon@contoso.com**.

    b. Na **nome próprio** texto, introduza o nome de utilizador, como **Eduarda**.

    c. Na **sobrenome** texto, digite o apelido do utilizador, como **simon**.

    d. Na **nome a apresentar** caixa de texto, introduza o nome a apresentar do utilizador, como **Eduarda Almeida**

    e. Na **palavra-passe** caixa de texto, escreva a palavra-passe.

    f. Clique em **Guardar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Comm100, Chat ao vivo.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Comm100, Chat ao vivo, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Comm100, Chat ao vivo**.

    ![A ligação Comm100, Chat ao vivo, na lista de aplicações](./media/comm100livechat-tutorial/tutorial_comm100livechat_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Comm100, Chat ao vivo no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Comm100, Chat ao vivo.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/comm100livechat-tutorial/tutorial_general_01.png
[2]: ./media/comm100livechat-tutorial/tutorial_general_02.png
[3]: ./media/comm100livechat-tutorial/tutorial_general_03.png
[4]: ./media/comm100livechat-tutorial/tutorial_general_04.png

[100]: ./media/comm100livechat-tutorial/tutorial_general_100.png

[200]: ./media/comm100livechat-tutorial/tutorial_general_200.png
[201]: ./media/comm100livechat-tutorial/tutorial_general_201.png
[202]: ./media/comm100livechat-tutorial/tutorial_general_202.png
[203]: ./media/comm100livechat-tutorial/tutorial_general_203.png

