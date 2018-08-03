---
title: 'Tutorial: Integração do Azure Active Directory com o ServiceNow | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e ao ServiceNow.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a5a1a264-7497-47e7-b129-a1b5b1ebff5b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 5d5c4d5e26fa21488dd637805a4c22bd3ed18a7f
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39421088"
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Tutorial: Integração do Azure Active Directory com ServiceNow

Neste tutorial, saiba como integrar o ServiceNow com o Azure Active Directory (Azure AD).

Integração do ServiceNow com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao ServiceNow.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada ao ServiceNow (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com ServiceNow, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Para o ServiceNow, uma instância ou um inquilino do ServiceNow, versão Calgary ou superior
- Para o ServiceNow Express, uma instância do ServiceNow Express, versão Helsínquia ou superior
- O inquilino do ServiceNow tem de ter o [vários fornecedor único início de sessão no plug-in](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) ativada. Isso pode ser feito [submeter um pedido de serviço](https://hi.service-now.com).
- Para a configuração automática, ative o plug-in de multi fornecedor do ServiceNow.

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o ServiceNow da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-servicenow-from-the-gallery"></a>Adicionando o ServiceNow da Galeria
Para configurar a integração do ServiceNow com o Azure AD, terá de adicionar ServiceNow a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar ServiceNow a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **ServiceNow**, selecione **ServiceNow** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![ServiceNow na lista de resultados](./media/servicenow-tutorial/tutorial_servicenow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com ServiceNow com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no ServiceNow para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no ServiceNow deve ser estabelecido.

ServiceNow, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com ServiceNow, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único do ServiceNow](#configure-azure-ad-single-sign-on-for-servicenow)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Configurar o Azure AD início de sessão único para Express do ServiceNow](#configure-azure-ad-single-sign-on-for-servicenow-express)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste do ServiceNow](#create-a-servicenow-test-user)**  - para ter um equivalente da Eduarda Almeida na ServiceNow que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on-for-servicenow"></a>Configurar o Azure AD início de sessão único do ServiceNow

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação ServiceNow.

**Para configurar o Azure AD início de sessão único com ServiceNow, execute os seguintes passos:**

1. No portal do Azure, sobre o **ServiceNow** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/servicenow-tutorial/tutorial_servicenow_samlbase.png)

1. Sobre o **ServiceNow domínio e URLs** secção, execute os seguintes passos:

    ![URLs de domínio do ServiceNow e únicas início de sessão em informações](./media/servicenow-tutorial/tutorial_servicenow_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<instance-name>.service-now.com/navpage.do`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<instance-name>.service-now.com`

    > [!NOTE] 
    > Estes valores não são reais. Terá de atualizar estes valores de URL de início de sessão real e o identificador que é explicado mais tarde no tutorial.

1. Sobre o **certificado de assinatura SAML** secção, execute os seguintes passos: 

    ![O link de download de certificado](./media/servicenow-tutorial/tutorial_servicenow_certificate.png)

    a. Clique no botão de cópia para copiar **Url de metadados de Federação de aplicação** e cole-o no bloco de notas, como este Url de metadados de Federação da aplicação irá ser utilizado mais tarde no tutorial.

    b. Clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/servicenow-tutorial/tutorial_general_400.png)

1. Inicie sessão sua aplicação ServiceNow como administrador.

1. Ativar a **integração - vários único início de sessão instalador de fornecedor** Plug-in ao seguir os passos seguintes:

    a. No painel de navegação no lado esquerdo, pesquise **System Definition** secção da barra de pesquisa e, em seguida, clique em **plug-ins**.

    ![Ativar o plug-in](./media/servicenow-tutorial/tutorial_servicenow_03.png "ativar Plug-in")

     b. Procure **integração - vários único início de sessão instalador de fornecedor**.

     ![Ativar o plug-in](./media/servicenow-tutorial/tutorial_servicenow_04.png "ativar Plug-in")

    c. Selecione o plug-in. Clique com o botão direito do rato e selecione **Activate/atualização**.

    d. Clique nas **Activate** botão.

1. Existem duas formas em que **ServiceNow** pode ser configurada automática e manual.

1. Para configurar **ServiceNow** automaticamente siga os passos abaixo

    a. Retorno para o **ServiceNow** início de sessão único na página no portal do Azure.

    b. Um clique configurar o serviço é fornecido para o ServiceNow ou seja, para que o Azure AD configurar automaticamente ServiceNow para autenticação baseada no SAML. Para ativar este serviço, aceda ao **configuração do ServiceNow** secção, clique em **configurar ServiceNow** para abrir a janela de início de sessão de configurar.

    ![Configurar o início de sessão único](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

    c. Introduza o nome da instância do ServiceNow, o nome de utilizador administrador e a palavra-passe de administrador no **configurar o início de sessão** formam e clique em **configurar agora**. Tenha em atenção que o nome de utilizador do administrador fornecido tem de ter o **security_admin** função atribuída no ServiceNow para que isso funcione. Caso contrário, para configurar manualmente o ServiceNow ao utilizar o Azure AD como fornecedor de identidade de SAML, clique em **configurar manualmente o início de sessão único** e copie o **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** da secção de referência rápida.

    ![Configurar o URL da aplicação](./media/servicenow-tutorial/configure.png "configurar o URL da aplicação")

    d. Inicie sessão sua aplicação ServiceNow como administrador.

    e. Na configuração automática todas as definições são configuradas no **ServiceNow** lado, mas a **certificado X.509** não está ativada por predefinição. Tem para mapeá-lo manualmente para o fornecedor de identidade no ServiceNow. Siga os passos abaixo para o mesmo:
    
    * No painel de navegação no lado esquerdo, clique em **fornecedores de identidade** sob **SSO de multi fornecedor**.

      ![Configurar o início de sessão único](./media/servicenow-tutorial/tutorial_servicenow_07.png "configurar início de sessão único")

    * Clique no fornecedor de identidade gerada automaticamente

      ![Configurar o início de sessão único](./media/servicenow-tutorial/tutorial_servicenow_08.png "configurar início de sessão único")

    * Desloque para baixo para o **certificado X.509** secção. Selecione **Editar**.

      ![Configurar o início de sessão único](./media/servicenow-tutorial/tutorial_servicenow_09.png "configurar início de sessão único")
    
    * Selecione o certificado e clique em ícone de seta para a direita para adicionar o certificado

      ![Configurar o início de sessão único](./media/servicenow-tutorial/tutorial_servicenow_11.png "configurar início de sessão único")

    * Clique em **Guardar**.

    * Clique em **Activate** no canto superior direito da página.

1. Para configurar **ServiceNow** manualmente siga os passos abaixo

1. Inicie sessão sua aplicação ServiceNow como administrador.

1. No painel de navegação no lado esquerdo, pesquise **SSO de multi fornecedor** secção da barra de pesquisa e, em seguida, clique em **propriedades**.

    ![Configurar o URL da aplicação](./media/servicenow-tutorial/tutorial_servicenow_06.png "configurar o URL da aplicação")

1. Sobre o **várias propriedades do fornecedor de SSO** caixa de diálogo, execute os seguintes passos:

    ![Configurar o URL da aplicação](./media/servicenow-tutorial/ic7694981.png "configurar o URL da aplicação")

    a. Como **ativar vários provedor SSO**, selecione **Sim**.

    b. Como **ativar automaticamente a importação de utilizadores de todos os fornecedores de identidade para a tabela de utilizador**, selecione **Sim**.

    c. Como **depuração de ativar o registo para o fornecedor de vários integração de SSO**, selecione **Sim**.

    d. No **o campo no utilizador de tabela que...**  caixa de texto, tipo **user_name**.

    e. Clique em **Guardar**.

1. No painel de navegação no lado esquerdo, pesquise **SSO de multi fornecedor** secção da barra de pesquisa e, em seguida, clique em **certificados x509**.

    ![Configurar o início de sessão único](./media/servicenow-tutorial/tutorial_servicenow_05.png "configurar início de sessão único")

1. Sobre o **certificados X.509** caixa de diálogo, clique em **New**.

    ![Configurar o início de sessão único](./media/servicenow-tutorial/ic7694974.png "configurar início de sessão único")

1. Sobre o **certificados X.509** caixa de diálogo, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/servicenow-tutorial/ic7694975.png "configurar início de sessão único")

    a. Na **Name** caixa de texto, escreva um nome para a sua configuração (por exemplo: **TestSAML2.0**).

    b. Selecione **Active Directory**.

    c. Como **formato**, selecione **PEM**.

    d. Como **tipo**, selecione **confiar no certificado de Store**.

    e. Abra o seu certificado codificado Base64 transferido a partir do Azure no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado PEM** caixa de texto.

     f. Clique em **submeter**.

1. No painel de navegação no lado esquerdo, clique em **fornecedores de identidade**.

    ![Configurar o início de sessão único](./media/servicenow-tutorial/tutorial_servicenow_07.png "configurar início de sessão único")

1. Sobre o **fornecedores de identidade** caixa de diálogo, clique em **New**.

    ![Configurar o início de sessão único](./media/servicenow-tutorial/ic7694977.png "configurar início de sessão único")

1. Sobre o **fornecedores de identidade** caixa de diálogo, clique em **SAML2 Update1?**.

    ![Configurar o início de sessão único](./media/servicenow-tutorial/ic7694978.png "configurar início de sessão único")

1. Na caixa de diálogo Propriedades de Update1 SAML2, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/servicenow-tutorial/idp.png "configurar início de sessão único")

    a. Selecione **URL** opção **importar metadados do fornecedor de identidade** caixa de diálogo.

    b. Introduza o **Url de metadados de Federação de aplicação** que copiou do portal do Azure.

    c. Clique em **importar**.

1. Lê o URL de metadados de IdP e preenche a todas as informações de campos.

    ![Configurar o início de sessão único](./media/servicenow-tutorial/ic7694982.png "configurar início de sessão único")

    a. Na **Name** caixa de texto, escreva um nome para a sua configuração (por exemplo, **SAML 2.0**).
    
    b. Cópia **home page do ServiceNow** valor, cole-a no **URL de início de sessão** caixa de texto no **ServiceNow domínio e URLs** secção no portal do Azure.

    > [!NOTE]
    > Home page de instância do ServiceNow é uma concatenação das suas **URL de inquilino ServieNow** e **/navpage.do** (por exemplo:`https://fabrikam.service-now.com/navpage.do`).

    c. Cópia **ID de entidade / emissor** valor, cole-o na **identificador** caixa de texto no **ServiceNow domínio e URLs** secção no portal do Azure.

    d. Clique em **Advanced**. Na **campo de utilizador** caixa de texto, tipo **e-mail** ou **user_name**, dependendo do que campo é utilizado para identificar exclusivamente os utilizadores na sua implementação do ServiceNow.

    > [!NOTE]
    > Pode configurar o Azure AD para emitir o ID de utilizador do Azure AD (nome principal de utilizador) ou o endereço de e-mail como o identificador exclusivo no SAML token ao aceder a **ServiceNow > atributos > início de sessão único** seção do portal do Azure e para mapear o campo desejado para o **nameidentifier** atributo. O valor armazenado para o atributo selecionado no Azure AD (por exemplo, nome principal de utilizador) tem de corresponder ao valor armazenado no ServiceNow para o campo introduzido (por exemplo, user_name)

    e. Sob **x509 certificado**, apresenta uma lista do certificado que criou no passo anterior.

    > [!NOTE]
    > ServiceNow não permite a ativação do Idp sem clicando no botão de ligação de teste, para substituir o mesmo, siga os passos abaixo.

1. Clique no ícone de menu do novo fornecedor de identidade que criou como parte da configuração e para a lista, selecione **copiar sys_id**

    ![Configurar o início de sessão único](./media/servicenow-tutorial/ic7694992.png "configurar início de sessão único")

1. Na caixa de pesquisa à esquerda superior, procure **sys_properties.list** e prima enter.

    ![Configurar o início de sessão único](./media/servicenow-tutorial/ic7694993.png "configurar início de sessão único")

1. Clique em **Novo**.

    ![Configurar o início de sessão único](./media/servicenow-tutorial/ic7694994.png "configurar início de sessão único")

1. Na **propriedade do sistema** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/servicenow-tutorial/ic7694995.png "configurar início de sessão único")

    a. Introduza `glide.authenticate.sso.redirect.idp` valor na caixa de texto nome.

    b. Na **valor** caixa de texto, colar a sys_id de cópia de valor que copiou nos passos anteriores.

    c. Selecione **privada**.

    d. Clique em **submeter**.

