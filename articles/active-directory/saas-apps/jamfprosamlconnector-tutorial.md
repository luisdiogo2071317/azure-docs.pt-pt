---
title: 'Tutorial: Integração do Azure Active Directory com o Jamf Pro | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Jamf Pro.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: 794039ee1a5b1cf3b382e0f0769383b1e033e982
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39046945"
---
# <a name="tutorial-azure-active-directory-integration-with-jamf-pro"></a>Tutorial: Integração do Azure Active Directory com o Jamf Pro

Neste tutorial, saiba como integrar o Jamf Pro com o Azure Active Directory (Azure AD).

Integrar o Jamf Pro com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Jamf Pro.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Jamf Pro (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Jamf Pro, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Jamf Pro logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Jamf Pro da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-jamf-pro-from-the-gallery"></a>Adicionando o Jamf Pro da Galeria
Para configurar a integração do Jamf Pro com o Azure AD, terá de adicionar o Jamf Pro a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o Jamf Pro a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **Jamf Pro**, selecione **Jamf Pro** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![O Jamf Pro na lista de resultados](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Jamf Pro com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no Jamf Pro para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Jamf Pro deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com o Jamf Pro, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste do Jamf Pro](#create-a-jamf-pro-test-user)**  - para ter um equivalente da Eduarda Almeida no Jamf Pro que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do Jamf Pro.

**Para configurar o Azure AD início de sessão único com o Jamf Pro, execute os seguintes passos:**

1. No portal do Azure, sobre o **Jamf Pro** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_samlbase.png)

3. Na **domínio de Jamf Pro e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação na **IDP** iniciada pelo modo:

    ![Domínio de Jamf Pro e URLs únicas início de sessão em informações](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url.png)

    a. Na **identificador (ID de entidade)** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.jamfcloud.com/saml/metadata`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.jamfcloud.com/saml/SSO`

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Domínio de Jamf Pro e URLs únicas início de sessão em informações](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.jamfcloud.com`
     
    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Obterá o valor do identificador real da **Single Sign-On** secção no portal do Jamf Pro, que é explicado mais tarde no tutorial. É possível extrair o real **subdomínio** valor do valor de identificador e usá-lo **subdomínio** informações no URL de início de sessão e o URL de resposta.

5. Sobre o **certificado de assinatura SAML** secção, clique no botão de cópia para copiar **Url de metadados de Federação de aplicação** e cole-o no bloco de notas.

    ![O link de download de certificado](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/jamfprosamlconnector-tutorial/tutorial_general_400.png)
    
7. Numa janela do browser web diferente, inicie sessão no site da sua empresa Jamf Pro como um administrador.

8. Clique nas **ícone de definições** do canto superior direito da página.

    ![Configuração do Jamf Pro](./media/jamfprosamlconnector-tutorial/configure1.png)

9. Clique em **início de sessão único**.

    ![Configuração do Jamf Pro](./media/jamfprosamlconnector-tutorial/configure2.png)

10. Desloque para baixo até **fornecedor de identidade** sob a **Single Sign-On** secção e execute os seguintes passos:

    ![Configuração do Jamf Pro](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. Selecione **outras** como uma opção a partir do **fornecedor de identidade** lista pendente.

    b. Na **outros fornecedor** caixa de texto, introduza **do Azure AD**.

    c. Selecione **URL de metadados** como uma opção da **origem de METADADOS do fornecedor de identidade** menu pendente e, na caixa de texto seguinte, cole o **Url de metadados de Federação de aplicação** valor que copiou do portal do Azure.

    d. Cópia a **ID de entidade** vlaue e colá-lo no **identificador (ID de entidade)** caixa de texto no **domínio de Jamf Pro e URLs** secção no portal do Azure.

    >[!NOTE]
    > Aqui `aadsso` é a parte de subdomínio (que é para fins de referência). Utilize este valor para concluir o início de sessão no URL e o URL de resposta no **domínio de Jamf Pro e URLs** secção no portal do Azure.

    e. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/jamfprosamlconnector-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/jamfprosamlconnector-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/jamfprosamlconnector-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/jamfprosamlconnector-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-jamf-pro-test-user"></a>Criar um utilizador de teste do Jamf Pro

Para ativar a utilizadores do Azure AD iniciar sessão no Jamf Pro, tem de ser aprovisionados no Jamf Pro. No caso do Jamf Pro, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa Jamf Pro como administrador.

2. Clique nas **ícone de definições** do canto superior direito da página.

    ![Adicionar o funcionário](./media/jamfprosamlconnector-tutorial/configure1.png)

3. Clique em **contas de utilizador do Jamf Pro & grupos**.

    ![Adicionar o funcionário](./media/jamfprosamlconnector-tutorial/user1.png)

4. Clique em **Novo**.

    ![Adicionar o funcionário](./media/jamfprosamlconnector-tutorial/user2.png)

5. Selecione **criar conta Standard**.

    ![Adicionar o funcionário](./media/jamfprosamlconnector-tutorial/user3.png)

6. Sobre o **nova conta** dailog, execute os seguintes passos:

    ![Adicionar o funcionário](./media/jamfprosamlconnector-tutorial/user4.png)

    a. Na **nome de utilizador** caixa de texto, escreva o nome completo do BrittaSimon.

    b. Selecione as opções apropriadas de acordo com a sua organização para o **nível de acesso**, **PRIVILÉGIO definir**e para **estado de acesso**.
    
    c. Na **FULLNAME** caixa de texto, escreva o nome completo da Eduarda Almeida.

    d. Na **endereço de E-Mail** caixa de texto, escreva o endereço de e-mail da conta da Eduarda Almeida.

    e. Na **palavra-passe** caixa de texto, escreva a palavra-passe do utilizador.

    f. Na **Verifique se a palavra-passe** caixa de texto, escreva a palavra-passe do utilizador.

    g. Clique em **Guardar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para o Jamf Pro.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida para Jamf Pro, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Jamf Pro**.

    ![A ligação do Jamf Pro na lista de aplicações](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do Jamf Pro no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do Jamf Pro.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/jamfprosamlconnector-tutorial/tutorial_general_01.png
[2]: ./media/jamfprosamlconnector-tutorial/tutorial_general_02.png
[3]: ./media/jamfprosamlconnector-tutorial/tutorial_general_03.png
[4]: ./media/jamfprosamlconnector-tutorial/tutorial_general_04.png

[100]: ./media/jamfprosamlconnector-tutorial/tutorial_general_100.png

[200]: ./media/jamfprosamlconnector-tutorial/tutorial_general_200.png
[201]: ./media/jamfprosamlconnector-tutorial/tutorial_general_201.png
[202]: ./media/jamfprosamlconnector-tutorial/tutorial_general_202.png
[203]: ./media/jamfprosamlconnector-tutorial/tutorial_general_203.png

