---
title: 'Tutorial: Integração do Active Directory do Azure com AnswerHub | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e AnswerHub.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: df88eee14ea8f3d7d7f8365a77750da67904793d
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56188057"
---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a>Tutorial: Integração do Active Directory do Azure com AnswerHub

Neste tutorial, saiba como integrar AnswerHub com o Azure Active Directory (Azure AD).
Integrar AnswerHub no Azure AD fornece as seguintes vantagens:

* Pode controlar no Azure AD que tenha acesso ao AnswerHub.
* Pode permitir que os utilizadores ser automaticamente sessão iniciada para AnswerHub (Single Sign-On) com as suas contas do Azure AD.
* Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com AnswerHub, terá dos seguintes itens:

* Uma subscrição do Azure AD. Se não tiver um ambiente do Azure AD, pode obter a versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/)
* AnswerHub logon único habilitado subscrição

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, configure e teste do Azure AD início de sessão único num ambiente de teste.

* Suporta AnswerHub **SP** iniciada SSO

## <a name="adding-answerhub-from-the-gallery"></a>Adicionando AnswerHub da Galeria

Para configurar a integração do AnswerHub com o Azure AD, terá de adicionar AnswerHub a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar AnswerHub a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![O botão do Azure Active Directory](common/select-azuread.png)

