---
title: 'Tutorial: Integração do Azure Active Directory com Appraisd | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Appraisd.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: db063306-4d0d-43ca-aae0-09f0426e7429
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2018
ms.author: jeedes
ms.openlocfilehash: dae9a59b89f03a50b1adaed55cd8f97c06906526
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/11/2018
ms.locfileid: "49118666"
---
# <a name="tutorial-azure-active-directory-integration-with-appraisd"></a>Tutorial: Integração do Azure Active Directory com Appraisd

Neste tutorial, saiba como integrar Appraisd com o Azure Active Directory (Azure AD).

Integrar Appraisd no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Appraisd.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Appraisd (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Appraisd, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Appraisd logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Appraisd da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-appraisd-from-the-gallery"></a>Adicionando Appraisd da Galeria
Para configurar a integração do Appraisd com o Azure AD, terá de adicionar Appraisd a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Appraisd a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![image](./media/appraisd-tutorial/selectazuread.png)

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![image](./media/appraisd-tutorial/a_select_app.png)
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![image](./media/appraisd-tutorial/a_new_app.png)

4. Na caixa de pesquisa, escreva **Appraisd**, selecione **Appraisd** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

     ![image](./media/appraisd-tutorial/tutorial_appraisd_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Appraisd com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Appraisd a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Appraisd deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Appraisd, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Appraisd](#create-an-appraisd-test-user)**  - para ter um equivalente da Eduarda Almeida na Appraisd que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Appraisd.

**Para configurar o Azure AD início de sessão único com Appraisd, execute os seguintes passos:**

1. Na [portal do Azure](https://portal.azure.com/), na **Appraisd** página de integração de aplicações, selecione **início de sessão único**.

    ![image](./media/appraisd-tutorial/B1_B2_Select_SSO.png)

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, selecione **SAML** modo para ativar o início de sessão único.

    ![image](./media/appraisd-tutorial/b1_b2_saml_sso.png)

3. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **configuração básica de SAML** caixa de diálogo.

    ![image](./media/appraisd-tutorial/b1-domains_and_urlsedit.png)

4. Sobre o **configuração básica de SAML** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_url.png)

    a. Clique em **definir URLs adicionais**. 

    b. Na **estado de reencaminhamento** caixa de texto, escreva um URL: `<TENANTCODE>`

    c. Se desejar configurar a aplicação no **SP** iniciadas pelo modo, na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://app.appraisd.com/saml/<TENANTCODE>`

    > [!NOTE]
    > Obter o valor de URL de início de sessão e estado de reencaminhamento real na página de configuração de SSO Appraisd do que é explicada mais tarde no tutorial.
    
5. Aplicação de Appraisd espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. Clique em **edite** botão para abrir **atributos de utilizador** caixa de diálogo.

    ![image](./media/appraisd-tutorial/i3-attribute.png)

6. No **afirmações de utilizador** secção sobre o **atributos de utilizador** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    a. Clique em **edite** botão para abrir o **afirmações de utilizador de gerir** caixa de diálogo.

    ![image](./media/appraisd-tutorial/i2-attribute.png)

    ![image](./media/appraisd-tutorial/i4-attribute.png)

    b. Partir do **atributo de origem** , selecione o valor do atributo.

    c. Clique em **Guardar**. 

7. Na **certificado de assinatura SAML** secção, clique em **transferir** para transferir o **certificado (Base64)** e guarde-o no seu computador.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_certficate.png) 

8. Sobre o **configurar Appraisd** secção, copie o URL adequado, de acordo com seus requisitos.

    a. URL de início de sessão

    b. Identificador do Azure Ad

    c. URL de fim de sessão

    ![image](./media/appraisd-tutorial/d1_samlsonfigure.png)

9. Numa janela do browser web diferente, inicie sessão no Appraisd como um administrador de segurança.

10. No canto superior direito da página, clique em **definições** ícone, em seguida, navegue até à **configuração**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_sett.png)

11. Do lado esquerdo do menu, clique em **SAML início de sessão único**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_single.png)

12. Sobre o **SAML 2.0 Single Sign-On configuração** página, execute os seguintes passos:
    
    ![image](./media/appraisd-tutorial/tutorial_appraisd_saml.png)

    a. Cópia a **estado de reencaminhamento do padrão** valor e cole-o na **estado de reencaminhamento** caixa de texto no **configuração básica de SAML** no portal do Azure.

    b. Copiar o **URL de início de sessão iniciadas pelo serviço** valor e cole-o na **URL de início de sessão** caixa de texto no **configuração básica de SAML** no portal do Azure.

13. Desloque para baixo na mesma página sob **identificar utilizadores**, execute os seguintes passos:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_identifying.png)

    a. Na **URL fornecedor de identidade única Sign-On** caixa de texto, cole o valor de **URL de início de sessão**, que copiou do portal do Azure e clique em **guardar**.

    b. Na **URL de emissor do fornecedor de identidade** caixa de texto, cole o valor de **identificador do Azure Ad**, que copiou do portal do Azure e clique em **guardar**.

    c. No bloco de notas, abra o certificado com codificação base 64 que transferiu a partir do portal do Azure, copie o seu conteúdo e, em seguida, cole-o para o **certificado X.509** caixa e clique em **guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![image](./media/appraisd-tutorial/d_users_and_groups.png)

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![image](./media/appraisd-tutorial/d_adduser.png)

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![image](./media/appraisd-tutorial/d_userproperties.png)

    a. Na **Name** , insira **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.
 
### <a name="create-an-appraisd-test-user"></a>Criar um utilizador de teste Appraisd

Para ativar o Azure AD os utilizadores iniciam sessão Appraisd, eles têm de ser aprovisionados em Appraisd. Appraisd, aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no Appraisd como um administrador de segurança.

2. No canto superior direito da página, clique em **definições** ícone, em seguida, navegue até à **Centro de administração**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_admin.png)

3. Na barra de ferramentas na parte superior da página, clique em **pessoas**, em seguida, navegue até à **adicionar um novo utilizador**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_user.png)

4. Sobre o **adicionar um novo utilizador** página, execute os seguintes passos:

    ![image](./media/appraisd-tutorial/tutorial_appraisd_newuser.png)
    
    a. Na **nome próprio** texto, introduza o nome de utilizador, como **Eduarda**.

    b. Na **Apelido** texto, digite o apelido do utilizador, como **simon**.

    c. Na **E-Mail** texto, introduza o e-mail do utilizador, como **Brittasimon@contoso.com**.

    d. Clique em **Adicionar utilizador**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Appraisd.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![image](./media/appraisd-tutorial/d_all_applications.png)

2. Na lista de aplicações, selecione **Appraisd**.

    ![image](./media/appraisd-tutorial/tutorial_appraisd_app.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![image](./media/appraisd-tutorial/d_leftpaneusers.png)

4. Selecione o **Add** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![image](./media/appraisd-tutorial/d_assign_user.png)

4. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

5. Na **adicionar atribuição** caixa de diálogo, selecione a **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Appraisd no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Appraisd.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)
