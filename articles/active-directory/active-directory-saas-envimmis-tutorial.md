---
title: 'Tutorial: Integração do Azure Active Directory com Envi MMIS | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Envi MMIS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ab89f8ee-2507-4625-94bc-b24ef3d5e006
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2018
ms.author: jeedes
ms.openlocfilehash: 0d338f7f56e668c25378403e88a6532b8b8573c5
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34344274"
---
# <a name="tutorial-azure-active-directory-integration-with-envi-mmis"></a>Tutorial: Integração do Azure Active Directory com Envi MMIS

Neste tutorial, irá aprender a integrar Envi MMIS com o Azure Active Directory (Azure AD).

Integrar Envi MMIS com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Envi MMIS.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada para Envi MMIS (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Envi MMIS, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Envi MMIS-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Envi MMIS na galeria do
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-envi-mmis-from-the-gallery"></a>Adicionar Envi MMIS na galeria do
Para configurar a integração de Envi MMIS com o Azure AD, tem de adicionar Envi MMIS na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Envi MMIS a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Envi MMIS**, selecione **Envi MMIS** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Envi MMIS na lista de resultados](./media/active-directory-saas-envimmis-tutorial/tutorial_envimmis_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Envi MMIS baseado na chamado "Britta Simon" um utilizador de teste.

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Envi MMIS é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Envi MMIS tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Envi MMIS, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Envi MMIS](#create-an-envi-mmis-test-user)**  - para ter um homólogo de Britta Simon MMIS Envi que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Envi MMIS.

**Para configurar o Azure AD-início de sessão único com Envi MMIS, execute os seguintes passos:**

1. No portal do Azure, no **Envi MMIS** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-envimmis-tutorial/tutorial_envimmis_samlbase.png)

3. No **Envi MMIS domínio e os URLs** secção, execute os seguintes passos, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Envi MMIS domínio e os URLs únicos de informações de início de sessão](./media/active-directory-saas-envimmis-tutorial/tutorial_envimmis_url.png)

    a. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://www.<CUSTOMER DOMAIN>.com/Account`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://www.<CUSTOMER DOMAIN>.com/Account/Acs`

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![Envi MMIS domínio e os URLs únicos de informações de início de sessão](./media/active-directory-saas-envimmis-tutorial/tutorial_envimmis_url1.png)

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://www.<CUSTOMER DOMAIN>.com/Account`
     
    > [!NOTE]
    > Estes valores não estiverem reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de cliente de MMIS Envi](mailto:support@ioscorp.com) para obter estes valores.

5. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-envimmis-tutorial/tutorial_envimmis_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-envimmis-tutorial/tutorial_general_400.png)

7. Numa janela do browser web diferente, inicie sessão no seu site Envi MMIS como administrador.

8. Clique em **meu domínio** separador.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-envimmis-tutorial/configure1.png)

9. Clique em **Editar**.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-envimmis-tutorial/configure2.png)

10. Selecione **utilizar autenticação remota** caixa de verificação e, em seguida, selecione **redirecionamento de HTTP** do **tipo de autenticação** pendente.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-envimmis-tutorial/configure3.png)

11. Selecione **recursos** separador e, em seguida, clique em **carregar metadados**.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-envimmis-tutorial/configure4.png)

12. No **carregar metadados** pop-up, execute os seguintes passos:

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-envimmis-tutorial/configure5.png)

    a. Selecione **ficheiro** opção do **carregar de** pendente.

    b. Carregar o ficheiro de metadados transferido a partir do portal do Azure ao selecionar o **escolha o ícone de ficheiro**.

    c. Clique em **OK**.

13. Depois de carregar o ficheiro de metadados transferido os campos irão obter preenchidos automaticamente. Clique em **atualização**

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-envimmis-tutorial/configure6.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-envimmis-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-envimmis-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-envimmis-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-envimmis-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-envi-mmis-test-user"></a>Criar um utilizador de teste Envi MMIS

Para permitir que os utilizadores do Azure AD iniciem sessão nos Envi MMIS, têm de ser aprovisionados para Envi MMIS.  
No caso de Envi MMIS, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa Envi MMIS como administrador.

2. Clique em **lista de utilizadores** separador.

    ![Adicionar empregado](./media/active-directory-saas-envimmis-tutorial/user1.png)

3. Clique em **adicionar utilizador** botão.

    ![Adicionar empregado](./media/active-directory-saas-envimmis-tutorial/user2.png)

4. No **adicionar utilizador** secção, execute os seguintes passos:

    ![Adicionar empregado](./media/active-directory-saas-envimmis-tutorial/user3.png)

    a. No **nome de utilizador** caixa de texto, como o tipo do nome de utilizador da conta de Britta Simon **brittasimon@contoso.com**.
    
    b. No **nome próprio** caixa de texto, como o tipo do nome próprio do BrittaSimon **Britta**.

    c. No **Apelido** caixa de texto, como o tipo o último nome do BrittaSimon **Simon**.

    d. Introduza o título do utilizador no **título** da caixa de texto.
    
    e. No **endereço de correio eletrónico** caixa de texto, como o tipo de endereço de correio eletrónico da conta de Britta Simon **brittasimon@contoso.com**.

    f. No **nome de utilizador de SSO** caixa de texto, como o tipo do nome de utilizador da conta de Britta Simon **brittasimon@contoso.com**.

    g. Clique em **Guardar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Envi MMIS.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a Envi MMIS, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Envi MMIS**.

    ![A ligação de Envi MMIS na lista de aplicações](./media/active-directory-saas-envimmis-tutorial/tutorial_envimmis_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Envi MMIS no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de Envi MMIS.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-envimmis-tutorial/tutorial_general_203.png

