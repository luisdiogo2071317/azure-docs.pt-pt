---
title: 'Tutorial: Integração do Azure Active Directory com o Salesforce | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Salesforce.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: 64b94baeaede9b05e953b69324648c63d97cea8e
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39045443"
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Tutorial: Integração do Azure Active Directory com o Salesforce

Neste tutorial, saiba como integrar o Salesforce com o Azure Active Directory (Azure AD).

Integração de Salesforce com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Salesforce.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Salesforce (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Salesforce, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Salesforce logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Salesforce a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-salesforce-from-the-gallery"></a>Adicionar Salesforce a partir da Galeria
Para configurar a integração do Salesforce para o Azure AD, terá de adicionar Salesforce a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Salesforce a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Salesforce**, selecione **Salesforce** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Salesforce na lista de resultados](./media/salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Salesforce, com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no Salesforce a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Salesforce tem de ser estabelecida.

No Salesforce, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Salesforce, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste do Salesforce](#create-a-salesforce-test-user)**  - para ter um equivalente da Eduarda Almeida no Salesforce, que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do Salesforce.

**Para configurar o Azure AD início de sessão único com o Salesforce, execute os seguintes passos:**

1. No portal do Azure, sobre o **Salesforce** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/salesforce-tutorial/tutorial_salesforce_samlbase.png)

3. Sobre o **Salesforce domínio e URLs** secção, execute os seguintes passos:

    ![URLs de domínio do Salesforce e únicas início de sessão em informações](./media/salesforce-tutorial/tutorial_salesforce_url.png)
    
    a. Na **URL de início de sessão** caixa de texto, digite o valor usando o seguinte padrão:
    
    Conta de empresa: `https://<subdomain>.my.salesforce.com`

    Conta de programador: `https://<subdomain>-dev-ed.my.salesforce.com`
    
    b. Na **identificador** caixa de texto, digite o valor usando o seguinte padrão:
    
    Conta de empresa: `https://<subdomain>.my.salesforce.com`

    Conta de programador: `https://<subdomain>-dev-ed.my.salesforce.com`
    
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão real e o identificador. Contacte [equipa de suporte de cliente do Salesforce](https://help.salesforce.com/support) obter esses valores.

4. Sobre o **certificado de assinatura SAML** secção, clique em **certificado** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/salesforce-tutorial/tutorial_salesforce_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/salesforce-tutorial/tutorial_general_400.png)

6. Sobre o **configuração do Salesforce** secção, clique em **configurar Salesforce** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração do Salesforce](./media/salesforce-tutorial/tutorial_salesforce_configure.png) 

7. Abra um novo separador no seu browser e inicie sessão na sua conta de administrador do Salesforce.

8. Clique nas **programa de configuração** sob **ícone de definições** no canto superior direito da página.

    ![Configurar o início de sessão único](./media/salesforce-tutorial/configure1.png)

9. Desloque para baixo para o **configurações** no painel de navegação, clique em **identidade** para expandir a secção relacionada. Em seguida, clique em **definições de início de sessão único**.

    ![Configurar o início de sessão único](./media/salesforce-tutorial/sf-admin-sso.png)

