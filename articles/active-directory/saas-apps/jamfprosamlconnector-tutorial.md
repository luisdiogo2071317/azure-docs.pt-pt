---
title: 'Tutorial: Integração do Azure Active Directory com o Jamf Pro | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Jamf Pro.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2018
ms.author: jeedes
ms.openlocfilehash: 94b8b935728110cd5dd07b2066e8320274e3b082
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39428422"
---
# <a name="tutorial-azure-active-directory-integration-with-jamf-pro"></a>Tutorial: Integração do Azure Active Directory com o Jamf Pro

Neste tutorial, saiba como integrar o Jamf Pro com o Azure Active Directory (Azure AD).

Integrar o Jamf Pro com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Jamf Pro.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Jamf Pro (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Jamf Pro, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Jamf Pro logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Jamf Pro da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-jamf-pro-from-the-gallery"></a>Adicionando o Jamf Pro da Galeria
Para configurar a integração do Jamf Pro com o Azure AD, terá de adicionar o Jamf Pro a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o Jamf Pro a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Jamf Pro**, selecione **Jamf Pro** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![O Jamf Pro na lista de resultados](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Jamf Pro com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no Jamf Pro para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Jamf Pro deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com o Jamf Pro, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste do Jamf Pro](#create-a-jamf-pro-test-user)**  - para ter um equivalente da Eduarda Almeida no Jamf Pro que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do Jamf Pro.

**Para configurar o Azure AD início de sessão único com o Jamf Pro, execute os seguintes passos:**

1. No portal do Azure, sobre o **Jamf Pro** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_samlbase.png)

1. Na **domínio de Jamf Pro e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação na **IDP** iniciada pelo modo:

    ![Domínio de Jamf Pro e URLs únicas início de sessão em informações](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url.png)

    a. Na **identificador (ID de entidade)** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.jamfcloud.com/saml/metadata`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.jamfcloud.com/saml/SSO`

1. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Domínio de Jamf Pro e URLs únicas início de sessão em informações](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.jamfcloud.com`
     
    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Obterá o valor do identificador real da **Single Sign-On** secção no portal do Jamf Pro, que é explicado mais tarde no tutorial. É possível extrair o real **subdomínio** valor do valor de identificador e usá-lo **subdomínio** informações no URL de início de sessão e o URL de resposta.

1. Sobre o **certificado de assinatura SAML** secção, clique no botão de cópia para copiar **Url de metadados de Federação de aplicação** e cole-o no bloco de notas.

    ![O link de download de certificado](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/jamfprosamlconnector-tutorial/tutorial_general_400.png)
    
1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Jamf Pro como um administrador.

1. Clique nas **ícone de definições** do canto superior direito da página.

    ![Configuração do Jamf Pro](./media/jamfprosamlconnector-tutorial/configure1.png)

1. Clique em **início de sessão único**.

    ![Configuração do Jamf Pro](./media/jamfprosamlconnector-tutorial/configure2.png)

1. Sobre o **Single Sign-On** página execute os seguintes passos:

    ![O Jamf Pro único](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_single.png)

    a. Selecione **servidor de Jamf Pro** para ativar o acesso de início de sessão único.

    b. Selecionando **omissão de permissões para todos os utilizadores** os utilizadores não serão redirecionados para a página de início de sessão do fornecedor de identidade para a autenticação, mas pode iniciar sessão no Jamf Pro diretamente em vez disso. Quando um usuário tentar acessar o Jamf Pro através do fornecedor de identidade, autorização e autenticação de SSO iniciado por IdP ocorre.

    c. Selecione o **NameID** opção para **mapeamento de utilizadores: SAML**. Por predefinição, esta definição está definida como **NameID** , mas pode definir um atributo personalizado.

    d. Selecione **E-Mail** para **mapeamento de utilizador: o JAMF PRO**. O Jamf Pro mapeia os atributos SAML enviados pelo IdP das seguintes formas: por utilizadores e grupos. Quando um usuário tenta acessar o Jamf Pro, por predefinição o Jamf Pro obtém informações sobre o utilizador a partir do fornecedor de identidade e compara com algo de contas de utilizador do Jamf Pro. Se a conta de utilizador de entrada não existir no Jamf Pro, em seguida, a correspondência de nomes de grupo ocorre.

    e. Cole o valor `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` no **o nome de ATRIBUTO de grupo** caixa de texto.
 
1. Sobre o mesmo desloque-se de página para baixo até **fornecedor de identidade** sob a **Single Sign-On** secção e execute os seguintes passos:

    ![Configuração do Jamf Pro](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. Selecione **outras** como uma opção a partir do **fornecedor de identidade** lista pendente.

    b. Na **outros fornecedor** caixa de texto, introduza **do Azure AD**.

    c. Selecione **URL de metadados** como uma opção da **origem de METADADOS do fornecedor de identidade** menu pendente e, na caixa de texto seguinte, cole o **Url de metadados de Federação de aplicação** valor que copiou do portal do Azure.

    d. Cópia a **ID de entidade** valor e cole-o no **identificador (ID de entidade)** caixa de texto no **domínio de Jamf Pro e URLs** secção no portal do Azure.

    >[!NOTE]
    > Este valor tênues é a parte de subdomínio. Utilize este valor para concluir o início de sessão no URL e o URL de resposta no **domínio de Jamf Pro e URLs** secção no portal do Azure.

    e. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/jamfprosamlconnector-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/jamfprosamlconnector-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/jamfprosamlconnector-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/jamfprosamlconnector-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-jamf-pro-test-user"></a>Criar um utilizador de teste do Jamf Pro

Para ativar a utilizadores do Azure AD iniciar sessão no Jamf Pro, tem de ser aprovisionados no Jamf Pro. No caso do Jamf Pro, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa Jamf Pro como administrador.

1. Clique nas **ícone de definições** do canto superior direito da página.

    ![Adicionar o funcionário](./media/jamfprosamlconnector-tutorial/configure1.png)

1. Clique em **contas de utilizador do Jamf Pro & grupos**.

    ![Adicionar o funcionário](./media/jamfprosamlconnector-tutorial/user1.png)

1. Clique em **Novo**.

    ![Adicionar o funcionário](./media/jamfprosamlconnector-tutorial/user2.png)

1. Selecione **criar conta Standard**.

    ![Adicionar o funcionário](./media/jamfprosamlconnector-tutorial/user3.png)

1. Sobre o **nova conta** dailog, execute os seguintes passos:

    ![Adicionar o funcionário](./media/jamfprosamlconnector-tutorial/user4.png)

    a. Na **nome de utilizador** caixa de texto, escreva o nome completo do BrittaSimon.

    b. Selecione as opções apropriadas de acordo com a sua organização para o **nível de acesso**, **PRIVILÉGIO definir**e para **estado de acesso**.
    
    c. Na **FULLNAME** caixa de texto, escreva o nome completo da Eduarda Almeida.

    d. Na **endereço de E-Mail** caixa de texto, escreva o endereço de e-mail da conta da Eduarda Almeida.

    e. Na **palavra-passe** caixa de texto, escreva a palavra-passe do utilizador.

    f. Na **Verifique se a palavra-passe** caixa de texto, escreva a palavra-passe do utilizador.

    g. Clique em **Guardar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para o Jamf Pro.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida para Jamf Pro, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Jamf Pro**.

    ![A ligação do Jamf Pro na lista de aplicações](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do Jamf Pro no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do Jamf Pro.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/jamfprosamlconnector-tutorial/tutorial_general_01.png
[2]: ./media/jamfprosamlconnector-tutorial/tutorial_general_02.png
[3]: ./media/jamfprosamlconnector-tutorial/tutorial_general_03.png
[4]: ./media/jamfprosamlconnector-tutorial/tutorial_general_04.png

[100]: ./media/jamfprosamlconnector-tutorial/tutorial_general_100.png

[200]: ./media/jamfprosamlconnector-tutorial/tutorial_general_200.png
[201]: ./media/jamfprosamlconnector-tutorial/tutorial_general_201.png
[202]: ./media/jamfprosamlconnector-tutorial/tutorial_general_202.png
[203]: ./media/jamfprosamlconnector-tutorial/tutorial_general_203.png

