---
title: 'Tutorial: Integração do Azure Active Directory com o SharePoint no local | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o SharePoint no local.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 85b8d4d0-3f6a-4913-b9d3-8cc327d8280d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2018
ms.author: jeedes
ms.openlocfilehash: 954eec8566173dd4707926d5f713a5cd509bdd9b
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/29/2018
ms.locfileid: "52620069"
---
# <a name="tutorial-azure-active-directory-integration-with-sharepoint-on-premises"></a>Tutorial: Integração do Azure Active Directory com o SharePoint no local

Neste tutorial, saiba como integrar o SharePoint no local com o Azure Active Directory (Azure AD).

Integrar o SharePoint no local com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao SharePoint no local.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o SharePoint no local (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o SharePoint no local, terá dos seguintes itens:

- Uma subscrição do Azure
- SharePoint no local único início de sessão ativado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar o SharePoint no local a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-sharepoint-on-premises-from-the-gallery"></a>Adicionar o SharePoint no local a partir da Galeria

Para configurar a integração do SharePoint no local com o Azure AD, terá de adicionar SharePoint no local a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o SharePoint no local a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **SharePoint no local**, selecione **SharePoint no local** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![SharePoint no local na lista de resultados](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o SharePoint no local com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no SharePoint no local para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no SharePoint no local tem de ser estabelecida.

Para configurar e testar o Azure AD início de sessão único com o SharePoint no local, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Conceder acesso ao utilizador de teste do SharePoint no local](#grant-access-to-sharePoint-on-premises-test-user)**  - para ter um equivalente da Eduarda Almeida no SharePoint no local que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo do SharePoint no local.

**Para configurar o Azure AD início de sessão único com o SharePoint no local, execute os seguintes passos:**

1. No portal do Azure, sobre o **SharePoint no local** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_samlbase.png)

3. Sobre o **SharePoint no local de domínio e URLs** secção, execute os seguintes passos:

    ![SharePoint no local e URLs de domínio únicas início de sessão em informações](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_url1.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<YourSharePointServerURL>/_trust/default.aspx`

    b. Na **identificador** caixa de texto, escreva o URL: `urn:sharepoint:federation`

    c. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<YourSharePointServerURL>/_trust/default.aspx`

4. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_certificate.png)

    > [!Note]
    > Tenha em atenção o caminho de ficheiro para o qual transferiu o ficheiro de certificado, conforme necessário para utilizá-lo mais tarde no script do PowerShell para a configuração.

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media\sharepoint-on-premises-tutorial/tutorial_general_400.png)

6. Sobre o **SharePoint no local configuração** secção, clique em **configurar o SharePoint no local** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML** partir o **secção de referência rápida.** Para **único URL de início de sessão no serviço**, utilizar o valor seguinte padrão: `https://login.microsoftonline.com/_my_directory_id_/wsfed` 

    > [!Note]
    > _my_directory_id_ é o id de inquilino da subscrição do Azure Ad.

    ![Configuração do SharePoint no local](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_configure.png)

    > [!NOTE]
    > Aplicação do SharePoint no local utiliza SAML 1.1 token, para que o Azure AD espera pedido WS Fed do servidor do SharePoint e após a autenticação, que emite o SAML 1.1. token.

7. Numa janela do browser web diferente, inicie sessão no seu site do SharePoint no local da empresa como administrador.

