---
title: 'Tutorial: Integração do Azure Active Directory com o Monitor de desempenho de AppNeta | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Monitor de desempenho de AppNeta.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 643a45fb-d6fc-4b32-b721-68899f8c7d44
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: jeedes
ms.openlocfilehash: ccedc0288e313df2639862a14078d8cad9951286
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054554"
---
# <a name="tutorial-azure-active-directory-integration-with-appneta-performance-monitor"></a>Tutorial: Integração do Azure Active Directory com o Monitor de desempenho de AppNeta

Neste tutorial, saiba como integrar o Monitor de desempenho de AppNeta com o Azure Active Directory (Azure AD).

Monitor de desempenho de AppNeta a integração com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Monitor de desempenho de AppNeta.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Monitor de desempenho de AppNeta (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Monitor de desempenho de AppNeta, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Monitor de desempenho de AppNeta logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Monitor de desempenho de AppNeta da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-appneta-performance-monitor-from-the-gallery"></a>Adicionando o Monitor de desempenho de AppNeta da Galeria
Para configurar a integração AppNeta do Monitor de desempenho para o Azure AD, terá de adicionar AppNeta Monitor de desempenho a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar AppNeta Monitor de desempenho a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Monitor de desempenho de AppNeta**, selecione **Monitor de desempenho de AppNeta** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Monitor de desempenho de AppNeta na lista de resultados](./media/appneta-tutorial/tutorial_appneta_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Monitor de desempenho de AppNeta com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no Monitor de desempenho de AppNeta a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Monitor de desempenho de AppNeta deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com o Monitor de desempenho de AppNeta, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste do Monitor de desempenho de AppNeta](#create-an-appneta-performance-monitor-test-user)**  - para ter um equivalente da Eduarda Almeida no Monitor de desempenho de AppNeta que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de Monitor de desempenho de AppNeta.

**Para configurar o Azure AD início de sessão único com o Monitor de desempenho de AppNeta, execute os seguintes passos:**

1. No portal do Azure, sobre o **AppNeta Monitor de desempenho** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/appneta-tutorial/tutorial_appneta_samlbase.png)

3. Sobre o **domínio de Monitor de desempenho de AppNeta e URLs** secção, execute os seguintes passos:

    ![Domínio de Monitor de desempenho de AppNeta e URLs únicas início de sessão em informações](./media/appneta-tutorial/tutorial_appneta_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.pm.appneta.com`

    b. Na **identificador** caixa de texto, digite o valor: `PingConnect`

    > [!NOTE] 
    > O valor de URL de início de sessão não é real. Atualize este valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente do Monitor de desempenho de AppNeta](mailto:support@appneta.com) para obter este valor. 

5. O aplicativo de Monitor de desempenho de AppNeta espera que as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizado à sua configuração de atributos de token SAML. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos da "**atributos de utilizador**" secção na página de integração de aplicações.

    ![Configurar o início de sessão único](./media/appneta-tutorial/attribute.png)

6. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem anterior e execute os seguintes passos:
           
    | Nome do Atributo | Valor do Atributo |
    | ---------------| ----------------|
    | firstName| User.givenName|
    | Apelido| User.Surname|
    | e-mail| user.userprincipalname|
    | nome| user.userprincipalname|
    | grupos   | user.assignedroles |
    | telefone| User.telephoneNumber |
    | título| user.jobtitle|

    > [!NOTE]
    > "grupos" refere-se para o grupo de segurança no Appneta que é mapeado para uma função no Azure AD. Consulte a [isso](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) documento que explica como criar funções personalizadas no Azure AD.
        
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/appneta-tutorial/tutorial_attribute_04.png)
    
    ![Configurar o início de sessão único](./media/appneta-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o espaço de nomes como aplicação em branco.
    
    e. Clique em **OK**.  

4. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/appneta-tutorial/tutorial_appneta_certificate.png) 

5. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/appneta-tutorial/tutorial_general_400.png)

6. Para configurar o início de sessão único em **Monitor de desempenho de AppNeta** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte do Monitor de desempenho de AppNeta](mailto:support@appneta.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/appneta-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/appneta-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/appneta-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/appneta-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-appneta-performance-monitor-test-user"></a>Criar um utilizador de teste do Monitor de desempenho de AppNeta

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida no Monitor de desempenho de AppNeta. Monitor de desempenho de AppNeta suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder AppNeta Monitor de desempenho, se não existir ainda.
>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte do Monitor de desempenho de AppNeta](mailto:support@appneta.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso AppNeta Monitor de desempenho.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida AppNeta Monitor de desempenho, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Monitor de desempenho de AppNeta**.

    ![A ligação de Monitor de desempenho de AppNeta na lista de aplicações](./media/appneta-tutorial/tutorial_appneta_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de Monitor de desempenho de AppNeta no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de Monitor de desempenho de AppNeta.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/appneta-tutorial/tutorial_general_01.png
[2]: ./media/appneta-tutorial/tutorial_general_02.png
[3]: ./media/appneta-tutorial/tutorial_general_03.png
[4]: ./media/appneta-tutorial/tutorial_general_04.png

[100]: ./media/appneta-tutorial/tutorial_general_100.png

[200]: ./media/appneta-tutorial/tutorial_general_200.png
[201]: ./media/appneta-tutorial/tutorial_general_201.png
[202]: ./media/appneta-tutorial/tutorial_general_202.png
[203]: ./media/appneta-tutorial/tutorial_general_203.png

