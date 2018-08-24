---
title: 'Tutorial: Integração do Azure Active Directory com o ArcGIS Enterprise | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o ArcGIS Enterprise.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 24809e9d-a4aa-4504-95a9-e4fcf484f431
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2018
ms.author: jeedes
ms.openlocfilehash: ca5bf7ae49cf120c0566419ccadeff92433c6467
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42820243"
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-enterprise"></a>Tutorial: Integração do Azure Active Directory com o ArcGIS Enterprise

Neste tutorial, saiba como integrar o ArcGIS Enterprise com o Azure Active Directory (Azure AD).

Integração empresarial de ArcGIS com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao ArcGIS Enterprise.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para a empresa ArcGIS (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o ArcGIS Enterprise, terá dos seguintes itens:

- Uma subscrição do Azure
- Uma empresa de ArcGIS logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando ArcGIS Enterprise da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-arcgis-enterprise-from-the-gallery"></a>Adicionando ArcGIS Enterprise da Galeria

Para configurar a integração do ArcGIS Enterprise com o Azure AD, terá de adicionar ArcGIS empresarial a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar ArcGIS empresarial a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **ArcGIS Enterprise**, selecione **ArcGIS Enterprise** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![O ArcGIS Enterprise na lista de resultados](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o ArcGIS Enterprise com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa de saber o que o usuário equivalente na empresa de ArcGIS for para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na empresa ArcGIS deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com o ArcGIS Enterprise, terá de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste de ArcGIS Enterprise](#create-an-arcgis-enterprise-test-user)**  - para ter um equivalente da Eduarda Almeida na empresa ArcGIS que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação empresarial do ArcGIS.

**Para configurar o Azure AD início de sessão único com o ArcGIS Enterprise, execute os seguintes passos:**

1. No portal do Azure, sobre o **ArcGIS Enterprise** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_samlbase.png)

3. Sobre o **ArcGIS Enterprise domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![O ArcGIS Enterprise domínio e URLs únicas início de sessão em informações](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_url1.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `<EXTERNAL_DNS_NAME>.portal`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<EXTERNAL_DNS_NAME>/portal/sharing/rest/oauth2/saml/signin2`

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![O ArcGIS Enterprise domínio e URLs únicas início de sessão em informações](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_url2.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<EXTERNAL_DNS_NAME>/portal/sharing/rest/oauth2/saml/signin`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte do ArcGIS Enterprise Client](mailto:nshampur@esri.com) obter esses valores. Obterá o valor do identificador da **definir o fornecedor de identidade** seção, que é explicada posteriormente neste tutorial.

5. Sobre o **certificado de assinatura SAML** secção, clique no botão de cópia para copiar **Url de metadados de Federação de aplicação** e cole-o no bloco de notas.

    ![O link de download de certificado](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_certificate.png)

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/arcgisenterprise-tutorial/tutorial_general_400.png)

7. Numa janela do browser web diferente, inicie sessão no site da sua empresa ArcGIS Enterprise como um administrador.

8. Selecione **organização > Editar definições**.

    ![Configuração empresarial ArcGIS](./media/arcgisenterprise-tutorial/configure1.png)

9. Selecione **segurança** separador.

    ![Configuração empresarial ArcGIS](./media/arcgisenterprise-tutorial/configure2.png)

10. Desloque para baixo para o **inícios de sessão empresarial através de SAML** secção e selecione **Definir início de sessão empresarial**.

    ![Configuração empresarial ArcGIS](./media/arcgisenterprise-tutorial/configure3.png)

11. Sobre o **definir o fornecedor de identidade** secção, execute os seguintes passos:

    ![Configuração empresarial ArcGIS](./media/arcgisenterprise-tutorial/configure4.png)

    a. Forneça um nome, como **do Azure Active Directory teste** no **nome** caixa de texto.

    b. Na **URL** caixa de texto, colar a **Url de metadados de Federação de aplicação** valor que copiou do portal do Azure.

    c. Clique em **Mostrar definições avançadas** e copie a **ID de entidade** valor e cole-o no **identificador** caixa de texto no **ArcGIS Enterprise domínio e URLs** secção do portal do Azure.
    
    ![Configuração empresarial ArcGIS](./media/arcgisenterprise-tutorial/configure5.png)

    d. Clique em **fornecedor de identidade de ATUALIZAÇÃO**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/arcgisenterprise-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/arcgisenterprise-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/arcgisenterprise-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/arcgisenterprise-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-an-arcgis-enterprise-test-user"></a>Criar um utilizador de teste do ArcGIS Enterprise

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida na empresa ArcGIS. O ArcGIS Enterprise suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder ArcGIS Enterprise, se não existir ainda.

> [!Note]
> Se precisar de criar manualmente um utilizador, contacte [equipa de suporte da empresa do ArcGIS](mailto:nshampur@esri.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso ao ArcGIS Enterprise.

![Atribuir a função de utilizador][200]

**Para atribuir a Eduarda Almeida a ArcGIS Enterprise, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **ArcGIS Enterprise**.

    ![A ligação de ArcGIS Enterprise na lista de aplicações](./media/arcgisenterprise-tutorial/tutorial_arcgisenterprise_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de ArcGIS Enterprise no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação empresarial do ArcGIS.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/arcgisenterprise-tutorial/tutorial_general_01.png
[2]: ./media/arcgisenterprise-tutorial/tutorial_general_02.png
[3]: ./media/arcgisenterprise-tutorial/tutorial_general_03.png
[4]: ./media/arcgisenterprise-tutorial/tutorial_general_04.png

[100]: ./media/arcgisenterprise-tutorial/tutorial_general_100.png

[200]: ./media/arcgisenterprise-tutorial/tutorial_general_200.png
[201]: ./media/arcgisenterprise-tutorial/tutorial_general_201.png
[202]: ./media/arcgisenterprise-tutorial/tutorial_general_202.png
[203]: ./media/arcgisenterprise-tutorial/tutorial_general_203.png