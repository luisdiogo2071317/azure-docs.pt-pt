---
title: 'Tutorial: Integração do Azure Active Directory com a autenticação de identidade de plataforma de Cloud SAP | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e autenticação de identidade da plataforma de Cloud de SAP.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: jeedes
ms.openlocfilehash: 266c9523f45294899e3cddbe782cbc54846eb119
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39422312"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-platform-identity-authentication"></a>Tutorial: Integração do Azure Active Directory com autenticação de identidade da plataforma de Cloud de SAP

Neste tutorial, saiba como integrar a autenticação de identidade da plataforma de Cloud de SAP no Azure Active Directory (Azure AD). Autenticação de identidade da plataforma de Cloud SAP é utilizada como um IdP de proxy para aceder às aplicações de SAP que utilizam o Azure AD como o IdP principal.

Ao integrar a autenticação de identidade da plataforma de Cloud de SAP com o Azure AD, obtém os seguintes benefícios:

- Pode controlar no Azure AD que tenha acesso às aplicações SAP.
- Pode permitir que os utilizadores iniciem sessão automaticamente nas aplicações com as suas contas do Azure AD de SAP.
- Pode gerir as suas contas num local central, o portal do Azure.

Para obter mais informações sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com autenticação de identidade da plataforma de Cloud de SAP, terá dos seguintes itens:

- Uma subscrição do Azure AD.
- Uma única sessão em ativada subscrição para autenticação de identidade da plataforma de Cloud de SAP.

> [!NOTE]
> Não é recomendado utilizar um ambiente de produção para testar os passos neste tutorial.