10. Sobre o **definições de início de sessão único** página, clique no **editar** botão.

    ![Configurar o início de sessão único](./media/salesforce-tutorial/sf-admin-sso-edit.png)
    
    > [!NOTE]
    > Se não conseguir ativar definições de início de sessão único para a sua conta do Salesforce, poderá ter de contactar [equipa de suporte de cliente do Salesforce](https://help.salesforce.com/support). 

11. Selecione **ativada SAML**e, em seguida, clique em **guardar**.

      ![Configurar o início de sessão único](./media/salesforce-tutorial/sf-enable-saml.png)
12. Para configurar o seu únicas início de sessão em definições de SAML, clique em **New**.

    ![Configurar o início de sessão único](./media/salesforce-tutorial/sf-admin-sso-new.png)

13. Sobre o **SAML único início de sessão definição editar** página, efetue as seguintes configurações:

    ![Configurar o início de sessão único](./media/salesforce-tutorial/sf-saml-config.png)

    a. Para o **nome** , digite um nome amigável para esta configuração. Fornecer um valor para **Name** preencher automaticamente o **nome da API** caixa de texto.

    b. Na **emissor** campo, cole o valor de **ID de entidade de SAML**, que copiou do portal do Azure.

    c. Na **caixa de texto do Id de entidade**, escreva o seu nome de domínio do Salesforce com o seguinte padrão:
      
      * Conta de empresa: `https://<subdomain>.my.salesforce.com`
      * Conta de programador: `https://<subdomain>-dev-ed.my.salesforce.com`
      
    d. Para carregar os **certificado do fornecedor de identidade**, clique em **Escolher ficheiro** para procurar e selecionar o ficheiro de certificado que transferiu a partir do portal do Azure.

    e. Como **tipo de identidade de SAML**, escolha uma das seguintes opções:
    
      * Selecione **asserção contém o nome do usuário Salesforce**, se o nome de utilizador do utilizador Salesforce está sendo passado na asserção de SAML

      * Selecione **asserção contém o ID de Federação do objeto de utilizador**, se o ID de Federação do objeto de utilizador que está sendo passado na asserção de SAML

      * Selecione **asserção contém o ID de uso do objeto de utilizador**, se o ID de utilizador do objeto de utilizador que está sendo passado na asserção de SAML

    f. Para **localização de identidade de SAML**, selecione **identidade é no elemento NameIdentifier da declaração de assunto**.

    g. Para **fornecedor iniciada solicitar vínculo de serviço**, selecione **redirecionamento de HTTP**.

    h. Na **URL de início de sessão do fornecedor de identidade** caixa de texto, cole o valor de **único URL de início de sessão no serviço**, que copiou do portal do Azure
    
    i. Por fim, clique em **guardar** para aplicar a suas SAML únicas início de sessão em definições.

14. No painel de navegação esquerdo no Salesforce, clique em **definições da empresa** para expandir a secção relacionada e, em seguida, clique em **meu domínio**.

    ![Configurar o início de sessão único](./media/salesforce-tutorial/sf-my-domain.png)

15. Desloque para baixo para o **configuração da autenticação** secção e clique nas **editar** botão.

    ![Configurar o início de sessão único](./media/salesforce-tutorial/sf-edit-auth-config.png)

16. Na **configuração da autenticação** secção, verifique o **página de início de sessão** como **serviço de autenticação** de sua configuração SAML SSO e clique em  **Guardar**.

    ![Configurar o início de sessão único](./media/salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Se for selecionado mais do que um serviço de autenticação, é pedido aos utilizadores para selecionar o serviço de autenticação que eles gostam de iniciar sessão com ao iniciar o início de sessão único para o seu ambiente do Salesforce. Se não quer isso aconteça, deve **deixe desmarcada a todos os outros serviços de autenticação**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/salesforce-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/salesforce-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/salesforce-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/salesforce-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-salesforce-test-user"></a>Criar um utilizador de teste do Salesforce

Nesta secção, um usuário chamado Eduarda Almeida é criado no Salesforce. Salesforce suporta o aprovisionamento de just-in-time, que está ativado por predefinição. Não existe nenhum item de ação para nesta secção. Se um utilizador já não existir no Salesforce, uma nova é criada quando tentar acessar o Salesforce. Salesforce também suporta o aprovisionamento automático de utilizadores, pode encontrar mais detalhes [aqui](salesforce-provisioning-tutorial.md) sobre como configurar o aprovisionamento automático de utilizadores.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao Salesforce.

![Atribuir a função de utilizador][200]

**Para atribuir a Eduarda Almeida ao Salesforce, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **Salesforce**.

    ![A ligação de Salesforce na lista de aplicações](./media/salesforce-tutorial/tutorial_salesforce_app.png)

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do Salesforce no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do Salesforce.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configurar o aprovisionamento do utilizador](salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/salesforce-tutorial/tutorial_general_01.png
[2]: ./media/salesforce-tutorial/tutorial_general_02.png
[3]: ./media/salesforce-tutorial/tutorial_general_03.png
[4]: ./media/salesforce-tutorial/tutorial_general_04.png

[100]: ./media/salesforce-tutorial/tutorial_general_100.png

[200]: ./media/salesforce-tutorial/tutorial_general_200.png
[201]: ./media/salesforce-tutorial/tutorial_general_201.png
[202]: ./media/salesforce-tutorial/tutorial_general_202.png
[203]: ./media/salesforce-tutorial/tutorial_general_203.png
