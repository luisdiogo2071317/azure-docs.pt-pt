---
title: "Tutorial: Integração do Azure Active Directory com Confluence SAML SSO pela Microsoft | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e Confluence SAML SSO pela Microsoft."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1ad1cf90-52bc-4b71-ab2b-9a5a1280fb2d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: jeedes
ms.openlocfilehash: 22189c3d2d2164ba0fa3c2d790c36361fb0f5854
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="tutorial-azure-active-directory-integration-with-confluence-saml-sso-by-microsoft"></a>Tutorial: Integração do Azure Active Directory com Confluence SAML SSO pela Microsoft

Neste tutorial, irá aprender integrar Confluence SAML SSO pelo Microsoft Azure Active Directory (Azure AD).

Integrar Confluence SAML SSO pela Microsoft com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Confluence SAML SSO pela Microsoft
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Confluence SAML SSO pela Microsoft (Single Sign-On) com as respetivas contas do Azure AD
- Pode gerir as contas numa localização central - portal do Azure

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="description"></a>Descrição:

Utilize a sua conta do Microsoft Azure Active Directory com o servidor de Atlassian Confluence para ativar o início de sessão único. Desta forma todos os utilizadores da organização podem utilizar as credenciais do Azure AD para iniciar sessão na aplicação Confluence. Este plug-in utiliza SAML 2.0 para Federação.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Confluence SAML SSO pela Microsoft, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Aplicação de servidor confluence instalada num servidor Windows de 64 bits (no local ou na nuvem de IaaS infraestrutura)
- Servidor confluence é ativadas por HTTPS
- Tenha em atenção que as versões suportadas para o plug-in de Confluence mencionadas abaixo secção.
- Servidor confluence está acessível na internet particularmente a página de início de sessão do Azure AD para autenticação e deve poder receber o token do Azure AD
- Credenciais de administrador estão configuradas em Confluence
- WebSudo está desativada na Confluence
- Utilizador de teste criado na aplicação de servidor Confluence

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção de Confluence. Testar a integração pela primeira vez no desenvolvimento ou de ambiente de teste da aplicação e, em seguida, utilizar o ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma avaliação de um mês aqui: [oferta de avaliação](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-confluence"></a>Versões suportadas do Confluence 

A partir de agora, são suportadas as seguintes versões do Confluence:

- Confluence: 5.0 à versão 5.10

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Confluence SAML SSO pela Microsoft a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-confluence-saml-sso-by-microsoft-from-the-gallery"></a>Adicionar Confluence SAML SSO pela Microsoft a partir da Galeria
Para configurar a integração de Confluence SAML SSO pela Microsoft com o Azure AD, tem de adicionar Confluence SAML SSO pela Microsoft da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Confluence SAML SSO pela Microsoft a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![Aplicações][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Confluence SAML SSO pela Microsoft**.

    ![Criar um utilizador de teste do Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_search.png)

5. No painel de resultados, selecione **Confluence SAML SSO pela Microsoft**e, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD de sessão único-
Nesta secção, configure e teste do Azure AD-início de sessão único com Confluence SAML SSO pela Microsoft baseada-se um utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo em Confluence SAML SSO pela Microsoft é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Confluence SAML SSO pela Microsoft tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Confluence SAML SSO pela Microsoft, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um Confluence SAML SSO pelo utilizador de teste do Microsoft](#creating-a-confluence-saml-sso-by-microsoft-test-user)**  - para ter um homólogo de Britta Simon Confluence SAML SSO pela Microsoft que está ligada a representação do Azure AD do utilizador.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, ativar o Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua Confluence SAML SSO aplicações da Microsoft.

**Para configurar o Azure AD-início de sessão único com Confluence SAML SSO pela Microsoft, execute os seguintes passos:**

1. No portal do Azure, no **Confluence SAML SSO pela Microsoft** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar o início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_samlbase.png)

3. No **Confluence SAML SSO ao Microsoft Domain e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_url.png)

    a. No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://<domain:port>/plugins/servlet/saml/auth`

    b. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://<domain:port>/`

    c. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. A porta é opcional se for um URL com nome. Estes valores são recebidos durante a configuração de Confluence Plug-in do, que é explicada mais tarde no tutorial.
 

