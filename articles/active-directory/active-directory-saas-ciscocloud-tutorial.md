---
title: 'Tutorial: Integração do Azure Active Directory Cisco nuvem | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e na nuvem de Cisco.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: db1cea1d-ff0a-4f0d-b5fd-50ca32702d56
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2018
ms.author: jeedes
ms.openlocfilehash: 8f41d8f508bb6ee5b50b7f304c703c4d9bed75d8
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-cloud"></a>Tutorial: Integração do Azure Active Directory nuvem Cisco

Neste tutorial, irá aprender a integrar Cisco Cloud com o Azure Active Directory (Azure AD).

Integrar Cisco Cloud com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à nuvem de Cisco.
- Pode permitir aos utilizadores automaticamente obter com sessão iniciada para a nuvem de Cisco (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Cisco nuvem, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um nuvem de Cisco início de sessão único subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. A adição de Cisco Cloud de galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-cisco-cloud-from-the-gallery"></a>A adição de Cisco Cloud de galeria
Para configurar a integração da Cisco nuvem com o Azure AD, tem de adicionar Cisco Cloud na Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar Cisco Cloud na galeria do, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **Cisco nuvem**, selecione **Cisco nuvem** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![Nuvem de Cisco na lista de resultados](./media/active-directory-saas-ciscocloud-tutorial/tutorial_ciscocloud_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com nuvem Cisco, com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber o que o utilizador homólogo na nuvem de Cisco é um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na nuvem de Cisco tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com Cisco nuvem, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste de nuvem Cisco](#create-a-cisco-cloud-test-user)**  - para ter um homólogo de Britta Simon na nuvem de Cisco que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação de nuvem Cisco.

**Para configurar o Azure AD-início de sessão único com Cisco nuvem, execute os seguintes passos:**

1. No portal do Azure, no **Cisco nuvem** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/active-directory-saas-ciscocloud-tutorial/tutorial_ciscocloud_samlbase.png)

3. No **Cisco nuvem domínio e os URLs** secção, execute os seguintes passos, se pretender configurar a aplicação no **IDP** iniciada modo:

    ![Domínio de nuvem Cisco e URLs único informações de início de sessão](./media/active-directory-saas-ciscocloud-tutorial/tutorial_ciscocloud_url.png)

    a. No **identificador** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `<subdomain>.cisco.com`

    b. No **URL de resposta** caixa de texto, escreva um URL a utilizar o padrão do seguinte: `https://<subdomain>.cisco.com/sp/ACS.saml2`

4. Verifique **Mostrar avançadas definições de URL** e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![Domínio de nuvem Cisco e URLs único informações de início de sessão](./media/active-directory-saas-ciscocloud-tutorial/tutorial_ciscocloud_url1.png)

    No **URL de início de sessão** caixa de texto, escreva um URL: `https://<subdomain>.cloudapps.cisco.com`
     
    > [!NOTE] 
    > Estes valores não estiverem reais. Atualize estes valores com o identificador real, URL de resposta e URL de início de sessão. Contacte [equipa de suporte de cliente de Cloud Cisco](mailto:cpr-ops@cisco.com) para obter estes valores. 

5. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-ciscocloud-tutorial/tutorial_general_400.png)

6. Para gerar o **URL de metadados**, execute os seguintes passos:

    a. Clique em **registos de aplicação**.
    
    ![Configurar o início de sessão único](./media/active-directory-saas-ciscocloud-tutorial/tutorial_ciscocloud_appregistrations.png)
   
    b. Clique em **pontos finais** para abrir **pontos finais** caixa de diálogo.  
    
    ![Configurar o início de sessão único](./media/active-directory-saas-ciscocloud-tutorial/tutorial_ciscocloud_endpointicon.png)

    c. Clique no botão Copiar para copiar **documento de METADADOS de Federação** url e cole-o bloco de notas.
    
    ![Configurar o início de sessão único](./media/active-directory-saas-ciscocloud-tutorial/tutorial_ciscocloud_endpoint.png)
     
    d. Agora, a página de propriedades do **Cisco nuvem** e copie o **ID da aplicação** utilizando **cópia** botão e cole-o bloco de notas.
 
    ![Configurar o início de sessão único](./media/active-directory-saas-ciscocloud-tutorial/tutorial_ciscocloud_appid.png)

    e. Gerar o **URL de metadados** utilizando o padrão do seguinte: `<FEDERATION METADATA DOCUMENT url>?appid=<application id>`

7. Para configurar o início de sessão único em **Cisco nuvem** lado, terá de enviar o transferido **URL de metadados** para [equipa de suporte de nuvem Cisco](mailto:cpr-ops@cisco.com). Se definir esta definição para que a ligação de SAML SSO corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/active-directory-saas-ciscocloud-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-ciscocloud-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/active-directory-saas-ciscocloud-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/active-directory-saas-ciscocloud-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-cisco-cloud-test-user"></a>Criar um utilizador de teste de nuvem Cisco

Nesta secção, vai criar um utilizador chamado Britta Simon na nuvem de Cisco. Trabalhar com [equipa de suporte de nuvem Cisco](mailto:cpr-ops@cisco.com) para adicionar os utilizadores na plataforma em nuvem da Cisco. Os utilizadores têm de ser criados e ativados antes de utilizar o início de sessão único

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso à nuvem de Cisco.

![Atribuir a função de utilizador][200] 

**Para atribuir Britta Simon Cisco nuvem, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. Na lista de aplicações, selecione **Cisco nuvem**.

    ![A ligação de nuvem Cisco na lista de aplicações](./media/active-directory-saas-ciscocloud-tutorial/tutorial_ciscocloud_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Teste o início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

Quando clica no mosaico de nuvem Cisco no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de nuvem Cisco.
Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ciscocloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ciscocloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ciscocloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ciscocloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ciscocloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ciscocloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ciscocloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ciscocloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ciscocloud-tutorial/tutorial_general_203.png

