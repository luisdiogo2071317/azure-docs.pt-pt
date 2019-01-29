---
title: 'Tutorial: Integração do Active Directory do Azure com o IBM Kenexa pesquisa empresarial | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o IBM Kenexa pesquisa empresarial.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: c7aac6da-f4bf-419e-9e1a-16b460641a52
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: a3c67fe6e6ed706d3e06a618ded9070f9f69ba19
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180250"
---
# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a>Tutorial: Integração do Active Directory do Azure com o IBM Kenexa pesquisa empresarial

Neste tutorial, saiba como integrar o IBM Kenexa pesquisa empresarial no Azure Active Directory (Azure AD).

Integrar o IBM Kenexa pesquisa empresarial no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao IBM Kenexa pesquisa empresarial.
- Pode permitir que os utilizadores iniciem sessão automaticamente nas IBM Kenexa pesquisa empresarial com o início de sessão único (SSO) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central: portal do Azure.

Se quiser saber mais sobre o software como uma integração de aplicação de serviço (SaaS) com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o IBM Kenexa pesquisa empresarial, terá dos seguintes itens:

- Uma subscrição do Azure
- Uma subscrição habilitados para IBM Kenexa pesquisa empresarial SSO

> [!NOTE]
> Quando testa os passos neste tutorial, é recomendável que não use um ambiente de produção.

Para testar os passos neste tutorial, siga as seguintes recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar SSO do Azure AD num ambiente de teste. O cenário descrito no tutorial consiste em dois blocos de construção principais:

* Adicionar IBM Kenexa pesquisa empresarial a partir da Galeria
* Configurando e testando o SSO do Azure AD

## <a name="add-ibm-kenexa-survey-enterprise-from-the-gallery"></a>Adicionar IBM Kenexa pesquisa empresarial a partir da Galeria
Para configurar a integração do IBM Kenexa pesquisa empresarial no Azure AD, adicione IBM Kenexa pesquisa empresarial a partir da Galeria à sua lista de aplicações de SaaS geridas.

Para adicionar IBM Kenexa pesquisa empresarial a partir da galeria, faça o seguinte:

