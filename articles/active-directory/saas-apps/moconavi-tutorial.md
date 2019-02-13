---
title: 'Tutorial: Integração do Active Directory do Azure com moconavi | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e moconavi.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e1916224-e1c2-426f-b233-0a2518fa41db
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 76d1621834c5096743f6d37dad94754ec8b259a4
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56196251"
---
# <a name="tutorial-azure-active-directory-integration-with-moconavi"></a>Tutorial: Integração do Active Directory do Azure com moconavi

Neste tutorial, saiba como integrar moconavi com o Azure Active Directory (Azure AD).

Integrar moconavi no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao moconavi.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para moconavi (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com moconavi, terá dos seguintes itens:

- Uma subscrição do Azure
- Um moconavi logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando moconavi da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-moconavi-from-the-gallery"></a>Adicionando moconavi da Galeria
Para configurar a integração do moconavi com o Azure AD, terá de adicionar moconavi a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar moconavi a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **moconavi**, selecione **moconavi** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![moconavi na lista de resultados](./media/moconavi-tutorial/tutorial_moconavi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com moconavi com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no moconavi a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no moconavi deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com moconavi, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste moconavi](#create-a-moconavi-test-user)**  - para ter um equivalente da Eduarda Almeida na moconavi que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo moconavi.

**Para configurar o Azure AD início de sessão único com moconavi, execute os seguintes passos:**

1. No portal do Azure, sobre o **moconavi** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/moconavi-tutorial/tutorial_moconavi_samlbase.png)

3. Sobre o **moconavi domínio e URLs** secção, execute os seguintes passos:

    ![moconavi domínio e URLs únicas início de sessão em informações](./media/moconavi-tutorial/tutorial_moconavi_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<yourserverurl>/moconavi-saml2/saml/login`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<yourserverurl>/moconavi-saml2`

    C. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<yourserverurl>/moconavi-saml2/saml/SSO`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão, identificador e o URL de resposta real. Contacte [equipa de suporte de cliente moconavi](mailto:support@recomot.co.jp) obter esses valores.

4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/moconavi-tutorial/tutorial_moconavi_certificate.png)

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/moconavi-tutorial/tutorial_general_400.png)

6. Para configurar o início de sessão único num **moconavi** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte de moconavi](mailto:support@recomot.co.jp). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/moconavi-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/moconavi-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/moconavi-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/moconavi-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-moconavi-test-user"></a>Criar um utilizador de teste moconavi

Nesta secção, vai criar um usuário chamado Eduarda Almeida no moconavi. Trabalhar com [equipa de suporte de moconavi](mailto:support@recomot.co.jp) para adicionar os utilizadores na plataforma moconavi. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso a moconavi.

![Atribuir a função de utilizador][200]

**Para atribuir a Eduarda Almeida a moconavi, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **moconavi**.

    ![A ligação de moconavi na lista de aplicações](./media/moconavi-tutorial/tutorial_moconavi_app.png)

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar o início de sessão único

1. Instale moconavi a partir da loja Microsoft.

2. Inicie moconavi.

3. Clique em **ligue-se a definição** botão.

    ![Teste de início de sessão único](./media/moconavi-tutorial/testing1.png)

4. Introduza `https://mcs-admin.moconavi.biz/gateway` numa **ligar ao URL** caixa de texto e, em seguida, clique em **feito** botão.

    ![Teste de início de sessão único](./media/moconavi-tutorial/testing2.png)

5. Na captura de ecrã seguinte, execute os seguintes passos:

    ![Teste de início de sessão único](./media/moconavi-tutorial/testing3.png)

    a. Introduza **chave de autenticação de entrada**:`azureAD` em **chave de autenticação de entrada** caixa de texto.

    b. Introduza **ID de utilizador de entrada**: `your ad account` em **ID de utilizador de entrada** caixa de texto.

    c. Clique em **início de sessão**.

6. Introduza a palavra-passe do Azure AD para **palavra-passe** caixa de texto e, em seguida, clique em **início de sessão** botão.

    ![Teste de início de sessão único](./media/moconavi-tutorial/testing4.png)

7. Autenticação do Azure AD é efetuada com êxito quando é apresentado o menu.

    ![Teste de início de sessão único](./media/moconavi-tutorial/testing5.png)

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

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

