---
title: 'Tutorial: Integração do Active Directory do Azure com a publicação de arco - SSO | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e publicação de arco - SSO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ae609583-f875-4cb8-b68e-1b0b7938e9a7
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d14b352f335d0e0f409c7723fd9395eac6f2a48
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56209478"
---
# <a name="tutorial-azure-active-directory-integration-with-arc-publishing---sso"></a>Tutorial: Integração do Active Directory do Azure com a publicação de arco - SSO

Neste tutorial, saiba como integrar a publicação de arco - SSO com o Azure Active Directory (Azure AD).
Integrar o arco de publicação - SSO com o Azure AD fornece as seguintes vantagens:

* Pode controlar no Azure AD que tenha acesso à publicação de arco - SSO.
* Pode permitir que os utilizadores ser automaticamente sessão iniciada para publicação de arco - SSO (Single Sign-On) com as suas contas do Azure AD.
* Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com a publicação de arco - SSO, terá dos seguintes itens:

* Uma subscrição do Azure AD. Se não tiver um ambiente do Azure AD, pode obter a versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/)
* Arco de publicação - SSO logon único habilitado subscrição

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, configure e teste do Azure AD início de sessão único num ambiente de teste.

* Arco de publicação - suporta SSO **SP e IDP** iniciada SSO
* Arco de publicação - suporta SSO **Just In Time** aprovisionamento de utilizadores

## <a name="adding-arc-publishing---sso-from-the-gallery"></a>Adicionar publicação de arco - SSO partir da Galeria

Para configurar a integração de publicação de arco - SSO para o Azure AD, terá de adicionar a publicação de arco - SSO partir da galeria, à sua lista de aplicações de SaaS geridas.

**Para adicionar publicação de arco - SSO a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![O botão do Azure Active Directory](common/select-azuread.png)

