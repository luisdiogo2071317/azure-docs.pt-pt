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
ms.date: 09/07/2018
ms.author: jeedes
ms.openlocfilehash: 247d18eb13f7bad10cbfd89891a80d2d1c6135c3
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44160552"
---
# <a name="tutorial-azure-active-directory-integration-with-snowflake"></a>Tutorial: Integração do Azure Active Directory com floco de neve

Neste tutorial, saiba como integrar floco de neve com o Azure Active Directory (Azure AD).

Integrar floco de neve no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Snowflake.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada ao Snowflake (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com floco de neve, terá dos seguintes itens:

- Uma subscrição do Azure
- Um floco de neve logon único habilitado subscrição
- Os clientes que não tem uma conta de floco de neve e gostariam de experimentá-lo através da Galeria de aplicações do Azure AD, consulte [isso](https://trial.snowflake.net/?cloud=azure&utm_source=azure-marketplace&utm_medium=referral&utm_campaign=self-service-azure-mp) ligação.

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

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste floco de neve](#create-a-snowflake-test-user)**  - para ter um equivalente da Eduarda Almeida na floco de neve que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo floco de neve.

**Para configurar o Azure AD início de sessão único com floco de neve, execute os seguintes passos:**

1. No portal do Azure, sobre o **floco de neve** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/snowflake-tutorial/tutorial_snowflake_samlbase.png)

3. Sobre o **floco de neve de domínio e URLs** secção, execute os seguintes passos:

    ![Floco de neve domínio e URLs únicas início de sessão em informações](./media/snowflake-tutorial/tutorial_snowflake_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<SNOWFLAKE-URL>.snowflakecomputing.com`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<SNOWFLAKE-URL>.snowflakecomputing.com/fed/login`

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Floco de neve domínio e URLs únicas início de sessão em informações](./media/snowflake-tutorial/tutorial_snowflake_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<SNOWFLAKE-URL>.snowflakecomputing.com`
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão.

5. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/snowflake-tutorial/tutorial_snowflake_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/snowflake-tutorial/tutorial_general_400.png)
    
7. Na **floco de neve Configuration** secção, clique em **configurar floco de neve** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de snowflake](./media/snowflake-tutorial/tutorial_snowflake_configure.png) 

8. Numa janela do browser web diferente, início de sessão para floco de neve como um administrador de segurança.

9. Executar o abaixo a consulta SQL na folha de cálculo, definindo a **certificado** valor para o **dowloaded certificado** e **ssoUrl** para o copiado **SAML Single Sign-On URL do serviço** do Azure AD para o valor, conforme mostrado abaixo.

    ![Sql floco de neve](./media/snowflake-tutorial/tutorial_snowflake_sql.png) 

    ```
    use role accountadmin;
    alter account set saml_identity_provider = '{
    "certificate": "<Paste the content of downloaded certificate from Azure portal>",
    "ssoUrl":"<SAML single sign-on service URL value which you have copied from the Azure portal>",
    "type":"custom",
    "label":"AzureAD"
    }';
    alter account set sso_login_page = TRUE;
    ```

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/snowflake-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/snowflake-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/snowflake-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/snowflake-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-snowflake-test-user"></a>Criar um utilizador de teste de Snowflake

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

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao Snowflake.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a floco de neve, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **floco de neve**.

    ![A ligação de Snowflake na lista de aplicações](./media/snowflake-tutorial/tutorial_snowflake_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de Snowflake no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de Snowflake.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/snowflake-tutorial/tutorial_general_01.png
[2]: ./media/snowflake-tutorial/tutorial_general_02.png
[3]: ./media/snowflake-tutorial/tutorial_general_03.png
[4]: ./media/snowflake-tutorial/tutorial_general_04.png

[100]: ./media/snowflake-tutorial/tutorial_general_100.png

[200]: ./media/snowflake-tutorial/tutorial_general_200.png
[201]: ./media/snowflake-tutorial/tutorial_general_201.png
[202]: ./media/snowflake-tutorial/tutorial_general_202.png
[203]: ./media/snowflake-tutorial/tutorial_general_203.png