4. Para gerar o **metadados** url, execute os seguintes passos:

    a. Clique em **registos de aplicação**.
    
    ![Configurar o início de sessão único](./media/active-directory-saas-Confluencemicrosoft-tutorial/appregistrations.png)
   
    b. Clique em **pontos finais** para abrir **pontos finais** caixa de diálogo.  
    
    ![Configurar o início de sessão único](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpointicon.png)

    c. Clique no botão Copiar para copiar **documento de METADADOS de Federação** url e cole-o bloco de notas.
    
    ![Configurar o início de sessão único](./media/active-directory-saas-Confluencemicrosoft-tutorial/endpoint.png)
     
    d. Agora, a página de propriedades do **Confluence SAML SSO pela Microsoft** e copie o **Id da aplicação** utilizando **cópia** botão e cole-o bloco de notas.
 
    ![Configurar o início de sessão único](./media/active-directory-saas-Confluencemicrosoft-tutorial/appid.png)

    e. Gerar o **URL de metadados** através do padrão de seguinte: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>` e copie este valor no bloco de notas, porque é utilizado mais tarde para a configuração do plug-in.

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/active-directory-saas-Confluencemicrosoft-tutorial/tutorial_general_400.png)

6. Numa janela do browser web diferente, inicie sessão à sua instância Confluence como administrador.

7. Paire o rato sobre o ícone e clique em de **suplementos**.
    
    ![Configurar o início de sessão único](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon1.png)

8. Transfira o plug-in do [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=56503). Carregar manualmente o plug-in fornecido através do Microsoft **carregar o suplemento** menu
    
    ![Configurar o início de sessão único](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon12.png)

9. Assim que o plug-in estiver instalado, é apresentado no **utilizador instalado** secção suplementos **Gerir suplemento** secção. Clique em **configurar** para configurar o plug-in de novo.
    
    ![Configurar o início de sessão único](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon13.png)

10. Execute os seguintes passos na página de configuração:

    ![Configurar o início de sessão único](./media/active-directory-saas-Confluencemicrosoft-tutorial/addon52.png)
 
    > [!TIP]
    > Certifique-se de que existe apenas um certificado mapeado contra a aplicação para que não há nenhum erro de resolver os metadados. Se existirem vários certificados, administrador obtém um erro ao resolver os metadados.

    a. No **URL de metadados** colar o **URL de metadados** gerado a partir do Azure AD e clique em de **resolver** botão. Lê o URL de metadados do IdP e preenche a todas as informações de campos.

    b. Copiar o **identificador, o URL de resposta e o URL de início de sessão** valores e cole-os no **identificador, o URL de resposta e o URL de início de sessão** caixas de texto em respetivamente **Confluence SAML SSO ao Microsoft Domain e URLs** secção no portal do Azure.

    c. No **nome do botão de início de sessão** escreva o nome do botão para a organização quer que os utilizadores vejam no ecrã de início de sessão.

    d. No **localizações de ID de utilizador de SAML**, selecione **ID de utilizador está a ser o elemento de NameIdentifier da declaração de assunto** ou **ID de utilizador está a ser um elemento de atributo**.  Este ID tem de ser o id de utilizador Confluence. Se o id de utilizador não corresponde, sistema não permitirá que os utilizadores iniciem sessão. 

    > [!Note]
    > Localização de ID de utilizador de SAML predefinida é o identificador de nome. Pode alterar isto para uma opção de atributo e introduza o nome do atributo adequado.
    
    e. Se selecionar **ID de utilizador está a ser um elemento de atributo** opção, em seguida, no **nome de atributo** caixa de texto, escreva o nome do atributo onde é esperado o Id de utilizador. 

    f. Se estiver a utilizar o domínio Federado (por exemplo, etc. do AD FS) com o Azure AD, em seguida, clique em de **ativar a deteção de Realm inicial** opção e configurar o **nome de domínio**.
    
    g. No **nome de domínio** escreva o nome de domínio aqui em caso do início de sessão baseado no AD FS.

    h. Verifique **ativar início de sessão único limite** se pretende terminar a sessão do Azure AD quando um utilizador terminar a sessão de Confluence. 

    i. Clique em **guardar** botão para guardar as definições.


> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No **portal do Azure**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_02.png) 

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_03.png) 

4. No **utilizador** diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/active-directory-saas-confluencemicrosoft-tutorial/create_aaduser_04.png) 

    a. No **nome** caixa de texto, tipo **BrittaSimon**.

    b. No **nome de utilizador** caixa de texto, tipo de **endereço de correio eletrónico** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e anote o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-confluence-saml-sso-by-microsoft-test-user"></a>Criar um Confluence SAML SSO pelo utilizador de teste da Microsoft

Para permitir que os utilizadores do Azure AD iniciem sessão nos Confluence no servidor no local, estes têm de ser aprovisionados para Confluence SAML SSO pela Microsoft. Confluence SAML SSO pela Microsoft, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu Confluence no servidor no local como administrador.

2. Paire o rato sobre o ícone e clique em de **gestão de utilizadores**.

    ![Adicionar empregado](./media/active-directory-saas-confluencemicrosoft-tutorial/user1.png) 

3. Na secção utilizadores, clique em **adicionar utilizadores** separador. No **adicionar um utilizador** diálogo página, execute os seguintes passos:

    ![Adicionar empregado](./media/active-directory-saas-confluencemicrosoft-tutorial/user2.png) 

    a. No **Username** caixa de texto, escreva a mensagem de e-mail do utilizador como Britta Simon.

    b. No **nome completo** caixa de texto, escreva o nome completo do utilizador como Britta Simon.

    c. No **E-Mail** caixa de texto, como o tipo de endereço de correio eletrónico do utilizador Brittasimon@contoso.com.

    d. No **palavra-passe** caixa de texto, escreva a palavra-passe para Britta Simon.

    e. Clique em **Confirmar palavra-passe** Reintroduza a palavra-passe.
    
    f. Clique em **adicionar** botão.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Confluence SAML SSO pela Microsoft.

![Atribua o utilizador][200] 

**Para atribuir Britta Simon a Confluence SAML SSO pela Microsoft, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Confluence SAML SSO pela Microsoft**.

    ![Configurar o início de sessão único](./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribua o utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![Atribua o utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Ao clicar o Confluence SAML SSO pelo mosaico da Microsoft no painel de acesso, deve obter automaticamente com sessão iniciada para sua Confluence SAML SSO por aplicações da Microsoft.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-confluencemicrosoft-tutorial/tutorial_general_203.png

