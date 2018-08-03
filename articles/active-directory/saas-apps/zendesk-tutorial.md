---
title: 'Tutorial: Integração do Azure Active Directory com o Zendesk | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Zendesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 9b467fa966c2a785677f47faaa4bb8bd3ed238e2
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39427606"
---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Tutorial: Integração do Azure Active Directory com o Zendesk

Neste tutorial, saiba como integrar o Zendesk com o Azure Active Directory (Azure AD).

Integração do Zendesk com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Zendesk.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada ao Zendesk (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Zendesk, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Zendesk logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Zendesk a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-zendesk-from-the-gallery"></a>Adicionar Zendesk a partir da Galeria
Para configurar a integração do Zendesk com o Azure AD, terá de adicionar Zendesk a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Zendesk a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Zendesk**, selecione **Zendesk** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Zendesk na lista de resultados](./media/zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Zendesk com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no Zendesk para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Zendesk deve ser estabelecido.

No Zendesk, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Zendesk, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste da Zendesk](#create-a-zendesk-test-user)**  - para ter um equivalente da Eduarda Almeida no Zendesk, que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do Zendesk.

**Para configurar o Azure AD início de sessão único com o Zendesk, execute os seguintes passos:**

1. No portal do Azure, sobre o **Zendesk** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/zendesk-tutorial/tutorial_zendesk_samlbase.png)

1. Sobre o **Zendesk domínio e URLs** secção, execute os seguintes passos:

    ![URLs de domínio do Zendesk e únicas início de sessão em informações](./media/zendesk-tutorial/tutorial_zendesk_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.zendesk.com`

    b. Na **identificador** caixa de texto, digite o valor usando o seguinte padrão: `<subdomain>.zendesk.com`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente do Zendesk](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise) obter esses valores.

1. Sobre o **certificado de assinatura SAML** secção, copie a **THUMBPRINT** valor do certificado.

    ![O link de download de certificado](./media/zendesk-tutorial/tutorial_zendesk_certificate.png)

1. Zendesk espera que as asserções SAML num formato específico. Existem não existem atributos SAML obrigatórios, mas, opcionalmente, pode adicionar um atributo de **atributos de utilizador** secção seguindo os passos abaixo: 

     ![Configurar o início de sessão único](./media/zendesk-tutorial/tutorial_zendesk_attributes1.png)

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único adicionar](./media/zendesk-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único addattb](./media/zendesk-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **OK**.

    > [!NOTE]
    > Utilizar atributos de extensão para adicionar os atributos que não estão no Azure AD por predefinição. Clique em [atributos de utilizador que podem ser definidos no SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) para obter a lista completa de atributos SAML que **Zendesk** aceita.

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/zendesk-tutorial/tutorial_general_400.png)

1. Na **configuração do Zendesk** secção, clique em **configurar Zendesk** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração do Zendesk](./media/zendesk-tutorial/tutorial_zendesk_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Zendesk como um administrador.

1. Clique em **administrador**.

1. No painel de navegação esquerdo, clique em **configurações**e, em seguida, clique em **segurança**.

1. Sobre o **segurança** página, execute os seguintes passos: 

     ![Segurança](./media/zendesk-tutorial/ic773089.png "segurança")

    ![Início de sessão único](./media/zendesk-tutorial/ic773090.png "início de sessão único")

     a. Clique nas **agentes e administrador** separador.

     b. Selecione **início de sessão único (SSO) e SAML**e, em seguida, selecione **SAML**.

     c. Na **URL de SSO SAML** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** que copiou do portal do Azure. 

     d. Na **URL de fim de sessão remoto** caixa de texto, cole o valor de **URL de fim de sessão** que copiou do portal do Azure.

     e. Na **impressão digital de certificado** caixa de texto, colar a **Thumbprint** valor do certificado que copiou do portal do Azure.

     f. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/zendesk-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/zendesk-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/zendesk-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/zendesk-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-zendesk-test-user"></a>Criar um utilizador de teste do Zendesk

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Zendesk. Zendesk suporta o aprovisionamento automático de utilizadores, que está por predefinição, ativada. Pode encontrar mais detalhes [aqui](zendesk-provisioning-tutorial.md) sobre como configurar o aprovisionamento automático de utilizadores.

**Se precisar de criar manualmente o utilizador, execute, os seguintes passos:**

> [!NOTE]
> **Utilizador final** contas são aprovisionadas automaticamente ao iniciar sessão. **Agente** e **administrador** contas têm de ser provisionado manualmente no **Zendesk** antes de iniciar sessão.

1. Inicie sessão no seu **Zendesk** inquilino.

1. Selecione o **lista de clientes** separador.

1. Selecione o **usuário** separador e clique em **Add**.

    ![Adicionar utilizador](./media/zendesk-tutorial/ic773632.png "adicionar utilizador")
1. Tipo de **Name** e **E-Mail** de uma conta do Azure AD existente que pretende aprovisionar e, em seguida, clique em **guardar**.

    ![Novo utilizador](./media/zendesk-tutorial/ic773633.png "novo utilizador")

> [!NOTE]
> Pode utilizar quaisquer outras Zendesk utilizador conta criação ferramentas ou APIs fornecidas pelo Zendesk para aprovisionar contas de utilizador do AAD.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao Zendesk.

![Atribuir a função de utilizador][200]

**Para atribuir a Eduarda Almeida ao Zendesk, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

1. Na lista de aplicações, selecione **Zendesk**.

    ![A ligação de Zendesk na lista de aplicações](./media/zendesk-tutorial/tutorial_zendesk_app.png)

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do Zendesk no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do Zendesk.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)
* [Configurar o aprovisionamento do utilizador](zendesk-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/zendesk-tutorial/tutorial_general_01.png
[2]: ./media/zendesk-tutorial/tutorial_general_02.png
[3]: ./media/zendesk-tutorial/tutorial_general_03.png
[4]: ./media/zendesk-tutorial/tutorial_general_04.png

[100]: ./media/zendesk-tutorial/tutorial_general_100.png

[200]: ./media/zendesk-tutorial/tutorial_general_200.png
[201]: ./media/zendesk-tutorial/tutorial_general_201.png
[202]: ./media/zendesk-tutorial/tutorial_general_202.png
[203]: ./media/zendesk-tutorial/tutorial_general_203.png
