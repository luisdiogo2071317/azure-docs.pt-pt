---
title: 'Tutorial: Integração do Azure Active Directory com floco de neve | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e floco de neve.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3488ac27-0417-4ad9-b9a3-08325fe8ea0d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2018
ms.author: jeedes
ms.openlocfilehash: 6e1f76548e2433f9bc4b0b26b0894a92b7ec0aa0
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50092564"
---
# <a name="tutorial-azure-active-directory-integration-with-snowflake"></a>Tutorial: Integração do Azure Active Directory com floco de neve

Neste tutorial, saiba como integrar floco de neve com o Azure Active Directory (Azure AD).

Integrar floco de neve no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Snowflake.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada ao Snowflake (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com floco de neve, terá dos seguintes itens:

- Uma subscrição do Azure
- Um floco de neve logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando floco de neve da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-snowflake-from-the-gallery"></a>Adicionando floco de neve da Galeria

Para configurar a integração de Snowflake no Azure AD, terá de adicionar floco de neve a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar floco de neve a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **floco de neve**, selecione **floco de neve** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Snowflake na lista de resultados](./media/snowflake-tutorial/tutorial_snowflake_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com floco de neve com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no floco de neve a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no floco de neve deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com floco de neve, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar utilizador de teste de Snowflake](#creating-snowflake-test-user)**  - para ter um equivalente da Eduarda Almeida na floco de neve que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo floco de neve.

**Para configurar o Azure AD início de sessão único com floco de neve, execute os seguintes passos:**

1. No portal do Azure, sobre o **floco de neve** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, clique em **selecione** para **SAML** modo para ativar o início de sessão único.

    ![Configurar o início de sessão único](common/tutorial_general_301.png)

3. Sobre o **definir a segurança de início de sessão único com o SAML** página, clique em **editar** ícone para abrir **configuração básica de SAML** caixa de diálogo.

    ![Configurar o início de sessão único](common/editconfigure.png)

4. Sobre o **configuração básica de SAML** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Floco de neve domínio e URLs únicas início de sessão em informações](./media/snowflake-tutorial/tutorial_snowflake_url1.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<SNOWFLAKE-URL>.snowflakecomputing.com`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<SNOWFLAKE-URL>.snowflakecomputing.com/fed/login`

5. Clique em **definir URLs adicionais** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Floco de neve domínio e URLs únicas início de sessão em informações](./media/snowflake-tutorial/tutorial_snowflake_url2.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<SNOWFLAKE-URL>.snowflakecomputing.com`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão.

6. No **certificado de assinatura SAML** página, além da **certificado de assinatura SAML** secção, clique em **transferir** para transferir **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/snowflake-tutorial/tutorial_snowflake_certificate.png) 

7. Sobre o **configurar floco de neve** secção, copie o URL adequado, de acordo com seus requisitos.

    a. URL de início de sessão

    b. Identificador do Azure AD

    c. URL de fim de sessão

    ![Configuração de snowflake](common/configuresection.png)

8. Numa janela do browser web diferente, início de sessão para floco de neve como um administrador de segurança.

9. **Mudar de função** para **ACCOUNTADMIN**, ao clicar no **perfil** no canto superior direito da página.

    > [!NOTE]
    > Isso é separado do contexto que selecionou no canto superior direito em seu nome de utilizador
    
    ![O administrador de Snowflake ](./media/snowflake-tutorial/tutorial_snowflake_accountadmin.png)

10. Abra o **transferido o certificado de Base 64** no bloco de notas. Copie o valor entre "---BEGIN CERTIFICATE---" e "---END CERTIFICATE---" e colá-lo para os pontos de exclamação junto a **certificado** abaixo. Na **ssoUrl**, cole **URL de início de sessão** valor que copiou do portal do Azure. Selecione o **todas as consultas** e clique em **executar**.

    ![Sql floco de neve](./media/snowflake-tutorial/tutorial_snowflake_sql.png)

    ```
    use role accountadmin;
    alter account set saml_identity_provider = '{
    "certificate": "<Paste the content of downloaded certificate from Azure portal>",
    "ssoUrl":"<Login URL value which you have copied from the Azure portal>",
    "type":"custom",
    "label":"AzureAD"
    }';
    alter account set sso_login_page = TRUE;
    ```

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

### <a name="creating-snowflake-test-user"></a>Criar utilizador de teste de Snowflake

Para ativar a utilizadores do Azure AD iniciar sessão no floco de neve, tem de ser aprovisionados em floco de neve. No floco de neve, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no floco de neve como um administrador de segurança.

2. **Mudar de função** para **ACCOUNTADMIN**, ao clicar no **perfil** no canto superior direito da página.  

    ![O administrador de Snowflake ](./media/snowflake-tutorial/tutorial_snowflake_accountadmin.png)

3. Criar o utilizador ao executar o abaixo a consulta SQL, garantir o "Nome de início de sessão" está definido como o nome de utilizador do Azure AD na planilha conforme mostrado abaixo.

    ![O adminsql floco de neve ](./media/snowflake-tutorial/tutorial_snowflake_usersql.png)

    ```
    use role accountadmin;
    CREATE USER britta_simon PASSWORD = '' LOGIN_NAME = 'BrittaSimon@contoso.com' DISPLAY_NAME = 'Britta Simon';
    ```

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao Snowflake.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **floco de neve**.

    ![Configurar o início de sessão único](./media/snowflake-tutorial/tutorial_snowflake_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

6. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.

### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de Snowflake no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de Snowflake.
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
