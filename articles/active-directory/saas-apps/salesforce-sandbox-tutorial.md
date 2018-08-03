---
title: 'Tutorial: Integração do Azure Active Directory com a Sandbox da Salesforce | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Salesforce Sandbox.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 2e050b363db7ab1d226c5aa6fffefb17c218d377
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39424297"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a>Tutorial: Integração do Azure Active Directory com a Sandbox de Salesforce

Neste tutorial, saiba como integrar o Sandbox de Salesforce com o Azure Active Directory (Azure AD).

Integrar o Salesforce Sandbox no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Sandbox do Salesforce.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para a área de segurança do Salesforce (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com a Sandbox da Salesforce, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma área de segurança do Salesforce logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Sandbox do Salesforce a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-salesforce-sandbox-from-the-gallery"></a>Adicionar Sandbox do Salesforce a partir da Galeria
Para configurar a integração de Salesforce Sandbox para o Azure AD, terá de adicionar área de segurança do Salesforce a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar área de segurança do Salesforce a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Salesforce Sandbox**, selecione **Sandbox de Salesforce** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Sandbox de Salesforce na lista de resultados](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com a Sandbox da Salesforce com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Salesforce Sandbox para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na Sandbox do Salesforce tem de ser estabelecida.

Na área de segurança do Salesforce, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com a Sandbox da Salesforce, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de área de segurança do Salesforce](#create-a-salesforce-sandbox-test-user)**  - para ter um equivalente da Eduarda Almeida na Sandbox do Salesforce que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de área de segurança do Salesforce.

**Para configurar o Azure AD início de sessão único com a Sandbox da Salesforce, execute os seguintes passos:**

1. No portal do Azure, sobre o **Salesforce Sandbox** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

1. Sobre o **Salesforce Sandbox domínio e URLs** secção, execute os seguintes passos:

    ![Domínio de Sandbox do Salesforce e URLs únicas início de sessão em informações](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_url.png)

    a. Na **URL de início de sessão** caixa de texto, digite o valor usando o seguinte padrão: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    b. Na **identificador** caixa de texto, digite o valor usando o seguinte padrão: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`
    
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão real e o identificador. Contacte [equipa de suporte de cliente do Salesforce](https://help.salesforce.com/support) obter esses valores.

1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/salesforce-sandbox-tutorial/tutorial_general_400.png)

1. Na **configuração do Salesforce Sandbox** secção, clique em **configurar Sandbox de Salesforce** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_configure.png) 

1. Abra um novo separador no seu browser e inicie sessão sua conta de administrador da área de segurança do Salesforce.

1. Clique nas **programa de configuração** sob **ícone de definições** no canto superior direito da página.

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/configure1.png)

1. Desloque para baixo para o **configurações** no painel de navegação, clique em **identidade** para expandir a secção relacionada. Em seguida, clique em **definições de início de sessão único**.

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/sf-admin-sso.png)

1. Selecione **ativada SAML**e, em seguida, clique em **guardar**.

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/sf-enable-saml.png)

1. Para configurar o seu únicas início de sessão em definições de SAML, clique em **New**.

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/sf-admin-sso-new.png)

1. Na secção de SAML único configurações de logon, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/sf-saml-config.png)

    a. Na **Name** caixa de texto, escreva o nome da configuração (por exemplo: *SPSSOWAAD_Test*). 

    b. Na **emissor** campo, cole o valor de **ID de entidade de SAML**, que copiou do portal do Azure

    c. Na **Id de entidade** caixa de texto, tipo `https://<instancename>--Sandbox.<entityid>.my.salesforce.com` se for a primeira instância de Sandbox do Salesforce, o que está a adicionar ao seu diretório. Se já tiver adicionado uma instância da área de segurança do Salesforce, em seguida, para o **ID de entidade** escreva o **URL de início de sessão**, que deve estar no seguinte formato: `https://<instancename>--Sandbox.<entityid>.my.salesforce.com`

    d. Para carregar os **certificado do fornecedor de identidade**, clique em **Escolher ficheiro** para procurar e selecionar o ficheiro de certificado que transferiu a partir do portal do Azure.

    e. Como **tipo de identidade de SAML**, escolha uma das seguintes opções:

      * Selecione **asserção contém o nome do usuário Salesforce**, se o nome de utilizador do utilizador Salesforce está sendo passado na asserção de SAML

      * Selecione **asserção contém o ID de Federação do objeto de utilizador**, se o ID de Federação do objeto de utilizador que está sendo passado na asserção de SAML

      * Selecione **asserção contém o ID de uso do objeto de utilizador**, se o ID de utilizador do objeto de utilizador que está sendo passado na asserção de SAML

    f. Como **localização de identidade de SAML**, selecione **identidade é no elemento NameIdentifier da declaração de assunto**.

    g. Como **fornecedor iniciada solicitar vínculo de serviço**, selecione **HTTP POST**.

    h. Na **URL de início de sessão do fornecedor de identidade** caixa de texto, cole o valor de **único URL de início de sessão no serviço**, que copiou do portal do Azure.

    i. SFDC não suporta a fim de sessão SAML.  Como solução, cole `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0` -o para o **URL de fim de sessão do fornecedor de identidade** caixa de texto.

    j. Clique em **Guardar**.

### <a name="enable-your-domain"></a>Ativar o seu domínio

Esta secção assume que já criou um domínio.  Para obter mais informações, consulte [definir seu nome de domínio](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).

**Para ativar o seu domínio, execute os seguintes passos:**

1. No painel de navegação esquerdo no Salesforce, clique em **definições da empresa** para expandir a secção relacionada e, em seguida, clique em **meu domínio**.

    ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/sf-my-domain.png)

   >[!NOTE]
   >Certifique-se de que o seu domínio foi configurado corretamente.

