---
title: 'Tutorial: Integração do Active Directory do Azure com Nimblex | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Nimblex.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d3e165a5-f062-4b50-ac0b-b400838e99cd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d7801b5ea73cf94439ae2974f91d2032f9bf8a3b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56166747"
---
# <a name="tutorial-azure-active-directory-integration-with-nimblex"></a>Tutorial: Integração do Active Directory do Azure com Nimblex

Neste tutorial, saiba como integrar Nimblex com o Azure Active Directory (Azure AD).

Integrar Nimblex no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Nimblex.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Nimblex (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Nimblex, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Nimblex logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Nimblex da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-nimblex-from-the-gallery"></a>Adicionando Nimblex da Galeria
Para configurar a integração do Nimblex com o Azure AD, terá de adicionar Nimblex a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Nimblex a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Nimblex**, selecione **Nimblex** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Nimblex na lista de resultados](./media/nimblex-tutorial/tutorial_nimblex_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Nimblex com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Nimblex a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Nimblex deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Nimblex, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Nimblex](#create-a-nimblex-test-user)**  - para ter um equivalente da Eduarda Almeida na Nimblex que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Nimblex.

**Para configurar o Azure AD início de sessão único com Nimblex, execute os seguintes passos:**

1. No portal do Azure, sobre o **Nimblex** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/nimblex-tutorial/tutorial_nimblex_samlbase.png)

3. Sobre o **Nimblex domínio e URLs** secção, execute os seguintes passos:

    ![Nimblex domínio e URLs únicas início de sessão em informações](./media/nimblex-tutorial/tutorial_nimblex_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<YOUR APPLICATION PATH>/Login.aspx`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<YOUR APPLICATION PATH>/`

    c. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<path-to-application>/SamlReply.aspx`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão, identificador e o URL de resposta real. Contacte [equipa de suporte de cliente Nimblex](mailto:support@ebms.com.au) obter esses valores.

4. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/nimblex-tutorial/tutorial_nimblex_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/nimblex-tutorial/tutorial_general_400.png)

6. Sobre o **Nimblex configuração** secção, clique em **configurar Nimblex** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de Nimblex](./media/nimblex-tutorial/tutorial_nimblex_configure.png) 

7. Numa janela do browser web diferente, inicie sessão no Nimblex como um administrador de segurança.

9. No canto superior direito da página, clique em **definições** logótipo.

    ![Definições de Nimblex](./media/nimblex-tutorial/tutorial_nimblex_settings.png)

10. Sobre o **painel de controlo** página, em **segurança** secção clique **início de sessão único**.

    ![Definições de Nimblex](./media/nimblex-tutorial/tutorial_nimblex_single.png)

11. Sobre o **gerir o início de sessão único** página, selecione o nome de instância e clique em **editar**.

    ![Nimblex saml](./media/nimblex-tutorial/tutorial_nimblex_saml.png)

12. Sobre o **Editar fornecedor de SSO** página, execute os seguintes passos:

    ![Nimblex saml](./media/nimblex-tutorial/tutorial_nimblex_sso.png)

    a. Na **Descrição** caixa de texto, escreva o seu nome de instância.

    b. No bloco de notas, abra o certificado com codificação base 64 que transferiu a partir do portal do Azure, copie o seu conteúdo e, em seguida, cole-o para o **certificado** caixa.

    c. Na **Url de destino de Sso de fornecedor de identidade** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure.

    d. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/nimblex-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/nimblex-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/nimblex-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/nimblex-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-nimblex-test-user"></a>Criar um utilizador de teste Nimblex

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Nimblex. Nimblex suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder Nimblex se não existir ainda.

>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de cliente Nimblex](mailto:support@ebms.com.au).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Nimblex.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Nimblex, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Nimblex**.

    ![A ligação de Nimblex na lista de aplicações](./media/nimblex-tutorial/tutorial_nimblex_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Nimblex no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Nimblex.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/nimblex-tutorial/tutorial_general_01.png
[2]: ./media/nimblex-tutorial/tutorial_general_02.png
[3]: ./media/nimblex-tutorial/tutorial_general_03.png
[4]: ./media/nimblex-tutorial/tutorial_general_04.png

[100]: ./media/nimblex-tutorial/tutorial_general_100.png

[200]: ./media/nimblex-tutorial/tutorial_general_200.png
[201]: ./media/nimblex-tutorial/tutorial_general_201.png
[202]: ./media/nimblex-tutorial/tutorial_general_202.png
[203]: ./media/nimblex-tutorial/tutorial_general_203.png

