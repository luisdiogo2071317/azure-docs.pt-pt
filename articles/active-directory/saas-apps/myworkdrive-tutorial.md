---
title: 'Tutorial: Integração do Azure Active Directory com MyWorkDrive | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e MyWorkDrive.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4d049778-3c7b-46c0-92a4-f2633a32334b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: jeedes
ms.openlocfilehash: 7644a8517840b4fdffe0bc47c5a9bb97d48f6322
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51686801"
---
# <a name="tutorial-azure-active-directory-integration-with-myworkdrive"></a>Tutorial: Integração do Azure Active Directory com MyWorkDrive

Neste tutorial, saiba como integrar a MyWorkDrive no Azure Active Directory (Azure AD).

Integrar a MyWorkDrive no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD, quem tem acesso a MyWorkDrive.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada a MyWorkDrive (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com MyWorkDrive, terá dos seguintes itens:

- Uma subscrição do Azure
- Um MyWorkDrive logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando a MyWorkDrive da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-myworkdrive-from-the-gallery"></a>Adicionando a MyWorkDrive da Galeria

Para configurar a integração da MyWorkDrive no Azure AD, terá de adicionar MyWorkDrive a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar MyWorkDrive a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

4. Na caixa de pesquisa, escreva **MyWorkDrive**, selecione **MyWorkDrive** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![MyWorkDrive na lista de resultados](./media/myworkdrive-tutorial/tutorial_myworkdrive_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com MyWorkDrive com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no MyWorkDrive para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no MyWorkDrive deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com MyWorkDrive, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste da MyWorkDrive](#creating-a-myworkdrive-test-user)**  - para ter um equivalente da Eduarda Almeida na MyWorkDrive que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo MyWorkDrive.

**Para configurar o Azure AD início de sessão único com MyWorkDrive, execute os seguintes passos:**

1. No portal do Azure, sobre o **MyWorkDrive** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **selecionar um método de início de sessão único** caixa de diálogo, clique em **selecione** para **SAML** modo para ativar o início de sessão único.

    ![Configurar o início de sessão único](common/tutorial_general_301.png)

3. Sobre o **definir a segurança de início de sessão único com o SAML** página, clique em **editar** ícone para abrir **configuração básica de SAML** caixa de diálogo.

    ![Configurar o início de sessão único](common/editconfigure.png)

4. Sobre o **configuração básica de SAML** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![MyWorkDrive domínio e URLs únicas início de sessão em informações](./media/myworkdrive-tutorial/tutorial_myworkdrive_url.png)

    Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<SERVER.DOMAIN.COM>/SAML/AssertionConsumerService.aspx`

5. Clique em **definir URLs adicionais** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![MyWorkDrive domínio e URLs únicas início de sessão em informações](./media/myworkdrive-tutorial/tutorial_myworkdrive_url1.png)

     Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<SERVER.DOMAIN.COM>/Account/Login-saml` 

    > [!NOTE]
    > Estes valores não são reais. Atualize estes valores com o URL de resposta real e o URL de início de sessão.  Entrada name:e.g de anfitrião do servidor da MyWorkDrive da sua própria empresa.
    > 
    > URL de resposta: `https://yourserver.yourdomain.com/SAML/AssertionConsumerService.aspx`
    > 
    > URL de início de sessão:`https://yourserver.yourdomain.com/Account/Login-saml`
    > 
    > Contacte a equipa de suporte de cliente da MyWorkDrive se não souber como configurar o seu próprio nome de anfitrião e o certificado SSL para esses valores.

6. No **certificado de assinatura SAML** página, além do **certificado de assinatura SAML** secção, clique em Copiar **ícone** para copiar **Url de metadados de Federação de aplicação**e guarde-o no seu computador....

    ![O link de download de certificado](./media/myworkdrive-tutorial/tutorial_myworkdrive_certificate.png)

7. Numa janela do browser web diferente, início de sessão para MyWorkDrive como um administrador de segurança.

8. No servidor MyWorkDrive no painel administração, clique em **ENTERPRISE** e execute os seguintes passos:

    ![O administrador](./media/myworkdrive-tutorial/tutorial_myworkdrive_admin.png)

    a. Ativar **SSO SAML/ADFS**.

    b. Selecione **SAML - Azure AD**

    c. Na **Url de metadados de Federação de aplicação do Azure** caixa de texto, cole o valor de **Url de metadados de Federação de aplicação** que copiou do portal do Azure.

    d. Clicar em **Guardar**

    >[!NOTE]
    >Para informações adicionais, veja a [artigo de suporte do Azure AD de MyWorkDrive](https://www.myworkdrive.com/support/saml-single-sign-on-azure-ad/).

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**, selecione **utilizadores**e, em seguida, selecione **todos os utilizadores**.

    ![Criar utilizador do Azure AD][100]

2. Selecione **novo utilizador** na parte superior do ecrã.

    ![Criar um utilizador de teste do Azure AD](common/create_aaduser_01.png) 

3. Nas propriedades do utilizador, execute os seguintes passos.

    ![Criar um utilizador de teste do Azure AD](common/create_aaduser_02.png)

    a. Na **Name** , insira **BrittaSimon**.
  
    b. Na **nome de utilizador** , digite **brittasimon@yourcompanydomain.extension**  
    Por exemplo, BrittaSimon@contoso.com

    c. Selecione **propriedades**, selecione a **palavra-passe de Show** caixa de verificação e, em seguida, anote o valor que é apresentado na caixa de palavra-passe.

    d. Selecione **Criar**.

### <a name="creating-a-myworkdrive-test-user"></a>Criar um utilizador de teste da MyWorkDrive

Nesta secção, vai criar um usuário chamado Eduarda Almeida no MyWorkDrive. Trabalhar com [equipa de suporte da MyWorkDrive](mailto:support@myworkdrive.com) para adicionar os utilizadores na plataforma da MyWorkDrive. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso a MyWorkDrive.

1. No portal do Azure, selecione **aplicações empresariais**, selecione **todos os aplicativos**.

    ![Atribuir utilizador][201]

2. Na lista de aplicações, selecione **MyWorkDrive**.

    ![Configurar o início de sessão único](./media/myworkdrive-tutorial/tutorial_myworkdrive_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. Na **utilizadores e grupos** caixa de diálogo select **Eduarda Almeida** na lista de utilizadores, em seguida, clique o **selecionar** na parte inferior do ecrã.

6. Na **adicionar atribuição** caixa de diálogo select a **atribuir** botão.

### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico da MyWorkDrive no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação da MyWorkDrive.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