1. Clique em **Novo**.

    ![Configurar o início de sessão único](./media/servicenow-tutorial/ic7694994.png "configurar início de sessão único")

1. Na **propriedade do sistema** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/servicenow-tutorial/ic7694996.png "configurar início de sessão único")

    a. Introduza `glide.authenticate.multisso.test.connection.mandatory` valor na caixa de texto nome.

    b. Na **valor** caixa de texto, introduza **falso**.

    c. Clique em **submeter**.

1. Depois de fazê-lo passos acima, agora poderá ativar o novo fornecedor de identidade e o SSO deve funcionar

> [!NOTE]
> Também tenha em atenção que, tem de testar a configuração do Idp novo numa nova janela de navegação anónima

### <a name="configure-azure-ad-single-sign-on-for-servicenow-express"></a>Configurar o Azure AD Single Sign-On para Express do ServiceNow

1. No portal do Azure, sobre o **ServiceNow** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Configurar o início de sessão único](./media/servicenow-tutorial/tutorial_servicenow_samlbase.png)

1. Sobre o **ServiceNow domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/servicenow-tutorial/tutorial_servicenow_url.png)

    a. Na **URL de início de sessão** caixa de texto, digite o valor usando o seguinte padrão: `https://<instance-name>.service-now.com/navpage.do`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão real e o identificador. Contacte [equipa de suporte de cliente do ServiceNow](https://www.servicenow.com/support/contact-support.html) obter esses valores.

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/servicenow-tutorial/tutorial_servicenow_certificates.png)

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/servicenow-tutorial/tutorial_general_400.png)

