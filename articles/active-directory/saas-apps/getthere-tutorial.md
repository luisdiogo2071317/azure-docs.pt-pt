---
title: 'Tutorial: Integração do Active Directory do Azure com GetThere | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e GetThere.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0441087e-953f-4b51-9842-316da7b72392
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b132da4a763490fa6c7a73c80f8e2e3a11b42e9b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56173135"
---
# <a name="tutorial-azure-active-directory-integration-with-getthere"></a>Tutorial: Integração do Active Directory do Azure com GetThere

Neste tutorial, saiba como integrar GetThere com o Azure Active Directory (Azure AD).

Integrar GetThere no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao GetThere.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para GetThere (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com GetThere, terá dos seguintes itens:

- Uma subscrição do Azure
- Um GetThere logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando GetThere da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-getthere-from-the-gallery"></a>Adicionando GetThere da Galeria
Para configurar a integração do GetThere com o Azure AD, terá de adicionar GetThere a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar GetThere a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![image](./media/getthere-tutorial/selectazuread.png)

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![image](./media/getthere-tutorial/a_select_app.png)
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![image](./media/getthere-tutorial/a_new_app.png)

4. Na caixa de pesquisa, escreva **GetThere**, selecione **GetThere** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

     ![image](./media/getthere-tutorial/tutorial_getthere_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com GetThere com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no GetThere a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no GetThere deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com GetThere, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste GetThere](#create-a-getthere-test-user)**  - para ter um equivalente da Eduarda Almeida na GetThere que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo GetThere.

**Para configurar o Azure AD início de sessão único com GetThere, execute os seguintes passos:**

1. Na [portal do Azure](https://portal.azure.com/), na **GetThere** página de integração de aplicações, selecione **início de sessão único**.

    ![image](./media/getthere-tutorial/B1_B2_Select_SSO.png)

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, selecione **SAML** modo para ativar o início de sessão único.

    ![image](./media/getthere-tutorial/b1_b2_saml_sso.png)

3. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **configuração básica de SAML** caixa de diálogo.

    ![image](./media/getthere-tutorial/b1-domains_and_urlsedit.png)

4. Sobre o **configuração básica de SAML** secção, execute os seguintes passos:

    ![image](./media/getthere-tutorial/tutorial_getthere_url.png)

    a. Na **identificador** texto, escreva qualquer uma do abaixo URLs:
    | |
    |--|
    | `getthere.com` |
    | `http://idp.getthere.com` |

    b. Na **URL de resposta** texto, escreva qualquer uma do abaixo URLs:
    | |
    |--|
    | `https://wx1.getthere.net/login/saml/post.act` |
    | `https://gtx2-gcte2.getthere.net/login/saml/post.act` |
    | `https://gtx2-gcte2.getthere.net/login/saml/ssoaasvalidate.act` |
    | `https://wx1.getthere.net/login/saml/ssoaavalidate.act` |
    
5. Aplicação GetThere espera que o exclusivo **nome de utilizador** valor na afirmação de nome de utilizador. Cliente pode mapear o valor correto para a afirmação de nome de utilizador. No exemplo abaixo, estamos mapeou a **nome de utilizador** ao **user.mail**, mas pode alterar o mapeamento de acordo com as definições da organização. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **atributos de utilizador** caixa de diálogo.

    ![image](./media/getthere-tutorial/i4-attribute.png)

6. No **afirmações de utilizador** secção sobre o **atributos de utilizador** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Name |  Atributo de origem |  Espaço de nomes |
    | ---------------| --------------- | --------------- |
    | Sitename | "O valor de acordo com a sua organização fornecer" | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/sitename |
    | Nome de utilizador |  user.mail | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/username |
    
    a. Clique em **Adicionar nova afirmação** para abrir o **afirmações de utilizador de gerir** caixa de diálogo.

    ![image](./media/getthere-tutorial/i2-attribute.png)

    ![image](./media/getthere-tutorial/i3-attribute.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Na **espaço de nomes** caixa de texto, escreva o espaço de nomes de atributo apresentado para essa linha.

    d. Selecione a origem de dado **atributo**.

    e. Partir do **atributo de origem** lista, digite o valor de atributo apresentado para essa linha.

    f. Clique em **Guardar**.

7. No **definido no início de sessão único com o SAML** página, além do **certificado de assinatura SAML** secção, clique em **transferir** para transferir o **certificado (Base64)** e guarde-o no seu computador.

    ![image](./media/getthere-tutorial/tutorial_getthere_certficate.png) 

8. Sobre o **configurar GetThere** secção, copie o URL adequado, de acordo com seus requisitos.

    Tenha em atenção que o URL pode dizer o seguinte:

    a. URL de início de sessão

    b. Azure Ad Identifier

    c. URL de fim de sessão

    ![image](./media/getthere-tutorial/d1_samlsonfigure.png) 

9. Para configurar o início de sessão único num **GetThere** lado, terá de enviar o transferido **certificado (Base64)** e copiado **URL de fim de sessão do URL de início de sessão, o identificador do Azure Ad,** para [ A equipa de suporte do cliente de GetThere](mailto:dataintegration@sabre.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![image](./media/getthere-tutorial/d_users_and_groups.png)

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![image](./media/getthere-tutorial/d_adduser.png)

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![image](./media/getthere-tutorial/d_userproperties.png)

    a. Na **Name** , insira **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.
 
### <a name="create-a-getthere-test-user"></a>Criar um utilizador de teste GetThere

Nesta secção, vai criar um usuário chamado Eduarda Almeida no GetThere. Trabalhar com [equipa de suporte de cliente GetThere](mailto:dataintegration@sabre.com) para adicionar os utilizadores na plataforma GetThere. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para GetThere.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![image](./media/getthere-tutorial/d_all_applications.png)

2. Na lista de aplicações, selecione **GetThere**.

    ![image](./media/getthere-tutorial/tutorial_getthere_app.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![image](./media/getthere-tutorial/d_leftpaneusers.png)

4. Selecione o **Add** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![image](./media/getthere-tutorial/d_assign_user.png)

4. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

5. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico GetThere no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo GetThere.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)
