---
title: 'Tutorial: Integração do Active Directory do Azure com Expensify | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Expensify.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 1e761484-7a2f-4321-91f4-6d5d0b69344e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2018
ms.author: jeedes
ms.openlocfilehash: 1f69ad3045693e1ca826b6e04443f97ac30bc377
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55156943"
---
# <a name="tutorial-azure-active-directory-integration-with-expensify"></a>Tutorial: Integração do Active Directory do Azure com Expensify

Neste tutorial, saiba como integrar Expensify com o Azure Active Directory (Azure AD).

Integrar Expensify no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Expensify.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Expensify (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Expensify, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Expensify logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Expensify da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-expensify-from-the-gallery"></a>Adicionando Expensify da Galeria

Para configurar a integração do Expensify com o Azure AD, terá de adicionar Expensify a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Expensify a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![image](./media/expensify-tutorial/selectazuread.png)

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![image](./media/expensify-tutorial/a_select_app.png)
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![image](./media/expensify-tutorial/a_new_app.png)

4. Na caixa de pesquisa, escreva **Expensify**, selecione **Expensify** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

     ![image](./media/expensify-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Expensify com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Expensify a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Expensify deve ser estabelecido.

Expensify, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Expensify, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Expensify](#create-an-expensify-test-user)**  - para ter um equivalente da Eduarda Almeida na Expensify que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Expensify.

**Para configurar o Azure AD início de sessão único com Expensify, execute os seguintes passos:**

1. Na [portal do Azure](https://portal.azure.com/), na **Expensify** página de integração de aplicações, selecione **início de sessão único**.

    ![image](./media/expensify-tutorial/b1_b2_select_sso.png)

2. Clique em **modo de início de sessão único de alteração** na parte superior do ecrã para selecionar o **SAML** modo.

      ![image](./media/expensify-tutorial/b1_b2_saml_ssso.png)

3. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, clique em **selecione** para **SAML** modo para ativar o início de sessão único.

    ![image](./media/expensify-tutorial/b1_b2_saml_sso.png)

4. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **configuração básica de SAML** caixa de diálogo.

    ![image](./media/expensify-tutorial/b1-domains_and_urlsedit.png)

5. Sobre o **configuração básica de SAML** secção, execute os seguintes passos:

    a. Na **URL de início de sessão** caixa de texto, escreva um URL como: `https://www.expensify.com/authentication/saml/login`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://www.<companyname>.expensify.com`

    ![image](./media/expensify-tutorial/b1-domains_and_urls.png)

    > [!NOTE] 
    > Substitua o <companyname> secção do URL de identificador com o domínio da sua empresa. Veja o exemplo de `https://contoso.expensify.com` acima. No Expensify, este é o nome do seu domínio conforme mostrado nos **definições > controlo de domínio**.

    ![Expensify informações de domínio](./media/expensify-tutorial/tutorial_expensify_domain.png)

6. No **definir a segurança de início de sessão único com o SAML** na página a **certificado de assinatura SAML** secção, clique em **transferir** para transferir o certificado apropriado, de acordo sua requisito e guarde-o no seu computador.

    ![image](./media/expensify-tutorial/certificatebase64.png)

7. Para ativar o SSO no Expensify, primeiro tem de ativar **controlo de domínio** no aplicativo. Pode ativar o controlo de domínio do aplicativo os passos listados [aqui](https://help.expensify.com/domain-control). Para obter suporte adicional, trabalhar com [equipa de suporte de cliente Expensify](mailto:help@expensify.com). Assim que tiver o controlo de domínio, siga estes passos:
   
    ![Configurar o início de sessão único](./media/expensify-tutorial/tutorial_expensify_51.png)
    
    a. Inicie sessão seu aplicativo Expensify.
    
    b. No painel esquerdo, clique em **configurações** e navegue até à **SAML**.
    
    c. Ativar/desativar a **início de sessão de SAML** opção como **ativado**.
    
    d. Abrir os metadados de Federação transferido a partir do Azure AD no bloco de notas, copie o conteúdo e, em seguida, cole-o para o **metadados de fornecedor de identidade** caixa de texto.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD 

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![image](./media/expensify-tutorial/d_users_and_groups.png)

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![image](./media/expensify-tutorial/d_adduser.png)

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![image](./media/expensify-tutorial/d_userproperties.png)

    a. Na **Name** campo introduza **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.
 
### <a name="create-an-expensify-test-user"></a>Criar um utilizador de teste Expensify

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Expensify. Trabalhar com [equipa de suporte de cliente Expensify](mailto:help@expensify.com) para adicionar os utilizadores na plataforma Expensify.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Expensify.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![image](./media/expensify-tutorial/d_all_applications.png)

2. Na lista de aplicações, selecione **Expensify**.

    ![image](./media/expensify-tutorial/d_all_proapplications.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![image](./media/expensify-tutorial/d_leftpaneusers.png)

4. Selecione o **Add** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![image](./media/expensify-tutorial/d_assign_user.png)

4. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

5. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Expensify no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Expensify.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)




