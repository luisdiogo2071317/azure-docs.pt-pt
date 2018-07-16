---
title: 'Tutorial: Integração do Azure Active Directory com confluência SAML SSO pela Microsoft | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e confluência SAML SSO pela Microsoft.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1ad1cf90-52bc-4b71-ab2b-9a5a1280fb2d
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 97b6507cb8c5a66b8b6ccbfccd2c838b4b3fb53a
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39053306"
---
# <a name="tutorial-azure-active-directory-integration-with-confluence-saml-sso-by-microsoft"></a>Tutorial: Integração do Azure Active Directory com confluência SAML SSO pela Microsoft

Neste tutorial, saiba como integrar confluência SAML SSO pela Microsoft no Azure Active Directory (Azure AD).

Integrar confluência SAML SSO pela Microsoft no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao confluência SAML SSO pela Microsoft
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para confluência SAML SSO pela Microsoft (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="description"></a>Descrição:

Utilize a sua conta do Microsoft Azure Active Directory com o servidor de confluência Atlassian para ativar o início de sessão único. Desta forma todos os seus utilizadores da organização podem utilizar as credenciais do Azure AD ao iniciar sessão na aplicação confluência. Este plug-in utiliza SAML 2.0 para a Federação.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com confluência SAML SSO pela Microsoft, precisa do seguinte:

- Uma subscrição do Azure AD
- Aplicação de servidor confluência instalada num servidor Windows de 64 bits (no local ou na cloud em infraestrutura IaaS)
- Servidor de confluência é ativadas por HTTPS
- Tenha em atenção de que as versões suportadas para o plug-in do confluência mencionadas abaixo de secção.
- Servidor de confluência está acessível na internet, especialmente para a página de início de sessão do Azure AD para autenticação e deve capaz de receber o token do Azure AD
- Credenciais de administrador são configuradas no confluência
- WebSudo está desativada na confluência
- Utilizador de teste criado no aplicativo de servidor confluência

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção de confluência. Teste a integração primeiro no desenvolvimento ou de ambiente de teste do aplicativo e, em seguida, utilize o ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma avaliação de um mês aqui: [oferta de avaliação](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-confluence"></a>Versões suportadas do confluência 

A partir de agora, os seguintes versões do confluência são suportadas:

- Confluência: 5.0 para 5.10

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar confluência SAML SSO pela Microsoft a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-confluence-saml-sso-by-microsoft-from-the-gallery"></a>Adicionar confluência SAML SSO pela Microsoft a partir da Galeria
Para configurar a integração do confluência SAML SSO pela Microsoft para o Azure AD, terá de adicionar confluência SAML SSO pela Microsoft a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar confluência SAML SSO pela Microsoft a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **confluência SAML SSO pela Microsoft**.

    ![Criar um utilizador de teste do Azure AD](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_search.png)

5. No painel de resultados, selecione **confluência SAML SSO pela Microsoft**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com confluência SAML SSO pela Microsoft com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de equivalente na confluência SAML SSO pela Microsoft para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na confluência SAML SSO pela Microsoft deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com confluência SAML SSO pela Microsoft, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um confluência SAML SSO por utilizador de teste da Microsoft](#creating-a-confluence-saml-sso-by-microsoft-test-user)**  - para ter um equivalente da Eduarda Almeida na confluência SAML SSO pela Microsoft que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão único no portal do Azure e configurar o início de sessão único na sua confluência SAML SSO pela aplicação da Microsoft.

**Para configurar o Azure AD início de sessão único com confluência SAML SSO pela Microsoft, execute os seguintes passos:**

1. No portal do Azure, sobre o **confluência SAML SSO pela Microsoft** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Configurar o início de sessão único](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_samlbase.png)

3. Sobre o **confluência SAML SSO pelo Microsoft Domain e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<domain:port>/plugins/servlet/saml/auth`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<domain:port>/`

    c. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. A porta é opcional, caso seja um URL com nome. Estes valores são recebidos durante a configuração de confluência Plug-in do, que é explicado mais tarde no tutorial.

4. Sobre o **certificado de assinatura SAML** secção, clique no botão de cópia para copiar **Url de metadados de Federação de aplicação** e cole-o no bloco de notas.
    
    ![Configurar o início de sessão único](./media/confluencemicrosoft-tutorial/tutorial_metadataurl.png)
     
5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/confluencemicrosoft-tutorial/tutorial_general_400.png)

6. Numa janela do browser web diferente, inicie sessão na sua instância de confluência como administrador.

7. Paire o rato sobre o ícone de roda dentada e clique nas **suplementos**.
    
    ![Configurar o início de sessão único](./media/confluencemicrosoft-tutorial/addon1.png)

