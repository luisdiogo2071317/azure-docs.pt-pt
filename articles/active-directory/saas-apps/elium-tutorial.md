---
title: 'Tutorial: Integração do Azure Active Directory com Elium | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Elium.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: fae344b3-5bd9-40e2-9a1d-448dcd58155f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: jeedes
ms.openlocfilehash: 2957fffecbf448fa456d80200aba9752569b5f69
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39042729"
---
# <a name="tutorial-azure-active-directory-integration-with-elium"></a>Tutorial: Integração do Azure Active Directory com Elium

Neste tutorial, saiba como integrar Elium com o Azure Active Directory (Azure AD).

Integrar Elium no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Elium.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Elium (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Elium, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Elium logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Elium da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-elium-from-the-gallery"></a>Adicionando Elium da Galeria
Para configurar a integração do Elium com o Azure AD, terá de adicionar Elium a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Elium a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Elium**, selecione **Elium** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Elium na lista de resultados](./media/elium-tutorial/tutorial_elium_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Elium com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Elium a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Elium deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com Elium, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste Elium](#create-an-elium-test-user)**  - para ter um equivalente da Eduarda Almeida na Elium que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Elium.

**Para configurar o Azure AD início de sessão único com Elium, execute os seguintes passos:**

1. No portal do Azure, sobre o **Elium** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/elium-tutorial/tutorial_elium_samlbase.png)

3. Sobre o **Elium domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Elium domínio e URLs únicas início de sessão em informações](./media/elium-tutorial/tutorial_elium_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<platform-domain>.elium.com/login/saml2/metadata`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<platform-domain>.elium.com/login/saml2/acs`

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Elium domínio e URLs únicas início de sessão em informações](./media/elium-tutorial/tutorial_elium_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: ` https://<platform-domain>.elium.com/login/saml2/login`
     
    > [!NOTE] 
    > Estes valores não são reais. Irá obter estes valores a partir da **o ficheiro de metadados de SP** disponível para download em `https://<platform-domain>.elium.com/login/saml2/metadata`, que é explicado posteriormente neste tutorial.

5. O aplicativo Elium espera que as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizado à sua configuração de atributos de token SAML. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos da "**atributos de utilizador**" secção na página de integração de aplicações.

    ![Configurar o início de sessão único](./media/elium-tutorial/tutorial_attribute.png)

6. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem anterior e execute os seguintes passos:
           
    | Nome do Atributo | Valor do Atributo |   
    | ---------------| ----------------|
    | e-mail   |User.Mail |
    | first_name| User.givenName |
    | last_name| User.Surname|
    | job_title| user.jobtitle|
    | Empresa| user.companyname|
    
    > [!NOTE]
    > Estas são as afirmações de predefinição. **Apenas a afirmação de e-mail é obrigatória**. Para o JIT aprovisionamento também e-mail apenas a afirmação é obrigatória. Outras afirmações personalizadas podem variar de plataforma de um cliente para outra plataforma de cliente.

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/elium-tutorial/tutorial_attribute_04.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    ![Configurar o início de sessão único](./media/elium-tutorial/tutorial_attribute_05.png)

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Deixe o espaço de nomes como aplicação em branco.
    
    e. Clique em **OK**. 

5. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/elium-tutorial/tutorial_elium_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/elium-tutorial/tutorial_general_400.png)
    
7. Numa janela do browser web diferente, inicie sessão no site da sua empresa Elium como administrador.

8. Clique nas **perfil de utilizador** no canto superior direito e selecione **administração**.

    ![Configurar o início de sessão único](./media/elium-tutorial/user1.png)

9. Selecione **segurança** separador.

    ![Configurar o início de sessão único](./media/elium-tutorial/user2.png)

10. Desloque para baixo para o **início de sessão único (SSO)** secção e execute os seguintes passos:

    ![Configurar o início de sessão único](./media/elium-tutorial/user3.png)

    a. Copie o valor da **Certifique-se de que a autenticação SAML2 funciona para a sua conta** e cole-a no **URL de início de sessão** caixa de texto a **Elium domínio e URLs** secção no Azure Portal.

    > [!NOTE]
    > Depois de configurar o SSO, pode sempre aceder a página de início de sessão remoto padrão no seguinte URL: `https://<platform_domain>/login/regular/login` 

    b. Selecione **SAML2 ativar a Federação** caixa de verificação.

    c. Selecione **JIT aprovisionamento** caixa de verificação.

    d. Abra o **metadados de SP** ao clicar no **transferir** botão.

    e. Procure o **entityID** no **SP metadados** ficheiro, copie o **entityID** valor e cole-a no **identificador** caixa de texto a  **Elium domínio e URLs** secção no portal do Azure. 

    ![Configurar o início de sessão único](./media/elium-tutorial/user4.png)

    f. Procure o **AssertionConsumerService** no **SP metadados** ficheiro, copie o **localização** valor e cole-a no **URL de resposta** caixa de texto a **Elium domínio e URLs** secção no portal do Azure.

    ![Configurar o início de sessão único](./media/elium-tutorial/user5.png)

    g. Abra o ficheiro de metadados baixado a partir do portal do Azure no bloco de notas, copiar o conteúdo e cole-o para o **metadados de IdP** caixa de texto.

    h. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/elium-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/elium-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/elium-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/elium-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-elium-test-user"></a>Criar um utilizador de teste Elium

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Elium. Elium suporta o aprovisionamento de just-in-time, que está por predefinição, ativada. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder Elium se não existir ainda.
>[!Note]
>Se precisar de criar manualmente um utilizador, contacte [equipa de suporte de Elium](mailto:support@elium.com).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Elium.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Elium, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Elium**.

    ![A ligação de Elium na lista de aplicações](./media/elium-tutorial/tutorial_elium_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Elium no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Elium.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/elium-tutorial/tutorial_general_01.png
[2]: ./media/elium-tutorial/tutorial_general_02.png
[3]: ./media/elium-tutorial/tutorial_general_03.png
[4]: ./media/elium-tutorial/tutorial_general_04.png

[100]: ./media/elium-tutorial/tutorial_general_100.png

[200]: ./media/elium-tutorial/tutorial_general_200.png
[201]: ./media/elium-tutorial/tutorial_general_201.png
[202]: ./media/elium-tutorial/tutorial_general_202.png
[203]: ./media/elium-tutorial/tutorial_general_203.png