2. Navegue para **aplicações empresariais** e, em seguida, selecione a **todos os aplicativos** opção.

    ![O painel de aplicações empresariais](common/enterprise-applications.png)

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, escreva **AnswerHub**, selecione **AnswerHub** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

     ![AnswerHub na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com AnswerHub com base num utilizador de teste **Eduarda Almeida**.
Para o início de sessão único funcionar, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no AnswerHub deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com AnswerHub, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Configurar AnswerHub Single Sign-On](#configure-answerhub-single-sign-on)**  - para configurar as definições de início de sessão único no lado do aplicativo.
3. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Criar utilizador de teste AnswerHub](#create-answerhub-test-user)**  - para ter um equivalente da Eduarda Almeida na AnswerHub que está ligado à representação de utilizador do Azure AD.
6. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, vai ativar o Azure AD início de sessão único no portal do Azure.

Para configurar o Azure AD início de sessão único com AnswerHub, execute os seguintes passos:

1. Na [portal do Azure](https://portal.azure.com/), na **AnswerHub** página de integração de aplicações, selecione **início de sessão único**.

    ![Configurar a ligação de início de sessão única](common/select-sso.png)

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, selecione **SAML/WS-Fed** modo para ativar o início de sessão único.

    ![Único início de sessão em modo de seleção](common/select-saml-option.png)

3. Sobre o **definir a segurança de início de sessão único com o SAML** página, clique em **editar** ícone para abrir **configuração básica de SAML** caixa de diálogo.

    ![Editar a configuração SAML do básico](common/edit-urls.png)

4. Sobre o **configuração básica de SAML** secção, execute os seguintes passos:

    ![AnswerHub domínio e URLs únicas início de sessão em informações](common/sp-identifier.png)

    a. Na **iniciar sessão no URL** caixa de texto, escreva um URL com o seguinte padrão: `https://<company>.answerhub.com`

    b. Na **identificador (ID de entidade)** caixa de texto, escreva um URL com o seguinte padrão: `https://<company>.answerhub.com`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o início de sessão real URL e o identificador. Contacte [equipa de suporte de cliente AnswerHub](mailto:success@answerhub.com) obter esses valores. Também pode consultar os padrões mostrados a **configuração básica de SAML** secção no portal do Azure.

5. No **definido no início de sessão único com o SAML** página, além do **certificado de assinatura SAML** secção, clique em **transferir** para transferir o **certificado (Base64)** entre as opções de determinado de acordo com seus requisitos e guarde-o no seu computador.

    ![O link de download de certificado](common/certificatebase64.png)

6. Sobre o **configurar AnswerHub** secção, copie os URLs apropriados de acordo com seus requisitos.

    ![URLs de configuração de cópia](common/copy-configuration-urls.png)

    a. URL de início de sessão

    b. Azure Ad Identifier

    c. URL de fim de sessão

### <a name="configure-answerhub-single-sign-on"></a>Configurar AnswerHub Single Sign-On

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa AnswerHub como um administrador.

    > [!NOTE]
    > Se precisar de ajuda para configurar AnswerHub, contacte [equipa de suporte do AnswerHub](mailto:success@answerhub.com.).

2. Aceda a **administração**.

3. Clique nas **utilizadores e grupos** separador.

4. No painel de navegação no lado esquerdo, no **definições de redes sociais** secção, clique em **configuração de SAML**.

5. Clique em **configuração do IDP** separador.

6. Sobre o **configuração do IDP** separador, execute os seguintes passos:

    ![Configuração SAML](./media/answerhub-tutorial/ic785172.png "configuração SAML")  
  
    a. Na **URL de início de sessão do IDP** caixa de texto, colar **URL de início de sessão** que copiou do portal do Azure.
  
    b. Na **URL de fim de sessão do IDP** caixa de texto, colar **URL de fim de sessão** valor que copiou do portal do Azure.

    c. Na **formato de identificador de nome de IDP** caixa de texto, introduza o utilizador identificador valor mesmo como selecionado no portal do Azure na **atributos de utilizador** secção.
  
    d. Clique em **chaves e certificados**.

7. Sobre o **chaves e certificados** separador, execute os seguintes passos:

    ![Chaves e certificados](./media/answerhub-tutorial/ic785173.png "chaves e certificados")  

    a. Abra o seu certificado codificado de base 64 que transferiu a partir do portal do Azure no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **IDP chave pública (x 509 formato)** caixa de texto.
  
    b. Clique em **Guardar**.

8. Sobre o **configuração do IDP** separador, clique em **guardar**.

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

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para AnswerHub.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**, em seguida, selecione **AnswerHub**.

    ![Painel de aplicações empresariais](common/enterprise-applications.png)

2. Na lista de aplicações, selecione **AnswerHub**.

    ![A ligação de AnswerHub na lista de aplicações](common/all-applications.png)

3. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"](common/users-groups-blade.png)

4. Clique nas **adicionar utilizador** botão, em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição](common/add-assign-user.png)

5. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

6. Se está esperando a qualquer valor de função a asserção de SAML, em seguida, no **selecionar função** caixa de diálogo selecione a função adequada para o utilizador na lista, em seguida, clique o **selecione** na parte inferior do ecrã.

7. Na **adicionar atribuição** clique da caixa de diálogo a **atribuir** botão.

### <a name="create-answerhub-test-user"></a>Criar utilizador de teste AnswerHub

Para ativar a utilizadores do Azure AD iniciar sessão no AnswerHub, tem de ser aprovisionados em AnswerHub. No caso de AnswerHub, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **AnswerHub** site da empresa como administrador.

2. Aceda a **administração**.

3. Clique nas **utilizadores e grupos** separador.

4. No painel de navegação no lado esquerdo, no **gerir utilizadores** secção, clique em **aos utilizadores criar ou importar** e, em seguida, clique em **utilizadores e grupos**.

   ![Os utilizadores e grupos](./media/answerhub-tutorial/ic785175.png "utilizadores e grupos")

5. Tipo de **endereço de E-Mail**, **nome de utilizador** e **palavra-passe** de uma conta válida do Azure Active Directory que pretende aprovisionar em caixas de texto relacionadas e, em seguida, clique em  **Guardar**.

> [!NOTE]
> Pode utilizar quaisquer outras AnswerHub utilizador conta criação ferramentas ou APIs fornecidas pelo AnswerHub para aprovisionar contas de utilizador do AAD.

### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico AnswerHub no painel de acesso, deve ser automaticamente sessão iniciada no AnswerHub para o qual configura o SSO. Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos Adicionais

- [ Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

