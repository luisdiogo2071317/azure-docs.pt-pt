---
title: 'Tutorial: Integração do Azure Active Directory com mindWireless | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e mindWireless.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bd00a339-27c9-4904-b66f-a95bf597ac3c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2018
ms.author: jeedes
ms.openlocfilehash: 6c6fe0a720795c67a7062f5a5971c699472fca07
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39434351"
---
# <a name="tutorial-azure-active-directory-integration-with-mindwireless"></a>Tutorial: Integração do Azure Active Directory com mindWireless

Neste tutorial, saiba como integrar mindWireless com o Azure Active Directory (Azure AD).

Integrar mindWireless no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao mindWireless.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para mindWireless (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com mindWireless, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um mindWireless logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando mindWireless da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-mindwireless-from-the-gallery"></a>Adicionando mindWireless da Galeria
Para configurar a integração do mindWireless com o Azure AD, terá de adicionar mindWireless a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar mindWireless a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **mindWireless**, selecione **mindWireless** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![mindWireless na lista de resultados](./media/mindwireless-tutorial/tutorial_mindwireless_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com mindWireless com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no mindWireless a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no mindWireless deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com mindWireless, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste mindWireless](#create-a-mindwireless-test-user)**  - para ter um equivalente da Eduarda Almeida na mindWireless que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo mindWireless.

**Para configurar o Azure AD início de sessão único com mindWireless, execute os seguintes passos:**

1. No portal do Azure, sobre o **mindWireless** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/mindwireless-tutorial/tutorial_mindwireless_samlbase.png)

1. Sobre o **mindWireless domínio e URLs** secção, execute os seguintes passos:

    ![mindWireless URLs de domínio e única informações de início de sessão](./media/mindwireless-tutorial/tutorial_mindwireless_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.mwsmart.com/`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.mwsmart.com/SAML/AssertionConsumerService.aspx`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador real e o URL de resposta. Contacte [equipa de suporte de mindWireless](mailto:sdulloor@mindwireless.com) obter esses valores.

1. O aplicativo mindWireless espera que as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizado à sua configuração de atributos de token SAML.

1. Captura de ecrã seguinte mostra um exemplo para o mesmo. O nome da afirmação sempre ser **ID de funcionário** e o valor dos quais podemos mapeou para user.employeeid, que contém o campo IDdeEmpregado do utilizador. Aqui o mapeamento de utilizador do Azure AD para mindWireless é feito a EmployeeID, mas pode mapeá-lo para um valor diferente que também com base nas suas definições de aplicação. Pode trabalhar com o [equipa de suporte de mindWireless](mailto:sdulloor@mindwireless.com) primeiro para utilizar o identificador correto de um usuário e mapear esse valor com o **identificação do funcionário** de afirmação.

    ![Configurar o início de sessão único](./media/mindwireless-tutorial/tutorial_attribute.png)

1. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem anterior e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo | Valor de espaço de nomes |
    | -------------- | --------------- | ----------------|
    | ID do Empregado | User.employeeid | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/mindwireless-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/mindwireless-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Na **espaço de nomes** caixa de texto, digite o valor de espaço de nomes mostrado para essa linha.
    
    e. Clique em **OK**.
    
1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/mindwireless-tutorial/tutorial_mindwireless_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/mindwireless-tutorial/tutorial_general_400.png)

1. Sobre o **mindWireless Configuration** secção, clique em **configurar mindWireless** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![mindWireless configuração](./media/mindwireless-tutorial/tutorial_mindwireless_configure.png) 

1. Para configurar o início de sessão único num **mindWireless** lado, terá de enviar o transferido **Certificate(Base64), SAML único início de sessão no URL do serviço**, e **ID de entidade de SAML** para [equipa de suporte de mindWireless](mailto:sdulloor@mindwireless.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/mindwireless-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/mindwireless-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/mindwireless-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/mindwireless-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-a-mindwireless-test-user"></a>Criar um utilizador de teste mindWireless

Nesta secção, vai criar um usuário chamado Eduarda Almeida no mindWireless. Trabalhar com [equipa de suporte de mindWireless](mailto:sdulloor@mindwireless.com) para adicionar os utilizadores na plataforma mindWireless. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso a mindWireless.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a mindWireless, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **mindWireless**.

    ![A ligação de mindWireless na lista de aplicações](./media/mindwireless-tutorial/tutorial_mindwireless_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico mindWireless no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo mindWireless.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/mindwireless-tutorial/tutorial_general_01.png
[2]: ./media/mindwireless-tutorial/tutorial_general_02.png
[3]: ./media/mindwireless-tutorial/tutorial_general_03.png
[4]: ./media/mindwireless-tutorial/tutorial_general_04.png

[100]: ./media/mindwireless-tutorial/tutorial_general_100.png

[200]: ./media/mindwireless-tutorial/tutorial_general_200.png
[201]: ./media/mindwireless-tutorial/tutorial_general_201.png
[202]: ./media/mindwireless-tutorial/tutorial_general_202.png
[203]: ./media/mindwireless-tutorial/tutorial_general_203.png

