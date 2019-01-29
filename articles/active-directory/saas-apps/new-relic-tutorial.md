---
title: 'Tutorial: Integração do Active Directory do Azure com o New Relic | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o New Relic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: jeedes
ms.openlocfilehash: a60360d8268d43d920ad878d372014fa6d9cc875
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55176867"
---
# <a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Tutorial: Integração do Active Directory do Azure com o New Relic

Neste tutorial, saiba como integrar o New Relic com o Azure Active Directory (Azure AD).

Integração de New Relic com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao New Relic.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o New Relic (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o New Relic, terá dos seguintes itens:

- Uma subscrição do Azure
- Um New Relic logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o New Relic da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-new-relic-from-the-gallery"></a>Adicionando o New Relic da Galeria
Para configurar a integração de New Relic para o Azure AD, terá de adicionar o New Relic na Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o New Relic a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **New Relic**, selecione **New Relic** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![O New Relic na lista de resultados](./media/new-relic-tutorial/tutorial_new-relic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o New Relic com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no New Relic para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado em New Relic deve ser estabelecido.

New Relic, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o New Relic, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de New Relic](#create-a-new-relic-test-user)**  - para ter um equivalente da Eduarda Almeida em New Relic que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de New Relic.

**Para configurar o Azure AD início de sessão único com o New Relic, execute os seguintes passos:**

1. No portal do Azure, sobre o **New Relic** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/new-relic-tutorial/tutorial_new-relic_samlbase.png)

1. Sobre o **novo Relic domínio e URLs** secção, execute os seguintes passos:

    ![Novo Relic domínio e URLs único informações de início de sessão](./media/new-relic-tutorial/tutorial_new-relic_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://rpm.newrelic.com/accounts/{acc_id}/sso/saml/login` -não se esqueça de substituir o seu ID da conta de New Relic.

    b. Na **identificador** caixa de texto, digite o valor: `rpm.newrelic.com`

1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/new-relic-tutorial/tutorial_new-relic_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/new-relic-tutorial/tutorial_general_400.png)

1. Sobre o **nova configuração de Relic** secção, clique em **configurar New Relic** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Nova configuração de Relic](./media/new-relic-tutorial/tutorial_new-relic_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no seu **New Relic** site da empresa como administrador.

1. No menu na parte superior, clique em **definições de conta**.
   
    ![Definições da conta](./media/new-relic-tutorial/ic797036.png "definições da conta")

1. Clique nas **segurança e autenticação** separador e, em seguida, clique nas **início de sessão único** separador.
   
    ![Início de sessão único](./media/new-relic-tutorial/ic797037.png "início de sessão único")

1. Na página de diálogo de SAML, execute os seguintes passos:
   
    ![SAML](./media/new-relic-tutorial/ic797038.png "SAML")
   
   a. Clique em **Escolher ficheiro** para carregar o certificado transferido do Azure Active Directory.

   b. Na **URL de início de sessão remoto** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure.
   
   c. Na **URL de destino de fim de sessão** caixa de texto, cole o valor de **URL de fim de sessão**, que copiou do portal do Azure.

   d. Clique em **salvar minhas alterações**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/new-relic-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/new-relic-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/new-relic-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/new-relic-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-new-relic-test-user"></a>Criar um utilizador de teste de New Relic

Para que os utilizadores do Azure Active Directory iniciar sessão no New Relic, tem de ser aprovisionados em New Relic. No caso de New Relic, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador para o New Relic, execute os seguintes passos:**

1. Inicie sessão no seu **New Relic** site da empresa como administrador.

1. No menu na parte superior, clique em **definições de conta**.
   
    ![Definições da conta](./media/new-relic-tutorial/ic797040.png "definições da conta")

1. Na **conta** painel no lado esquerdo, clique em **resumo**e, em seguida, clique em **adicionar utilizador**.
   
    ![Definições da conta](./media/new-relic-tutorial/ic797041.png "definições da conta")

1. Sobre o **utilizadores ativos** caixa de diálogo, execute os seguintes passos:
   
    ![Utilizadores ativos](./media/new-relic-tutorial/ic797042.png "utilizadores ativos")
   
    a. Na **E-Mail** caixa de texto, escreva o endereço de e-mail de um utilizador válido do Azure Active Directory que pretende aprovisionar.

    b. Como **função** selecionar **utilizador**.

    c. Clique em **adicionar este utilizador**.

>[!NOTE]
>Pode utilizar quaisquer outras New Relic utilizador conta criação ferramentas ou APIs fornecidas pelo New Relic para aprovisionar contas de utilizador do AAD.
> 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso a New Relic.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a New Relic, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **New Relic**.

    ![A ligação de New Relic na lista de aplicações](./media/new-relic-tutorial/tutorial_new-relic_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de New Relic no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo de New Relic.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/new-relic-tutorial/tutorial_general_01.png
[2]: ./media/new-relic-tutorial/tutorial_general_02.png
[3]: ./media/new-relic-tutorial/tutorial_general_03.png
[4]: ./media/new-relic-tutorial/tutorial_general_04.png

[100]: ./media/new-relic-tutorial/tutorial_general_100.png

[200]: ./media/new-relic-tutorial/tutorial_general_200.png
[201]: ./media/new-relic-tutorial/tutorial_general_201.png
[202]: ./media/new-relic-tutorial/tutorial_general_202.png
[203]: ./media/new-relic-tutorial/tutorial_general_203.png

