---
title: 'Tutorial: Integração do Azure Active Directory com FiscalNote | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e FiscalNote.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 55274f26-be7e-4514-964c-7186ecb55c4a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/16/2018
ms.author: jeedes
ms.openlocfilehash: 1eb11b52a89e25603939af9ec42d922d6e7ac0df
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39047414"
---
# <a name="tutorial-azure-active-directory-integration-with-fiscalnote"></a>Tutorial: Integração do Azure Active Directory com FiscalNote

Neste tutorial, saiba como integrar FiscalNote com o Azure Active Directory (Azure AD).

Integrar FiscalNote no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao FiscalNote.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para FiscalNote (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com FiscalNote, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um FiscalNote logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando FiscalNote da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-fiscalnote-from-the-gallery"></a>Adicionando FiscalNote da Galeria
Para configurar a integração do FiscalNote com o Azure AD, terá de adicionar FiscalNote a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar FiscalNote a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **FiscalNote**, selecione **FiscalNote** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![FiscalNote na lista de resultados](./media/fiscalnote-tutorial/tutorial_fiscalnote_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com FiscalNote com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no FiscalNote a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no FiscalNote deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com FiscalNote, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste FiscalNote](#create-a-fiscalnote-test-user)**  - para ter um equivalente da Eduarda Almeida na FiscalNote que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo FiscalNote.

**Para configurar o Azure AD início de sessão único com FiscalNote, execute os seguintes passos:**

1. No portal do Azure, sobre o **FiscalNote** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/fiscalnote-tutorial/tutorial_fiscalnote_samlbase.png)

3. Sobre o **FiscalNote domínio e URLs** secção, execute os seguintes passos:

    ![FiscalNote domínio e URLs únicas início de sessão em informações](./media/fiscalnote-tutorial/tutorial_fiscalnote_url.png)
    
    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<InstanceName>.fiscalnote.com/login?client=<ClientID>&redirect_uri=https://app.fiscalnote.com/saml-login.html&audience=https://api.fiscalnote.com/&connection=<CONNECTION_NAME>&response_type=id_token%20token`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `urn:auth0:fiscalnote:<CONNECTIONNAME>`

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente FiscalNote](mailto:support@fiscalnote.com) obter esses valores.

4. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Raw)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/fiscalnote-tutorial/tutorial_fiscalnote_certificate.png)

5. O aplicativo FiscalNote espera que as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizado à sua configuração de atributos de token SAML. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos da "**atributos de utilizador**" secção na página de integração de aplicações.

    ![Configurar o início de sessão único](./media/fiscalnote-tutorial/tutorial_attribute.png)

6. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem anterior e execute os seguintes passos:
           
    | Nome do Atributo | Valor do Atributo |
    | ---------------| ----------------|
    | nome | user.userprincipalname|
    | givenName| User.givenName|
    | familyName| User.Surname|
    | e-mail| User.Mail|
    
    a. Remover atributos existentes e adicionar novos atributos. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/fiscalnote-tutorial/tutorial_attribute_04.png)
    
    ![Configurar o início de sessão único](./media/fiscalnote-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o espaço de nomes como aplicação em branco.
    
    e. Clique em **OK**.

7. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/fiscalnote-tutorial/tutorial_general_400.png)

8. Sobre o **FiscalNote configuração** secção, clique em **configurar FiscalNote** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de FiscalNote](./media/fiscalnote-tutorial/tutorial_fiscalnote_configure.png) 

9. Para configurar o início de sessão único num **FiscalNote** lado, terá de enviar o transferido **Certificate(Raw), o URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** para [FiscalNote equipa de suporte](mailto:support@fiscalnote.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/fiscalnote-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/fiscalnote-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/fiscalnote-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/fiscalnote-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-fiscalnote-test-user"></a>Criar um utilizador de teste FiscalNote

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no FiscalNote. FiscalNote suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder FiscalNote se não existir ainda.
>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de FiscalNote](mailto:support@fiscalnote.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para FiscalNote.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a FiscalNote, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **FiscalNote**.

    ![A ligação de FiscalNote na lista de aplicações](./media/fiscalnote-tutorial/tutorial_fiscalnote_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico FiscalNote no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo FiscalNote.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/fiscalnote-tutorial/tutorial_general_01.png
[2]: ./media/fiscalnote-tutorial/tutorial_general_02.png
[3]: ./media/fiscalnote-tutorial/tutorial_general_03.png
[4]: ./media/fiscalnote-tutorial/tutorial_general_04.png

[100]: ./media/fiscalnote-tutorial/tutorial_general_100.png

[200]: ./media/fiscalnote-tutorial/tutorial_general_200.png
[201]: ./media/fiscalnote-tutorial/tutorial_general_201.png
[202]: ./media/fiscalnote-tutorial/tutorial_general_202.png
[203]: ./media/fiscalnote-tutorial/tutorial_general_203.png

