---
title: 'Tutorial: Integração do Azure Active Directory com serviços de gestão de assistência | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e os serviços de gestão de assistência.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 1f56e612-728b-4203-a545-a81dc5efda00
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2018
ms.author: jeedes
ms.openlocfilehash: abf882d7b06083080ad5cb3c2a20390a76a48139
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="tutorial-azure-active-directory-integration-with-attendance-management-services"></a>Tutorial: Integração do Azure Active Directory com serviços de gestão de assistência

Neste tutorial, irá aprender a integrar os serviços de gestão de assistência com o Azure Active Directory (Azure AD).

Integração dos serviços de gestão de assistência com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso aos serviços de gestão de assistência.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada assistência nos serviços de gestão (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com os serviços de gestão de assistência, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Dos serviços de gestão de assistência-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. A adição de serviços de gestão de assistência de galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-attendance-management-services-from-the-gallery"></a>A adição de serviços de gestão de assistência de galeria
Para configurar a integração dos serviços de gestão de assistência com o Azure AD, tem de adicionar serviços de gestão de assistência da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar serviços de gestão de assistência da galeria do, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **dos serviços de gestão de assistência**, selecione **dos serviços de gestão de assistência** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Serviços de gestão de assistência na lista de resultados](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com os serviços de gestão de assistência com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo nos serviços de gestão de assistência é um utilizador no Azure AD. Por outras palavras, uma relação entre um utilizador do Azure AD e o utilizador relacionado nos serviços de gestão de assistência de ligação tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com os serviços de gestão de assistência, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste de serviços de gestão de assistência](#create-an-attendance-management-service-test-user)**  - para ter um homólogo de Britta Simon nos serviços de gestão de assistência está ligado a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação de serviços de gestão de assistência.

**Para configurar o Azure AD-início de sessão único com os serviços de gestão de assistência, execute os seguintes passos:**

1. No portal do Azure, no **dos serviços de gestão de assistência** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_samlbase.png)

3. No **assistência gestão dos serviços de domínio e os URLs** secção, execute os seguintes passos:

    ![Assistência gestão dos serviços de domínio e os URLs únicos de informações de início de sessão](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_url.png)

    a. No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://id.obc.jp/<tenant information >/`

    b. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://id.obc.jp/<tenant information >/`

    > [!NOTE]
    > Estes valores não estiverem reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente de serviços de gestão de assistência](http://www.obcnet.jp/) para obter estes valores.

4. No **certificado de assinatura de SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_400.png)

6. No **configuração de serviços de gestão de assistência** secção, clique em **configurar serviços de gestão de assistência** para abrir **configurar início de sessão** janela. Copiar o **ID de entidade de SAML e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configuração de serviços de gestão de assistência](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_configure.png) 

7. Numa janela do browser diferente, início de sessão no site da sua empresa dos serviços de gestão de assistência como administrador.

8. Clique em **autenticação SAML** sob o **secção de gestão de segurança**.

    ![Configuração de serviços de gestão de assistência](./media/active-directory-saas-attendancemanagementservices-tutorial/user1.png)

9. Execute os seguintes passos:

    ![Configuração de serviços de gestão de assistência](./media/active-directory-saas-attendancemanagementservices-tutorial/user2.png)

    a. Selecione **autenticação SAML utilize**.

    b. No **identificador** caixa de texto, cole o valor de **ID de entidade de SAML**, que copiou do portal do Azure. 

    c. No **URL de ponto final de autenticação** caixa de texto, cole o valor de **único início de sessão no URL do serviço SAML**, que copiou do portal do Azure.

    d. Clique em **selecione um ficheiro** para carregar o certificado que transferiu a partir do Azure AD.

    e. Selecione **desative a autenticação de palavra-passe**.

    f. Clique em **registo**

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação! Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-attendancemanagementservices-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-attendancemanagementservices-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-attendancemanagementservices-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-attendancemanagementservices-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-attendance-management-services-test-user"></a>Criar um utilizador de teste de serviços de gestão de assistência

Para permitir que os utilizadores do Azure AD iniciem sessão nos serviços de gestão de assistência, tem de ser aprovisionadas para os serviços de gestão de assistência. No caso dos serviços de gestão de assistência, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa dos serviços de gestão de assistência como administrador.

2. Clique em **gestão de utilizadores** sob o **secção de gestão de segurança**.

    ![Adicionar empregado](./media/active-directory-saas-attendancemanagementservices-tutorial/user5.png)

3. Clique em **novo início de sessão de regras**.

    ![Adicionar empregado](./media/active-directory-saas-attendancemanagementservices-tutorial/user3.png)

4. No **OBCiD informações** secção, execute os seguintes passos:

    ![Adicionar empregado](./media/active-directory-saas-attendancemanagementservices-tutorial/user4.png)

    a. No **OBCiD** caixa de texto, como o tipo de mensagem de correio eletrónico do utilizador **BrittaSimon@contoso.com**.

    b. No **palavra-passe** caixa de texto, escreva a palavra-passe do utilizador.

    c. Clique em **registo**


### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso aos serviços de gestão de assistência.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon assistência nos serviços de gestão, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **dos serviços de gestão de assistência**.

    ![A ligação de serviços de gestão de assistência na lista de aplicações](./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_attendancemanagementservices_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico serviços de gestão de assistência no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de serviços de gestão de assistência.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-attendancemanagementservices-tutorial/tutorial_general_203.png

