---
title: 'Tutorial: Integração do Azure Active Directory com o Office de Virtual de 8 x 8 | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory diretório e 8 x 8 Virtual Office.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: jeedes
ms.openlocfilehash: 53db637bf7ad47896747b491fcbe31123fdb104e
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741815"
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Tutorial: Integração do Azure Active Directory com o Office de Virtual de 8 x 8

Neste tutorial, saiba como integrar o Office de Virtual de 8 x 8 com o Azure Active Directory (Azure AD).

Integração de 8 x 8 Office Virtual com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Office de Virtual de 8 x 8.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Office de Virtual de 8 x 8 (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Office de Virtual de 8 x 8, precisa do seguinte:

- Uma subscrição do Azure
- Um 8 x 8 Virtual Office-início de sessão único de subscrição ativada

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar 8 x 8 Virtual Office a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-8x8-virtual-office-from-the-gallery"></a>Adicionar 8 x 8 Virtual Office a partir da Galeria

Para configurar a integração do Office de Virtual de 8 x 8 com o Azure AD, terá de adicionar 8 x 8 Virtual Office a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar 8 x 8 Virtual Office a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **8 x 8 Virtual Office**, selecione **8 x 8 Virtual Office** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Office de Virtual de 8 x 8 na lista de resultados](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com 8 x 8 que virtual Office com base num utilizador de teste chamado "Eduarda Almeida".

Para início de sessão único funcione, o Azure AD precisa de saber o que o utilizador de contraparte 8 x 8 Virtual Office for para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado 8 x 8 Virtual Office deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com o Office de Virtual de 8 x 8, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de escritório Virtual 8 x 8](#creating-a-8x8-virtual-office-test-user)**  - para ter um equivalente da Eduarda Almeida no escritório de Virtual de 8 x 8 que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na aplicação do Virtual Office de 8 x 8.

**Para configurar o Azure AD início de sessão único com o Office de Virtual de 8 x 8, execute os seguintes passos:**

1. No portal do Azure, sobre o **8 x 8 Virtual Office** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, clique em **selecione** para **SAML** modo para ativar o início de sessão único.

    ![Configurar o início de sessão único](common/tutorial_general_301.png)

3. Sobre o **definir a segurança de início de sessão único com o SAML** página, clique em **editar** ícone para abrir **configuração básica de SAML** caixa de diálogo.

    ![Configurar o início de sessão único](common/editconfigure.png)

4. Sobre o **configuração básica de SAML** secção, execute os seguintes passos:

    ![8 x 8 URLs de domínio Virtual do Office e únicas início de sessão em informações](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_url.png)

    a. Na **identificador** caixa de texto, escreva um URL: `https://sso.8x8.com/saml2`

    b. Na **URL de resposta** caixa de texto, escreva um URL: `https://sso.8x8.com/saml2`

5. No **certificado de assinatura SAML** página, além da **certificado de assinatura SAML** secção, clique em **transferir** para transferir **certificado (bruto)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_certificate.png) 

6. Sobre o **configurar o Office de Virtual de 8 x 8** secção, copie o URL adequado, de acordo com seus requisitos.

    a. URL de início de sessão

    b. Identificador do Azure AD

    c. URL de fim de sessão

    ![Configuração do Virtual Office de 8 x 8](common/configuresection.png)

7. Início de sessão no seu inquilino do Virtual Office 8 x 8 como administrador.

8. Selecione **Gestor de conta do Virtual Office** no painel de aplicação.

    ![Configurar no lado de aplicação](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

9. Selecione **Business** conta para gerir e clique em **sessão** botão.

    ![Configurar no lado de aplicação](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

10. Clique em **contas** guia na lista de menu.

    ![Configurar no lado de aplicação](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

11. Clique em **início de sessão único** na lista de contas.
  
    ![Configurar no lado de aplicação](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

12. Selecione **início de sessão único** sob métodos de autenticação e clique em **SAML**.

    ![Configurar no lado de aplicação](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

13. Na **SAML início de sessão único** secção, execute os seguintes passos:

    ![Configurar no lado de aplicação](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)

    a. Na **URL de início de sessão** caixa de texto, colar **URL de início de sessão** valor que copiou do portal do Azure.

    b. Na **URL de fim de sessão** caixa de texto, colar **URL de fim de sessão** valor que copiou do portal do Azure.

    c. Na **URL de emissor** caixa de texto, colar **do Azure AD identificador** valor que copiou do portal do Azure.

    d. Clique em **procurar** botão para carregar o certificado que transferiu a partir do portal do Azure.

    e. Clique no botão **Guardar**.

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![Criar utilizador do Azure AD][100]

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![Criar um utilizador de teste do Azure AD](common/create_aaduser_01.png) 

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![Criar um utilizador de teste do Azure AD](common/create_aaduser_02.png)

    a. Na **Name** , insira **BrittaSimon**.
  
    b. Na **nome de utilizador** , digite **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.
  
### <a name="creating-a-8x8-virtual-office-test-user"></a>Criar um utilizador de teste de escritório Virtual 8 x 8

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida no escritório de Virtual de 8 x 8. 8 x 8 Virtual Office suporta o aprovisionamento just-in-time, que está por predefinição, ativada.

Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder a 8 x 8 Virtual Office se não existir ainda.

> [!NOTE]
> Se precisar de criar manualmente um utilizador, terá de contactar o [equipa de suporte do Virtual Office 8 x 8](https://www.8x8.com/about-us/contact-us).

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao Office de Virtual de 8 x 8.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **8 x 8 Virtual Office**.

    ![Configurar o início de sessão único](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

6. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.

### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de escritório Virtual 8 x 8 no painel de acesso, deve obter automaticamente com sessão iniciada a aplicação do Virtual Office de 8 x 8.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
