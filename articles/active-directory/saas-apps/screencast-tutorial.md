---
title: 'Tutorial: Integração do Azure Active Directory Screencast-O Matic com | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Screencast-O Matic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 525ad47d-5488-40e2-aeaf-ae6183745682
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: jeedes
ms.openlocfilehash: 7212e0b07b525286f0b194a53c6780269630ad9c
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320731"
---
# <a name="tutorial-azure-active-directory-integration-with-screencast-o-matic"></a>Tutorial: Integração do Azure Active Directory Screencast-O Matic com

Neste tutorial, irá aprender a integrar Screencast-O Matic com o Azure Active Directory (Azure AD).

Integrar Screencast-O Matic com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Screencast-O Matic.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Screencast-ão-Matic (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Screencast-O Matic, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Matic Screencast-ão-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Screencast-O Matic partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-screencast-o-matic-from-the-gallery"></a>Adicionar Screencast-O Matic partir da Galeria
Para configurar a integração de Screencast-O Matic com o Azure AD, terá de adicionar Screencast-O Matic a Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Screencast-O Matic a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Matic Screencast-ão**, selecione **Matic Screencast-ão** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Screencast-ão-Matic na lista de resultados](./media/screencast-tutorial/tutorial_screencast_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Screencast-ão-Matic com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo Screencast-O Matic no é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Matic Screencast-ão tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Screencast-O Matic, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Matic Screencast-ão](#create-a-screencast-o-matic-test-user)**  - para ter um homólogo de Britta Simon Screencast-ão-Matic que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Screencast-O Matic.

**Para configurar o Azure AD-início de sessão único com Screencast-O Matic, execute os seguintes passos:**

1. No portal do Azure, no **Matic Screencast-ão** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/screencast-tutorial/tutorial_screencast_samlbase.png)

3. No **Screencast-O Matic domínio e os URLs** secção, execute os seguintes passos:

    ![Domínio screencast-O Matic e os URLs únicos de informações de início de sessão](./media/screencast-tutorial/tutorial_screencast_url.png)

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://screencast-o-matic.com/<InstanceName>`

    > [!NOTE] 
    > O valor do URL de início de sessão não é real. Atualize o valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente Matic Screencast-ão](mailto:support@screencast-o-matic.com) para obter o valor. 
 
4. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/screencast-tutorial/tutorial_screencast_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/screencast-tutorial/tutorial_general_400.png)

6. Numa janela do browser web diferente, no início de sessão Screencast-ão-Matic como administrador.

7. Clique em **subscrição**.

    ![A subscrição](./media/screencast-tutorial/tutorial_screencast_sub.png)

8. Em **página acesso** secção, clique em **configuração**.

    ![O acesso](./media/screencast-tutorial/tutorial_screencast_setup.png)

9. No **página de configuração de acesso**, execute os seguintes passos:

    * Em **acesso URL** secção, escreva o seu instancename na caixa de texto especificada.

    ![O acesso](./media/screencast-tutorial/tutorial_screencast_access.png)

    * Selecione **necessitam de utilizador de domínio** em **restrição de utilizador de SAML (opcional)** secção.

    * Em **carregar ficheiro de XML de metadados do IDP**, clique em **Escolher ficheiro** para carregar os metadados do que transferiu a partir do portal do Azure.

    * Clique em **OK**. 

    ![O acesso](./media/screencast-tutorial/tutorial_screencast_save.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/screencast-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/screencast-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/screencast-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/screencast-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-screencast-o-matic-test-user"></a>Criar um Screencast-O Matic utilizador de teste

O objetivo desta secção consiste em criar um utilizador chamado Britta Simon Screencast-O Matic. Matic screencast-ão suporta aprovisionamento just-in-time, que é por predefinição ativada. Não há nenhum item de ação para si nesta secção. Um novo utilizador é criado durante a tentativa de aceder Matic Screencast-ão se não existir ainda.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de cliente Matic Screencast-ão](mailto:support@screencast-o-matic.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, ative Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Screencast-O Matic.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon Screencast-O Matic a, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Matic Screencast-ão**.

    ![A ligação Screencast-O Matic na lista de aplicações](./media/screencast-tutorial/tutorial_screencast_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Screencast-O Matic no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação Screencast-O Matic.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/screencast-tutorial/tutorial_general_01.png
[2]: ./media/screencast-tutorial/tutorial_general_02.png
[3]: ./media/screencast-tutorial/tutorial_general_03.png
[4]: ./media/screencast-tutorial/tutorial_general_04.png

[100]: ./media/screencast-tutorial/tutorial_general_100.png

[200]: ./media/screencast-tutorial/tutorial_general_200.png
[201]: ./media/screencast-tutorial/tutorial_general_201.png
[202]: ./media/screencast-tutorial/tutorial_general_202.png
[203]: ./media/screencast-tutorial/tutorial_general_203.png

