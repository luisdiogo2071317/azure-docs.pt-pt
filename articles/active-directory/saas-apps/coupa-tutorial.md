---
title: 'Tutorial: Integração do Azure Active Directory com Coupa | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Coupa.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: jeedes
ms.openlocfilehash: 36c7bfeece12fe2bb2738e50de03ba2c8fc07bfa
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37111951"
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a>Tutorial: Integração do Azure Active Directory com Coupa

Neste tutorial, irá aprender a integrar Coupa com o Azure Active Directory (Azure AD).

Integrar Coupa com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Coupa.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Coupa (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Coupa, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Coupa-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Coupa a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-coupa-from-the-gallery"></a>Adicionar Coupa a partir da Galeria
Para configurar a integração de Coupa com o Azure AD, terá de adicionar Coupa a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Coupa a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone.

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]

3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Coupa**, selecione **Coupa** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Coupa na lista de resultados](./media/coupa-tutorial/tutorial_coupa_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Coupa com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Coupa é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Coupa tem de ser estabelecida.

No Coupa, atribua o valor do **nome de utilizador** no Azure AD como o valor a **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD-início de sessão único com Coupa, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Coupa](#create-a-coupa-test-user)**  - para ter um homólogo de Britta Simon Coupa que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Coupa.

**Para configurar o Azure AD-início de sessão único com Coupa, execute os seguintes passos:**

1. No portal do Azure, no **Coupa** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/coupa-tutorial/tutorial_coupa_samlbase.png)

3. No **Coupa domínio e os URLs** secção, execute os seguintes passos:

    ![Domínio Coupa e os URLs únicos de informações de início de sessão](./media/coupa-tutorial/tutorial_coupa_url.png)

    a. No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<companyname>.coupahost.com`

    > [!NOTE]
    > O valor do URL de início de sessão não é real. Atualize este valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente Coupa](https://success.coupa.com/Support/Contact_Us?) para obter este valor.

    b. No **identificador** caixa de texto, escreva o URL:

    | Ambiente  | do IdP |
    |:-------------|----|
    | Sandbox | `devsso35.coupahost.com`|
    | Produção | `prdsso40.coupahost.com`|
    | | |

    c. No **URL de resposta** caixa de texto, escreva o URL:

    | Ambiente | do IdP |
    |------------- |----|
    | Sandbox | `https://devsso35.coupahost.com/sp/ACS.saml2`|
    | Produção | `https://prdsso40.coupahost.com/sp/ACS.saml2`|
    | | |

4. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/coupa-tutorial/tutorial_coupa_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/coupa-tutorial/tutorial_general_400.png)

6. Inicie sessão site da sua empresa Coupa como administrador.

7. Aceda a **configuração \> controlo de segurança**.

   ![Controlos de segurança](./media/coupa-tutorial/ic791900.png "controlos de segurança")

8. No **inicie sessão com credenciais de Coupa** secção, execute os seguintes passos:

    ![Metadados Coupa SP](./media/coupa-tutorial/ic791901.png "Coupa SP metadados")

    a. Selecione **iniciar sessão utilizando SAML**.

    b. Clique em **procurar** para carregar os metadados transferido a partir do portal do Azure.

    c. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/coupa-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/coupa-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/coupa-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/coupa-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-coupa-test-user"></a>Criar um utilizador de teste Coupa

Para permitir que os utilizadores do Azure AD sessão Coupa, têm de ser aprovisionados para Coupa.  

* No caso de Coupa, o aprovisionamento é uma tarefa manual.

**Para configurar o aprovisionamento de utilizadores, execute os seguintes passos:**

1. Inicie sessão no seu **Coupa** site da empresa como administrador.

2. No menu na parte superior, clique em **configuração**e, em seguida, clique em **utilizadores**.

   ![Os utilizadores](./media/coupa-tutorial/ic791908.png "utilizadores")

3. Clique em **Criar**.

   ![Criar utilizadores](./media/coupa-tutorial/ic791909.png "criar utilizadores")

4. No **criar utilizador** secção, execute os seguintes passos:

   ![Detalhes de utilizador](./media/coupa-tutorial/ic791910.png "detalhes de utilizador")

   a. Tipo de **início de sessão**, **nome próprio**, **Apelido**, **ID de início de sessão único**, **E-Mail** atributos de um conta válida do Azure Active Directory que pretende aprovisionar num relacionados caixas de texto.

   b. Clique em **Criar**.

   >[!NOTE]
   >O marcador de posição de conta do Azure Active Directory irá receber um e-mail com uma ligação para confirmar a conta para ficar ativa.
   >

>[!NOTE]
>Pode utilizar quaisquer outras Coupa utilizador conta criação ferramentas ou APIs fornecidas pelo Coupa para aprovisionar contas de utilizador do AAD.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Coupa.

![Atribuir a função de utilizador][200]

**Para atribuir Britta Simon a Coupa, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201]

2. Na lista de aplicações, selecione **Coupa**.

    ![A ligação de Coupa na lista de aplicações](./media/coupa-tutorial/tutorial_coupa_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Coupa no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de Coupa.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/coupa-tutorial/tutorial_general_01.png
[2]: ./media/coupa-tutorial/tutorial_general_02.png
[3]: ./media/coupa-tutorial/tutorial_general_03.png
[4]: ./media/coupa-tutorial/tutorial_general_04.png

[100]: ./media/coupa-tutorial/tutorial_general_100.png

[200]: ./media/coupa-tutorial/tutorial_general_200.png
[201]: ./media/coupa-tutorial/tutorial_general_201.png
[202]: ./media/coupa-tutorial/tutorial_general_202.png
[203]: ./media/coupa-tutorial/tutorial_general_203.png
