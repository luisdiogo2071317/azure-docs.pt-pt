---
title: "Tutorial: Integração do Azure Active Directory com TOPdesk - segura | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e TOPdesk - segura."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8e06ee33-18f9-4c05-9168-e6b162079d88
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: ca3362bc3f966adaf9940f6eb4bec5235c6ea7d8
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Tutorial: Integração do Azure Active Directory com TOPdesk - segura

Neste tutorial, irá aprender a integrar TOPdesk - seguro com o Azure Active Directory (Azure AD).

Integrar TOPdesk - segura com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao TOPdesk - segura.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para TOPdesk - seguro (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com TOPdesk - segura, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um TOPdesk - segura-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar TOPdesk - seguro a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-topdesk---secure-from-the-gallery"></a>Adicionar TOPdesk - seguro a partir da Galeria
Para configurar a integração de TOPdesk - seguro com o Azure AD, terá de adicionar TOPdesk - seguro a partir da Galeria à sua lista de SaaS gerido aplicações.

**Para adicionar TOPdesk - seguro a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **TOPdesk - segura**, selecione **TOPdesk - segura** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![TOPdesk - seguro na lista de resultados](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com TOPdesk - segura com base num utilizador teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no TOPdesk - segura é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no TOPdesk - segura tem de ser estabelecida.

No TOPdesk - seguro, atribua o valor do **nome de utilizador** no Azure AD como o valor a **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD-início de sessão único com TOPdesk - segura, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um TOPdesk - utilizador de teste seguro](#create-a-topdesk---secure-test-user)**  - para ter um homólogo de Britta Simon no TOPdesk - segura que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua TOPdesk - aplicação segura.

**Para configurar o Azure AD-início de sessão único com TOPdesk - proteger, execute os seguintes passos:**

1. No portal do Azure, no **TOPdesk - segura** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_samlbase.png)

3. No **TOPdesk - domínio seguro e URLs** secção, execute os seguintes passos:

    ![TOPdesk - domínio seguro e URLs único informações de início de sessão](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_url.png)

    a. No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://<companyname>.topdesk.net`

    b. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://<companyname>.topdesk.net/tas/secure/login/verify`

    c. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://<companyname>.topdesk.net/tas/public/login/saml`

    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o URL de início de sessão e o identificador real. URL de resposta é explicado mais tarde no tutorial. Contacte [TOPdesk - a equipa de suporte de cliente Secure](http://www.topdesk.com/us/support) para obter estes valores. 

4. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_400.png)

6. No **TOPdesk - configuração Secure** secção, clique em **configurar TOPdesk - segura** para abrir **configurar início de sessão** janela. Copiar o **Sign-Out URL, o ID de entidade de SAML e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![TOPdesk - uma configuração segura](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_configure.png)
    
7. Inicie sessão no seu **TOPdesk - segura** site da empresa como administrador.

8. No **TOPdesk** menu, clique em **definições**.

    ![Definições](./media/active-directory-saas-topdesk-secure-tutorial/ic790598.png "definições")

9. Clique em **definições de início de sessão**.

    ![Definições de início de sessão](./media/active-directory-saas-topdesk-secure-tutorial/ic790599.png "definições de início de sessão")

10. Expanda o **definições de início de sessão** e, em seguida, clique em **geral**.

    ![Geral](./media/active-directory-saas-topdesk-secure-tutorial/ic790600.png "geral")

11. No **seguro** secção o **início de sessão SAML** configuração secção, execute os seguintes passos:

    ![Definições técnicas](./media/active-directory-saas-topdesk-secure-tutorial/ic790855.png "definições técnicas")
   
    a. Clique em **transferir** para transferir o ficheiro de metadados pública e, em seguida, guarde-o localmente no seu computador.
   
    b. Abra o ficheiro de metadados e, em seguida, localize o **AssertionConsumerService** nós.
    
    ![Serviço de consumidor asserção](./media/active-directory-saas-topdesk-secure-tutorial/ic790856.png "asserção consumidor serviço")
   
    c. Copiar o **AssertionConsumerService** valor, cole este valor na caixa de texto de resposta de URL no **TOPdesk - domínio seguro e URLs** secção.

12. Para criar um ficheiro de certificado, execute os seguintes passos:
    
    ![Certificado](./media/active-directory-saas-topdesk-secure-tutorial/ic790606.png "certificado")
    
    a. Abra o ficheiro de metadados transferido a partir do portal do Azure.

    b. Expanda o **RoleDescriptor** nó que tenha um **xsi: type** de **sejam fornecidas: ApplicationServiceType**.

    c. Copie o valor do **certificado X509** nós.

    d. Guardar o copiados **certificado X509** valor localmente no seu computador num ficheiro.

13. No **pública** secção, clique em **adicionar**.
    
    ![Adicionar](./media/active-directory-saas-topdesk-secure-tutorial/ic790607.png "adicionar")

14. No **Assistente de configuração de SAML** diálogo página, execute os seguintes passos:
    
    ![Assistente de configuração de SAML](./media/active-directory-saas-topdesk-secure-tutorial/ic790608.png "Assistente de configuração de SAML")
    
    a. Para carregar o ficheiro de metadados transferidos do portal do Azure, em **metadados da Federação**, clique em **procurar**.

    b. Para carregar o ficheiro de certificado em **certificados RSA ()**, clique em **procurar**.

    c. Carregar o ficheiro de logótipo obteve da equipa de suporte TOPdesk, em **ícone de logótipo**, clique em **procurar**.

    d. No **atributo de nome de utilizador** caixa de texto, tipo `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. No **nome a apresentar** caixa de texto, escreva um nome para a sua configuração.

    f. Clique em **Guardar**.

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-topdesk-secure-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-topdesk-secure-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-topdesk-secure-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-topdesk---secure-test-user"></a>Criar um TOPdesk - utilizador de teste seguro

Para permitir que os utilizadores do Azure AD sessão TOPdesk - segura, estes têm de ser aprovisionados para TOPdesk - segura.  
No caso de TOPdesk - seguro, o aprovisionamento é uma tarefa manual.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Para configurar o aprovisionamento de utilizadores, execute os seguintes passos:
1. Inicie sessão no seu **TOPdesk - segura** site da empresa como administrador.
2. No menu na parte superior, clique em **TOPdesk \> novo \> ficheiros de suporte \> operador**.
   
    ![Operador](./media/active-directory-saas-topdesk-secure-tutorial/ic790610.png "operador")

3. No **operador New** caixa de diálogo, execute os seguintes passos:
   
    ![Operador New](./media/active-directory-saas-topdesk-secure-tutorial/ic790611.png "operador New")
   
    a. Clique em de **geral** separador.
   
    b. No **Apelido** caixa de texto, como o tipo apelido do utilizador **Simon**.
   
    c. Selecione um **Site** para a conta no **localização** secção.
   
    d. No **nome de início de sessão** caixa de texto do **TOPdesk início de sessão** secção, escreva um nome de início de sessão para os seus utilizadores.
   
    e. Clique em **Guardar**.

> [!NOTE]
> Pode utilizar qualquer outro TOPdesk - ferramentas de criação de conta de utilizador segura ou APIs fornecidas pelos TOPdesk - segura para aprovisionar contas de utilizador do AAD.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para TOPdesk - segura.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a TOPdesk - proteger, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **TOPdesk - segura**.

    ![TOPdesk - ligação segura na lista de aplicações](./media/active-directory-saas-topdesk-secure-tutorial/tutorial_topdesk-secure_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Ao clicar em TOPdesk - mosaico segura no painel de acesso, deve obter automaticamente com sessão iniciada para sua TOPdesk - aplicação segura.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-topdesk-secure-tutorial/tutorial_general_203.png