8. **Configurar um novo provedor de identidade fidedignos no SharePoint Server 2016**

    Inicie sessão no servidor do SharePoint Server 2016 e abra a Shell de gestão do SharePoint 2016. Preencha os valores de $realm (valor do identificador da SharePoint no local de domínio e URLs de seção no portal do Azure), $wsfedurl (único início de sessão no URL do serviço) e $filepath (caminho de ficheiro para o qual transferiu o ficheiro de certificado) do Azure, portal e execução os seguintes comandos para configurar um novo provedor de identidade fidedignos.

    > [!TIP]
    > Se estiver familiarizado com o com o PowerShell ou desejar saber mais sobre o funcionamento do PowerShell, veja [SharePoint PowerShell](https://docs.microsoft.com/powershell/sharepoint/overview?view=sharepoint-ps). 

    ```
    $realm = "<Identifier value from the SharePoint on-premises Domain and URLs section in the Azure portal>"
    $wsfedurl="<SAML single sign-on service URL value which you have copied from the Azure portal>"
    $filepath="<Full path to SAML signing certificate file which you have downloaded from the Azure portal>"
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($filepath)
    New-SPTrustedRootAuthority -Name "AzureAD" -Certificate $cert
    $map = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
    $map2 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname" -IncomingClaimTypeDisplayName "GivenName" -SameAsIncoming
    $map3 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" -IncomingClaimTypeDisplayName "SurName" -SameAsIncoming
    $map4 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress" -IncomingClaimTypeDisplayName "Email" -SameAsIncoming
    $ap = New-SPTrustedIdentityTokenIssuer -Name "AzureAD" -Description "SharePoint secured by Azure AD" -realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map,$map2,$map3,$map4 -SignInUrl $wsfedurl -IdentifierClaim "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"
    ```

    Em seguida, siga estes passos para ativar o fornecedor de identidade fidedignos para a sua aplicação:

    a. Na Administração Central, navegue até **Manage Web Application** e selecionar a aplicação web que pretende proteger com o Azure AD.

    b. No Friso, clique em **provedores de autenticação** e escolha a zona que deseja usar.

    c. Selecione **fornecedor de identidade fidedignos** e selecione o fornecedor de identidade que acabou de registar com o nome *AzureAD*.

    d. Sobre a definição de URL da página de início de sessão, selecione **página de início de sessão personalizada** e forneça o valor "/_trust/".

    e. Clique em **OK**.

    ![Configurar o seu fornecedor de autenticação](./media\sharepoint-on-premises-tutorial/fig10-configauthprovider.png)

    > [!NOTE]
    > Alguns dos utilizadores externos vão não é possível utilizar esta integração de início de sessão única, tal como respetivo UPN irá ter mangled valor algo como `MYEMAIL_outlook.com#ext#@TENANT.onmicrosoft.com`. Em breve, permitirá aos clientes configuração da aplicação sobre como lidar com o UPN, dependendo do tipo de utilizador. Depois disso, todos os seus utilizadores de convidado devem ser capazes de utilizar SSO de forma totalmente integrada como os funcionários da organização.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media\sharepoint-on-premises-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media\sharepoint-on-premises-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media\sharepoint-on-premises-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media\sharepoint-on-premises-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="grant-access-to-sharepoint-on-premises-test-user"></a>Conceder acesso ao utilizador de teste do SharePoint no local

Os utilizadores que irão iniciar sessão no Azure AD e aceder ao SharePoint devem ser concedidos acesso à aplicação utilize os seguintes passos para definir as permissões para aceder à aplicação web.

1. Na Administração Central, clique em **gestão de aplicações**.

2. No **gestão de aplicações** página, além da **aplicativos Web** secção, clique em **gerenciar aplicativos web**.

3. Clique na aplicação web apropriado e, em seguida, clique em **política de utilizador**.

4. Na política para a aplicação Web, clique em **adicionar usuários**.

    ![Procurar um utilizador, por sua declaração de nome](./media\sharepoint-on-premises-tutorial/fig11-searchbynameclaim.png)

5. Na **adicionar utilizadores** caixa de diálogo, clique em da zona apropriada na **zonas**e, em seguida, clique em **seguinte**.

6. Na **política para a aplicação Web** caixa de diálogo a **aos utilizadores escolher** secção, clique no **procurar** ícone.

7. Na **encontrar** caixa de texto, tipo a **nome principal de utilizador** valor para a qual configurou o aplicativo do SharePoint no local no Azure AD e clique em **pesquisa**. </br>Exemplo: *brittasimon@contoso.com*.

8. Sob o cabeçalho do AzureAD na vista de lista, selecione a propriedade de nome e clique em **Add** , em seguida, clique em **OK** para fechar a caixa de diálogo.

9. Em permissões, clique em **controlo total**.

    ![Conceder controle total a um utilizador de afirmações](./media\sharepoint-on-premises-tutorial/fig12-grantfullcontrol.png)

10. Clique em **Finish**e, em seguida, clique em **OK**.

### <a name="configuring-one-trusted-identity-provider-for-multiple-web-applications"></a>Configurar um fornecedor de identidade fidedignos para várias aplicações web

A configuração funciona para um aplicativo web único, mas precisa configuração adicional, se pretende usar o mesmo provedor de identidade fidedignos para várias aplicações web. Por exemplo, suponha estendida uma aplicação web para utilizar o URL `https://portal.contoso.local` e agora pretende autenticar os utilizadores para `https://sales.contoso.local` também. Para fazer isso, é necessário atualizar o fornecedor de identidade para honrar o parâmetro WReply e atualizar o registo de aplicação no Azure AD para adicionar um URL de resposta.

1. No Portal do Azure, abra o diretório do Azure AD. Clique em **registos de aplicações**, em seguida, clique em **ver todas as aplicações**. Clique na aplicação que criou anteriormente (integração de SAML do SharePoint).

2. Clique em **definições**.

3. No painel Definições, clique em **URLs de resposta**. 

4. Adicionar o URL para a aplicação web adicionais com `/_trust/default.aspx` anexado ao URL (por exemplo, `https://sales.contoso.local/_trust/default.aspx`) e clique em **guardar**.

5. No servidor do SharePoint, abra a **Shell de gestão do SharePoint 2016** e execute os seguintes comandos, utilizando o nome do emissor de tokens de identidade fidedignos que utilizou anteriormente.

    ```
    $t = Get-SPTrustedIdentityTokenIssuer "AzureAD"
    $t.UseWReplyParameter=$true
    $t.Update()
    ```
6. Na Administração Central, vá para a aplicação web e ativar o fornecedor de identidade fidedignos existente. Lembre-se também de configurar o URL da página de início de sessão como uma página de início de sessão personalizado `/_trust/`.

7. Na Administração Central, clique na aplicação web e escolha **política de utilizador**. Adicione um utilizador com as permissões adequadas, como demonstrado anteriormente neste artigo.

### <a name="fixing-people-picker"></a>Corrigir o selecionador de pessoas

Os utilizadores agora podem iniciar sessão no SharePoint 2016 através de identidades do Azure AD, mas ainda há oportunidades para aprimoramento da experiência do usuário. Por exemplo, um utilizador a procurar apresenta vários resultados de pesquisa no selecionador de pessoas. Existe um resultado de pesquisa para cada um dos tipos de 3 afirmação que foram criados no mapeamento de declaração. Para escolher um utilizador com o Seletor de pessoas, tem de introduzir o respetivo nome de utilizador exatamente e escolha o **nome** resultado de afirmação.

![Os resultados de pesquisa de afirmações](./media\sharepoint-on-premises-tutorial/fig16-claimssearchresults.png)

Não existe nenhuma validação nos valores que procurar, que pode levar a erros de ortografia ou tipo para atribuir como de afirmação de escolher acidentalmente incorretos de utilizadores do **Apelido** de afirmação. Isso pode impedir que os utilizadores acedam a recursos com êxito.

Para ajudá-lo com este cenário, há uma código-fonte aberto chamada de solução [AzureCP](https://yvand.github.io/AzureCP/) que fornece um fornecedor de afirmações personalizadas para o SharePoint 2016. Irá utilizar o Azure AD Graph para resolver o que os utilizadores que introduzam e efetuarem a validação. Saiba mais em [AzureCP](https://yvand.github.io/AzureCP/).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao SharePoint no local.

![Atribuir a função de utilizador][200]

**Para atribuir a Eduarda Almeida ao SharePoint no local, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **SharePoint no local**.

    ![A ligação do SharePoint na lista de aplicações](./media\sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_app.png)

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico no local do SharePoint no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação no local do SharePoint.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)
* [Utilizar o Azure AD para autenticação de servidor do SharePoint](https://docs.microsoft.com/office365/enterprise/using-azure-ad-for-sharepoint-server-authentication)

<!--Image references-->

[1]: ./media/sharepoint-on-premises-tutorial/tutorial_general_01.png
[2]: ./media/sharepoint-on-premises-tutorial/tutorial_general_02.png
[3]: ./media/sharepoint-on-premises-tutorial/tutorial_general_03.png
[4]: ./media/sharepoint-on-premises-tutorial/tutorial_general_04.png

[100]: ./media/sharepoint-on-premises-tutorial/tutorial_general_100.png

[200]: ./media/sharepoint-on-premises-tutorial/tutorial_general_200.png
[201]: ./media/sharepoint-on-premises-tutorial/tutorial_general_201.png
[202]: ./media/sharepoint-on-premises-tutorial/tutorial_general_202.png
[203]: ./media/sharepoint-on-premises-tutorial/tutorial_general_203.png
