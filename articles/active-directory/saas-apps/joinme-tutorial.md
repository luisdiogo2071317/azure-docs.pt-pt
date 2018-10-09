---
title: 'Tutorial: Integração do Azure Active Directory com join.me | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e join.me.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cda5ea0d-3270-4ba5-ad81-4df108eaad12
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.openlocfilehash: dc393b8701bd72ea7f9de543de33116ea3cb5653
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48871741"
---
# <a name="tutorial-azure-active-directory-integration-with-joinme"></a>Tutorial: Integração do Azure Active Directory com join.me

Neste tutorial, saiba como integrar join.me com o Azure Active Directory (Azure AD).

Integrar join.me no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao join.me.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para join.me (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com join.me, terá dos seguintes itens:

- Uma subscrição do Azure
- Um join.me logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando join.me da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-joinme-from-the-gallery"></a>Adicionando join.me da Galeria
Para configurar a integração do join.me com o Azure AD, terá de adicionar join.me a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar join.me a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![image](./media/joinme-tutorial/selectazuread.png)

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![image](./media/joinme-tutorial/a_select_app.png)
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![image](./media/joinme-tutorial/a_new_app.png)

4. Na caixa de pesquisa, escreva **join.me**, selecione **join.me** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

     ![image](./media/joinme-tutorial/tutorial_joinme_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com join.me com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no join.me a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no join.me deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com join.me, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste join.me](#create-a-joinme-test-user)**  - para ter um equivalente da Eduarda Almeida na join.me que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo join.me.

**Para configurar o Azure AD início de sessão único com join.me, execute os seguintes passos:**

1. Na [portal do Azure](https://portal.azure.com/), na **join.me** página de integração de aplicações, selecione **início de sessão único**.

    ![image](./media/joinme-tutorial/b1_b2_select_sso.png)

2. Clique em **modo de início de sessão único de alteração** na parte superior do ecrã para selecionar o **SAML** modo.

      ![image](./media/joinme-tutorial/b1_b2_saml_ssso.png)

3. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, clique em **selecione** para **SAML** modo para ativar o início de sessão único.

    ![image](./media/joinme-tutorial/b1_b2_saml_sso.png)

4. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **configuração básica de SAML** caixa de diálogo.

    ![image](./media/joinme-tutorial/b1-domains_and_urlsedit.png)

5. Sobre o **configuração básica de SAML** secção, o utilizador não tem de realizar qualquer passo como a aplicação já está pré-integrada com o Azure.

     ![image](./media/joinme-tutorial/tutorial_joinme_url.png)
 
6. No **definir a segurança de início de sessão único com o SAML** na página a **certificado de assinatura SAML** secção, clique em botão Copiar para copiar **Url de metadados de Federação de aplicação** e guarde-o no seu computador.

    ![image](./media/joinme-tutorial/certificatebase64.png) 

7. Para configurar o início de sessão único num **join.me** lado, terá de enviar o **Url de metadados de Federação de aplicação** que copiou do portal do Azure para [equipa de suporte de join.me](https://help.join.me/s/?language). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![image](./media/joinme-tutorial/d_users_and_groups.png)

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![image](./media/joinme-tutorial/d_adduser.png)

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![image](./media/joinme-tutorial/d_userproperties.png)

    a. Na **Name** campo introduza **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.
  
### <a name="create-a-joinme-test-user"></a>Criar um utilizador de teste join.me

Nesta secção, vai criar um usuário chamado Eduarda Almeida no join.me. Trabalhar com [equipa de suporte de join.me](https://help.join.me/s/?language) para adicionar os utilizadores na plataforma join.me. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso a join.me.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![image](./media/joinme-tutorial/d_all_applications.png)

2. Na lista de aplicações, selecione **join.me**.

    ![image](./media/joinme-tutorial/tutorial_joinme_app.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![image](./media/joinme-tutorial/d_leftpaneusers.png)

4. Selecione o **Add** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![image](./media/joinme-tutorial/d_assign_user.png)

4. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

5. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico join.me no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo join.me.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

