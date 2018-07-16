---
title: 'Tutorial: Integração do Azure Active Directory com Teamphoria | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Teamphoria.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2018
ms.author: jeedes
ms.openlocfilehash: 794945caeea113dc6f1cc2ab5e11a76c3e88c83e
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041913"
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a>Tutorial: Integração do Azure Active Directory com Teamphoria

Neste tutorial, saiba como integrar Teamphoria com o Azure Active Directory (Azure AD).

Integrar Teamphoria no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Teamphoria
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Teamphoria (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Teamphoria, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Teamphoria início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Teamphoria da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-teamphoria-from-the-gallery"></a>Adicionando Teamphoria da Galeria
Para configurar a integração do Teamphoria com o Azure AD, terá de adicionar Teamphoria a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Teamphoria a partir da galeria, execute os seguintes passos:**

1. Na  **[Portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Clique em **adicionar** botão na parte superior da caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Teamphoria**.

    ![Criar um utilizador de teste do Azure AD](./media/teamphoria-tutorial/tutorial_teamphoria_search.png)

5. No painel de resultados, selecione **Teamphoria**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/teamphoria-tutorial/tutorial_teamphoria_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Teamphoria com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Teamphoria a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Teamphoria deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Teamphoria, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Teamphoria](#creating-a-teamphoria-test-user)**  - para ter um equivalente da Eduarda Almeida na Teamphoria que está ligado à representação de ela do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Teamphoria.

**Para configurar o Azure AD início de sessão único com Teamphoria, execute os seguintes passos:**

1. No portal do Azure, sobre o **Teamphoria** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

3. Sobre o **Teamphoria domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/tutorial_teamphoria_url.png)

    Na **URL de início de sessão** caixa de texto, digite a URL usando o seguinte padrão: `https://<sub-domain>.teamphoria.com/login`   

    > [!NOTE] 
    > O valor de URL de início de sessão não é real. Terá de atualizar este valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente Teamphoria](https://www.teamphoria.com/) para obter o URL de início de sessão.

4. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o certificado no seu computador.

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/tutorial_teamphoria_certificate.png)

5. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/tutorial_general_400.png)

6. Sobre o **Teamphoria configuração** secção, clique em **configurar Teamphoria** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/tutorial_teamphoria_configure.png)

7. Para configurar o início de sessão único num **Teamphoria** lado, início de sessão para a sua aplicação Teamphoria como administrador.

8. Aceda a **definições de administrador** opção na barra de ferramentas da lado esquerda e, no separador configurar, clique em **início de sessão único** para abrir a janela de configuração de SSO.

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/admin_sso_configure.png)

9. Clique em **adicionar novo fornecedor de identidade** opção no canto superior direito para abrir o formulário para adicionar as definições de SSO.

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/add_new_identity_provider.png)

10. Introduza os detalhes nos campos, conforme descrito abaixo-

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/Teamphoria_sso_save.png)

    a. **NOME a apresentar**: introduza o nome a apresentar do plug-in na página de administração.

    b. **NOME do botão**: O nome do separador que será apresentado na página de início de sessão para iniciar sessão através do SSO.

    c. **CERTIFICADO**: abrir o certificado anteriormente transferido a partir do portal do Azure no bloco de notas, copie o conteúdo do mesmo e cole-o aqui na caixa.

    d. **PONTO de entrada**: colar a **SAML único início de sessão no URL do serviço** copiou anteriormente a partir do portal do Azure.

    e. Mude a opção para **ON** e clique em **guardar**.

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/teamphoria-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    ![Criar um utilizador de teste do Azure AD](./media/teamphoria-tutorial/create_aaduser_02.png) 

3. Na parte superior da caixa de diálogo, clique em **Add** para abrir o **utilizador** caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/teamphoria-tutorial/create_aaduser_03.png)

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/teamphoria-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.

### <a name="creating-a-teamphoria-test-user"></a>Criar um utilizador de teste Teamphoria

Para habilitar logon Teamphoria de utilizadores do Azure AD, tem de ser aprovisionados em Teamphoria. No caso de Teamphoria, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa Teamphoria como administrador.

2. Clique em **administrador** definições na barra de ferramentas à esquerda e, no **GERIR** separador clique em **utilizadores** para abrir a página de administração para os utilizadores.

    ![Adicionar o funcionário](./media/teamphoria-tutorial/admin_manage_users.png)

3. Clique nas **MANUAL CONVIDAR** opção.

    ![Convidar pessoas](./media/teamphoria-tutorial/admin_manage_add_users.png)

4. Nesta página, realize após a ação.
    
    ![Convidar pessoas](./media/teamphoria-tutorial/manual_user_invite.png)

    a. Na **endereço de E-Mail** caixa de texto, o **endereço de e-mail** de BrittaSimon.

    b. Na **nome próprio** caixa de texto, tipo **Eduarda**.

    c. Na **SOBRENOME** caixa de texto, tipo **Simon**.

    d. Clique em **convite 1 utilizador**. Utilizador tem de aceitar o convite a ser criados no sistema.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Teamphoria.

![Atribuir utilizador][200]

**Para atribuir a Eduarda Almeida a Teamphoria, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **Teamphoria**.

    ![Configurar o início de sessão único](./media/teamphoria-tutorial/tutorial_teamphoria_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Se pretender testar as definições de início de sessão únicas, abra o painel de acesso. Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/teamphoria-tutorial/tutorial_general_01.png
[2]: ./media/teamphoria-tutorial/tutorial_general_02.png
[3]: ./media/teamphoria-tutorial/tutorial_general_03.png
[4]: ./media/teamphoria-tutorial/tutorial_general_04.png

[100]: ./media/teamphoria-tutorial/tutorial_general_100.png

[200]: ./media/teamphoria-tutorial/tutorial_general_200.png
[201]: ./media/teamphoria-tutorial/tutorial_general_201.png
[202]: ./media/teamphoria-tutorial/tutorial_general_202.png
[203]: ./media/teamphoria-tutorial/tutorial_general_203.png