1. Um clique configurar o serviço é fornecido para o ServiceNow ou seja, para que o Azure AD configurar automaticamente ServiceNow para autenticação baseada no SAML. Para ativar este serviço, aceda ao **configuração do ServiceNow** secção, clique em **configurar ServiceNow** para abrir a janela de início de sessão de configurar.

    ![Configurar o início de sessão único](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

1. Introduza o nome da instância do ServiceNow, o nome de utilizador administrador e a palavra-passe de administrador no **configurar o início de sessão** formam e clique em **configurar agora**. Tenha em atenção que o nome de utilizador do administrador fornecido tem de ter o **security_admin** função atribuída no ServiceNow para que isso funcione. Caso contrário, para configurar manualmente o ServiceNow ao utilizar o Azure AD como fornecedor de identidade de SAML, clique em **configurar manualmente o início de sessão único** e copie o **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** da secção de referência rápida.

    ![Configurar o URL da aplicação](./media/servicenow-tutorial/configure.png "configurar o URL da aplicação")

1. Inicie sessão sua aplicação ServiceNow Express como administrador.

1. No painel de navegação no lado esquerdo, clique em **Single Sign-On**.

    ![Configurar o URL da aplicação](./media/servicenow-tutorial/ic7694980ex.png "configurar o URL da aplicação")

1. Sobre o **Single Sign-On** caixa de diálogo, clique no ícone de configuração no canto superior direito e defina as propriedades seguintes:

    ![Configurar o URL da aplicação](./media/servicenow-tutorial/ic7694981ex.png "configurar o URL da aplicação")

    a. Botão de alternar **ativar vários provedor SSO** à direita.
    
    b. Botão de alternar **depuração de ativar o registo para o fornecedor de vários integração de SSO** à direita.
    
    c. No **o campo no utilizador de tabela que...**  caixa de texto, tipo **user_name**.

1. Sobre o **início de sessão único** caixa de diálogo, clique em **adicionar novo certificado**.

    ![Configurar o início de sessão único](./media/servicenow-tutorial/ic7694973ex.png "configurar início de sessão único")

1. Sobre o **certificados X.509** caixa de diálogo, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/servicenow-tutorial/ic7694975.png "configurar início de sessão único")

    a. Na **Name** caixa de texto, escreva um nome para a sua configuração (por exemplo: **TestSAML2.0**).

    b. Selecione **Active Directory**.

    c. Como **formato**, selecione **PEM**.

    d. Como **tipo**, selecione **confiar no certificado de Store**.

    e. Abra o seu certificado codificado Base64 transferido a partir do portal do Azure no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado PEM** caixa de texto.

    f. Clique em **Update**

