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
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2018
ms.author: jeedes
ms.openlocfilehash: 2103c5c8c08a6aebfc1168c8fbdb4181dbe3a997
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047838"
---
# <a name="tutorial-azure-active-directory-integration-with-myworkdrive"></a>Tutorial: Integração do Azure Active Directory com MyWorkDrive

Neste tutorial, saiba como integrar a MyWorkDrive no Azure Active Directory (Azure AD).

Integrar a MyWorkDrive no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD, quem tem acesso a MyWorkDrive.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada a MyWorkDrive (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

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

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste da MyWorkDrive](#create-a-myworkdrive-test-user)**  - para ter um equivalente da Eduarda Almeida na MyWorkDrive que está ligado à representação de utilizador do Azure AD.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo MyWorkDrive.

**Para configurar o Azure AD início de sessão único com MyWorkDrive, execute os seguintes passos:**

1. No portal do Azure, sobre o **MyWorkDrive** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/myworkdrive-tutorial/tutorial_myworkdrive_samlbase.png)

3. Sobre o **MyWorkDrive domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![MyWorkDrive domínio e URLs únicas início de sessão em informações](./media/myworkdrive-tutorial/tutorial_myworkdrive_url.png)

    Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<SERVER.DOMAIN.COM>/SAML/AssertionConsumerService.aspx`

4. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![MyWorkDrive domínio e URLs únicas início de sessão em informações](./media/myworkdrive-tutorial/tutorial_myworkdrive_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<SERVER.DOMAIN.COM>/Account/Login-saml`
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de resposta real e o URL de início de sessão. Contacte [equipa de suporte de cliente da MyWorkDrive](mailto:support@myworkdrive.com) obter esses valores. 

5. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/myworkdrive-tutorial/tutorial_myworkdrive_certificate.png) 

6. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/myworkdrive-tutorial/tutorial_general_400.png)
    
7. Na **configuração da MyWorkDrive** secção, clique em **configurar MyWorkDrive** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração da MyWorkDrive](./media/myworkdrive-tutorial/tutorial_myworkdrive_configure.png) 

8. Para configurar o início de sessão único num **MyWorkDrive** lado, terá de enviar o transferido **certificado (Base64)**, **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço**para [equipa de suporte da MyWorkDrive](mailto:support@myworkdrive.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/myworkdrive-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/myworkdrive-tutorial/create_aaduser_02.png)

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/myworkdrive-tutorial/create_aaduser_03.png)

4. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/myworkdrive-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-myworkdrive-test-user"></a>Criar um utilizador de teste da MyWorkDrive

Nesta secção, vai criar um usuário chamado Eduarda Almeida no MyWorkDrive. Trabalhar com [equipa de suporte da MyWorkDrive](mailto:support@myworkdrive.com) para adicionar os utilizadores na plataforma da MyWorkDrive. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso a MyWorkDrive.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a MyWorkDrive, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **MyWorkDrive**.

    ![A ligação da MyWorkDrive na lista de aplicações](./media/myworkdrive-tutorial/tutorial_myworkdrive_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico da MyWorkDrive no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação da MyWorkDrive.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/myworkdrive-tutorial/tutorial_general_01.png
[2]: ./media/myworkdrive-tutorial/tutorial_general_02.png
[3]: ./media/myworkdrive-tutorial/tutorial_general_03.png
[4]: ./media/myworkdrive-tutorial/tutorial_general_04.png

[100]: ./media/myworkdrive-tutorial/tutorial_general_100.png

[200]: ./media/myworkdrive-tutorial/tutorial_general_200.png
[201]: ./media/myworkdrive-tutorial/tutorial_general_201.png
[202]: ./media/myworkdrive-tutorial/tutorial_general_202.png
[203]: ./media/myworkdrive-tutorial/tutorial_general_203.png