1. Na [portal do Azure](https://portal.azure.com), no painel esquerdo, clique nas **Azure Active Directory** botão. 

    ![O botão do Azure Active Directory][1]

1. Selecione **aplicações empresariais**e, em seguida, selecione **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar uma aplicação, clique nas **nova aplicação** botão.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **IBM Kenexa pesquisa empresarial**.

    ![Criar um utilizador de teste do Azure AD](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_search.png)

1. Na lista de resultados, selecione **IBM Kenexa pesquisa empresarial**e, em seguida, clique nas **Add** botão para adicionar a aplicação.

    ![IBM Kenexa pesquisa empresarial na lista de resultados](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único
Nesta secção, configurar e testar o SSO do Azure AD com o IBM Kenexa pesquisa empresarial com base num utilizador de teste chamado "Eduarda Almeida."

Para SSO para funcionar, o Azure AD precisa identificar a contrapartida de utilizador do IBM Kenexa pesquisa empresarial no Azure AD. Em outras palavras, do Azure AD tem de estabelecer uma relação de ligação entre um utilizador do Azure AD e um utilizador relacionado no IBM Kenexa pesquisa empresarial.

Para estabelecer a relação de ligação, atribuir o valor do **nome de utilizador** no IBM Kenexa pesquisa empresarial, como o valor da **Username** no Azure AD.

Para configurar e testar o SSO do Azure AD com o IBM Kenexa pesquisa empresarial, conclua os blocos de construção nas próximas duas secções.

### <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Nesta secção, ativar o SSO do Azure AD no portal do Azure e configurar o SSO na sua aplicação empresarial de pesquisa do IBM Kenexa efetuando o seguinte procedimento:

1. No portal do Azure, sobre o **IBM Kenexa pesquisa empresarial** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o IBM Kenexa pesquisa empresarial única início de sessão na ligação][4]

1. Na **início de sessão único** caixa de diálogo a **modo** caixa, selecione **baseado em SAML logon** para ativar o SSO.
 
    ![Caixa de diálogo de início de sessão único](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_samlbase.png)

1. Na **IBM Kenexa pesquisa empresarial domínio e URLs** secção, execute os seguintes passos:

    ![IBM Kenexa pesquisa empresarial domínio e URLs únicas início de sessão em informações](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://surveys.kenexa.com/<companycode>`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://surveys.kenexa.com/<companycode>/tools/sso.asp`

    > [!NOTE] 
    > Os valores anteriores não são reais. Atualizá-las com o identificador real e URL de resposta. Para obter os valores reais, entre em contato com o [equipa de suporte da empresa de pesquisa do IBM Kenexa](https://www.ibm.com/support/home/?lnk=fcw).

1. Sob **certificado de assinatura SAML**, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado para o seu computador.

    ![O link de download de certificado (Base64)](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_certificate.png) 

    O aplicativo empresarial de pesquisa do IBM Kenexa espera-se receber as asserções de segurança asserções Markup Language (SAML) num formato específico, o que requer a adição de mapeamentos de atributos personalizados para a configuração dos seus atributos de token SAML. O valor da afirmação de identificador de utilizador na resposta têm de corresponder o ID de SSO está configurado no sistema Kenexa. Para mapear o identificador de utilizador adequada na sua organização como protocolo de datagrama de Internet do SSO (IDP), trabalhar com o [equipa de suporte da empresa de pesquisa do IBM Kenexa](https://www.ibm.com/support/home/?lnk=fcw). 

    Por predefinição, o Azure AD define o identificador de utilizador como o valor de nome principal (UPN) do utilizador. Pode alterar este valor sobre o **atributo** separador, conforme mostrado na captura de ecrã seguinte. A integração funciona apenas depois de concluir o mapeamento corretamente.
    
    ![A caixa de diálogo de atributos de utilizador](./media/kenexasurvey-tutorial/tutorial_attribute.png) 

1. Clique em **Guardar**.

    ![A configurar início de sessão único botão Save](./media/kenexasurvey-tutorial/tutorial_general_400.png)

1. Para abrir o **configurar o início de sessão** janela, em **IBM Kenexa pesquisa Enterprise Configuration**, clique em **configurar empresarial de pesquisa do IBM Kenexa**. 
 
    ![A ligação de configurar o IBM Kenexa pesquisa empresarial](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_configure.png)

1. Copiar o **URL de fim de sessão**, **ID de entidade de SAML**, e **SAML único início de sessão no URL do serviço** valores do **referência rápida** secção.

1. No **configurar o início de sessão** janela, em **referência rápida**, copiar o **URL de fim de sessão**, **ID de entidade de SAML**, e **SAML único início de sessão no URL do serviço** valores.

1. Configurar o SSO a **IBM Kenexa pesquisa empresarial** lado, envie o transferido **certificado (Base64)**, **URL de fim de sessão**, **ID de entidade de SAML**, e **SAML único início de sessão no URL do serviço** valores para o [equipa de suporte da empresa de pesquisa do IBM Kenexa](https://www.ibm.com/support/home/?lnk=fcw).

> [!TIP]
> Pode consultar para uma versão concisa destas instruções no [portal do Azure](https://portal.azure.com) enquanto estiver a configurar a aplicação. Depois de adicionar a aplicação a partir do **do Active Directory** > **aplicações empresariais** secção, basta clicar o **início de sessão único** separador e, em seguida, aceder a documentação por meio do Embedded a **configuração** secção no final. Para saber mais sobre a funcionalidade de documentação do embedded, veja [documentação do embedded do Azure AD](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
Nesta secção, vai criar o utilizador de teste Eduarda Almeida no portal do Azure, fazendo o seguinte:

![Criar um utilizador de teste do Azure AD][100]

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/kenexasurvey-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.
    
    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/kenexasurvey-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.
 
    ![Botão Adicionar](./media/kenexasurvey-tutorial/create_aaduser_03.png) 

1. Na **utilizador** diálogo caixa, execute os seguintes passos:
 
    ![A caixa de diálogo de utilizador](./media/kenexasurvey-tutorial/create_aaduser_04.png) 

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-ibm-kenexa-survey-enterprise-test-user"></a>Criar um utilizador de teste do IBM Kenexa pesquisa empresarial

Nesta secção, vai criar um usuário chamado Eduarda Almeida no IBM Kenexa pesquisa empresarial. 

Para criar utilizadores no sistema IBM Kenexa pesquisa empresarial e mapear o ID de SSO para os mesmos, pode trabalhar com o [equipa de suporte da empresa de pesquisa do IBM Kenexa](https://www.ibm.com/support/home/?lnk=fcw). Este valor de ID de SSO também deve ser mapeado para o valor do identificador de utilizador do Azure AD. Pode alterar esta predefinição sobre o **atributo** separador.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar utilizador Eduarda Almeida para utilizar o SSO do Azure ao conceder acesso para IBM Kenexa pesquisa empresarial.

![Atribuir a função de utilizador][200] 

Para atribuir utilizador Eduarda Almeida a IBM Kenexa pesquisa empresarial, faça o seguinte:

1. No portal do Azure, abra o **aplicativos** modo de exibição, vá para o **diretório** visualizar, selecione **aplicações empresariais**e, em seguida, clique em **todas as aplicações** .

    ![A "Aplicações empresariais" e os links de "Todas as aplicações"][201] 

1. Na **aplicativos** lista, selecione **IBM Kenexa pesquisa empresarial**.

    ![A ligação da IBM Kenexa pesquisa empresarial na lista de aplicações](./media/kenexasurvey-tutorial/tutorial_kenexasurvey_app.png) 

1. No painel esquerdo, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202] 

1. Clique nas **Add** botão e, em seguida, no **adicionar atribuição** painel, selecione **utilizadores e grupos**.

    ![O painel Adicionar atribuição][203]

1. Na **utilizadores e grupos** caixa de diálogo a **utilizadores** lista, selecione **Eduarda Almeida**.

1. Na **utilizadores e grupos** caixa de diálogo, clique nas **selecione** botão.

1. Na **adicionar atribuição** caixa de diálogo, clique nas **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração de SSO do Azure AD utilizando o painel de acesso.

Quando clica no **IBM Kenexa pesquisa empresarial** mosaico no painel de acesso, deve ser automaticamente a sessão iniciada sua aplicação IBM Kenexa pesquisa empresarial.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/kenexasurvey-tutorial/tutorial_general_04.png

[100]: ./media/kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/kenexasurvey-tutorial/tutorial_general_201.png
[202]: ./media/kenexasurvey-tutorial/tutorial_general_202.png
[203]: ./media/kenexasurvey-tutorial/tutorial_general_203.png

 
