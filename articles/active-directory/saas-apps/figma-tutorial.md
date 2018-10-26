---
title: 'Tutorial: Integração do Azure Active Directory com Figma | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Figma.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8569cae1-87dd-4c40-9bbb-527ac80d6a96
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/22/2018
ms.author: jeedes
ms.openlocfilehash: b57fdb3f039a9395133854f8b4d4f2095e3a4f9b
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50095250"
---
# <a name="tutorial-azure-active-directory-integration-with-figma"></a>Tutorial: Integração do Azure Active Directory com Figma

Neste tutorial, saiba como integrar Figma com o Azure Active Directory (Azure AD).

Integrar Figma no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Figma.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Figma (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Figma, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Figma [logon único habilitado subscrição](https://www.figma.com/pricing/)

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção. Novos clientes e os subscritores ativos do Figma Professional Team poderão entrar em contato Figma para [atualizar a respetiva subscrição](https://www.figma.com/pricing/) para o escalão de organização Figma.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Figma da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-figma-from-the-gallery"></a>Adicionando Figma da Galeria

Para configurar a integração do Figma com o Azure AD, terá de adicionar Figma a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Figma a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Figma**. Selecione **Figma** no painel de resultados e, em seguida, clique o **Add** botão para adicionar a aplicação.

    ![Figma na lista de resultados](./media/figma-tutorial/tutorial_figma_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Figma com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa estar ligado ao Figma.  Para configurar e testar o Azure AD início de sessão único com Figma, conclua os seguintes passos:

1. [**Contacte a equipa de suporte de Figma** ](mailto:support@figma.com?subject=SAML+Config) para iniciar uma configuração de SAML para a sua organização e obter um ORG_SAML_CONFIG_ID.
2. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
3. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
4. **[Criar um utilizador de teste Figma](#creating-a-figma-test-user)**  - para ter um equivalente da Eduarda Almeida na Figma que está ligado à representação de utilizador do Azure AD.
5. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
6. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Figma.

**Para configurar o Azure AD início de sessão único com Figma, execute os seguintes passos:**

1. No portal do Azure, sobre o **Figma** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, clique em **selecione** para **SAML** modo para ativar o início de sessão único.

    ![Configurar o início de sessão único](./media/figma-tutorial/tutorial_general_301.png)

3. Se precisar de alterar para **SAML** modo de qualquer outro modo, clique em **alterar único início de sessão em modo** na parte superior do ecrã.

    ![Configurar o início de sessão único](./media/figma-tutorial/tutorial_general_300.png)

4. Sobre o **definir a segurança de início de sessão único com o SAML** página, clique em **editar** ícone para abrir **configuração básica de SAML** caixa de diálogo.

    ![Configurar o início de sessão único](./media/figma-tutorial/tutorial_general_302.png)

5. Sobre o **configuração básica de SAML** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Figma domínio e URLs únicas início de sessão em informações](./media/figma-tutorial/tutorial_figma_url1.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://www.figma.com/saml/<ORG_SAML_CONFIG_ID>`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://www.figma.com/saml/<ORG_SAML_CONFIG_ID>/consume`

6. Clique em **definir URLs adicionais** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Figma domínio e URLs únicas início de sessão em informações](./media/figma-tutorial/tutorial_figma_url2.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://www.figma.com/saml/<ORG_SAML_CONFIG_ID>/start`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de Figma](mailto:support@figma.com?subject=SAML+Config) obter esses valores.

7. Aplicação de Figma espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos do **atributos de utilizador** secção na página de integração de aplicações. Sobre o **definido no início de sessão único com o SAML** página, clique em **editar** botão para abrir **atributos de utilizador** caixa de diálogo.

    ![Secção de atributo](./media/figma-tutorial/edit_attribute.png)

8. No **afirmações de utilizador** secção sobre o **atributos de utilizador** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:

    | Nome | Atributo de origem|
    | ---------------| --------- |
    | `externalId` | `user.mailnickname` |
    | `displayName` | `user.displayname` |
    | `title` | `user.jobtitle` |
    | `emailaddress` | `user.mail` |
    | `familyName` | `user.surname` |
    | `givenName` | `givenName` |
    | `userName` | `user.userprincipalname` |

    a. Clique em **Adicionar nova afirmação** para abrir o **afirmações de utilizador de gerir** caixa de diálogo.

    ![Novo atributo](./media/figma-tutorial/new_save_attribute.png)

    ![Adicionar Atributo](./media/figma-tutorial/new_attribute_details.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Deixe o **espaço de nomes** em branco.

    d. Selecione a origem de dado **atributo**.

    e. Partir do **atributo de origem** lista, digite o valor de atributo apresentado para essa linha.

    f. Clique em **Ok**

    g. Clique em **Guardar**.

9. No **definir a segurança de início de sessão único com o SAML** na página a **certificado de assinatura SAML** secção, clique em botão Copiar para copiar **Url de metadados de Federação de aplicação** e guarde-o no seu computador.

    ![O link de download de certificado](./media/figma-tutorial/tutorial_figma_certificate.png)

10. Para configurar o início de sessão único no lado de Figma, preencha este formulário: [ https://goo.gl/forms/XkRB1z5ed4eVUzXn2 ](https://goo.gl/forms/XkRB1z5ed4eVUzXn2). Ele vai aceitar sua **Url de metadados de Federação de aplicação** do passo n. º 9.

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![Criar utilizador do Azure AD][100]

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![Criar um utilizador de teste do Azure AD](./media/figma-tutorial/create_aaduser_01.png) 

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![Criar um utilizador de teste do Azure AD](./media/figma-tutorial/create_aaduser_02.png)

    a. Na **Name** , insira **BrittaSimon**.
  
    b. Na **nome de utilizador** , digite **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.

### <a name="creating-a-figma-test-user"></a>Criar um utilizador de teste Figma

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Figma. Figma suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder Figma se não existir ainda.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para HubSpot SAML.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **HubSpot SAML**.

    ![Configurar o início de sessão único](./media/figma-tutorial/tutorial_figma_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

6. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.

### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Figma no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Figma.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/figma-tutorial/tutorial_general_01.png
[2]: ./media/figma-tutorial/tutorial_general_02.png
[3]: ./media/figma-tutorial/tutorial_general_03.png
[4]: ./media/figma-tutorial/tutorial_general_04.png

[100]: ./media/figma-tutorial/tutorial_general_100.png

[200]: ./media/figma-tutorial/tutorial_general_200.png
[201]: ./media/figma-tutorial/tutorial_general_201.png
[202]: ./media/figma-tutorial/tutorial_general_202.png
[203]: ./media/figma-tutorial/tutorial_general_203.png