8. Transfira o plug-in do [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=56503). Carregar manualmente o plug-in fornecido pela Microsoft usando **carregar o suplemento** menu. A transferência de plug-in é coberta [contrato de serviço do Microsoft](https://www.microsoft.com/en-us/servicesagreement/). 
    
    ![Configurar o início de sessão único](./media/confluencemicrosoft-tutorial/addon12.png)

9. Assim que o plug-in estiver instalado, ele aparece na **utilizador instalado** secção de suplementos do **suplemento gerir** secção. Clique em **configurar** para configurar o plug-in de novo.
    
    ![Configurar o início de sessão único](./media/confluencemicrosoft-tutorial/addon13.png)

10. Execute os seguintes passos na página de configuração:

    ![Configurar o início de sessão único](./media/confluencemicrosoft-tutorial/addon52.png)

    > [!TIP]
    > Certifique-se de que existe apenas um certificado mapeado em relação a aplicação, para que não há nenhum erro na resolução de metadados. Se existirem vários certificados, o administrador obtém um erro ao resolver os metadados.

    a. Na **URL de metadados** caixa de texto, colar **Url de metadados de Federação de aplicação** valor que copiou do portal do Azure e clique nas **resolver** botão. Lê o URL de metadados de IdP e preenche a todas as informações de campos.

    b. Copiar o **identificador, o URL de resposta e o URL de início de sessão** valores e cole-os na **identificador, o URL de resposta e o URL de início de sessão** respectivamente em caixas de texto **confluência SAML SSO pelo Microsoft Domain e URLs**  secção no portal do Azure.

    c. Na **nome do botão de início de sessão** escreva o nome do botão, a organização quer que os utilizadores para ver no ecrã de início de sessão.

    d. Na **locais de ID de usuário de SAML**, selecione **ID de utilizador é no elemento NameIdentifier da declaração do requerente** ou **ID de utilizador está num elemento de atributo**.  Este ID deve ser o id de utilizador confluência. Se o id de utilizador não for encontrado, sistema não permitirá que os utilizadores iniciem sessão. 

    > [!Note]
    > Localização de ID de utilizador de SAML predefinida é o identificador de nome. Pode alterá-la para uma opção de atributo e introduza o nome do atributo adequado.
    
    e. Se selecionou **ID de utilizador está num elemento de atributo** opção, em seguida, no **nome do atributo** caixa de texto, escreva o nome do atributo onde é esperado o Id de utilizador. 

    f. Se estiver a utilizar o domínio Federado (como etc. do AD FS) com o Azure AD, em seguida, clique nas **ativar a deteção de Realm inicial** opção e configurar o **nome de domínio**.
    
    g. Na **nome de domínio** escreva o nome de domínio aqui em caso do início de sessão baseado no AD FS.

    h. Verifique **ativar o início de sessão único horizontalmente** se deseja terminar sessão do Azure AD quando um utilizador terminar a sessão de confluência. 

    i. Clique em **guardar** botão para guardar as definições.

    > [!NOTE]
    > Para obter mais informações sobre a instalação e resolução de problemas, visite [Guia do administrador do conector MS confluência SSO](../ms-confluence-jira-plugin-adminguide.md) e também há [FAQ](../ms-confluence-jira-plugin-faq.md) para sua assistência

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/confluencemicrosoft-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/confluencemicrosoft-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/confluencemicrosoft-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/confluencemicrosoft-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-confluence-saml-sso-by-microsoft-test-user"></a>Criar um confluência SAML SSO por utilizador de teste da Microsoft

Para ativar a utilizadores do Azure AD iniciar sessão no servidor no local de confluência, eles têm de ser aprovisionados na confluência SAML SSO pela Microsoft. Confluência SAML SSO pela Microsoft, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu servidor no local de confluência como administrador.

2. Paire o rato sobre o ícone de roda dentada e clique nas **gestão de utilizadores**.

    ![Adicionar o funcionário](./media/confluencemicrosoft-tutorial/user1.png) 

3. Na secção utilizadores, clique em **adicionar utilizadores** separador. Sobre o **adicionar um utilizador** caixa de diálogo página, execute os seguintes passos:

    ![Adicionar o funcionário](./media/confluencemicrosoft-tutorial/user2.png) 

    a. Na **nome de utilizador** caixa de texto, escreva a mensagem de e-mail do utilizador, como a Eduarda Almeida.

    b. Na **FullName** caixa de texto, escreva o nome completo do utilizador, como a Eduarda Almeida.

    c. Na **E-Mail** caixa de texto, como o tipo de endereço de e-mail do utilizador Brittasimon@contoso.com.

    d. Na **palavra-passe** caixa de texto, escreva a palavra-passe para Eduarda Almeida.

    e. Clique em **Confirmar palavra-passe** Reintroduza a palavra-passe.
    
    f. Clique em **adicionar** botão.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para confluência SAML SSO pela Microsoft.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a confluência SAML SSO pela Microsoft, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **confluência SAML SSO pela Microsoft**.

    ![Configurar o início de sessão único](./media/confluencemicrosoft-tutorial/tutorial_confluencemicrosoft_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Ao clicar o confluência SAML SSO ao mosaico da Microsoft no painel de acesso, deve obter automaticamente com sessão iniciada para sua confluência SAML SSO pela aplicação da Microsoft.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/confluencemicrosoft-tutorial/tutorial_general_01.png
[2]: ./media/confluencemicrosoft-tutorial/tutorial_general_02.png
[3]: ./media/confluencemicrosoft-tutorial/tutorial_general_03.png
[4]: ./media/confluencemicrosoft-tutorial/tutorial_general_04.png

[100]: ./media/confluencemicrosoft-tutorial/tutorial_general_100.png

[200]: ./media/confluencemicrosoft-tutorial/tutorial_general_200.png
[201]: ./media/confluencemicrosoft-tutorial/tutorial_general_201.png
[202]: ./media/confluencemicrosoft-tutorial/tutorial_general_202.png
[203]: ./media/confluencemicrosoft-tutorial/tutorial_general_203.png
