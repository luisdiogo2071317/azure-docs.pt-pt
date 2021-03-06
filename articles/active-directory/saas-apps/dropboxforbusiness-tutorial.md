---
title: 'Tutorial: Integração do Active Directory do Azure com o Dropbox for Business | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Dropbox for Business.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/20/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 948121ad6b3a0f666fcc8fa64096bab91927b474
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56186153"
---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Tutorial: Integração do Active Directory do Azure com o Dropbox for Business

Neste tutorial, saiba como integrar o Dropbox for Business no Azure Active Directory (Azure AD).
Integrar o Dropbox for Business no Azure AD fornece as seguintes vantagens:

* Pode controlar no Azure AD que tenha acesso ao Dropbox para empresas.
* Pode permitir que os utilizadores ser automaticamente sessão iniciada para o Dropbox for Business (Single Sign-On) com as suas contas do Azure AD.
* Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Dropbox for Business, terá dos seguintes itens:

* Uma subscrição do Azure AD. Se não tiver um ambiente do Azure AD, pode obter a versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/)
* Subscrição de ativado o Dropbox for Business início de sessão único

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, configure e teste do Azure AD início de sessão único num ambiente de teste.

* O Dropbox for Business suporta **SP** iniciada SSO

* O Dropbox for Business suporta **Just In Time** aprovisionamento de utilizadores

## <a name="adding-dropbox-for-business-from-the-gallery"></a>Adicionando o Dropbox for Business da Galeria

Para configurar a integração do Dropbox para empresas com o Azure AD, terá de adicionar Dropbox para a empresa a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Dropbox para a empresa a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![O botão do Azure Active Directory](common/select-azuread.png)

