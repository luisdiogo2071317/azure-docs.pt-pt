---
title: 'Tutorial: Integração do Azure Active Directory com Vidyard | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Vidyard.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bed7df23-6e13-4e7c-b4cc-53ed4804664d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2018
ms.author: jeedes
ms.openlocfilehash: 1ad4259ef0e0ca948d677944855ce732b3de98fd
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/18/2018
ms.locfileid: "35948105"
---
# <a name="tutorial-azure-active-directory-integration-with-vidyard"></a>Tutorial: Integração do Azure Active Directory com Vidyard

Neste tutorial, irá aprender a integrar Vidyard com o Azure Active Directory (Azure AD).

Integrar Vidyard com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Vidyard.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Vidyard (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Vidyard, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Vidyard-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Vidyard a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-vidyard-from-the-gallery"></a>Adicionar Vidyard a partir da Galeria
Para configurar a integração de Vidyard com o Azure AD, terá de adicionar Vidyard a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Vidyard a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Vidyard**, selecione **Vidyard** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Vidyard na lista de resultados](./media/vidyard-tutorial/tutorial_vidyard_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Vidyard com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Vidyard é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Vidyard tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Vidyard, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Vidyard](#create-a-vidyard-test-user)**  - para ter um homólogo de Britta Simon Vidyard que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Vidyard.

**Para configurar o Azure AD-início de sessão único com Vidyard, execute os seguintes passos:**

1. No portal do Azure, no **Vidyard** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/vidyard-tutorial/tutorial_vidyard_samlbase.png)

3. No **Vidyard domínio e os URLs** secção, execute os seguintes passos, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Domínio Vidyard e os URLs únicos de informações de início de sessão](./media/vidyard-tutorial/tutorial_vidyard_url2.png)

    a. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://secure.vidyard.com/sso/saml/<unique id>/metadata`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://secure.vidyard.com/sso/saml/<unique id>/consume`

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![Domínio Vidyard e os URLs únicos de informações de início de sessão](./media/vidyard-tutorial/tutorial_vidyard_url1.png)

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://secure.vidyard.com/sso/saml/<unique id>/login`

    > [!NOTE]
    > Estes valores não estiverem reais. Atualizar estes valores com o identificador de real, URL de resposta e início de sessão no URL, que é explicada mais tarde no tutorial

1. No **certificado de assinatura de SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/vidyard-tutorial/tutorial_vidyard_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/vidyard-tutorial/tutorial_general_400.png)

7. No **Vidyard configuração** secção, clique em **configurar Vidyard** para abrir **configurar início de sessão** janela. Copiar o **único início de sessão no URL do serviço SAML** do **secção de referência rápida.**

    ![Configuração de Vidyard](./media/vidyard-tutorial/tutorial_vidyard_configure.png)

8. Numa janela do browser web diferente, inicie sessão no site da sua empresa Vidyard Software como um administrador.

9. A partir do Vidyard dashboard, selecione **grupo** > **segurança**

    ![Configuração de Vidyard](./media/vidyard-tutorial/configure1.png)

1. Clique em **novo perfil** separador.

    ![Configuração de Vidyard](./media/vidyard-tutorial/configure2.png)

11. No **configuração SAML** secção, execute os seguintes passos:

    ![Configuração de Vidyard](./media/vidyard-tutorial/configure3.png)

    a. Introduza o nome do perfil geral no **nome do perfil** caixa de texto.

    b. Cópia **página de início de sessão de utilizador de SSO** valor e colar na **iniciar sessão no URL** textbox em **secção Vidyard domínio e os URLs** no portal do Azure.

    c. Cópia **ACS URL** valor e colar na **URL de resposta** textbox em **secção Vidyard domínio e os URLs** no portal do Azure.

    d. Cópia **URL de emissor/metadados** valor e colar na **identificador** textbox em **secção Vidyard domínio e os URLs** no portal do Azure.

    e. Abra o ficheiro de certificado transferido a partir do portal do Azure no bloco de notas e, em seguida, cole-o para o **certificado x. 509** caixa de texto.

    f. No **URL de ponto final de SAML** caixa de texto, cole o valor de **único início de sessão no URL do serviço SAML** copiados a partir do portal do Azure.

    g. Clique em **confirmar**.

12. A partir do separador de início de sessão único, selecione **atribuir** junto a um perfil existente

    ![Configuração de Vidyard](./media/vidyard-tutorial/configure4.png)

    > [!NOTE]
    > Assim que tiver criado um perfil SSO, atribuí-lo a quaisquer grupos para os quais os utilizadores precisarão de acesso através do Azure. Se o utilizador não existe dentro do grupo ao qual foram atribuídos, Vidyard automaticamente irá criar uma conta de utilizador e atribuir a respetiva função em tempo real.

13. Selecione o seu grupo de organização, que é visível no **grupos disponíveis para atribuir**.

    ![Configuração de Vidyard](./media/vidyard-tutorial/configure5.png)

14. Pode ver os grupos atribuídos sob o **grupos atualmente atribuídos**. Selecione uma função para o grupo de acordo com a sua organização e clique em **confirmar**.

    ![Configuração de Vidyard](./media/vidyard-tutorial/configure6.png)

    > [!NOTE]
    > Para obter mais informações, consulte [este documento](https://knowledge.vidyard.com/saml-single-sign-on-authentication/saml-based-single-sign-on-sso-in-vidyard).

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/vidyard-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/vidyard-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/vidyard-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/vidyard-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-vidyard-test-user"></a>Criar um utilizador de teste Vidyard

O objetivo desta secção consiste em criar um utilizador chamado Britta Simon Vidyard. Vidyard suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não há nenhum item de ação para si nesta secção. Um novo utilizador é criado durante a tentativa de aceder Vidyard se não existir ainda.
>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de Vidyard](mailto:support@vidyard.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Vidyard.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a Vidyard, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Vidyard**.

    ![A ligação de Vidyard na lista de aplicações](./media/vidyard-tutorial/tutorial_vidyard_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Vidyard no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de Vidyard.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/vidyard-tutorial/tutorial_general_01.png
[2]: ./media/vidyard-tutorial/tutorial_general_02.png
[3]: ./media/vidyard-tutorial/tutorial_general_03.png
[4]: ./media/vidyard-tutorial/tutorial_general_04.png

[100]: ./media/vidyard-tutorial/tutorial_general_100.png

[200]: ./media/vidyard-tutorial/tutorial_general_200.png
[201]: ./media/vidyard-tutorial/tutorial_general_201.png
[202]: ./media/vidyard-tutorial/tutorial_general_202.png
[203]: ./media/vidyard-tutorial/tutorial_general_203.png

