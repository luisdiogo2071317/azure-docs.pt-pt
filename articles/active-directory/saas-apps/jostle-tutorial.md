---
title: 'Tutorial: Integração do Azure Active Directory com Jostle | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Jostle.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 9ca4ca1f-8f68-4225-81a6-1666b486d6a8
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 33b92ebf91b07c15b25f48b54da1ed6fcc89c5ff
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39052113"
---
# <a name="tutorial-azure-active-directory-integration-with-jostle"></a>Tutorial: Integração do Azure Active Directory com Jostle

Neste tutorial, saiba como integrar Jostle com o Azure Active Directory (Azure AD).

Integrar Jostle no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Jostle
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Jostle (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Jostle, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Jostle início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Jostle da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-jostle-from-the-gallery"></a>Adicionando Jostle da Galeria
Para configurar a integração do Jostle com o Azure AD, terá de adicionar Jostle a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Jostle a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]

3. Clique em **adicionar** na parte superior da janela.

    ![add_01](./media/jostle-tutorial/add_01.png)

4. Na caixa de pesquisa sob **adicionar uma aplicação** tipo **Jostle**.

    ![add_02](./media/jostle-tutorial/add_02.png)

5. No painel de resultados, selecione **Jostle**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/jostle-tutorial/tutorial_jostle_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Jostle com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Jostle a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Jostle deve ser estabelecido.

Jostle, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Jostle, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Jostle](#creating-a-jostle-test-user)**  - para ter um equivalente da Eduarda Almeida na Jostle que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Jostle.

**Para configurar o Azure AD início de sessão único com Jostle, execute os seguintes passos:**

1. No portal do Azure, sobre o **Jostle** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Configurar o início de sessão único](./media/jostle-tutorial/tutorial_jostle_samlbase.png)

3. Sobre o **Jostle domínio e URLs** secção, execute os seguintes passos:

    ![url_01](./media/jostle-tutorial/url_01.png)

    a. Na **URL de início de sessão** caixa de texto, introduza: `https://login-prod.jostle.us`

    b. Na **identificador** caixa de texto, introduza: `https://jostle.us`

    c. A caixa de verificação junto a **Mostrar definições de URL avançadas**

    d. Na **URL de resposta** caixa de texto, introduza: `https://login-prod.jostle.us/saml/SSO/alias/newjostle.us`

4. Sobre o **atributos de utilizador** secção, para o **identificador de utilizador** , insira: `user.userprincipalname`

    ![url_02](./media/jostle-tutorial/url_02.png)

5. Clique em **guardar** na parte superior da janela.

6. Aceda a **certificado de assinatura SAML** e certifique-se de que está definida como **Active Directory**. Em seguida, clique em **metadados XML** para transferir o ficheiro de metadados.

    ![url_03](./media/jostle-tutorial/url_03.png)

7. Para configurar o início de sessão único no lado do Jostle, terá de enviar o XML de metadados baixado para [equipa de suporte de Jostle](mailto:support@jostle.me). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
>

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/jostle-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.

    ![Criar um utilizador de teste do Azure AD](./media/jostle-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.

    ![Criar um utilizador de teste do Azure AD](./media/jostle-tutorial/create_aaduser_03.png)

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:

    ![Criar um utilizador de teste do Azure AD](./media/jostle-tutorial/create_aaduser_04.png)

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.

### <a name="creating-a-jostle-test-user"></a>Criar um utilizador de teste Jostle

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Jostle. Se não souber como adicionar Eduarda Almeida Jostle, entre em contato com [equipa de suporte de Jostle](mailto:support@jostle.me) para adicionar o utilizador de teste e ativar o SSO.

> [!NOTE]
> O titular da conta do Azure Active Directory recebe uma mensagem de e-mail e segue uma ligação para confirmar a respetiva conta até se tornar Active Directory.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Jostle.

![Atribuir utilizador][200]

**Para atribuir a Eduarda Almeida a Jostle, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **Jostle**.

    ![Configurar o início de sessão único](./media/jostle-tutorial/tutorial_jostle_app.png)

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Jostle no painel de acesso, deve obter automaticamente a página de início de sessão de aplicativo Jostle.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/jostle-tutorial/tutorial_general_01.png
[2]: ./media/jostle-tutorial/tutorial_general_02.png
[3]: ./media/jostle-tutorial/tutorial_general_03.png
[4]: ./media/jostle-tutorial/tutorial_general_04.png

[100]: ./media/jostle-tutorial/tutorial_general_100.png

[200]: ./media/jostle-tutorial/tutorial_general_200.png
[201]: ./media/jostle-tutorial/tutorial_general_201.png
[202]: ./media/jostle-tutorial/tutorial_general_202.png
[203]: ./media/jostle-tutorial/tutorial_general_203.png
