---
title: 'Tutorial: Integração do Azure Active Directory com o SharePoint no local | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e o SharePoint no local.
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
ms.date: 05/25/2018
ms.author: jeedes
ms.openlocfilehash: 1657d686337d067a8df9107b7d102fc34c1f1b8d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655294"
---
# <a name="tutorial-azure-active-directory-integration-with-sharepoint-on-premises"></a>Tutorial: Integração do Azure Active Directory com o SharePoint no local

Neste tutorial, irá aprender a integração do SharePoint no local com o Azure Active Directory (Azure AD).

Integração do SharePoint no local com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao SharePoint no local.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para SharePoint no local (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o SharePoint no local, terá dos seguintes itens:

- Uma subscrição do Azure AD
- SharePoint no local único início de sessão ativado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar SharePoint no local a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-sharepoint-on-premises-from-the-gallery"></a>Adicionar SharePoint no local a partir da Galeria
Para configurar a integração do SharePoint no local com o Azure AD, terá de adicionar SharePoint no local a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar SharePoint no local a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone.

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]

3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **SharePoint no local**, selecione **SharePoint no local** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![SharePoint no local na lista de resultados](./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com o SharePoint no local com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo do SharePoint no local é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no SharePoint no local tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com o SharePoint no local, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste do SharePoint no local](#create-a-sharePoint-on-premises-test-user)**  - para ter um homólogo de Britta Simon do SharePoint no local que está ligado a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação no local do SharePoint.

**Para configurar o Azure AD-início de sessão único com o SharePoint no local, execute os seguintes passos:**

1. No portal do Azure, no **SharePoint no local** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_samlbase.png)

