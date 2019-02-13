---
title: 'Tutorial: Integração do Active Directory do Azure com OpsGenie | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e OpsGenie.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 41b59b22-a61d-4fe6-ab0d-6c3991d1375f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f9858fc38698ae2c5bd272a3494bcf02bce2d8e9
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56194602"
---
# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a>Tutorial: Integração do Active Directory do Azure com OpsGenie

Neste tutorial, saiba como integrar OpsGenie com o Azure Active Directory (Azure AD).

Integrar OpsGenie no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao OpsGenie
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para OpsGenie (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com OpsGenie, terá dos seguintes itens:

- Uma subscrição do Azure
- Um OpsGenie logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando OpsGenie da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-opsgenie-from-the-gallery"></a>Adicionando OpsGenie da Galeria
Para configurar a integração do OpsGenie com o Azure AD, terá de adicionar OpsGenie a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar OpsGenie a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **OpsGenie**.

    ![Criar um utilizador de teste do Azure AD](./media/opsgenie-tutorial/tutorial_opsgenie_search.png)

1. No painel de resultados, selecione **OpsGenie**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/opsgenie-tutorial/tutorial_opsgenie_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com OpsGenie com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no OpsGenie a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no OpsGenie deve ser estabelecido.

OpsGenie, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com OpsGenie, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste OpsGenie](#creating-a-opsgenie-test-user)**  - para ter um equivalente da Eduarda Almeida na OpsGenie que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo OpsGenie.

**Para configurar o Azure AD início de sessão único com OpsGenie, execute os seguintes passos:**

1. No portal do Azure, sobre o **OpsGenie** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/opsgenie-tutorial/tutorial_opsgenie_samlbase.png)

1. Sobre o **OpsGenie domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/opsgenie-tutorial/tutorial_opsgenie_url.png)

    Na **URL de início de sessão** caixa de texto, escreva o URL: `https://app.opsgenie.com/auth/login`

1. Sobre o **certificado de assinatura SAML** secção, clique no botão de cópia para copiar **Url de metadados de Federação de aplicação** e cole-o no bloco de notas.

    ![O link de download de certificado](./media/opsgenie-tutorial/tutorial_opsgenie_certificate.png)

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/opsgenie-tutorial/tutorial_general_400.png)

1. Sobre o **OpsGenie configuração** secção, clique em **configurar OpsGenie** para abrir **configurar início de sessão** janela. Copiar o **SAML único início de sessão no URL do serviço** da seção de referência rápida.

    ![Configurar o início de sessão único](./media/opsgenie-tutorial/tutorial_opsgenie_configure.png)

1. Abra outra instância do browser e, em seguida, iniciar sessão na OpsGenie como administrador.

1. Clique em **configurações**e, em seguida, clique nas **início de sessão único** separador.
   
    ![OpsGenie Single Sign-On](./media/opsgenie-tutorial/tutorial_opsgenie_06.png)

1. Para ativar o SSO, selecione **ativado**.
   
    ![Definições de OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_07.png) 

1. Na **fornecedor** secção, clique nas **Azure Active Directory** separador.
   
    ![Definições de OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_08.png) 

1. Na página de diálogo do Azure Active Directory, execute os seguintes passos:
   
    ![Definições de OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_09.png)
    
    a. Na **SAML 2.0 Endpoint** caixa de texto, colar **único início de sessão no URL do serviço**valor que copiou do portal do Azure.
    
    b. Na **Url de metadados:** caixa de texto, colar **Url de metadados de Federação de aplicação** valor que copiou do portal do Azure.
    
    c. Clique em **guardar alterações**.

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/opsgenie-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/opsgenie-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/opsgenie-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/opsgenie-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-opsgenie-test-user"></a>Criar um utilizador de teste OpsGenie

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no OpsGenie. 

1. Numa janela do browser web, inicie sessão no seu inquilino OpsGenie como administrador.

1. Navegue até à lista de utilizadores clicando **utilizador** no painel esquerdo.
   
   ![Definições de OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_10.png) 

1. Clique em **adicionar utilizador**.

1. Sobre o **adicionar utilizador** caixa de diálogo, execute os seguintes passos:
   
   ![Definições de OpsGenie](./media/opsgenie-tutorial/tutorial_opsgenie_11.png)
   
   a. Na **E-Mail** caixa de texto, o endereço de e-mail typu BrittaSimon resolvidos no Azure Active Directory.
   
   b. Na **FullName** caixa de texto, tipo **Eduarda Almeida**.
   
   c. Clique em **Guardar**. 

>[!NOTE]
>Eduarda obtém uma mensagem de e-mail com instruções sobre como configurar o seu perfil.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para OpsGenie.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a OpsGenie, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **OpsGenie**.

    ![Configurar o início de sessão único](./media/opsgenie-tutorial/tutorial_opsgenie_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção consiste em testar a configuração de SSO do Azure AD através do painel de acesso.

Quando clica no mosaico OpsGenie no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo OpsGenie.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/opsgenie-tutorial/tutorial_general_04.png

[100]: ./media/opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/opsgenie-tutorial/tutorial_general_201.png
[202]: ./media/opsgenie-tutorial/tutorial_general_202.png
[203]: ./media/opsgenie-tutorial/tutorial_general_203.png

