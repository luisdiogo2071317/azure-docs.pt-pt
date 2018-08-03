---
title: 'Tutorial: Integração do Azure Active Directory com OpenAthens | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e OpenAthens.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: dd4adfc7-e238-41d5-8b25-1811f08078b6
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2017
ms.author: jeedes
ms.openlocfilehash: 269b216a94b1233c5f9f9a634fda3c05e46cac90
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435911"
---
# <a name="tutorial-azure-active-directory-integration-with-openathens"></a>Tutorial: Integração do Azure Active Directory com OpenAthens

Neste tutorial, saiba como integrar OpenAthens com o Azure Active Directory (Azure AD).

Integrar OpenAthens no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao OpenAthens.
- Pode permitir aos utilizadores iniciar sessão automaticamente OpenAthens (início de sessão único) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central, o portal do Azure.

Para obter mais informações sobre a integração de aplicações SaaS com o Azure AD, consulte [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com OpenAthens, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um OpenAthens logon único habilitado subscrição

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação gratuita de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando OpenAthens da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-openathens-from-the-gallery"></a>Adicionando OpenAthens da Galeria
Para configurar a integração do OpenAthens com o Azure AD, terá de adicionar OpenAthens a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar OpenAthens a partir da Galeria**

1. Na [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione a **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue até **aplicações empresariais**e, em seguida, aceda à **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, selecione o **nova aplicação** botão na parte superior da caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **OpenAthens**, selecione **OpenAthens** no painel de resultados e, em seguida, selecione o **Add** botão.

    ![OpenAthens na lista de resultados](./media/openathens-tutorial/tutorial_openathens_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com OpenAthens com base num utilizador de teste com o nome "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no OpenAthens ao utilizador no Azure AD. Em outras palavras, tem de estabelecer uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no OpenAthens.

OpenAthens, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com OpenAthens, tem de concluir os seguintes blocos de construção:

1. [Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on), para permitir aos utilizadores utilizar esta funcionalidade.
1. [Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user), para testar o Azure AD início de sessão único com Eduarda Almeida.
1. [Criar um utilizador de teste OpenAthens](#create-a-openathens-test-user), ter um equivalente da Eduarda Almeida na OpenAthens que está ligado a representação do Azure AD do utilizador.
1. [Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user), para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. [Testar início de sessão único](#test-single-sign-on), para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo OpenAthens.

**Para configurar o Azure AD início de sessão único com OpenAthens**

1. No portal do Azure, sobre o **OpenAthens** página de integração de aplicações, selecione **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Para ativar o início de sessão único, na **início de sessão único** caixa de diálogo, selecione **baseado em SAML início de sessão** como o **modo**.
 
    ![Caixa de diálogo de início de sessão único](./media/openathens-tutorial/tutorial_openathens_samlbase.png)

1. Na **OpenAthens domínio e URLs** , digite o valor `https://login.openathens.net/saml/2/metadata-sp` no **identificador** caixa de texto.

    ![Informações de início de sessão de único OpenAthens domínio e URLs](./media/openathens-tutorial/tutorial_openathens_url.png)

1. Na **certificado de assinatura SAML** secção, selecione **XML de metadados**e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O certificado de assinatura de AMSL ligação de transferência](./media/openathens-tutorial/tutorial_openathens_certificate.png) 

1. Selecione o botão **Guardar**.

    ![O início de sessão único botão Save](./media/openathens-tutorial/tutorial_general_400.png)

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa OpenAthens como administrador.

1. Selecione **conexões** da lista sob a **gestão** separador. 

    ![Configurar o início de sessão único](./media/openathens-tutorial/tutorial_openathens_application1.png)

1. Selecione **SAML 1.1/2.0**e, em seguida, selecione a **configurar** botão.

    ![Configurar o início de sessão único](./media/openathens-tutorial/tutorial_openathens_application2.png)
    
1. Para adicionar a configuração, selecione o **navegue** para carregar o ficheiro. XML de metadados que transferiu a partir do portal do Azure e, em seguida, selecione **Add**.

    ![Configurar o início de sessão único](./media/openathens-tutorial/tutorial_openathens_application3.png)

1. Execute os seguintes passos sob o **detalhes** separador.

    ![Configurar o início de sessão único](./media/openathens-tutorial/tutorial_openathens_application4.png)

    a. Na **mapeamento de nome de exibição**, selecione **utilizar atributo**.

    b. Na **atributo de nome de exibição** texto, digite o valor `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    c. Na **mapeamento de utilizador exclusivo**, selecione **utilizar atributo**.

    d. Na **atributo de utilizador exclusivo** texto, digite o valor `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    e. Na **estado**, selecione todas as três caixas de verificação.

    f. Na **criar contas locais**, selecione **automaticamente**.

    g. Selecione **guardar alterações**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação. Depois de adicionar esta aplicação a partir da **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador e acessar o embedded documentação através da **configuração** seção na parte inferior. Para obter mais informações sobre a funcionalidade de documentação do embedded, veja a [documentação do embedded do Azure AD](https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado "Eduarda Almeida."

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD**

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**.

    ![O botão do Azure Active Directory](./media/openathens-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, selecione **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/openathens-tutorial/create_aaduser_02.png)

1. Para abrir o **utilizador** caixa de diálogo, selecione **Add** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/openathens-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/openathens-tutorial/create_aaduser_04.png)

    a. Na **Name** caixa de texto, escreva **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, escreva o endereço de e-mail para Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa de texto.

    d. Selecione **Criar**.
  
### <a name="create-an-openathens-test-user"></a>Criar um utilizador de teste OpenAthens

OpenAthens suporta o aprovisionamento de just-in-time e os utilizadores são criados automaticamente após a autenticação com êxito. Não precisa de realizar qualquer ação indicadas nesta secção.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para OpenAthens.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a OpenAthens**

1. No portal do Azure, abra os aplicativos ver, navegue para a vista de diretório e aceda a **aplicações empresariais**e, em seguida, selecione **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na **aplicativos** lista, selecione **OpenAthens**.

    ![A ligação de OpenAthens na lista de aplicações](./media/openathens-tutorial/tutorial_openathens_app.png)  

1. No menu à esquerda, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Selecione o **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** painel.

    ![O painel Adicionar atribuição][203]

1. Na **utilizadores e grupos** lista, selecione **Eduarda Almeida**.

1. Selecione o **selecionar** botão no **utilizadores e grupos** lista.

1. Selecione o **atribua** botão no **adicionar atribuição** painel.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar seu única início de sessão em configuração do Azure AD utilizando o painel de acesso.

Quando seleciona a **OpenAthens** mosaico no painel de acesso, deve ser automaticamente a sessão iniciada sua aplicação OpenAthens.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* Para obter uma lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory, consulte [tutoriais de integração de aplicações de SaaS para utilização com o Azure AD](tutorial-list.md).
* Para obter mais informações sobre o acesso a aplicações e início de sessão único com o Azure Active Directory, consulte [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

<!--Image references-->

[1]: ./media/openathens-tutorial/tutorial_general_01.png
[2]: ./media/openathens-tutorial/tutorial_general_02.png
[3]: ./media/openathens-tutorial/tutorial_general_03.png
[4]: ./media/openathens-tutorial/tutorial_general_04.png

[100]: ./media/openathens-tutorial/tutorial_general_100.png

[200]: ./media/openathens-tutorial/tutorial_general_200.png
[201]: ./media/openathens-tutorial/tutorial_general_201.png
[202]: ./media/openathens-tutorial/tutorial_general_202.png
[203]: ./media/openathens-tutorial/tutorial_general_203.png