3. No **SharePoint no local domínios e URLs** secção, execute os seguintes passos:

    ![SharePoint no local URLs e de domínio único início de sessão informações](./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_url1.png)

    a. No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<YourSharePointServerURL>/_trust/default.aspx`

    b. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `urn:sharepoint:<YourSharePointServerURL>`

    > [!NOTE]
    > Estes valores não estiverem reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [SharePoint no local a equipa de suporte do cliente](https://support.office.com/) para obter estes valores.

4. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador com a extensão. cer. Copie e cole o caminho completo do ficheiro de metadados transferido no bloco de notas.

    ![A hiperligação de transferência do certificado](./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_certificate.png)

5. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_400.png)

6. No **SharePoint no local configuração** secção, clique em **configurar o SharePoint no local** para abrir **configurar início de sessão** janela. Copiar o **URL Single Sign-On serviço** do **secção de referência rápida.**

    ![Configuração do SharePoint no local](./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_configure.png)

    > [!NOTE]
    > Aplicações do SharePoint no local utiliza SAML 1.1 token, pelo que o Azure AD espera pedido WS Fed do servidor do SharePoint e após a autenticação, que emite o 1.1 SAML. token.

7. Numa janela do browser web diferente, inicie sessão no seu site do SharePoint no local da empresa como administrador.

8. **Configurar um novo fornecedor de identidade fidedigna no SharePoint Server 2016**

    Inicie sessão no servidor do SharePoint Server 2016 e abra a Shell de gestão do SharePoint 2016. Preencha os valores de $realm, $wsfedurl e $filepath do portal do Azure e execute os seguintes comandos para configurar um novo fornecedor de identidade fidedigna.

    > [!TIP]
    > Se estiver familiarizado com o PowerShell ou se quiser saber mais sobre como funciona o PowerShell, consulte [SharePoint PowerShell](https://docs.microsoft.com/en-us/powershell/sharepoint/overview?view=sharepoint-ps). 

    ```
    $realm = "<Identifier value from the SharePoint on-premises Domain and URLs section in the Azure portal>"
    $wsfedurl="<SAML single sign-on service URL value which you have copied from the Azure portal>"
    $filepath="<Full path to SAML signing certificate file which you have copied from the Azure portal>"
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($filepath)
    New-SPTrustedRootAuthority -Name "AzureAD" -Certificate $cert
    $map = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
    $map2 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname" -IncomingClaimTypeDisplayName "GivenName" -SameAsIncoming
    $map3 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" -IncomingClaimTypeDisplayName "SurName" -SameAsIncoming
    $ap = New-SPTrustedIdentityTokenIssuer -Name "AzureAD" -Description "SharePoint secured by Azure AD" -realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map,$map2,$map3 -SignInUrl $wsfedurl -IdentifierClaim "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"
    ```

    Em seguida, siga estes passos para ativar o fornecedor de identidade fidedigna para a sua aplicação:

    a. Na Administração Central, navegue para **Gerir aplicação de Web** e selecione a aplicação web que pretende proteger com o Azure AD.

    b. No Friso, clique em **fornecedores de autenticação** e escolha a zona que pretende utilizar.

    c. Selecione **fornecedor de identidade fidedigna** e selecione o fornecedor de identificar que acabou de ser registado com o nome *AzureAD*.

    d. A definição do URL de página de início de sessão, selecione **página de início de sessão personalizada** e forneça o valor "/_trust/".

    e. Clique em **OK**.

    ![Configurar o fornecedor de autenticação](./media\active-directory-saas-sharepoint-on-premises-tutorial/fig10-configauthprovider.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media\active-directory-saas-sharepoint-on-premises-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media\active-directory-saas-sharepoint-on-premises-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media\active-directory-saas-sharepoint-on-premises-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media\active-directory-saas-sharepoint-on-premises-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-sharepoint-on-premises-test-user"></a>Criar um utilizador de teste do SharePoint no local

1. Na Administração Central, clique em **gestão de aplicações**.

2. No **gestão de aplicações** na página de **aplicações Web** secção, clique em **gerir aplicações web**.

3. Clique na aplicação web adequadas e, em seguida, clique em **política de utilizador**.

4. Política de aplicação Web, clique em **adicionar utilizadores**.

    ![Procurar um utilizador pelo respetiva afirmação de nome](./media\active-directory-saas-sharepoint-on-premises-tutorial/fig11-searchbynameclaim.png)

5. No **adicionar utilizadores** caixa de diálogo, clique em da zona apropriada no **zonas**e, em seguida, clique em **seguinte**.

6. No **política da aplicação Web** caixa de diálogo a **aos utilizadores escolher** secção, clique em de **procurar** ícone.

7. No **localizar** caixa de texto, escreva o nome de início de sessão para um utilizador no seu diretório e clique em **pesquisa**. </br>Exemplo: *demouser@blueskyabove.onmicrosoft.com*.

8. Sob o cabeçalho de AzureAD na vista de lista, seleccione a propriedade de nome e clique em **adicionar** , em seguida, clique em **OK** para fechar a caixa de diálogo.

9. Em permissões, clique em **controlo total**.

    ![Conceder controlo total para um utilizador de afirmações](./media\active-directory-saas-sharepoint-on-premises-tutorial/fig12-grantfullcontrol.png)

10. Clique em **concluir**e, em seguida, clique em **OK**.

### <a name="fixing-people-picker"></a>Corrigir selecionador de pessoas

Os utilizadores podem agora iniciar sessão para o SharePoint 2016 utilizar identidades do Azure AD, mas ainda existem oportunidades de melhoria à experiência de utilizador. Por exemplo, procurar um utilizador, apresenta os resultados da pesquisa vários no selecionador de pessoas. Não há um resultado de pesquisa para cada um dos tipos de 3 afirmação que foram criados no mapeamento de afirmação. Para escolher um utilizador utilizando o selecionador de pessoas, tem de escrever o respetivo nome de utilizador exatamente e escolha o **nome** resultado de afirmação.

![Resultados da pesquisa de afirmações](./media\active-directory-saas-sharepoint-on-premises-tutorial/fig16-claimssearchresults.png)

Não existe nenhuma validação nos valores de procurar, que pode levar a misspellings ou utilizadores acidentalmente escolher a incorreta afirmação tipo atribuir como o **Apelido** de afirmação. Isto pode impedir que os utilizadores acedem a recursos com êxito.

Para ajudá-lo com este cenário, é um open source solução denominada [AzureCP](https://yvand.github.io/AzureCP/) que fornece um fornecedor de afirmações personalizadas para o SharePoint 2016. Irá utilizar o Azure AD Graph para resolver a que os utilizadores podem introduzir e efetua a validação. Saiba mais em [AzureCP](https://yvand.github.io/AzureCP/).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso ao SharePoint no local.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon ao SharePoint no local, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201]

2. Na lista de aplicações, selecione **SharePoint no local**.

    ![A ligação do SharePoint na lista de aplicações](./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_sharepointonpremises_app.png)

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico do SharePoint no local no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação no local do SharePoint.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_01.png
[2]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_02.png
[3]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_03.png
[4]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_04.png

[100]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_100.png

[200]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_200.png
[201]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_201.png
[202]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_202.png
[203]: ./media\active-directory-saas-sharepoint-on-premises-tutorial/tutorial_general_203.png

