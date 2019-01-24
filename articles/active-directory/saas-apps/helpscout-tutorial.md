---
title: 'Tutorial: Integração do Active Directory do Azure com ajudar Scout | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e ajudar a Scout.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/14/2017
ms.author: jeedes
ms.openlocfilehash: 367967358d55eda40603b71893333080d7faa81a
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54816264"
---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a>Tutorial: Integração do Active Directory do Azure com ajudar Scout

Neste tutorial, saiba como integrar Scout ajudar com o Azure Active Directory (Azure AD).

Integração do Scout ajudar com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso para ajudar a Scout.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para ajudar a Scout (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com ajudar Scout, precisa do seguinte:

- Uma subscrição do Azure
- Um ajuda Scout início de sessão único na subscrição ativado

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Scout ajudar a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-help-scout-from-the-gallery"></a>Adicionar Scout ajudar a partir da Galeria
Para configurar a integração do Scout ajudar com o Azure AD, terá de adicionar Scout ajudar a partir da galeria, à sua lista de aplicações de SaaS geridas.

**Para adicionar Scout ajudar a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **ajudar Scout**, selecione **Scout ajudar** no painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Scout de ajuda na lista de resultados](./media/helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com ajudar Scout, com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Scout ajuda a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado em ajudar a Scout deve ser estabelecido.

Ajuda do Scout utiliza endereços de e-mail para inícios de sessão, por isso, para estabelecer a relação de ligação, utilize o mesmo **endereço de e-mail** como **nome de utilizador** no Azure AD.

Para configurar e testar o Azure AD início de sessão único com Scout ajudar, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de ajudar a Scout](#create-a-help-scout-test-user)**  - para ter um equivalente da Eduarda Almeida na ajuda Scout que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão único no portal do Azure e configurar o início de sessão único em seu aplicativo de ajudar a Scout.

**Para configurar o Azure AD início de sessão único com Scout ajudar a, execute os seguintes passos:**

1. No portal do Azure, no **ajudar Scout** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/helpscout-tutorial/tutorial_helpscout_samlbase.png)

1. Na **ajudar o Scout domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Ajudar a Scout domínio e URLs únicas início de sessão em informações](./media/helpscout-tutorial/tutorial_helpscout_url.png)

    a. **Identificador** é o **"(ID de entidade do fornecedor de serviço) do URI de audiência"** do Scout ajudar, começa com `urn:`

    b. **URL de resposta** é o **"Postback URL (URL do serviço de consumidor de asserção)"** do Scout ajudar, começa com `https://` 

    > [!NOTE] 
    > Os valores estes URLs são para apenas demonstração. Tem de atualizar estes valores a partir do URL de resposta e o identificador real. Obtém estes valores a partir da **Single Sign-On** separador na seção de autenticação, o que é explicada mais tarde no tutorial.

1. Se desejar configurar a aplicação no **SP** modo iniciado, verificação **Mostrar definições de URL avançadas** e executar o passo seguinte:

    ![Ajudar a Scout domínio e URLs únicas início de sessão em informações](./media/helpscout-tutorial/tutorial_helpscout_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL como: `https://secure.helpscout.net/members/login/`
     
1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/helpscout-tutorial/tutorial_helpscout_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/helpscout-tutorial/tutorial_general_400.png)


1. Sobre o **ajudar a configuração do Scout** secção, clique em **Scout de ajudar a configurar** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir a **seção de referência rápida**.

    ![Configurar o início de sessão único](./media/helpscout-tutorial/config.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa ajudar Scout como administrador.

1. Assim que está conectado na, clique em **"Gerir"** no menu superior e, em seguida, selecione **"Empresa"** no menu pendente.

    ![Configurar o início de sessão único](./media/helpscout-tutorial/settings1.png) 
 
1. Selecione **"Autenticação"** no menu esquerdo. 

    ![Configurar o início de sessão único](./media/helpscout-tutorial/settings2.png) 

1. Isto leva-o para a secção de definições de SAML e execute os seguintes passos:

    ![Configurar o início de sessão único](./media/helpscout-tutorial/settings3.png) 
 
    a. Copiar o **pós-back URL (URL do serviço de consumidor de asserção)** valor e cole o valor no **URL de resposta** caixa no portal do Azure, em ajudar a Scout **domínio e URLs** secção.
    
    b. Cópia a **URI de audiência (ID de entidade do fornecedor de serviço)** valor e cole o valor no **identificador** caixa no portal do Azure, em ajudar a Scout **domínio e URLs** secção.

1. Botão de alternar **ativar SAML** no e execute os seguintes passos:

    ![Configurar o início de sessão único](./media/helpscout-tutorial/settings4.png) 
 
    a. Na **URL de início de sessão único** caixa de texto, cole o valor de **único URL de início de sessão no serviço**, que copiou do portal do Azure.
    
    b. Clique em **carregar certificado** para carregar o **Certificate(Base64)** transferido a partir do portal do Azure.

    c. Introduza a e-mail domínios e.x. - sua organização `contoso.com` no **domínios de E-Mail** caixa de texto. É possível separar vários domínios com uma vírgula. Em qualquer altura um utilizador de Scout ajudar ou administrador que insere esse domínio específico no [Scout ajudam a iniciar sessão na página](https://secure.helpscout.net/members/login/) serão encaminhados para o fornecedor de identidade para autenticar com as respetivas credenciais.

    d. Por último, pode alternar **início de sessão SAML força em** se pretende que os utilizadores apenas iniciar sessão para ajudar a Scout através de através deste método. Se ainda quiser deixar a opção para os mesmos iniciar sessão com as credenciais do Scout ajudar, pode deixá-lo ativado/desativado desativado. Mesmo se esta opção estiver ativada, um proprietário da conta sempre será capaz de iniciar sessão para ajudar a Scout, com a palavra-passe de conta.

    e. Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/helpscout-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/helpscout-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/helpscout-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/helpscout-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-help-scout-test-user"></a>Criar um utilizador de teste de ajudar a Scout

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida na ajuda Scout. Ajuda do Scout suporta o aprovisionamento de just-in-time, que está por predefinição, ativada.

Não existe nenhum item de ação para nesta secção. Se um utilizador já não existir no Scout ajudar, uma nova é criada quando tentar acessar ajudar Scout.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para ajudar a Scout.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida para ajudar a Scout, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **ajudar Scout**.

    ![A ligação de ajuda Scout na lista de aplicações](./media/helpscout-tutorial/tutorial_helpscout_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do Scout ajudar no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de ajudar a Scout.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/helpscout-tutorial/tutorial_general_01.png
[2]: ./media/helpscout-tutorial/tutorial_general_02.png
[3]: ./media/helpscout-tutorial/tutorial_general_03.png
[4]: ./media/helpscout-tutorial/tutorial_general_04.png

[100]: ./media/helpscout-tutorial/tutorial_general_100.png

[200]: ./media/helpscout-tutorial/tutorial_general_200.png
[201]: ./media/helpscout-tutorial/tutorial_general_201.png
[202]: ./media/helpscout-tutorial/tutorial_general_202.png
[203]: ./media/helpscout-tutorial/tutorial_general_203.png