1. Na **configuração da autenticação** secção, clique em **editar**, em seguida, como **serviço de autenticação**, selecione o nome SAML único início de sessão da definição do anterior secção e, finalmente, clique em **guardar**.

   ![Configurar o início de sessão único](./media/salesforce-sandbox-tutorial/sf-edit-auth-config.png)

Assim que tiver um domínio configurado, seus usuários devem usar o URL de domínio para início de sessão para a área de segurança do Salesforce.

Para obter o valor do URL, clique no perfil SSO que criou na secção anterior.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/salesforce-sandbox-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/salesforce-sandbox-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/salesforce-sandbox-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/salesforce-sandbox-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-salesforce-sandbox-test-user"></a>Criar um utilizador de teste de Sandbox de Salesforce

Nesta secção, um usuário chamado Eduarda Almeida é criado na área de segurança do Salesforce. Sandbox de Salesforce suporta o aprovisionamento de just-in-time, que está ativado por predefinição. Não existe nenhum item de ação para nesta secção. Se um utilizador já não existir na área de segurança do Salesforce, uma nova é criada quando tentar acessar a área de segurança do Salesforce. Sandbox de Salesforce também suporta o aprovisionamento automático de utilizadores, pode encontrar mais detalhes [aqui](salesforce-sandbox-provisioning-tutorial.md) sobre como configurar o aprovisionamento automático de utilizadores.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida a utilizar do Azure-início de sessão único, conceder acesso à área de segurança do Salesforce.

![Atribuir a função de utilizador][200]

**Para atribuir a Eduarda Almeida a área de segurança do Salesforce, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Salesforce Sandbox**.

    ![A ligação de área de segurança do Salesforce na lista de aplicações](./media/salesforce-sandbox-tutorial/tutorial_salesforcesandbox_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de área de segurança do Salesforce no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo de área de segurança do Salesforce.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)
* [Configurar o aprovisionamento do utilizador](salesforce-sandbox-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/salesforce-sandbox-tutorial/tutorial_general_01.png
[2]: ./media/salesforce-sandbox-tutorial/tutorial_general_02.png
[3]: ./media/salesforce-sandbox-tutorial/tutorial_general_03.png
[4]: ./media/salesforce-sandbox-tutorial/tutorial_general_04.png

[100]: ./media/salesforce-sandbox-tutorial/tutorial_general_100.png

[200]: ./media/salesforce-sandbox-tutorial/tutorial_general_200.png
[201]: ./media/salesforce-sandbox-tutorial/tutorial_general_201.png
[202]: ./media/salesforce-sandbox-tutorial/tutorial_general_202.png
[203]: ./media/salesforce-sandbox-tutorial/tutorial_general_203.png