1. Sobre o **início de sessão único** caixa de diálogo, clique em **adicionar IdP novo**.

    ![Configurar o início de sessão único](./media/servicenow-tutorial/ic7694976ex.png "configurar início de sessão único")

1. Sobre o **adicionar novo fornecedor de identidade** caixa de diálogo, em **configurar o fornecedor de identidade**, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/servicenow-tutorial/ic7694982ex.png "configurar início de sessão único")

    a. Na **Name** caixa de texto, escreva um nome para a sua configuração (por exemplo: **SAML 2.0**).

    b. Na **URL de fornecedor de identidade** campo, cole o valor de **ID do fornecedor de identidade**, que copiou do portal do Azure.
    
    c. Na **AuthnRequest do fornecedor de identidade** campo, cole o valor de **URL de pedido de autenticação**, que copiou do portal do Azure.

    d. Na **SingleLogoutRequest do fornecedor de identidade** campo, cole o valor de **URL de serviço de fim de sessão único**, que copiou do portal do Azure

    e. Como **certificado do fornecedor de identidade**, selecione o certificado que criou no passo anterior.

1. Clique em **definições avançadas**e, em **propriedades adicionais do fornecedor de identidade**, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/servicenow-tutorial/ic7694983ex.png "configurar início de sessão único")

    a. Na **protocolo de enlace para o IDP SingleLogoutRequest** caixa de texto, tipo **urn: oasis: nomes: tc: SAML:2.0:bindings:HTTP-redirecionar**.

    b. Na **política NameID** caixa de texto, tipo **urn: oasis: nomes: tc: SAML:1.1:nameid-formato: não especificado**.

    c. Na **método AuthnContextClassRef**, tipo `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.

    d. Desmarcar **criar um AuthnContextClass**.

1. Sob **propriedades adicionais do fornecedor de serviço**, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/servicenow-tutorial/ic7694984ex.png "configurar início de sessão único")

    a. Na **home page do ServiceNow** caixa de texto, escreva o URL da sua home page de instância de ServiceNow.

    > [!NOTE]
    > Home page de instância do ServiceNow é uma concatenação das suas **URL de inquilino ServieNow** e **/navpage.do** (por exemplo: `https://fabrikam.service-now.com/navpage.do`).

    b. Na **ID de entidade / emissor** caixa de texto, escreva o URL do seu inquilino do ServiceNow.

    c. Na **URI de audiência** caixa de texto, escreva o URL do seu inquilino do ServiceNow.

    d. Na **relógio inclinar** caixa de texto, tipo **60**.

    e. Na **campo de utilizador** caixa de texto, tipo **e-mail** ou **user_name**, dependendo do que campo é utilizado para identificar exclusivamente os utilizadores na sua implementação do ServiceNow.

    > [!NOTE]
    > Pode configurar o Azure AD para emitir o ID de utilizador do Azure AD (nome principal de utilizador) ou o endereço de e-mail como o identificador exclusivo no SAML token ao aceder a **ServiceNow > atributos > início de sessão único** seção do portal do Azure e para mapear o campo desejado para o **nameidentifier** atributo. O valor armazenado para o atributo selecionado no Azure AD (por exemplo, nome principal de utilizador) tem de corresponder ao valor armazenado no ServiceNow para o campo introduzido (por exemplo, user_name)

    f. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/servicenow-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/servicenow-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/servicenow-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/servicenow-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-servicenow-test-user"></a>Criar um utilizador de teste do ServiceNow

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no ServiceNow. ServiceNow suporta o aprovisionamento automático de utilizadores, que está por predefinição, ativada. Pode encontrar mais detalhes [aqui](servicenow-provisioning-tutorial.md) sobre como configurar o aprovisionamento automático de utilizadores.

> [!NOTE]
> Se precisar de criar manualmente um utilizador, terá de contactar o [equipa de suporte de cliente do ServiceNow](https://www.servicenow.com/support/contact-support.html)

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao ServiceNow.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida ao ServiceNow, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **ServiceNow**.

    ![A ligação do ServiceNow na lista de aplicações](./media/servicenow-tutorial/tutorial_servicenow_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de ServiceNow, no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação ServiceNow.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)
* [Configurar o aprovisionamento do utilizador](servicenow-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/servicenow-tutorial/tutorial_general_01.png
[2]: ./media/servicenow-tutorial/tutorial_general_02.png
[3]: ./media/servicenow-tutorial/tutorial_general_03.png
[4]: ./media/servicenow-tutorial/tutorial_general_04.png

[100]: ./media/servicenow-tutorial/tutorial_general_100.png

[200]: ./media/servicenow-tutorial/tutorial_general_200.png
[201]: ./media/servicenow-tutorial/tutorial_general_201.png
[202]: ./media/servicenow-tutorial/tutorial_general_202.png
[203]: ./media/servicenow-tutorial/tutorial_general_203.png
