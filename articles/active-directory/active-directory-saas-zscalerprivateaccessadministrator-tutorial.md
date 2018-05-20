---
title: 'Tutorial: Integração do Azure Active Directory com o administrador de acesso privado Zscaler | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e o administrador de acesso do Zscaler privada.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: c87392a7-e7fe-4cdc-a8e6-afe1ed975172
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2018
ms.author: jeedes
ms.openlocfilehash: c3918e4b54d60d6f609e99e24ed2f4b2995bf80c
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-administrator"></a>Tutorial: Integração do Azure Active Directory com o administrador de acesso Zscaler privada

Neste tutorial, irá aprender a integrar o administrador de acesso do Zscaler privada com o Azure Active Directory (Azure AD).

Integrar o administrador de acesso de Zscaler privada com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao administrador de acesso do Zscaler privada.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada Zscaler privada o administrador de acesso (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o administrador de acesso privado Zscaler, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um administrador de acesso privado Zscaler-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar administrador de acesso privado Zscaler na galeria do
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-zscaler-private-access-administrator-from-the-gallery"></a>Adicionar administrador de acesso privado Zscaler na galeria do
Para configurar a integração do administrador de acesso de Zscaler privada com o Azure AD, tem de adicionar administrador de acesso privado Zscaler na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar administrador de acesso privado Zscaler na galeria do, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Zscaler ao administrador de acesso privado**, selecione **Zscaler ao administrador de acesso privado** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar o aplicação.

    ![Administrador de acesso privado Zscaler na lista de resultados](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Zscaler privada administrador de acesso com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no administrador de acesso do privada Zscaler é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o administrador de acesso no Zscaler privada relacionados com o utilizador tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com o administrador de acesso Zscaler privada, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste de administrador de acesso privado Zscaler](#create-a-zscaler-private-access-administrator-test-user)**  - para ter um homólogo de Britta Simon no Zscaler privada administrador de acesso que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Zscaler privada ao administrador de acesso.

**Para configurar do Azure AD-início de sessão único com o administrador de acesso privado Zscaler, execute os seguintes passos:**

1. No portal do Azure, no **administrador de acesso privado Zscaler** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_samlbase.png)

3. No **URLs e de domínio de administrador de acesso privado Zscaler** secção se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Domínio de administrador de acesso privado Zscaler e URLs único informações de início de sessão](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_url.png)

    a. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.private.zscaler.com/auth/metadata`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.private.zscaler.com/auth/sso`

    c. Verifique **Mostrar avançadas definições de URL**

    d. No **RelayState** caixa de texto, escreva um valor: `idpadminsso`

4.  Se pretender configurar a aplicação no **SP** modo iniciado execute os seguintes passos:

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.private.zscaler.com/auth/sso`

    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o URL de identificador, o URL de resposta e início de sessão real. Contacte [equipa de suporte de administrador de acesso privado Zscaler](https://help.zscaler.com/zpa-submit-ticket) para obter estes valores.
 
5. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_400.png)

7. Numa janela do browser web diferente, início de sessão Zscaler privada o administrador de acesso como administrador.

8. Na parte superior, clique em **administração** e navegue para **autenticação** secção clique **IdP configuração**.

    ![Administrador de acesso privado Zscaler admin](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

9. No canto superior direito, clique em **Adicionar configuração de IdP**. 

    ![Administrador de acesso privado Zscaler addidp](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addpidp.png)

10. No **Adicionar configuração de IdP** página execute os seguintes passos:
 
    ![Administrador de acesso privado Zscaler idpselect](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_idpselect.png)

    a. Clique em **selecionar ficheiro** para carregar o ficheiro de metadados transferido do Azure AD no **carregar de ficheiro de metadados do IdP** campo.

    b. Também lê o **IdP metadados** do Azure AD e preenche todas as informações de campos, conforme mostrado abaixo.

    ![Administrador de acesso privado Zscaler idpconfig](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/idpconfig.png)

    c. Selecione **início de sessão único** como **administrador**.

    d. Selecione o seu domínio de **domínios** campo.
    
    e. Clique em **Guardar**.

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-a-zscaler-private-access-administrator-test-user"></a>Criar um utilizador de teste de administrador de acesso do Zscaler privada

Para ativar a utilizadores do Azure AD para início de sessão para o administrador de acesso Zscaler privada, deve ser aprovisionado para o administrador de acesso do Zscaler privada. No caso de Zscaler privada ao administrador de acesso, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa ao administrador de acesso privado Zscaler como administrador.

2. Na parte superior, clique em **administração** e navegue para **autenticação** secção clique **IdP configuração**.

    ![Administrador de acesso privado Zscaler admin](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_admin.png)

3. Clique em **administradores** do lado esquerdo do menu.

    ![Administrador de administrador de acesso de Zscaler privada](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_adminstrator.png)

4. No canto superior direito, clique em **Adicionar administrador**:

    ![Administrador de acesso privado Zscaler Adicionar administrador](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_addadmin.png)

5. No **Adicionar administrador** página, execute os seguintes passos:

    ![Administração de utilizador de administrador de acesso de Zscaler privada](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_useradmin.png)

    a. No **Username** caixa de texto, introduza o e-mail do utilizador, como **BrittaSimon@contoso.com**.

    b. No **palavra-passe** caixa de texto, escreva a palavra-passe.

    c. No **Confirmar palavra-passe** caixa de texto, escreva a palavra-passe.

    d. Selecione **função** como **administrador de acesso privado Zscaler**.

    e. No **E-Mail** caixa de texto, introduza o e-mail do utilizador, como **BrittaSimon@contoso.com**.

    f. No **Phone** caixa de texto, escreva o número de telefone.

    g. No **fuso horário** caixa de texto, selecione o fuso horário.

    h. Clique em **Guardar**.  

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para o administrador de acesso do Zscaler privada.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon para o administrador de acesso do privada Zscaler, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **administrador de acesso privado Zscaler**.

    ![A ligação de administrador de acesso privado Zscaler na lista de aplicações](./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_zscalerprivateaccessadministrator_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Zscaler privada ao administrador de acesso no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de administrador de acesso de Zscaler privada.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscalerprivateaccessadministrator-tutorial/tutorial_general_203.png

