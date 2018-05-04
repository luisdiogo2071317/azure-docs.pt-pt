---
title: 'Tutorial: Integração do Azure Active Directory com OrgChart agora | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e OrgChart agora.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 50a1522f-81de-4d14-9b6b-dd27bb1338a4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2018
ms.author: jeedes
ms.openlocfilehash: 8af344f6aa3cd666d655405f927542850b770ae5
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/30/2018
---
# <a name="tutorial-azure-active-directory-integration-with-orgchart-now"></a>Tutorial: Integração do Azure Active Directory com OrgChart agora

Neste tutorial, irá aprender a integrar OrgChart agora com o Azure Active Directory (Azure AD).

Integrar OrgChart agora com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao OrgChart agora.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada para OrgChart agora (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com OrgChart agora, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um OrgChart agora início de sessão único subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar OrgChart agora a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-orgchart-now-from-the-gallery"></a>Adicionar OrgChart agora a partir da Galeria
Para configurar a integração de OrgChart agora com o Azure AD, tem de adicionar OrgChart agora na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar OrgChart agora a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **OrgChart agora**, selecione **OrgChart agora** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![OrgChart agora na lista de resultados](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com OrgChart agora baseado na chamado "Britta Simon" um utilizador de teste.

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no OrgChart agora é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no OrgChart agora tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com OrgChart agora, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste agora OrgChart](#create-an-orgchart-now-test-user)**  - para ter um homólogo de Britta Simon OrgChart agora que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação OrgChart agora.

**Para configurar o Azure AD-início de sessão único com OrgChart agora, execute os seguintes passos:**

1. No portal do Azure, no **OrgChart agora** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_samlbase.png)

3. No **OrgChart agora o domínio e os URLs** secção, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![OrgChart agora o domínio e os URLs únicos de informações de início de sessão](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_url.png)

    No **identificador** caixa de texto, escreva um URL: `https://sso2.orgchartnow.com`

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![OrgChart agora o domínio e os URLs únicos de informações de início de sessão](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_url1.png)

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://sso2.orgchartnow.com/Shibboleth.sso/Login?entityID=<YourEntityID>&target=https://sso2.orgchartnow.com`
     
    > [!NOTE]
    > `<YourEntityID>` o ID de entidade de SAML é copiado da secção de referência rápida, descrita mais adiante no tutorial.

5. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_400.png)
    
7. No **OrgChart agora configuração** secção, clique em **configurar agora OrgChart** para abrir **configurar início de sessão** janela. Copiar o **ID de entidade de SAML** do **secção de referência rápida** e utilizá-lo para concluir **URL de início de sessão** no **OrgChart agora o domínio e os URLs secção**.

    ![Configuração de OrgChart agora](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_configure.png) 

8. Para configurar o início de sessão único em **OrgChart agora** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte de OrgChart agora](mailto:ocnsupport@officeworksoftware.com). Se definir esta definição para que a ligação de SAML SSO corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-orgchartnow-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-orgchartnow-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-orgchartnow-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-orgchartnow-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-orgchart-now-test-user"></a>Criar um utilizador de teste OrgChart agora

Para permitir que os utilizadores do Azure AD iniciem sessão nos OrgChart agora, têm de ser aprovisionados para OrgChart agora. 

1. OrgChart agora suporta o aprovisionamento just-in-time, que é por predefinição ativada. Um novo utilizador é criado durante a tentativa de aceder OrgChart agora se não existir ainda. A funcionalidade de aprovisionamento de utilizadores de just-in-time só irão criar um **só de leitura** utilizador quando um pedido SSO é proveniente de um IDP reconhecido e o e-mail na asserção SAML não foi encontrado na lista de utilizadores. Para este automaticamente a funcionalidade de aprovisionamento tem de criar um grupo de acesso intitulado **geral** no OrgChart agora. Siga os passos abaixo para criar um grupo de acesso:

    a. Vá para o **gerir grupos** opção depois de clicar o **gear** no canto superior direito da IU.

    ![Grupos de OrgChart agora](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_manage.png)  

    b. Selecione o **adicionar** ícone e o nome do grupo de **geral** , em seguida, clique em **OK**. 

    ![Adicionar OrgChart agora](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_add.png)

    c. Selecione as pastas que pretende gerais ou só de leitura aos utilizadores ser capazes de aceder:

    ![Pastas de OrgChart agora](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_chart.png)

    d. **Bloqueio** as pastas para que apenas os utilizadores administradores podem modificá-las. Em seguida, prima **OK**.

    ![Bloqueio OrgChart agora](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_lock.png)

2. Para criar **Admin** utilizadores e **leitura/escrita** utilizadores, tem de criar manualmente um utilizador para aceder ao respetivo nível de privilégio através de SSO. Para Aprovisionar uma conta de utilizador, execute os seguintes passos:

    a. Inicie sessão no OrgChart agora como um administrador de segurança.

    b.  Clique em **definições** na parte superior direita canto e, em seguida, navegue para **gerir utilizadores**.

    ![Definições de OrgChart agora](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_settings.png)

    c. Clique em **adicionar** e execute os seguintes passos:

    ![Gerir OrgChart agora](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_manageusers.png)

    * No **ID de utilizador** caixa de texto, introduza o ID de utilizador como **brittasimon@contoso.com**.

    * No **endereço de correio eletrónico** texto, introduza o e-mail do utilizador, como **brittasimon@contoso.com**.

    * Clique em **Adicionar**.
    
### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para OrgChart agora.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a OrgChart agora, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **OrgChart agora**.

    ![A ligação de OrgChart agora na lista de aplicações](./media/active-directory-saas-orgchartnow-tutorial/tutorial_orgchartnow_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico OrgChart agora no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação agora OrgChart.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-orgchartnow-tutorial/tutorial_general_203.png

