---
title: 'Tutorial: Integração do Azure Active Directory com moconavi | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e moconavi.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e1916224-e1c2-426f-b233-0a2518fa41db
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: jeedes
ms.openlocfilehash: 3467b823e6c91d34ebd48c7f8bc29558a79c59e5
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36229551"
---
# <a name="tutorial-azure-active-directory-integration-with-moconavi"></a>Tutorial: Integração do Azure Active Directory com moconavi

Neste tutorial, irá aprender a integrar moconavi com o Azure Active Directory (Azure AD).

Integrar moconavi com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao moconavi.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para moconavi (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com moconavi, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um moconavi-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar moconavi a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-moconavi-from-the-gallery"></a>Adicionar moconavi a partir da Galeria
Para configurar a integração de moconavi com o Azure AD, terá de adicionar moconavi a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar moconavi a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]

3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **moconavi**, selecione **moconavi** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![moconavi na lista de resultados](./media/moconavi-tutorial/tutorial_moconavi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com moconavi com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no moconavi é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no moconavi tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com moconavi, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste moconavi](#create-a-moconavi-test-user)**  - para ter um homólogo de Britta Simon no moconavi que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação moconavi.

**Para configurar o Azure AD-início de sessão único com moconavi, execute os seguintes passos:**

1. No portal do Azure, no **moconavi** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/moconavi-tutorial/tutorial_moconavi_samlbase.png)

3. No **moconavi domínios e URLs** secção, execute os seguintes passos:

    ![moconavi URLs e de domínio único início de sessão informações](./media/moconavi-tutorial/tutorial_moconavi_url.png)

    a. No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<yourserverurl>/moconavi-saml2/saml/login`

    b. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<yourserverurl>/moconavi-saml2`

    C. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<yourserverurl>/moconavi-saml2/saml/SSO`

    > [!NOTE]
    > Estes valores não estiverem reais. Atualize estes valores com o URL de início de sessão, identificador e o URL de resposta real. Contacte [equipa de suporte de cliente moconavi](mailto:support@recomot.co.jp) para obter estes valores.

4. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/moconavi-tutorial/tutorial_moconavi_certificate.png)

5. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/moconavi-tutorial/tutorial_general_400.png)

6. Para configurar o início de sessão único em **moconavi** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte de moconavi](mailto:support@recomot.co.jp). Se definir esta definição para que a ligação de SAML SSO corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/moconavi-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/moconavi-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/moconavi-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/moconavi-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-moconavi-test-user"></a>Criar um utilizador de teste moconavi

Nesta secção, vai criar um utilizador chamado Britta Simon moconavi. Trabalhar com [equipa de suporte de moconavi](mailto:support@recomot.co.jp) para adicionar os utilizadores na plataforma moconavi. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso aos moconavi.

![Atribuir a função de utilizador][200]

**Para atribuir Britta Simon a moconavi, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201]

2. Na lista de aplicações, selecione **moconavi**.

    ![A ligação de moconavi na lista de aplicações](./media/moconavi-tutorial/tutorial_moconavi_app.png)

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar início de sessão único

1. Instale moconavi a partir da loja de Microsoft.

2. Inicie moconavi.

3. Clique em **ligar definição** botão.

    ![Teste o início de sessão único](./media/moconavi-tutorial/testing1.png)

4. Introduza `https://mcs-admin.moconavi.biz/gateway` para **ligar ao URL** caixa de texto e, em seguida, clique em **feito** botão.

    ![Teste o início de sessão único](./media/moconavi-tutorial/testing2.png)

5. Na captura de ecrã seguinte, execute os seguintes passos:

    ![Teste o início de sessão único](./media/moconavi-tutorial/testing3.png)

    a. Introduza **chave de autenticação de entrada**:`azureAD` para **chave de autenticação de entrada** caixa de texto.

    b. Introduza **ID de utilizador de entrada**: `your ad account` para **ID de utilizador de entrada** caixa de texto.

    c. Clique em **início de sessão**.

6. Introduza a palavra-passe do Azure AD para **palavra-passe** caixa de texto e, em seguida, clique em **início de sessão** botão.

    ![Teste o início de sessão único](./media/moconavi-tutorial/testing4.png)

7. Azure AD autenticação é efetuada com êxito quando é apresentado o menu.

    ![Teste o início de sessão único](./media/moconavi-tutorial/testing5.png)

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/moconavi-tutorial/tutorial_general_01.png
[2]: ./media/moconavi-tutorial/tutorial_general_02.png
[3]: ./media/moconavi-tutorial/tutorial_general_03.png
[4]: ./media/moconavi-tutorial/tutorial_general_04.png

[100]: ./media/moconavi-tutorial/tutorial_general_100.png

[200]: ./media/moconavi-tutorial/tutorial_general_200.png
[201]: ./media/moconavi-tutorial/tutorial_general_201.png
[202]: ./media/moconavi-tutorial/tutorial_general_202.png
[203]: ./media/moconavi-tutorial/tutorial_general_203.png