2. Navegue para **aplicações empresariais** e, em seguida, selecione a **todos os aplicativos** opção.

    ![O painel de aplicações empresariais](common/enterprise-applications.png)

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, escreva **publicação arco - SSO**, selecione **publicação arco - SSO** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

     ![Publicação de arco - SSO na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configurar e testar o Azure AD início de sessão único com o arco de publicação - SSO com base num utilizador de teste **Eduarda Almeida**.
Para o início de sessão único para o trabalho, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na publicação de arco - SSO tem de ser estabelecida.

Para configurar e testar o Azure AD início de sessão único com a publicação de arco - SSO, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Configurar a publicação de arco - SSO Single Sign-On](#configure-arc-publishing---sso-single-sign-on)**  - para configurar as definições de início de sessão único no lado do aplicativo.
3. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Criar a publicação de arco - utilizador de teste de SSO](#create-arc-publishing---sso-test-user)**  - para ter um equivalente da Eduarda Almeida na publicação de arco - SSO que está ligado à representação de utilizador do Azure AD.
6. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, vai ativar o Azure AD início de sessão único no portal do Azure.

Para configurar o Azure AD início de sessão único com a publicação de arco - SSO, execute os seguintes passos:

1. Na [portal do Azure](https://portal.azure.com/), na **publicação arco - SSO** página de integração de aplicações, selecione **início de sessão único**.

    ![Configurar a ligação de início de sessão única](common/select-sso.png)

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, selecione **SAML/WS-Fed** modo para ativar o início de sessão único.

    ![Único início de sessão em modo de seleção](common/select-saml-option.png)

3. Sobre o **definir a segurança de início de sessão único com o SAML** página, clique em **editar** ícone para abrir **configuração básica de SAML** caixa de diálogo.

    ![Editar a configuração SAML do básico](common/edit-urls.png)

4. Sobre o **configuração básica de SAML** secção, se desejar configurar a aplicação no **IDP** iniciada pelo modo, execute os seguintes passos:

    ![Publicação de arco - SSO domínio e URLs únicas início de sessão em informações](common/idp-intiated.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://www.okta.com/saml2/service-provider/<Unique ID>`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`

5. Clique em **definir URLs adicionais** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Publicação de arco - SSO domínio e URLs únicas início de sessão em informações](common/metadata-upload-additional-signon.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão:  `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o URL de identificador, o URL de resposta e início de sessão real. Contacte [publicação arco - equipa de suporte de cliente de SSO](mailto:inf@washpost.com) obter esses valores. Também pode consultar os padrões mostrados a **configuração básica de SAML** secção no portal do Azure.

6. Arco de publicação - aplicação SSO espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **atributos de utilizador** caixa de diálogo.

    ![image](common/edit-attribute.png)

7. No **afirmações de utilizador** secção sobre o **atributos de utilizador** caixa de diálogo, editar as afirmações utilizando **ícone de edição** ou adicionar as afirmações utilizando **Adicionar nova afirmação**para configurar o atributo de token SAML conforme mostrado na imagem acima e execute os seguintes passos: 

    | Name | Atributo de origem|
    | ---------------| --------------- |    
    | firstName | user.givenname |
    | lastName | user.surname |
    | e-mail | user.mail |
    | grupos | user.assignedroles |


    a. Clique em **Adicionar nova afirmação** para abrir o **afirmações de utilizador de gerir** caixa de diálogo.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Deixe o **espaço de nomes** em branco.

    d. Selecione a origem de dado **atributo**.

    e. Partir do **atributo de origem** lista, digite o valor de atributo apresentado para essa linha.

    f. Clique em **Ok**

    g. Clique em **Guardar**.

    > [!NOTE]
    > Aqui o **grupos** atributo está mapeado com **user.assignedroles**. Estes são criadas no Azure AD para mapear os nomes de grupo na aplicação de funções personalizadas. Pode obter mais orientações [aqui](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) sobre como criar funções personalizadas no Azure AD. 

8. No **definido no início de sessão único com o SAML** página, além do **certificado de assinatura SAML** secção, clique em **transferir** para transferir o **certificado (Base64)** entre as opções de determinado de acordo com seus requisitos e guarde-o no seu computador.

    ![O link de download de certificado](common/certificatebase64.png)

9. Sobre o **configurar a publicação de arco - SSO** secção, copie os URLs apropriados de acordo com seus requisitos.

    ![URLs de configuração de cópia](common/copy-configuration-urls.png)

    a. URL de início de sessão

    b. Azure Ad Identifier

    c. URL de fim de sessão

### <a name="configure-arc-publishing---sso-single-sign-on"></a>Configurar a publicação de arco - SSO início de sessão único

Para configurar o início de sessão único num **publicação arco - SSO** lado, terá de enviar o transferido **certificado (Base64)** e adequadas copiados URLs a partir do portal do Azure para [publicação arco - SSO equipa de suporte](mailto:inf@washpost.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](common/users.png)

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![Novo utilizador botão](common/new-user.png)

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![A caixa de diálogo de utilizador](common/user-properties.png)

    a. Na **Name** campo introduza **BrittaSimon**.
  
    b. Na **nome de utilizador** tipo de campo **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida a utilizar do Azure-início de sessão único, conceder acesso à publicação de arco - SSO.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**, em seguida, selecione **publicação de arco - SSO**.

    ![Painel de aplicações empresariais](common/enterprise-applications.png)

2. Na lista de aplicações, selecione **publicação arco - SSO**.

    ![A publicação de arco - ligação SSO na lista de aplicações](common/all-applications.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"](common/users-groups-blade.png)

4. Clique nas **adicionar utilizador** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição](common/add-assign-user.png)

5. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

6. Se está esperando a qualquer valor de função a asserção de SAML, em seguida, no **selecionar função** caixa de diálogo selecione a função adequada para o utilizador na lista, em seguida, clique o **selecione** na parte inferior do ecrã.

7. Na **adicionar atribuição** clique da caixa de diálogo a **atribuir** botão.

### <a name="create-arc-publishing---sso-test-user"></a>Criar a publicação de arco - utilizador de teste SSO

Nesta secção, um usuário chamado Eduarda Almeida é criado na publicação de arco - SSO. Publicação de arco - SSO suporta just-in-time de provisionamento de usuários, que está ativada por predefinição. Não existe nenhum item de ação para nesta secção. Se um utilizador já não existe na publicação de arco - SSO, é criado um novo após a autenticação.

> [!Note]
> Se precisar de criar manualmente um utilizador, contacte [publicação arco - equipa de suporte SSO](mailto:inf@washpost.com).

### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Ao clicar o arco publicação - mosaico SSO no painel de acesso, deve ser automaticamente sessão iniciada no arco publicação - SSO para o qual configura o SSO. Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos Adicionais

- [ Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
