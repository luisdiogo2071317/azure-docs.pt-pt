---
title: 'Tutorial: LOB aplicação ativada para integração do Azure Active Directory com o SAML 1.1 Token | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e SAML 1.1 Token de aplicação LOB ativada.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ced1d88d-0e48-40d5-9aea-ef991cd9d270
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: fca447b24a299fed116356ca0f985020e079344b
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317607"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-11-token-enabled-lob-app"></a>Tutorial: LOB aplicação ativada para integração do Azure Active Directory com o SAML 1.1 Token

Neste tutorial, irá aprender a integrar SAML 1.1 Token LOB aplicação ativada com o Azure Active Directory (Azure AD).

Integrar SAML 1.1 Token LOB aplicação ativada com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao SAML 1.1 Token ativada aplicação LOB.
- Pode permitir aos utilizadores obter automaticamente iniciada no SAML 1.1 Token aplicação LOB (Single Sign-On) ativada com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar o Azure LOB aplicação ativada para a integração do AD com o SAML 1.1 Token, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Token SAML 1.1 ativada aplicação LOB único início de sessão ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar SAML 1.1 Token com a aplicação de LOB na galeria do
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-saml-11-token-enabled-lob-app-from-the-gallery"></a>Adicionar SAML 1.1 Token com a aplicação de LOB na galeria do
Para configurar a integração de SAML 1.1 Token LOB aplicação ativada com o Azure AD, tem de adicionar que SAML 1.1 Token ativada LOB aplicação na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar que SAML 1.1 Token de aplicação LOB na galeria do ativada, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **SAML 1.1 Token de aplicação LOB ativada**, selecione **SAML 1.1 Token de aplicação LOB ativada** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![SAML 1.1 Token LOB aplicação ativada na lista de resultados](./media/saml-tutorial/tutorial_saml_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com o SAML 1.1 Token aplicação LOB ativada com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber que o utilizador homólogo no SAML 1.1 Token aplicação LOB ativada é um utilizador no Azure AD. Por outras palavras, uma relação entre um utilizador do Azure AD e o utilizador relacionado no SAML 1.1 Token de ligação ativar aplicação LOB tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com o SAML 1.1 Token de aplicação de LOB ativada, tem de concluir os seguintes blocos de criação:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um SAML 1.1 Token ativada utilizador de teste de aplicação LOB](#create-a-saml-11-token-enabled-lob-app-test-user)**  - para ter um homólogo de Britta Simon no SAML 1.1 Token ativada aplicações LOB que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único no seu Token SAML 1.1 ativar aplicações de LOB aplicação.

**Para configurar o Azure AD-início de sessão único com o SAML 1.1 Token LOB aplicação ativada, execute os seguintes passos:**

1. No portal do Azure, no **SAML 1.1 Token de aplicação LOB ativada** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/saml-tutorial/tutorial_saml_samlbase.png)

3. No **ativada SAML 1.1 Token domínio de aplicação LOB e URLs** secção, execute os seguintes passos:

    ![Domínio de aplicação LOB e URLs único início de sessão informações de ativado SAML 1.1 Token](./media/saml-tutorial/tutorial_saml_url.png)

    a. No **iniciar sessão no URL** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://your-app-url`

    b. No **identificador (ID de entidade)** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://your-app-url`
     
    > [!NOTE] 
    > Estes valores não estiverem reais. Substitua estes valores com urls específicos de aplicação.  

5. No **certificado de assinatura de SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/saml-tutorial/tutorial_saml_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/saml-tutorial/tutorial_general_400.png)
    
7. No **SAML 1.1 Token ativada a configuração de aplicação LOB** secção, clique em **configurar SAML 1.1 Token de aplicação LOB ativada** para abrir **configurar início de sessão** janela. Copiar o **Sign-Out URL, o ID de entidade de SAML e o único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configuração de aplicação LOB de ativado SAML 1.1 Token](./media/saml-tutorial/tutorial_saml_configure.png) 

8. Para configurar o início de sessão único em **SAML 1.1 Token de aplicação LOB ativada** lado, terá de enviar o transferido **certificado (Base64), Sign-Out URL, ID de entidade de SAML e único início de sessão no URL do serviço SAML** para equipa de suporte da aplicação. Se definir esta definição para que a ligação de SAML SSO corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/saml-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/saml-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/saml-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/saml-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-saml-11-token-enabled-lob-app-test-user"></a>Criar um SAML 1.1 Token ativada utilizador de teste de aplicação LOB

Nesta secção, vai criar um utilizador chamado Britta Simon no SAML 1.1 Token de aplicação LOB ativada. Trabalhar com a aplicação suportar equipa para criar o utilizador no lado de aplicação. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para SAML 1.1 Token de aplicação de LOB ativada.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon SAML 1.1 Token ativadas aplicações LOB, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **SAML 1.1 Token de aplicação LOB ativada**.

    ![O Token SAML 1.1 ativada ligação da aplicação de LOB na lista de aplicações](./media/saml-tutorial/tutorial_saml_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Ao clicar no Token SAML 1.1 ativada mosaico de aplicação LOB no painel de acesso, deve obter automaticamente iniciada no seu Token SAML 1.1 ativar aplicações de LOB aplicação.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/saml-tutorial/tutorial_general_01.png
[2]: ./media/saml-tutorial/tutorial_general_02.png
[3]: ./media/saml-tutorial/tutorial_general_03.png
[4]: ./media/saml-tutorial/tutorial_general_04.png

[100]: ./media/saml-tutorial/tutorial_general_100.png

[200]: ./media/saml-tutorial/tutorial_general_200.png
[201]: ./media/saml-tutorial/tutorial_general_201.png
[202]: ./media/saml-tutorial/tutorial_general_202.png
[203]: ./media/saml-tutorial/tutorial_general_203.png
