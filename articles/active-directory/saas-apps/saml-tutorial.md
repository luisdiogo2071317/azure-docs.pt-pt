---
title: 'Tutorial: Integração do Azure Active Directory com o SAML 1.1 Token de aplicação LOB ativada | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e SAML 1.1 Token de aplicação LOB para ativado.
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
ms.openlocfilehash: dc07dce24152f9f58253ad96c80f6b004cd1198b
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39050788"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-11-token-enabled-lob-app"></a>Tutorial: Integração do Azure Active Directory com o SAML 1.1 Token de aplicação LOB para ativado

Neste tutorial, saiba como integrar SAML 1.1 Token LOB aplicação ativada com o Azure Active Directory (Azure AD).

Integração de SAML 1.1 Token LOB aplicação ativada com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Token do SAML 1.1 habilitado aplicação LOB.
- Pode permitir que os utilizadores recebem automaticamente com sessão iniciada para SAML 1.1 Token aplicação LOB do departamento (Single Sign-On) ativada com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar o Azure aplicação LOB para a integração do AD com o SAML 1.1 Token ativada, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Token SAML 1.1 habilitado aplicação LOB para início de sessão ativada subscrição individual

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Token do SAML 1.1 ativada uma aplicação LOB a partir da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-saml-11-token-enabled-lob-app-from-the-gallery"></a>Adicionar Token do SAML 1.1 ativada uma aplicação LOB a partir da Galeria
Para configurar a integração de SAML 1.1 Token ativada aplicação LOB para o Azure AD, tem de adicionar que SAML 1.1 Token de aplicação LOB a partir da Galeria sua lista de aplicações de SaaS geridas ativada.

**Para adicionar o que token do SAML 1.1 habilitado aplicação LOB a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **SAML 1.1 Token de aplicação LOB ativada**, selecione **SAML 1.1 Token de aplicação LOB para ativado** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![SAML 1.1 Token aplicação LOB ativada na lista de resultados](./media/saml-tutorial/tutorial_saml_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o SAML 1.1 Token aplicação LOB ativada com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa de saber o que o utilizador de contraparte no SAML 1.1 Token for de aplicação LOB do departamento de ativada para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no SAML 1.1 Token ativada necessidades de aplicação LOB seja estabelecida.

Para configurar e testar o Azure AD início de sessão único com o SAML 1.1 Token ativado aplicação LOB, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um SAML 1.1 Token ativada utilizador de teste de aplicação LOB](#create-a-saml-11-token-enabled-lob-app-test-user)**  - para ter um equivalente da Eduarda Almeida no SAML 1.1 Token ativada aplicação LOB que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, ativar o Azure AD início de sessão único no portal do Azure e configurar o início de sessão único no seu Token SAML 1.1 aplicação LOB aplicação com suporte.

**Para configurar o Azure AD início de sessão único com o SAML 1.1 Token ativado aplicação LOB, execute os seguintes passos:**

1. No portal do Azure, sobre o **SAML 1.1 Token de aplicação LOB ativada** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/saml-tutorial/tutorial_saml_samlbase.png)

3. Sobre o **ativada SAML 1.1 Token URLs de domínio de aplicativo de LOB e** secção, execute os seguintes passos:

    ![SAML 1.1 Token ativada URLs de LOB do departamento de domínio de aplicativo e únicas início de sessão em informações](./media/saml-tutorial/tutorial_saml_url.png)

    a. Na **iniciar sessão no URL** caixa de texto, escreva um URL com o seguinte padrão: `https://your-app-url`

    b. Na **identificador (ID de entidade)** caixa de texto, escreva um URL com o seguinte padrão: `https://your-app-url`
     
    > [!NOTE] 
    > Estes valores não são reais. Substitua estes valores com urls específicos da aplicação.  

5. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/saml-tutorial/tutorial_saml_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/saml-tutorial/tutorial_general_400.png)
    
7. Na **SAML 1.1 Token ativada a configuração de aplicações de LOB** secção, clique em **configurar SAML 1.1 Token de aplicação LOB para ativado** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![SAML 1.1 Token ativada a configuração de aplicações LOB](./media/saml-tutorial/tutorial_saml_configure.png) 

8. Para configurar o início de sessão único num **SAML 1.1 Token de aplicação LOB ativada** lado, terá de enviar o transferido **certificado (Base64), o URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** para equipa de suporte do aplicativo. Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/saml-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/saml-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/saml-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/saml-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-saml-11-token-enabled-lob-app-test-user"></a>Criar um SAML 1.1 Token ativada utilizador de teste de aplicação LOB

Nesta secção, vai criar um usuário chamado Eduarda Almeida no SAML 1.1 Token de aplicação LOB para ativado. Trabalho com a aplicação suporta a equipe para criar o utilizador no lado do aplicativo. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar a Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para SAML 1.1 Token de aplicação LOB para ativado.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida para SAML 1.1 Token ativado aplicação LOB, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **SAML 1.1 Token de aplicação LOB ativada**.

    ![O Token SAML 1.1 habilitado a ligação de aplicação LOB na lista de aplicações](./media/saml-tutorial/tutorial_saml_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no Token SAML 1.1 ativado mosaico de aplicação LOB no painel de acesso, deve obter automaticamente com sessão iniciada para um aplicativo de aplicação LOB de seu Token SAML 1.1 habilitado.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



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
