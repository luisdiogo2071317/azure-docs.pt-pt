---
title: 'Tutorial: Integração do Azure Active Directory com o Qlik sentido Enterprise | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Qlik sentido empresarial.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: b2c48c8c4566fb143f0a356f342d21580b1b6359
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39420262"
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Tutorial: Integração do Azure Active Directory com o Qlik sentido Enterprise

Neste tutorial, saiba como integrar o Qlik sentido empresarial no Azure Active Directory (Azure AD).

Integrar o Qlik sentido empresarial no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Qlik sentido empresarial.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para a empresa de sentido Qlik (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Qlik sentido empresarial, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma empresa de sentido Qlik logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma avaliação de um mês aqui: [oferta de avaliação](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Qlik sentido empresarial da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Adicionando Qlik sentido empresarial da Galeria
Para configurar a integração do Qlik sentido Enterprise no Azure AD, terá de adicionar Qlik sentido empresarial a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Qlik sentido empresarial a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Qlik sentido empresarial**, selecione **Qlik sentido empresarial** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Qlik sentido Enterprise na lista de resultados](./media/qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Qlik sentido empresarial com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Qlik sentido empresarial para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado Qlik sentido empresa deve ser estabelecido.

Empresa de sentido Qlik, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **nome de utilizador** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Qlik sentido empresarial, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Qlik sentido Enterprise](#create-a-qlik-sense-enterprise-test-user)**  - para ter um equivalente da Eduarda Almeida Qlik sentido empresa que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação Qlik sentido empresarial.

**Para configurar o Azure AD início de sessão único com Qlik sentido empresarial, execute os seguintes passos:**

1. No portal do Azure, sobre o **Qlik sentido empresarial** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_samlbase.png)

1. Sobre o **Qlik sentido empresarial domínio e URLs** secção, execute os seguintes passos:

    ![Qlik sentido empresarial domínio e URLs únicas início de sessão em informações](./media/qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`
    
    > [!NOTE]
    > Tenha em atenção a barra no final desse URI. É necessário.
    
    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão:
    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão real e o identificador, que são explicado posteriormente neste tutorial ou contacte [equipa de suporte de Qlik sentido Enterprise Client](https://www.qlik.com/us/services/support) obter esses valores. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/qliksense-enterprise-tutorial/tutorial_general_400.png)

1. Prepare o ficheiro XML de metadados de Federação para que pode carregar que para o servidor Qlik Sense.
   
    > [!NOTE]
    > Antes de carregar os metadados do IdP para o servidor de Qlik Sense, o ficheiro tem de ser editadas para remover as informações para garantir a operação adequada entre o Azure AD e Qlik Sense server.
    
    ![QlikSense][qs24]
   
    a. Abra o ficheiro de FederationMetaData.xml que transferiu a partir do portal do Azure num editor de texto.
   
    b. Procure o valor **RoleDescriptor**.  Existem quatro entradas (dois pares de abertura e fecho de marcas de elemento).
   
    c. Elimine entre as etiquetas de RoleDescriptor e todas as informações do ficheiro.
   
    d. Guarde o ficheiro e mantê-lo nas proximidades para utilização posterior neste documento.

1. Navegue para o Qlik sentido Qlik Management Console (QMC) como um utilizador que pode criar as configurações de virtual proxy.

1. No QMC, clique nas **Virtual Proxies** item de menu.
   
    ![QlikSense][qs6] 

1. Na parte inferior do ecrã, clique nas **criar novo** botão.
   
    ![QlikSense][qs7]

1. É apresentado o ecrã de edição de proxy Virtual.  No lado direito da tela é um menu para tornar as opções de configuração visível.
   
    ![QlikSense][qs9]

1. Com a opção de menu de identificação marcada, introduza as informações de identificação para a configuração de proxy virtual do Azure.
    
    ![QlikSense][qs8]  
    
    a. O **Descrição** campo é um nome amigável para a configuração de virtual proxy.  Introduza um valor para uma descrição.
    
    b. O **prefixo** campo identifica o ponto de extremidade do virtual proxy para ligar ao Qlik Sense com o Azure AD início de sessão único.  Introduza um nome de prefixo exclusivo para este proxy virtual.
    
    c. **Tempo limite de inatividade de sessão (minutos)** é o tempo limite para as ligações através deste proxy virtual.
    
    d. O **nome de cabeçalho do cookie de sessão** é o nome do cookie armazenar o identificador de sessão para a sessão de Qlik Sense um utilizador recebe após a autenticação com êxito.  Este nome tem de ser exclusivo.

1. Clique na opção de menu de autenticação para o tornar visível.  É apresentado o ecrã de autenticação.
    
    ![QlikSense][qs10]
    
    a. O **modo de acesso anônimo** pendente determina se os utilizadores anónimos podem aceder Qlik Sense através do proxy virtual.  A opção predefinida não é nenhum utilizador anónimo.
    
    b. O **método de autenticação** suspenso determina o esquema de autenticação do proxy virtual irá utilizar.  Selecione SAML da lista pendente.  Mais opções aparecem como resultado.
    
    c. Na **campo URI de anfitrião SAML**, os utilizadores de nome de anfitrião introduzirem para aceder ao Qlik Sense através deste proxy virtual SAML de entrada.  O nome do anfitrião é o uri do servidor Qlik Sense.
    
    d. Na **ID de entidade SAML**, introduza o mesmo valor que introduziu para o campo URI de anfitrião SAML.
    
    e. O **metadados de SAML IdP** é o ficheiro editado anteriormente na **Editar metadados de Federação a partir de configuração do Azure AD** secção.  **Antes de carregar os metadados de IdP, o ficheiro tem de ser editado** remover as informações para garantir a operação adequada entre o Azure AD e Qlik Sense server.  **Consulte as instruções acima se o ficheiro ainda não foi possível editá-lo.**  Se o ficheiro foi editado, clique no botão Procurar e selecione o ficheiro de metadados editado carregá-lo para a configuração de virtual proxy.
    
    f. Introduza a referência do atributo de nome ou o esquema para o atributo SAML que representa a **UserID** do Azure AD envia para o servidor de Qlik Sense.  Informações de referência de esquema estão disponíveis na configuração de postagem de ecrãs da aplicação do Azure.  Para utilizar o atributo de nome, introduza `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    g. Introduza o valor para o **diretório de utilizador** que será anexado aos utilizadores quando fizer a autenticação de servidor Qlik Sense através do Azure AD.  Valores embutidos em código tem de ser rodeados por **quadrado parêntesis Retos []**.  Para utilizar um atributo enviado na asserção de SAML do Azure AD, introduza o nome do atributo nesta caixa de texto **sem** Parênteses Retos.
    
    h. O **algoritmo de assinatura de SAML** define o certificado de fornecedor (neste servidor Qlik Sense maiúsculas) do serviço de assinatura para a configuração de virtual proxy.  Se o servidor de Qlik Sense utiliza um certificado fidedigno gerado usando o Microsoft RSA avançada e o fornecedor de criptografia AES, altere o algoritmo de assinatura de SAML para **SHA-256**.
    
    i. A seção de mapeamento do atributo SAML permite atributos adicionais, como grupos para ser enviado para Qlik Sense para utilização em regras de segurança.

1. Clique nas **BALANCEAMENTO de carga** opção de menu para torná-lo visível.  É apresentado o ecrã de balanceamento de carga.
    
    ![QlikSense][qs11]

1. Clique nas **adicionar novo nó de servidor** botão, o nó de motor selecione ou nós Qlik Sense enviará sessões para fins de balanceamento de carga e, clique no **Add** botão.
    
    ![QlikSense][qs12]

1. Clique na opção de menu avançado para torná-lo visível. É apresentado o ecrã avançado.
    
    ![QlikSense][qs13]
    
    A lista de permissões de anfitrião identifica os nomes de anfitrião são aceites quando ligar ao servidor Qlik Sense.  **Introduza o nome de anfitrião os utilizadores irão especificar quando ligar ao servidor Qlik Sense.** O nome do anfitrião é o mesmo valor que o uri de anfitrião SAML sem a https://.

1. Clique nas **aplicar** botão.
    
    ![QlikSense][qs14]

1. Clique em OK para aceitar a mensagem de aviso que indica a proxies ligados para o proxy virtual serão reiniciados.
    
    ![QlikSense][qs15]

1. No lado direito da tela, é apresentado o menu de itens associados.  Clique nas **Proxies** opção de menu.
    
    ![QlikSense][qs16]

1. É apresentado o ecrã de proxy.  Clique nas **Link** na parte inferior para ligar um proxy para o proxy virtual.
    
    ![QlikSense][qs17]

1. Selecione o nó de proxy que irá suportar esta ligação de virtual proxy e clique nas **Link** botão.  Depois de associar, o proxy será listado sob proxies associados.
    
    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

1. Após cerca de cinco a dez segundos, será apresentada a mensagem de QMC atualizar.  Clique nas **QMC atualizar** botão.
    
    ![QlikSense][qs20]

1. Quando atualiza o QMC, clique nas **Virtual proxies** item de menu. A nova entrada de virtual proxy SAML está listada na tabela na tela.  Único clique na entrada virtual proxy.
    
    ![QlikSense][qs51]

1. Na parte inferior do ecrã, irá ativar o botão de metadados de transferir SP.  Clique nas **SP transferir metadados** botão para guardar os metadados para um ficheiro.
    
    ![QlikSense][qs52]

1. Abra o ficheiro de metadados de sp.  Observe a **entityID** entrada e o **AssertionConsumerService** entrada.  Estes valores são equivalentes para o **identificador** e o **iniciar sessão no URL** na configuração da aplicação do Azure AD. Cole esses valores no **Qlik sentido empresarial domínio e URLs** secção na configuração da aplicação do Azure AD Se eles não correspondem, em seguida, deve substituir o Assistente de configuração de aplicações do Azure AD.
    
    ![QlikSense][qs53]

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

   ![O botão do Azure Active Directory](./media/qliksense-enterprise-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

   !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/qliksense-enterprise-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

   ![Botão Adicionar](./media/qliksense-enterprise-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

   ![A caixa de diálogo de utilizador](./media/qliksense-enterprise-tutorial/create_aaduser_04.png)

   a. Na **Name** , escreva **BrittaSimon**.

   b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

   c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

   d. Clique em **Criar**.
 
### <a name="create-a-qlik-sense-enterprise-test-user"></a>Criar um utilizador de teste Qlik sentido empresarial

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Qlik sentido empresarial. Trabalhar com [equipa de suporte de Qlik sentido Enterprise Client](https://www.qlik.com/us/services/support) para adicionar os utilizadores na plataforma Qlik sentido empresarial. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar do Azure-início de sessão único, concedendo acesso para Qlik sentido empresarial.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Qlik sentido empresarial, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Qlik sentido empresarial**.

    ![A ligação de Qlik sentido Enterprise na lista de aplicações](./media/qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Qlik sentido empresarial no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação Qlik sentido empresarial. 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/qliksense-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/qliksense-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/qliksense-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/qliksense-enterprise-tutorial/tutorial_general_203.png

[qs6]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

