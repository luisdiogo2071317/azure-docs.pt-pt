---
title: 'Tutorial: Integração do Azure Active Directory com Coupa | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Coupa.
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
ms.openlocfilehash: 4bf40f76f5a8f788305b4dc9f91523f53fb59acf
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39448089"
---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a>Tutorial: Integração do Azure Active Directory com Coupa

Neste tutorial, saiba como integrar Coupa com o Azure Active Directory (Azure AD).

Integrar Coupa no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Coupa.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Coupa (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Coupa, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Coupa logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Coupa da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-coupa-from-the-gallery"></a>Adicionando Coupa da Galeria
Para configurar a integração do Coupa com o Azure AD, terá de adicionar Coupa a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Coupa a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Coupa**, selecione **Coupa** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Coupa na lista de resultados](./media/coupa-tutorial/tutorial_coupa_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Coupa com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Coupa a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Coupa deve ser estabelecido.

Coupa, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Coupa, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Coupa](#create-a-coupa-test-user)**  - para ter um equivalente da Eduarda Almeida na Coupa que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Coupa.

**Para configurar o Azure AD início de sessão único com Coupa, execute os seguintes passos:**

1. No portal do Azure, sobre o **Coupa** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/coupa-tutorial/tutorial_coupa_samlbase.png)

1. Sobre o **Coupa domínio e URLs** secção, execute os seguintes passos:

    ![Coupa domínio e URLs únicas início de sessão em informações](./media/coupa-tutorial/tutorial_coupa_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.coupahost.com`

    > [!NOTE]
    > O valor de URL de início de sessão não é real. Atualize este valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente Coupa](https://success.coupa.com/Support/Contact_Us?) para obter este valor.

    b. Na **identificador** caixa de texto, escreva o URL:

    | Ambiente  | do IdP |
    |:-------------|----|
    | Sandbox | `devsso35.coupahost.com`|
    | Produção | `prdsso40.coupahost.com`|
    | | |

    c. Na **URL de resposta** caixa de texto, escreva o URL:

    | Ambiente | do IdP |
    |------------- |----|
    | Sandbox | `https://devsso35.coupahost.com/sp/ACS.saml2`|
    | Produção | `https://prdsso40.coupahost.com/sp/ACS.saml2`|
    | | |

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/coupa-tutorial/tutorial_coupa_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/coupa-tutorial/tutorial_general_400.png)

1. Inicie sessão site da sua empresa Coupa como um administrador.

1. Aceda a **programa de configuração \> controlo de segurança**.

   ![Controlos de segurança](./media/coupa-tutorial/ic791900.png "controlos de segurança")

1. Na **inicie sessão com credenciais de Coupa** secção, execute os seguintes passos:

    ![Metadados de Coupa SP](./media/coupa-tutorial/ic791901.png "Coupa SP metadados")

    a. Selecione **iniciar sessão com o SAML**.

    b. Clique em **procurar** para carregar os metadados transferido a partir do portal do Azure.

    c. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/coupa-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/coupa-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/coupa-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/coupa-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-coupa-test-user"></a>Criar um utilizador de teste Coupa

Para habilitar logon Coupa de utilizadores do Azure AD, tem de ser aprovisionados em Coupa.  

* No caso de Coupa, o aprovisionamento é uma tarefa manual.

**Para configurar o aprovisionamento de utilizadores, execute os seguintes passos:**

1. Inicie sessão no seu **Coupa** site da empresa como administrador.

1. No menu na parte superior, clique em **programa de configuração**e, em seguida, clique em **utilizadores**.

   ![Os utilizadores](./media/coupa-tutorial/ic791908.png "utilizadores")

1. Clique em **Criar**.

   ![Criar usuários](./media/coupa-tutorial/ic791909.png "criar utilizadores")

1. Na **criar utilizador** secção, execute os seguintes passos:

   ![Detalhes de utilizador](./media/coupa-tutorial/ic791910.png "detalhes de utilizador")

   a. Tipo de **início de sessão**, **nome próprio**, **Apelido**, **ID de início de sessão único**, **E-Mail** atributos de um conta válida do Azure Active Directory que pretende aprovisionar em caixas de texto relacionadas.

   b. Clique em **Criar**.

   >[!NOTE]
   >O titular da conta do Azure Active Directory irá receber um e-mail com uma ligação para confirmar a conta até se tornar Active Directory.
   >

>[!NOTE]
>Pode utilizar quaisquer outras Coupa utilizador conta criação ferramentas ou APIs fornecidas pelo Coupa para aprovisionar contas de utilizador do AAD.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Coupa.

![Atribuir a função de utilizador][200]

**Para atribuir a Eduarda Almeida a Coupa, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

1. Na lista de aplicações, selecione **Coupa**.

    ![A ligação de Coupa na lista de aplicações](./media/coupa-tutorial/tutorial_coupa_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Coupa no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Coupa.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

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