2. Navegue para **aplicações empresariais** e, em seguida, selecione a **todos os aplicativos** opção.

    ![O painel de aplicações empresariais](common/enterprise-applications.png)

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, escreva **Dropbox para empresas**, selecione **Dropbox para empresas** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

     ![Dropbox para a empresa na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Dropbox para empresas com base num utilizador de teste **Eduarda Almeida**.
Para o início de sessão único funcionar, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na Dropbox para empresas tem de ser estabelecida.

Para configurar e testar o Azure AD início de sessão único com o Dropbox for Business, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Configurar o Dropbox para negócios início de sessão único](#configure-dropbox-for-business-single-sign-on)**  - para configurar as definições de início de sessão único no lado do aplicativo.
3. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Criar Dropbox para o utilizador de teste de negócios](#create-dropbox-for-business-test-user)**  - para ter um equivalente da Eduarda Almeida na Dropbox para a empresa que está ligado à representação de utilizador do Azure AD.
6. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, vai ativar o Azure AD início de sessão único no portal do Azure.

Para configurar o Azure AD início de sessão único com o Dropbox for Business, execute os seguintes passos:

1. Na [portal do Azure](https://portal.azure.com/), na **Dropbox para empresas** página de integração de aplicações, selecione **início de sessão único**.

    ![Configurar a ligação de início de sessão única](common/select-sso.png)

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, selecione **SAML/WS-Fed** modo para ativar o início de sessão único.

    ![Único início de sessão em modo de seleção](common/select-saml-option.png)

3. Sobre o **definir a segurança de início de sessão único com o SAML** página, clique em **editar** ícone para abrir **configuração básica de SAML** caixa de diálogo.

    ![Editar a configuração SAML do básico](common/edit-urls.png)

4. Sobre o **configuração básica de SAML** secção, execute os seguintes passos:

    ![Dropbox para domínio de negócios e URLs únicos início de sessão em informações](common/sp-identifier.png)

    a. Na **iniciar sessão no URL** caixa de texto, escreva um URL com o seguinte padrão: `https://www.dropbox.com/sso/<id>`

    b. Na **identificador (ID de entidade)** caixa de texto, escreva um valor: `Dropbox`

    > [!NOTE]
    > O valor de URL de início de sessão anterior não é o valor real. Atualizar o valor com o início de sessão no URL real, que é explicado mais tarde no tutorial.

4. No **definido no início de sessão único com o SAML** página, além do **certificado de assinatura SAML** secção, clique em **transferir** para transferir o **certificado (Base64)** entre as opções de determinado de acordo com seus requisitos e guarde-o no seu computador.

    ![O link de download de certificado](common/certificatebase64.png)

6. Sobre o **configurar o Dropbox for Business** secção, copie os URLs apropriados de acordo com seus requisitos.

    ![URLs de configuração de cópia](common/copy-configuration-urls.png)

    a. URL de início de sessão

    b. Azure Ad Identifier

    c. URL de fim de sessão

### <a name="configure-dropbox-for-business-single-sign-on"></a>Configurar o Dropbox para o negócio início de sessão único

1. Para configurar o início de sessão único num **Dropbox para empresas** lado, visite no seu Dropbox para o inquilino de empresas e inicie sessão seu Dropbox para o inquilino de negócios.

    ![Configurar o início de sessão único](./media/dropboxforbusiness-tutorial/ic769509.png "configurar início de sessão único")

2. Clique nas **ícone de utilizador** e selecione **definições** separador.

    ![Configurar o início de sessão único](./media/dropboxforbusiness-tutorial/configure1.png "configurar início de sessão único")

3. No painel de navegação no lado esquerdo, clique em **consola de administração**.

    ![Configurar o início de sessão único](./media/dropboxforbusiness-tutorial/configure2.png "configurar início de sessão único")

4. Sobre o **consola de administração**, clique em **definições** no painel de navegação esquerdo.

    ![Configurar o início de sessão único](./media/dropboxforbusiness-tutorial/configure3.png "configurar início de sessão único")

5. Selecione **início de sessão único** opção sob a **autenticação** secção.

    ![Configurar o início de sessão único](./media/dropboxforbusiness-tutorial/configure4.png "configurar início de sessão único")

6. Na **início de sessão único** secção, execute os seguintes passos:  

    ![Configurar o início de sessão único](./media/dropboxforbusiness-tutorial/configure5.png "configurar início de sessão único")

    a. Selecione **necessários** como uma opção na lista pendente para o **início de sessão único**.

    b. Clique em **adicione o URL de início de sessão** e, no **URL de início de sessão da fornecedor de identidade** caixa de texto, colar a **URL de início de sessão** valor que copiou do portal do Azure e, em seguida, selecione  **Feito**.

    ![Configurar o início de sessão único](./media/dropboxforbusiness-tutorial/configure6.png "configurar início de sessão único")

    c. Clique em **carregar certificado**e, em seguida, navegue até sua **ficheiro de certificado codificado Base64** que transferiu do portal do Azure.

    d. Clique em **copiar ligação** e cole o valor copiado para o **URL de início de sessão** caixa de texto de **Dropbox para o domínio de negócios e URLs** secção no portal do Azure.

    e. Clique em **Guardar**.

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

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para o Dropbox for Business.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**, em seguida, selecione **o Dropbox for Business**.

    ![Painel de aplicações empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, escreva e selecione **Dropbox para empresas**.

    ![O Dropbox para ligação de negócios na lista de aplicações](common/all-applications.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"](common/users-groups-blade.png)

4. Clique nas **adicionar utilizador** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição](common/add-assign-user.png)

5. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

6. Se está esperando a qualquer valor de função a asserção de SAML, em seguida, no **selecionar função** caixa de diálogo selecione a função adequada para o utilizador na lista, em seguida, clique o **selecione** na parte inferior do ecrã.

7. Na **adicionar atribuição** clique da caixa de diálogo a **atribuir** botão.

### <a name="create-dropbox-for-business-test-user"></a>Criar Dropbox para o utilizador de teste de negócios

Nesta secção, um usuário chamado Eduarda Almeida é criado na Dropbox para empresas. O Dropbox for Business suporta o aprovisionamento de utilizadores de just-in-time, que está ativado por predefinição. Não existe nenhum item de ação para nesta secção. Se um utilizador já não existir no Dropbox para empresas, é criado um novo após a autenticação.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte o [Dropbox para a equipa de suporte de empresas do cliente](https://www.dropbox.com/business/contact)

### <a name="test-single-sign-on"></a>Testar o início de sessão único 

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica Dropbox para o mosaico de negócios no painel de acesso, deve ser automaticamente sessão iniciada no Dropbox para empresas para o qual configura o SSO. Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos Adicionais

- [ Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