Para testar os passos neste tutorial, siga as seguintes recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, [obtenha uma avaliação gratuita de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário que é descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando autenticação de identidade da plataforma de Cloud de SAP da Galeria
1. Configuração e teste do Azure AD início de sessão único

Antes de aprofundar os detalhes técnicos, é fundamental para compreender os conceitos que pretende examinar. A autenticação de identidade da plataforma de Cloud de SAP e serviços de Federação do Active Directory permitem implementar SSO em aplicações ou serviços que estão protegidos pelo Azure AD (como um IdP) com aplicações SAP e serviços que estão protegidos pelo SAP Cloud Autenticação de identidade da plataforma.

Atualmente, a autenticação de identidade da plataforma de Cloud de SAP atua como um fornecedor de identidade do Proxy de aplicações SAP. O Azure Active Directory funciona, por sua vez, como líder no fornecimento de identidade nessa configuração. 

O diagrama seguinte ilustra essa relação:

![Criar um utilizador de teste do Azure AD](./media/sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

Com esta configuração, o seu inquilino de autenticação de identidade da plataforma de Cloud de SAP está configurado como um aplicativo confiável no Azure Active Directory. 

Todas as aplicações SAP e serviços que pretende proteger desta forma, em seguida, são configurados na consola de gestão de autenticação de identidade da plataforma de Cloud de SAP.

Por conseguinte, a autorização para conceder acesso a serviços e aplicações SAP precisa ser feita na autenticação de identidade do SAP Cloud Platform (em oposição ao Azure Active Directory).

Ao configurar a autenticação de identidade da plataforma de Cloud de SAP como uma aplicação através do Azure Active Directory Marketplace, não terá de configurar afirmações individuais ou asserções SAML.

>[!NOTE] 
>Atualmente, apenas SSO de Web foi testado por ambas as partes. Os fluxos que são necessários para a API de aplicações ou a comunicação de API para API devem funcionar, mas ainda não foram testados. Serão testados durante as atividades subsequentes.
>

## <a name="add-sap-cloud-platform-identity-authentication-from-the-gallery"></a>Adicionar autenticação de identidade da plataforma de Cloud de SAP a partir da Galeria
Para configurar a integração de autenticação de identidade da plataforma de Cloud de SAP no Azure AD, terá de adicionar autenticação de identidade da plataforma de Cloud de SAP a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar a autenticação de identidade da plataforma de Cloud de SAP da galeria, siga os passos seguintes:**

1. Na [portal do Azure](https://portal.azure.com), no painel de navegação esquerdo, selecione a **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Aceda a **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar novos a nova aplicação, selecione o **nova aplicação** botão na parte superior da caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **autenticação de identidade da plataforma de Cloud de SAP**. 

1. Selecione **autenticação de identidade da plataforma de Cloud de SAP** no painel de resultados e, em seguida, selecione a **Add** botão.

    ![Autenticação de identidade do SAP Cloud Platform na lista de resultados](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com autenticação de identidade da plataforma de Cloud de SAP. Configurar e testá-lo com um utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber quem é o utilizador de equivalente na autenticação de identidade da plataforma de Cloud de SAP. Em outras palavras, tem de estabelecer uma ligação entre um utilizador do Azure AD e o utilizador relacionado na autenticação de identidade da plataforma de Cloud de SAP.

Na autenticação de identidade de plataforma de Cloud SAP, atribua o valor **nome de utilizador** o mesmo valor como **nome de utilizador** no Azure AD. Agora que estabeleceu a ligação entre os dois usuários.

Para configurar e testar o Azure AD início de sessão único com autenticação de identidade da plataforma de Cloud de SAP, execute os seguintes blocos de construção:

1. [Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on) para permitir aos utilizadores utilizar esta funcionalidade.
1. [Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user) para testar o Azure AD início de sessão único com Eduarda Almeida.
1. [Criar um utilizador de teste de autenticação de identidade da plataforma de Cloud de SAP](#create-an-sap-cloud-platform-identity-authentication-test-user) para ter um equivalente da Eduarda Almeida no SAP Cloud Platform autenticação de identidade que está ligado à representação de utilizador do Azure AD.
1. [Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user) para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. [Testar início de sessão único](#test-single-sign-on) para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação de autenticação de identidade da plataforma de Cloud de SAP.

**Para configurar o Azure AD início de sessão único com autenticação de identidade da plataforma de Cloud de SAP, siga os passos seguintes:**

1. No portal do Azure, sobre o **autenticação de identidade da plataforma de Cloud de SAP** página de integração de aplicações, selecione **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Na **início de sessão único** caixa de diálogo em **baseado em SAML início de sessão**, selecione **modo** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_samlbase.png)

1. Se quiser configurar a aplicação no **IDP** iniciadas pelo modo, na **URLs de domínio de autenticação de identidade de plataforma na Cloud SAP e** secção, execute os seguintes passos:  

    ![URLs de domínio de autenticação de identidade de plataforma na Cloud SAP e únicas início de sessão em informações](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_url.png)

    a. Na **identificador** caixa, escreva um URL com o seguinte padrão: `<IAS-tenant-id>.accounts.ondemand.com`

    b. Na **URL de resposta** caixa, escreva um URL com o seguinte padrão: `https://<IAS-tenant-id>.accounts.ondemand.com/saml2/idp/acs/<IAS-tenant-id>.accounts.ondemand.com`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador real e o URL de resposta. Contacte os [equipa de suporte de cliente de autenticação de identidade de plataforma de Cloud de SAP](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) obter esses valores. Se não sabe o valor do identificador, leia a documentação de autenticação de identidade da plataforma de Cloud de SAP sobre [configuração do inquilino SAML 2.0](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).

1. Se quiser configurar a aplicação no **SP** modo iniciado, selecione **Mostrar definições de URL avançadas**.

    ![URLs de domínio de autenticação de identidade de plataforma na Cloud SAP e únicas início de sessão em informações](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_url1.png)

    Na **URL de início de sessão** caixa, escreva um URL com o seguinte padrão: `{YOUR BUSINESS APPLICATION URL}`.

    > [!NOTE]
    > Este valor não é real. Atualize este valor com o URL de início de sessão real. Utilize o seu negócio específico início de sessão URL da aplicação. Contacte os [equipa de suporte de cliente de autenticação de identidade de plataforma de Cloud de SAP](https://cloudplatform.sap.com/capabilities/security/trustcenter.html) se tiver qualquer dúvida.

1. Na **certificado de assinatura SAML** secção, selecione **XML de metadados**. Em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_certificate.png)

1. Aplicação de autenticação de identidade de plataforma em nuvem SAP espera que as asserções SAML num formato específico. Gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicativo. Captura de ecrã seguinte mostra um exemplo do formato. 

    ![Configurar o início de sessão único](./media/sap-hana-cloud-platform-identity-authentication-tutorial/attribute.png)

1. Se a sua aplicação SAP espera que um atributo, tal como **firstName**, adicionar o **firstName** atributo o **atributos de utilizador** secção. Esta opção está disponível na **início de sessão único** caixa de diálogo da **atributos de token SAML** caixa de diálogo....

    a. Para abrir o **adicionar atributo** caixa de diálogo, selecione **adicionar atributo**. 
    
    ![Configurar o início de sessão único](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_attribute_04.png)
    
    ![Configurar o início de sessão único](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_attribute_05.png)
    
    b. Na **Name** , escreva o nome de atributo **firstName**.
    
    c. Partir do **valor** , selecione o valor do atributo **user.givenname**.
    
    d. Selecione **Ok**.

1. Selecione o botão **Guardar**.

    ![Configurar o início de sessão único botão Save](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_400.png)

1. Na **configuração de autenticação de identidade de plataforma de Cloud de SAP** secção, selecione **configurar a SAP Cloud plataforma de identidade de autenticação** para abrir o **configurar início de sessão**janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de autenticação de identidade de plataforma de Cloud SAP](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_configure.png) 

1. Para obter SSO configurado para a sua aplicação, vá para a consola de administração de autenticação de identidade da plataforma de Cloud de SAP. O URL tem o seguinte padrão: `https://<tenant-id>.accounts.ondemand.com/admin`. Em seguida, leia a documentação sobre a autenticação de identidade do SAP Cloud Platform em [integração com o Microsoft Azure AD](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html). 

1. No portal do Azure, selecione o **guardar** botão.

1. Continue com o seguinte procedimento apenas se pretende adicionar e ativar o SSO para outra aplicação SAP. Repita os passos na secção **adicionar SAP Cloud Platform autenticação de identidade na galeria do**.

1. No portal do Azure, sobre o **autenticação de identidade da plataforma de Cloud de SAP** página de integração de aplicações, selecione **início de sessão ligado em**.

    ![Configurar o início de sessão ligado](./media/sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)

1. Guarde a configuração.

>[!NOTE] 
>A nova aplicação tira partido da configuração única início de sessão do aplicativo SAP anterior. Certifique-se de que utilizar os mesmos fornecedores de identidade empresarial na consola de administração de autenticação de identidade da plataforma de Cloud de SAP.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com) enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador e acessar o embedded documentação através da **configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded em [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, siga os passos seguintes:**

1. No portal do Azure, no painel esquerdo, selecione o **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, selecione **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_02.png)

1. Para abrir o **utilizador** caixa de diálogo, selecione **Add** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, siga os passos seguintes:

    ![A caixa de diálogo de utilizador](./media/sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Selecione **Criar**.
 
### <a name="create-an-sap-cloud-platform-identity-authentication-test-user"></a>Criar um utilizador de teste de autenticação de identidade da plataforma de Cloud de SAP

Não precisa de criar um utilizador na autenticação de identidade da plataforma de Cloud de SAP. Os utilizadores que estão no arquivo de utilizador do Azure AD podem utilizar a funcionalidade SSO.

Autenticação de identidade da plataforma de Cloud SAP suporta a opção de Federação de identidades. Esta opção permite que o aplicativo verificar se os utilizadores que são autenticados pelo fornecedor de identidade empresarial existem no arquivo de SAP Cloud plataforma de identidade autenticação de utilizador. 

A opção de Federação de identidades está desativada por predefinição. Se estiver ativada a Federação de identidades, apenas os utilizadores que são importados na autenticação de identidade da plataforma de Cloud de SAP podem aceder à aplicação. 

Para obter mais informações sobre como ativar ou desativar o Federação de identidades com autenticação de identidade da plataforma de Cloud de SAP, consulte "Ativar a Federação com o SAP Cloud plataforma identidade autenticação de identidade" no [configurar a Federação de identidades com o Autenticação de identidade do utilizador Store de SAP Cloud Platform](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para autenticação de identidade da plataforma de Cloud de SAP.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a autenticação de identidade da plataforma de Cloud de SAP, siga os passos seguintes:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, vá para o modo de exibição do diretório. Em seguida, aceda a **aplicações empresariais**e, em seguida, selecione **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **autenticação de identidade da plataforma de Cloud de SAP**.

    ![A ligação de autenticação de identidade da plataforma de Cloud de SAP na lista de aplicações](./media/sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sapcpia_app.png)  

1. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Selecione o **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. Na **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique nas **selecionar** botão no **utilizadores e grupos** caixa de diálogo.

1. Selecione o **atribua** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar seu única início de sessão em configuração do Azure AD utilizando o painel de acesso.

Ao selecionar o mosaico de autenticação de identidade da plataforma de Cloud de SAP no painel de acesso, é automaticamente feita na sua aplicação de autenticação de identidade da plataforma de Cloud de SAP.

Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/sapcloudauth-tutorial/tutorial_general_01.png
[2]: ./media/sapcloudauth-tutorial/tutorial_general_02.png
[3]: ./media/sapcloudauth-tutorial/tutorial_general_03.png
[4]: ./media/sapcloudauth-tutorial/tutorial_general_04.png

[100]: ./media/sapcloudauth-tutorial/tutorial_general_100.png

[200]: ./media/sapcloudauth-tutorial/tutorial_general_200.png
[201]: ./media/sapcloudauth-tutorial/tutorial_general_201.png
[202]: ./media/sapcloudauth-tutorial/tutorial_general_202.png
[203]: ./media/sapcloudauth-tutorial/tutorial_general_203.png
