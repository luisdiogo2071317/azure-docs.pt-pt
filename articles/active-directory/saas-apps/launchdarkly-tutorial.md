---
title: 'Tutorial: Integração do Azure Active Directory com LaunchDarkly | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e LaunchDarkly.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0e9cb37e-16bf-493b-bfc8-8d9840545a1e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: 1692b10074666be30e37da655220d9586ad366df
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/18/2018
ms.locfileid: "35947622"
---
# <a name="tutorial-azure-active-directory-integration-with-launchdarkly"></a>Tutorial: Integração do Azure Active Directory com LaunchDarkly

Neste tutorial, irá aprender a integrar LaunchDarkly com o Azure Active Directory (Azure AD).

Integrar LaunchDarkly com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao LaunchDarkly.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para LaunchDarkly (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com LaunchDarkly, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um LaunchDarkly-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar LaunchDarkly a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-launchdarkly-from-the-gallery"></a>Adicionar LaunchDarkly a partir da Galeria
Para configurar a integração de LaunchDarkly com o Azure AD, terá de adicionar LaunchDarkly a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar LaunchDarkly a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **LaunchDarkly**, selecione **LaunchDarkly** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![LaunchDarkly na lista de resultados](./media/launchdarkly-tutorial/tutorial_launchdarkly_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com LaunchDarkly com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no LaunchDarkly é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no LaunchDarkly tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com LaunchDarkly, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste LaunchDarkly](#create-a-launchdarkly-test-user)**  - para ter um homólogo de Britta Simon LaunchDarkly que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação LaunchDarkly.

**Para configurar o Azure AD-início de sessão único com LaunchDarkly, execute os seguintes passos:**

1. No portal do Azure, no **LaunchDarkly** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/launchdarkly-tutorial/tutorial_launchdarkly_samlbase.png)

3. No **LaunchDarkly domínio e os URLs** secção, execute os seguintes passos, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Domínio LaunchDarkly e os URLs únicos de informações de início de sessão](./media/launchdarkly-tutorial/tutorial_launchdarkly_url.png)

    a. No **identificador (ID de entidade)** caixa de texto, escreva o URL: `app.launchdarkly.com`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://app.launchdarkly.com/trust/saml2/acs/<customers-unique-id>`
    
    > [!NOTE]
    > O valor do URL de resposta não é real. Atualizar o valor com o URL de resposta real, que é explicada mais tarde no tutorial.

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![Domínio LaunchDarkly e os URLs únicos de informações de início de sessão](./media/launchdarkly-tutorial/tutorial_launchdarkly_url1.png)

    No **URL de início de sessão** caixa de texto, escreva o URL: `https://app.launchdarkly.com`

5. No **certificado de assinatura de SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/launchdarkly-tutorial/tutorial_launchdarkly_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/launchdarkly-tutorial/tutorial_general_400.png)
    
7. No **LaunchDarkly configuração** secção, clique em **configurar LaunchDarkly** para abrir **configurar início de sessão** janela. Copiar o **URL Single Sign-On serviço** do **secção de referência rápida.**

    ![Configuração de LaunchDarkly](./media/launchdarkly-tutorial/tutorial_launchdarkly_configure.png)

8. Numa janela do browser web diferente, inicie sessão no site da sua empresa LaunchDarkly como administrador.

9. Selecione **as definições da conta** partir do painel de navegação esquerdo.

    ![Configuração de LaunchDarkly](./media/launchdarkly-tutorial/configure1.png)

10. Clique em **segurança** separador.

    ![Configuração de LaunchDarkly](./media/launchdarkly-tutorial/configure2.png)

11. Clique em **ATIVAR SSO** e, em seguida, **Editar configuração de SAML**.

    ![Configuração de LaunchDarkly](./media/launchdarkly-tutorial/configure3.png)

12. No **editar a configuração SAML** secção, execute os seguintes passos:

    ![Configuração de LaunchDarkly](./media/launchdarkly-tutorial/configure4.png)

    a. Copiar o **URL do serviço SAML consumidor** para a instância e colá-lo na caixa de texto do URL de resposta no **LaunchDarkly domínio e os URLs** secção no portal do Azure.

    b. No **URL de início de sessão** caixa de texto, cole o **URL Single Sign-On serviço** valor que copiou do portal do Azure.

    c. Abrir o certificado transferido a partir do portal do Azure no bloco de notas, copie o conteúdo e, em seguida, cole-o para o **certificado x. 509** caixa ou diretamente pode carregar o certificado clicando a **carregar uma**.

    d. Clicar em **Guardar**

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/launchdarkly-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/launchdarkly-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/launchdarkly-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/launchdarkly-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-launchdarkly-test-user"></a>Criar um utilizador de teste LaunchDarkly

O objetivo desta secção consiste em criar um utilizador chamado Britta Simon LaunchDarkly. LaunchDarkly suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não há nenhum item de ação para si nesta secção. Um novo utilizador é criado durante a tentativa de aceder LaunchDarkly se não existir ainda.
>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de cliente LaunchDarkly](mailto:support@launchdarkly.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para LaunchDarkly.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a LaunchDarkly, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **LaunchDarkly**.

    ![A ligação de LaunchDarkly na lista de aplicações](./media/launchdarkly-tutorial/tutorial_launchdarkly_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico LaunchDarkly no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de LaunchDarkly.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/launchdarkly-tutorial/tutorial_general_01.png
[2]: ./media/launchdarkly-tutorial/tutorial_general_02.png
[3]: ./media/launchdarkly-tutorial/tutorial_general_03.png
[4]: ./media/launchdarkly-tutorial/tutorial_general_04.png

[100]: ./media/launchdarkly-tutorial/tutorial_general_100.png

[200]: ./media/launchdarkly-tutorial/tutorial_general_200.png
[201]: ./media/launchdarkly-tutorial/tutorial_general_201.png
[202]: ./media/launchdarkly-tutorial/tutorial_general_202.png
[203]: ./media/launchdarkly-tutorial/tutorial_general_203.png

