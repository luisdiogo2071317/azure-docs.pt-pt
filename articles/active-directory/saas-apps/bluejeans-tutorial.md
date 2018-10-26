---
title: 'Tutorial: Integração do Azure Active Directory com BlueJeans | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e BlueJeans.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2018
ms.author: jeedes
ms.openlocfilehash: cff1512c56dba9907adbf1bb4452f11d47d0787d
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50095233"
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Tutorial: Integração do Azure Active Directory com BlueJeans

Neste tutorial, saiba como integrar BlueJeans com o Azure Active Directory (Azure AD).

Integrar BlueJeans no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao BlueJeans.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para BlueJeans (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com BlueJeans, terá dos seguintes itens:

- Uma subscrição do Azure
- Um BlueJeans logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando BlueJeans da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-bluejeans-from-the-gallery"></a>Adicionando BlueJeans da Galeria

Para configurar a integração do BlueJeans com o Azure AD, terá de adicionar BlueJeans a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar BlueJeans a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **BlueJeans**, selecione **BlueJeans** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![BlueJeans na lista de resultados](./media/bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com BlueJeans com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no BlueJeans a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no BlueJeans deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com BlueJeans, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste BlueJeans](#creating-a-bluejeans-test-user)**  - para ter um equivalente da Eduarda Almeida na BlueJeans que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo BlueJeans.

**Para configurar o Azure AD início de sessão único com BlueJeans, execute os seguintes passos:**

1. No portal do Azure, sobre o **BlueJeans** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, clique em **selecione** para **SAML** modo para ativar o início de sessão único.

    ![Configurar o início de sessão único](./media/bluejeans-tutorial/tutorial_general_301.png)

4. Sobre o **definir a segurança de início de sessão único com o SAML** página, clique em **editar** ícone para abrir **configuração básica de SAML** caixa de diálogo.

    ![Configurar o início de sessão único](./media/bluejeans-tutorial/tutorial_bluejeans_editurl.png)

5. Sobre o **configuração básica de SAML** secção, execute os seguintes passos:

    ![BlueJeans domínio e URLs únicas início de sessão em informações](./media/bluejeans-tutorial/tutorial_bluejeans_url.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.BlueJeans.com`

    > [!NOTE]
    > O valor de início de sessão não é real. Atualize o valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente BlueJeans](https://support.bluejeans.com/contact) para obter o valor.

6. No **certificado de assinatura SAML** página, além da **certificado de assinatura SAML** secção, clique em **transferir** para transferir **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/bluejeans-tutorial/tutorial_bluejeans_certficate.png) 

7. Sobre o **configurar BlueJeans** secção, copie o URL adequado, de acordo com seus requisitos.

    a. URL de início de sessão

    b. Identificador do Azure AD

    c. URL de fim de sessão

    ![Configuração de blueJeans](./media/bluejeans-tutorial/tutorial_bluejeans_configure.png) 

8. Numa janela do browser web diferente, inicie sessão no seu **BlueJeans** site da empresa como administrador.

9. Aceda a **administrador \> definições de grupo \> segurança**.

    ![Admin](./media/bluejeans-tutorial/IC785868.png "Admin")

10. Na **segurança** secção, execute os seguintes passos:

    ![SAML início de sessão único](./media/bluejeans-tutorial/IC785869.png "SAML início de sessão único")

    a. Selecione **SAML início de sessão único**.

    b. Selecione **ativar o aprovisionamento automático**.

11. Avançar com os seguintes passos:

    ![Caminho de certificado](./media/bluejeans-tutorial/IC785870.png "caminho de certificado")

    a. Clique em **Escolher ficheiro**, para carregar o certificado com codificação base 64 que transferiu do portal do Azure.

    b. Na **URL de início de sessão** caixa de texto, cole o valor de **URL de início de sessão** que copiou do portal do Azure.

    c. Na **URL de alteração de palavra-passe** caixa de texto, cole o valor de **alterar o URL da palavra-passe** que copiou do portal do Azure.

    d. Na **URL de fim de sessão** caixa de texto, cole o valor de **URL de fim de sessão** que copiou do portal do Azure.

12. Avançar com os seguintes passos:

    ![Guardar alterações](./media/bluejeans-tutorial/IC785874.png "guardar alterações")

    a. Na **id de utilizador** caixa de texto, tipo `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    b. Na **E-Mail** caixa de texto, tipo `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    c. Clique em **guardar alterações**.

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![Criar utilizador do Azure AD][100]

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![Criar um utilizador de teste do Azure AD](./media/bluejeans-tutorial/create_aaduser_01.png) 

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![Criar um utilizador de teste do Azure AD](./media/bluejeans-tutorial/create_aaduser_02.png)

    a. Na **Name** , insira **BrittaSimon**.
  
    b. Na **nome de utilizador** , digite **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.

### <a name="creating-a-bluejeans-test-user"></a>Criar um utilizador de teste BlueJeans

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no BlueJeans. BlueJeans suporta o aprovisionamento automático de utilizadores, que está por predefinição, ativada. Pode encontrar mais detalhes [aqui](bluejeans-provisioning-tutorial.md) sobre como configurar o aprovisionamento automático de utilizadores.

**Se precisar de criar manualmente o utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **BlueJeans** site da empresa como administrador.

2. Aceda a **administrador \> GERIR utilizadores \> adicionar utilizador**.

    ![Admin](./media/bluejeans-tutorial/IC785877.png "Admin")

    >[!IMPORTANT]
    >O **adicionar utilizador** separador apenas está disponível se, no **guia de segurança**, **ativar o aprovisionamento automático** está desmarcada. 

3. Na **adicionar utilizador** secção, execute os seguintes passos:

    ![Adicionar utilizador](./media/bluejeans-tutorial/IC785886.png "adicionar utilizador")

    a. Na **nome próprio** texto, introduza o nome de utilizador, como **Eduarda**.

    b. Na **sobrenome** texto, digite o apelido do utilizador, como **simon**.

    c. Na **escolher um nome de utilizador BlueJeans** texto, introduza o nome de utilizador do utilizador, como **Brittasimon**

    d. Na **criar uma palavra-passe** texto, introduza a palavra-passe.

    e. Na **empresa** texto, introduza a sua empresa.

    f. Na **endereço de E-Mail** texto, introduza o e-mail do utilizador, como **brittasimon@contoso.com**.

    g. Na **criar um I.D de reunião BlueJeans** texto, introduza o ID de reunião.

    h. Na **escolher um código de acesso do moderador** texto, introduza o código de acesso.

    i. Clique em **continuar**.

    ![Utilizador Addd](./media/bluejeans-tutorial/IC785887.png "Addd utilizador")

    J. Clique em **adicionar utilizador**.

>[!NOTE]
>Pode utilizar quaisquer outras BlueJeans utilizador conta criação ferramentas ou APIs fornecidas pelo BlueJeans para aprovisionar contas de utilizador do Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para BlueJeans.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **BlueJeans**.

    ![Configurar o início de sessão único](./media/bluejeans-tutorial/tutorial_bluejeans_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

6. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.

### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico BlueJeans no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo BlueJeans.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/bluejeans-tutorial/tutorial_general_203.png
