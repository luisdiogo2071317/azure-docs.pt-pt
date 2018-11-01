---
title: 'Tutorial: Integração do Azure Active Directory com a linha WORKS | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e FUNCIONA de linha.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 725f32fd-d0ad-49c7-b137-1cc246bf85d7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: jeedes
ms.openlocfilehash: 0437bd46537d8b7024618706aba12660abca5512
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50414697"
---
# <a name="tutorial-azure-active-directory-integration-with-line-works"></a>Tutorial: Integração do Azure Active Directory com a linha FUNCIONA

Neste tutorial, saiba como integrar a linha FUNCIONA com o Azure Active Directory (Azure AD).

Integração de linha FUNCIONA com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao FUNCIONA de linha.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para a linha WORKS (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com linha FUNCIONA, precisa do seguinte:

- Uma subscrição do Azure
- Uma linha FUNCIONA logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando a linha FUNCIONA da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-line-works-from-the-gallery"></a>Adicionando a linha FUNCIONA da Galeria
Para configurar a integração de linha FUNCIONA com o Azure AD, terá de Adicionar linha FUNCIONA a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar FUNCIONA de linha a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![image](./media/worksmobile-tutorial/selectazuread.png)

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![image](./media/worksmobile-tutorial/a_select_app.png)
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![image](./media/worksmobile-tutorial/a_new_app.png)

4. Na caixa de pesquisa, escreva **linha FUNCIONA**, selecione **FUNCIONA de linha** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

     ![image](./media/worksmobile-tutorial/tutorial_lineworks_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com FUNCIONA de linha com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de equivalente na linha FUNCIONA para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na linha WORKS deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com linha FUNCIONA, precisa concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de linha FUNCIONA](#create-a-line-works-test-user)**  - para ter um equivalente da Eduarda Almeida na FUNCIONA de linha que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação de linha FUNCIONA.

**Para configurar o Azure AD início de sessão único com linha FUNCIONA, execute os seguintes passos:**

1. Na [portal do Azure](https://portal.azure.com/), na **FUNCIONA de linha** página de integração de aplicações, selecione **início de sessão único**.

    ![image](./media/worksmobile-tutorial/b1_b2_select_sso.png)

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, clique em **selecione** para **SAML** modo para ativar o início de sessão único.

    ![image](./media/worksmobile-tutorial/b1_b2_saml_sso.png)

3. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **configuração básica de SAML** caixa de diálogo.

    ![image](./media/worksmobile-tutorial/b1-domains_and_urlsedit.png)

4. Sobre o **configuração básica de SAML** secção, execute os seguintes passos:

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://auth.worksmobile.com/d/login/<domain>/?userId=<ID@domain>`

    b. Na **identificador** caixa de texto, escreva um URL: `worksmobile.com`

    ![image](./media/worksmobile-tutorial/tutorial_lineworks_url.png)

5. No **definido no início de sessão único com o SAML** página, além do **certificado de assinatura SAML** secção, clique em **transferir** para transferir o **certificado (bruto)** e guarde-o no seu computador.

    ![image](./media/worksmobile-tutorial/tutorial_lineworks_certficate.png)

6. Na **configurar a linha FUNCIONA** secção, copie o URL adequado, de acordo com seus requisitos.

    a. URL de início de sessão

    b. Identificador do Azure AD

    c. URL de fim de sessão

    ![image](./media/worksmobile-tutorial/d1_samlsonfigure.png) 

7. Para configurar o início de sessão único em **linha FUNCIONA** lado, leia o [documentos de linha FUNCIONA SSO](https://developers.worksmobile.com/jp/document/1001080101) e configurar uma definição de linha FUNCIONA.

> [!NOTE]
> Precisa converter o ficheiro de certificado transferido de .cert. pem


### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![image](./media/worksmobile-tutorial/d_users_and_groups.png)

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![image](./media/worksmobile-tutorial/d_adduser.png)

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![image](./media/worksmobile-tutorial/d_userproperties.png)

    a. Na **Name** campo introduza **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.
 
### <a name="create-a-line-works-test-user"></a>Criar um utilizador de teste de linha FUNCIONA

Nesta secção, vai criar um usuário chamado Eduarda Almeida na linha FUNCIONA. Acesso [página de administração de linha FUNCIONA](https://admin.worksmobile.com) e adicione os utilizadores na plataforma FUNCIONA de linha.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida a utilizar do Azure-início de sessão único, conceder acesso à linha FUNCIONA.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![image](./media/worksmobile-tutorial/d_all_applications.png)

2. Na lista de aplicações, selecione **linha FUNCIONA**.

    ![image](./media/worksmobile-tutorial/tutorial_lineworks_app.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![image](./media/worksmobile-tutorial/d_leftpaneusers.png)

4. Selecione o **Add** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![image](./media/worksmobile-tutorial/d_assign_user.png)

4. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

5. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de linha FUNCIONA no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de linha FUNCIONA.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)


