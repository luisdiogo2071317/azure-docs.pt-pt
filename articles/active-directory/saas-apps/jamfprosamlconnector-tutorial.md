---
title: 'Tutorial: Integração do Azure Active Directory com Jamf Pro | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e Jamf Pro.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: 4711aa48e19a604d7b9b81e0f40fc661f5c68da2
ms.sourcegitcommit: c851842d113a7078c378d78d94fea8ff5948c337
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/18/2018
ms.locfileid: "35955718"
---
# <a name="tutorial-azure-active-directory-integration-with-jamf-pro"></a>Tutorial: Integração do Azure Active Directory com Jamf Pro

Neste tutorial, irá aprender a integrar Jamf Pro com o Azure Active Directory (Azure AD).

Integrar Jamf Pro com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Jamf Pro.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada para Jamf Pro (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Jamf Pro, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Jamf Pro-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar Jamf Pro a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-jamf-pro-from-the-gallery"></a>Adicionar Jamf Pro a partir da Galeria
Para configurar a integração do Jamf Pro com o Azure AD, tem de adicionar Jamf Pro na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Jamf Pro a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Jamf Pro**, selecione **Jamf Pro** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Jamf Pro na lista de resultados](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com Jamf Pro baseado na chamado "Britta Simon" um utilizador de teste.

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo no Jamf Pro for para um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Jamf Pro tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Jamf Pro, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste Jamf Pro](#create-a-jamf-pro-test-user)**  - para ter um homólogo de Britta Simon Jamf Pro que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação Jamf Pro.

**Para configurar o Azure AD-início de sessão único com Jamf Pro, execute os seguintes passos:**

1. No portal do Azure, no **Jamf Pro** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_samlbase.png)

3. No **domínio profissional Jamf e URLs** secção, execute os seguintes passos, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Domínio profissional Jamf e os URLs únicos de informações de início de sessão](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url.png)

    a. No **identificador (ID de entidade)** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.jamfcloud.com/saml/metadata`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.jamfcloud.com/saml/SSO`

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![Domínio profissional Jamf e os URLs únicos de informações de início de sessão](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_url1.png)

    No **URL de início de sessão** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.jamfcloud.com`
     
    > [!NOTE]
    > Estes valores não estiverem reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Irá obter o valor real do identificador de **Single Sign-On** secção portal de Jamf Pro, que é explicada mais tarde no tutorial. Pode extrair o real **subdomínio** valor a partir do valor do identificador e utilize essa **subdomínio** informações no URL de início de sessão e o URL de resposta.

5. No **certificado de assinatura de SAML** secção, clique no botão Copiar para copiar **Url de metadados de Federação de aplicação** e cole-o bloco de notas.

    ![A hiperligação de transferência do certificado](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/jamfprosamlconnector-tutorial/tutorial_general_400.png)
    
7. Numa janela do browser web diferente, inicie sessão no site da sua empresa Jamf Pro como administrador.

8. Clique em de **ícone definições** do canto superior direito da página.

    ![Configuração de Jamf Pro](./media/jamfprosamlconnector-tutorial/configure1.png)

9. Clique em **início de sessão único**.

    ![Configuração de Jamf Pro](./media/jamfprosamlconnector-tutorial/configure2.png)

10. Desloque para baixo até **fornecedor de identidade** sob o **Single Sign-On** secção e execute os seguintes passos:

    ![Configuração de Jamf Pro](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. Selecione **outros** como uma opção do **fornecedor de identidade** pendente.

    b. No **outro fornecedor** caixa de texto, introduza **do Azure AD**.

    c. Selecione **URL de metadados** como uma opção do **origem de METADADOS do fornecedor de identidade** pendente e na caixa de texto seguinte, cole o **Url de metadados de Federação de aplicação** valor que ter copiado do portal do Azure.

    d. Copiar o **ID de entidade** vlaue e cole-o para o **identificador (ID de entidade)** textbox em **domínio profissional Jamf e URLs** secção no portal do Azure.

    >[!NOTE]
    > Aqui `aadsso` é a parte do subdomínio (ou seja, para efeitos de referência). Utilize este valor para concluir o URL de início de sessão e o URL de resposta no **domínio profissional Jamf e URLs** secção no portal do Azure.

    e. Clique em **Guardar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/jamfprosamlconnector-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/jamfprosamlconnector-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/jamfprosamlconnector-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/jamfprosamlconnector-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-jamf-pro-test-user"></a>Criar um utilizador de teste Jamf Pro

Para permitir que os utilizadores do Azure AD iniciem sessão nos Jamf Pro, têm de ser aprovisionados para Jamf Pro. No caso de Jamf Pro, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa Jamf Pro como administrador.

2. Clique em de **ícone definições** do canto superior direito da página.

    ![Adicionar empregado](./media/jamfprosamlconnector-tutorial/configure1.png)

3. Clique em **contas de utilizador Jamf Pro & grupos**.

    ![Adicionar empregado](./media/jamfprosamlconnector-tutorial/user1.png)

4. Clique em **Novo**.

    ![Adicionar empregado](./media/jamfprosamlconnector-tutorial/user2.png)

5. Selecione **criar conta Standard**.

    ![Adicionar empregado](./media/jamfprosamlconnector-tutorial/user3.png)

6. No **nova conta** dailog, execute os seguintes passos:

    ![Adicionar empregado](./media/jamfprosamlconnector-tutorial/user4.png)

    a. No **USERNAME** caixa de texto, escreva o nome completo do BrittaSimon.

    b. Selecione as opções apropriadas de acordo com a sua organização para **nível de acesso**, **PRIVILÉGIO definir**e para **estado de acesso**.
    
    c. No **nome completo** caixa de texto, escreva o nome completo do Britta Simon.

    d. No **endereço de correio ELETRÓNICO** caixa de texto, escreva o endereço de correio eletrónico da conta de Britta Simon.

    e. No **palavra-passe** caixa de texto, escreva a palavra-passe do utilizador.

    f. No **Certifique-se a palavra-passe** caixa de texto, escreva a palavra-passe do utilizador.

    g. Clique em **Guardar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso para Jamf Pro.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon a Jamf Pro, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Jamf Pro**.

    ![A ligação de Jamf Pro na lista de aplicações](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico Jamf Pro no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de Jamf Pro.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

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

