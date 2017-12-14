---
title: "Tutorial: Integração do Azure Active Directory com SilkRoad vida Suite | Microsoft Docs"
description: "Saiba como configurar o início de sessão entre o Azure Active Directory e SilkRoad vida Suite."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: jeedes
ms.openlocfilehash: 0d6af7af7d6b28ff3ea9d518a65b8267a83b71b4
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2017
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Tutorial: Integração do Azure Active Directory com SilkRoad vida Suite

Neste tutorial, irá aprender a integrar SilkRoad vida Suite com o Azure Active Directory (Azure AD).

Integrar SilkRoad vida Suite com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao SilkRoad vida Suite.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada para SilkRoad vida Suite (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com SilkRoad vida Suite, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um conjunto de vida de SilkRoad início de sessão único subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar SilkRoad vida Suite na galeria do
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-silkroad-life-suite-from-the-gallery"></a>Adicionar SilkRoad vida Suite na galeria do
Para configurar a integração do SilkRoad vida Suite com o Azure AD, tem de adicionar SilkRoad vida Suite na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar SilkRoad vida Suite a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **SilkRoad vida Suite**, selecione **SilkRoad vida Suite** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![SilkRoad vida Suite na lista de resultados](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com SilkRoad vida Suite com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no SilkRoad vida Suite é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no SilkRoad vida Suite tem de ser estabelecida.

No SilkRoad vida Suite, atribua o valor do **nome de utilizador** no Azure AD como o valor a **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD-início de sessão único com SilkRoad vida Suite, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste SilkRoad vida Suite](#create-a-silkroad-life-suite-test-user)**  - para ter um homólogo de Britta Simon SilkRoad vida Suite que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação SilkRoad vida Suite.

**Para configurar o Azure AD-início de sessão único com SilkRoad vida Suite, execute os seguintes passos:**

1. No portal do Azure, no **SilkRoad vida Suite** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_samlbase.png)

3. No **SilkRoad vida Suite domínio e os URLs** secção, execute os seguintes passos:

    ![SilkRoad vida Suite domínio e os URLs únicos de informações de início de sessão](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_url1.png)

    a. No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte:`https://<subdomain>.silkroad-eng.com/Authentication/`

    b. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: 
    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/SP` |
    | `https://<subdomain>.silkroad.com/Authentication/SP` |

    c. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: 
    | |
    |--|
    | `https://<subdomain>.silkroad-eng.com/Authentication/` |
    | `https://<subdomain>.silkroad.com/Authentication/` |
     
    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de cliente de conjunto de vida de SilkRoad](https://www.silkroad.com/locations/) para obter estes valores. 

4. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_400.png)
    
6. No **SilkRoad vida Suite configuração** secção, clique em **configurar Suite de vida de SilkRoad** para abrir **configurar início de sessão** janela. Copiar o **Sign-Out URL, o ID de entidade de SAML e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configuração de Suite SilkRoad vida](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_configure.png) 

7. Início de sessão no site da sua empresa SilkRoad como administrador. 
 
    >[!NOTE] 
    > Para obter acesso à aplicação SilkRoad vida Suite autenticação para configurar a Federação com o Microsoft Azure AD, contacte o suporte de SilkRoad ou o seu representante SilkRoad serviços.

8. Aceda a **fornecedor de serviços**e, em seguida, clique em **detalhes de Federação**. 
   
    ![Azure AD início de sessão único][10]

9. Clique em **transferir metadados de Federação**e, em seguida, guarde o ficheiro de metadados no seu computador.
   
    ![Azure AD início de sessão único][11] 

10. No seu **SilkRoad** aplicação, clique em **autenticação origens**.
   
    ![Azure AD início de sessão único][12] 

11. Clique em **Adicionar origem de autenticação**. 
   
    ![Azure AD início de sessão único][13] 

12. No **Adicionar origem de autenticação** secção, execute os seguintes passos: 
   
    ![Azure AD início de sessão único][14]
  
    a. Em **opção 2 - ficheiro de metadados**, clique em **procurar** carregar o ficheiro de metadados transferido a partir do portal do Azure.
  
    b. Clique em **criar fornecedor de identidade com dados de ficheiro**.

13. No **autenticação origens** secção, clique em **editar**. 
    
     ![Azure AD início de sessão único][15] 

14. No **Editar origem de autenticação** caixa de diálogo, execute os seguintes passos: 
    
     ![Azure AD início de sessão único][16] 

    a. Como **ativado**, selecione **Sim**.

    b. No **EntityId** caixa de texto, cole o valor de **ID de entidade de SAML** que copiou do portal do Azure.
   
    c. No **IdP Descrição** caixa de texto, digite uma descrição para a sua configuração (por exemplo: *SSO do Azure AD*).

    d. No **ficheiro de metadados** caixa de texto, carregue o **metadados** ficheiro que transferiu a partir do portal do Azure.
  
    e. No **IdP nome** caixa de texto, escreva um nome que é específica para a configuração (por exemplo: *Azure SP*).
  
    f. No **URL do serviço fim de sessão** caixa de texto, cole o valor de **Sign-Out URL** que copiou do portal do Azure.

    g. No **início de sessão no URL do serviço** caixa de texto, cole o valor de **único início de sessão no URL do serviço SAML** que copiou do portal do Azure.

    h. Clique em **Guardar**.

15. Desative todas as outras origens de autenticação. 
    
     ![Azure AD início de sessão único][17]

> [!TIP]
> Pode agora ler estas instruções dentro de uma versão concisa o [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir do **do Active Directory > aplicações da empresa** secção, basta clicar no **Single Sign-On** separador e aceder à documentação do embedded através de **configuração** secção na parte inferior. Pode ler mais sobre a funcionalidade de documentação incorporados aqui: [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-silkroad-life-suite-test-user"></a>Criar um utilizador de teste SilkRoad vida Suite

Nesta secção, vai criar um utilizador chamado Britta Simon no SilkRoad vida Suite. Trabalhar com [equipa de suporte de cliente de conjunto de vida de SilkRoad](https://www.silkroad.com/locations/) para adicionar os utilizadores na plataforma SilkRoad vida Suite. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para SilkRoad vida Suite.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a SilkRoad vida Suite, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **SilkRoad vida Suite**.

    ![A ligação de SilkRoad vida Suite na lista de aplicações](./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroadlifesuite_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico SilkRoad vida Suite no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de SilkRoad vida Suite.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
