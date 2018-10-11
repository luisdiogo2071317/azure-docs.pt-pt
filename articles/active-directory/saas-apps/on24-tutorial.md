---
title: 'Tutorial: Integração do Azure Active Directory com a ligação de SAML de ambiente Virtual ON24 | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e ON24 ligação de SAML de ambiente Virtual.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d4028fb5-b2ad-4c5d-b123-7b675c509d64
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.openlocfilehash: 1ec18f0013a7fa640395a8b8bedd9df8b0924c3a
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49071323"
---
# <a name="tutorial-azure-active-directory-integration-with-on24-virtual-environment-saml-connection"></a>Tutorial: Integração do Azure Active Directory com a ligação de SAML de ambiente Virtual ON24

Neste tutorial, saiba como integrar ON24 ligação de SAML de ambiente Virtual com o Azure Active Directory (Azure AD).

Integrar ON24 ligação de SAML de ambiente Virtual no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao ON24 ligação de SAML de ambiente Virtual.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para ON24 Virtual ambiente SAML Connection (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com ligação de SAML de ambiente Virtual ON24, terá dos seguintes itens:

- Uma subscrição do Azure
- Uma ligação de SAML de ambiente Virtual ON24 logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar ligação de SAML de ambiente Virtual ON24 partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-on24-virtual-environment-saml-connection-from-the-gallery"></a>Adicionar ligação de SAML de ambiente Virtual ON24 partir da Galeria
Para configurar a integração de conexão de SAML de ambiente Virtual ON24 com o Azure AD, terá de adicionar ligação de SAML de ambiente Virtual ON24 a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar a ligação de SAML de ambiente Virtual ON24 partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![image](./media/on24-tutorial/selectazuread.png)

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![image](./media/on24-tutorial/a_select_app.png)
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![image](./media/on24-tutorial/a_new_app.png)

4. Na caixa de pesquisa, escreva **ligação de SAML de ambiente Virtual ON24**, selecione **ligação de SAML de ambiente Virtual ON24** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

     ![image](./media/on24-tutorial/tutorial_on24_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com ON24 Virtual ambiente SAML ligação com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de equivalente na ligação de SAML de ambiente Virtual ON24 a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na ligação de SAML de ambiente Virtual ON24 deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com ligação de SAML de ambiente Virtual ON24, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de ligação de SAML de ambiente Virtual ON24](#create-an-on24-virtual-environment-saml-connection-test-user)**  - para ter um equivalente da Eduarda Almeida na ON24 ambiente SAML ligação Virtual que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação de ligação de SAML de ambiente Virtual ON24.

**Para configurar o Azure AD início de sessão único com ligação de SAML de ambiente Virtual ON24, execute os seguintes passos:**

1. Na [portal do Azure](https://portal.azure.com/), na **ligação de SAML de ambiente Virtual ON24** página de integração de aplicações, selecione **início de sessão único**.

    ![image](./media/on24-tutorial/B1_B2_Select_SSO.png)

2. Clique em **alterar único início de sessão em modo** na parte superior do ecrã para selecionar o **SAML** modo.

      ![image](./media/on24-tutorial/b1_b2_saml_ssso.png)

3. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, selecione **SAML** modo para ativar o início de sessão único.

    ![image](./media/on24-tutorial/b1_b2_saml_sso.png)

4. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **configuração básica de SAML** caixa de diálogo.

    ![image](./media/on24-tutorial/b1-domains_and_urlsedit.png)

5. Sobre o **configuração básica de SAML** secção execute os seguintes passos, se desejar configurar a aplicação no **IDP** intiated modo:

    ![image](./media/on24-tutorial/tutorial_on24_url.png)

    a. Na **identificador** caixa de texto, escreva um URL:

     **URL de ambiente de produção**
    
    `SAML-VSHOW.on24.com`

    `SAML-Gateway.on24.com` 

    `SAP PROD SAML-EliteAudience.on24.com` 
                
     **URL de ambiente de controle de qualidade**
    
    `SAMLQA-VSHOW.on24.com` 

    `SAMLQA-Gateway.on24.com` 

    `SAMLQA-EliteAudience.on24.com`
 
    b. Na **URL de resposta** caixa de texto, escreva um URL:
    
     **URL de ambiente de produção**
    
    `https://federation.on24.com/sp/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1WU2hvdy5vbjI0LmNvbSJ9/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1HYXRld2F5Lm9uMjQuY29tIn0/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1FbGl0ZUF1ZGllbmNlLm9uMjQuY29tIn0/ACS.saml2`

     **URL de ambiente de controle de qualidade**
    
    `https://qafederation.on24.com/sp/ACS.saml2`

    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLVZzaG93Lm9uMjQuY29tIn0/ACS.saml2`

    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLUdhdGV3YXkub24yNC5jb20ifQ/ACS.saml2`
     
    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLUVsaXRlQXVkaWVuY2Uub24yNC5jb20ifQ/ACS.saml2` 

    c. Clique em **definir URLs adicionais**. 

    d. Na **estado de reencaminhamento** caixa de texto, escreva um URL: `https://vshow.on24.com/vshow/ms_azure_saml_test?r=<ID>`

    e. Se desejar configurar a aplicação no **SP** intiated modo, na **URL de início de sessão** caixa de texto, escreva um URL: `https://vshow.on24.com/vshow/<INSTANCENAME>`

6. No **definir a segurança de início de sessão único com o SAML** na página a **certificado de assinatura SAML** secção, clique em **transferir** para transferir o certificado apropriado, de acordo sua requisito e guarde-o no seu computador.

    ![image](./media/on24-tutorial/tutorial_on24_certificate.png) 

7. Para configurar o início de sessão único num **ligação de SAML de ambiente Virtual ON24** lado, terá de enviar o certificado/metadados que transferiu a partir do portal do Azure para [ON24 ligação de SAML de ambiente Virtual equipa de suporte](https://www.on24.com/about-us/support/). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![image](./media/on24-tutorial/d_users_and_groups.png)

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![image](./media/on24-tutorial/d_adduser.png)

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![image](./media/on24-tutorial/d_userproperties.png)

    a. Na **Name** campo introduza **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.
 
### <a name="create-an-on24-virtual-environment-saml-connection-test-user"></a>Criar um utilizador de teste de ligação de SAML de ambiente Virtual ON24

Nesta secção, vai criar um usuário chamado Eduarda Almeida na ligação de SAML de ambiente Virtual ON24. Trabalhar com [equipa de suporte de ligação de SAML de ambiente Virtual ON24](https://www.on24.com/about-us/support/) para adicionar os utilizadores na plataforma ON24 ligação de SAML de ambiente Virtual. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida a utilizar do Azure-início de sessão único, conceder acesso à ligação de SAML de ambiente Virtual ON24.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![image](./media/on24-tutorial/d_all_applications.png)

2. Na lista de aplicações, selecione **ligação de SAML de ambiente Virtual ON24**.

    ![image](./media/on24-tutorial/tutorial_on24_app.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![image](./media/on24-tutorial/d_leftpaneusers.png)

4. Selecione o **Add** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![image](./media/on24-tutorial/d_assign_user.png)

4. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

5. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de ligação de SAML de ambiente Virtual ON24 no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de ligação de SAML de ambiente Virtual ON24.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

