---
title: 'Tutorial: Integração do Azure Active Directory com caixa | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e a caixa.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3b565c8d-35e2-482a-b2f4-bf8fd7d8731f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.openlocfilehash: f5aa724e9848c9794eef093aef15b0aaed9cae97
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435765"
---
# <a name="integrate-azure-active-directory-with-box"></a>Integrar o Azure Active Directory com caixa

Neste tutorial, saiba como integrar o Azure Active Directory (Azure AD) com caixa.

Com a integração do Azure AD com caixa, obtenha os seguintes benefícios:

- Pode controlar no Azure AD que tenha acesso à caixa.
- Pode permitir que os utilizadores é feita automaticamente a caixa (início de sessão único, ou SSO) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central, o portal do Azure.

Para saber mais sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com caixa, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma subscrição caixa SSO ativado

> [!NOTE]
> Quando testa os passos neste tutorial, é recomendável que faça *não* utilizar um ambiente de produção.

Para testar os passos neste tutorial, siga as seguintes recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. 

O cenário que é descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando a caixa da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="add-box-from-the-gallery"></a>Caixa de adicionar a partir da Galeria
Para configurar a integração do Azure AD com caixa, adicione caixa partir da Galeria à sua lista de aplicações geridas do SaaS, fazendo o seguinte:

1. Na [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione **Azure Active Directory**. 

    ![O botão do Azure Active Directory][1]

1. Selecione **aplicações empresariais** > **todas as aplicações**.

    ![A janela de "Aplicações empresariais"][2]
    
1. Para adicionar uma nova aplicação, selecione o **nova aplicação** botão na parte superior da janela.

    ![O "novo aplicativo" botão][3]

1. Na caixa de pesquisa, escreva **caixa**, selecione **caixa** na lista de resultados e, em seguida, selecione **Add**.

    ![Caixa na lista de resultados](./media/box-tutorial/tutorial_box_search.png)
### <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configurar e testar o Azure AD início de sessão único com caixa, com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa identificar o utilizador de caixa e sua contraparte no Azure AD. Em outras palavras, é necessário estabelecer uma relação de ligação entre um utilizador do Azure AD e o mesmo utilizador na caixa.

Para estabelecer a relação de ligação, atribuir como a caixa *nome de utilizador* o valor da *nome de utilizador* no Azure AD.

Para configurar e testar o Azure AD início de sessão único com caixa, conclua os blocos de construção nas seções a seguir cinco.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Ativar o Azure AD início de sessão único no portal do Azure e configurar o início de sessão único em seu aplicativo de caixa, fazendo o seguinte:

1. No portal do Azure, no **caixa** janela de integração de aplicativo, selecione **início de sessão único**.

    ![A ligação de "Início de sessão único"][4]

1. Na **início de sessão único** janela, na **modo de início de sessão único** caixa, selecione **baseado em SAML logon**.
 
    ![A janela de "Início de sessão único"](./media/box-tutorial/tutorial_box_samlbase.png)

1. Sob **caixa de domínio e URLs**, efetue o seguinte procedimento:

    !["Caixa domínio e URLs" únicas início de sessão em informações](./media/box-tutorial/url3.png)

    a. Na **URL de início de sessão** caixa, escreva um URL no seguinte formato: *https://\<subdomínio >. box.com*.

    b. Na **identificador** caixa de texto, tipo **box.net**.
     
    > [!NOTE] 
    > Os valores anteriores não são reais. Atualize-as com o URL de início de sessão real e o identificador. Para obter os valores, entre em contato com o [equipa de suporte de cliente de caixa](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire). 

1. Sob **certificado de assinatura SAML**, selecione **XML de metadados**e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/box-tutorial/tutorial_box_certificate.png) 

1. Selecione **Guardar**.

    ![Configurar o botão único início de sessão em Guardar](./media/box-tutorial/tutorial_general_400.png)
    
1. Para configurar o SSO para a sua aplicação, siga o procedimento [configurar o SSO no seu próprio](https://community.box.com/t5/How-to-Guides-for-Admins/Setting-Up-Single-Sign-On-SSO-for-your-Enterprise/ta-p/1263#ssoonyourown).

> [!NOTE] 
> Se não é possível ativar as definições de SSO para a sua conta de caixa, poderá ter de contactar o [equipa de suporte de cliente de caixa](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) e forneça o ficheiro transferido do XML.

> [!TIP]
> Como estamos configurando o aplicativo, pode ler uma versão concisa das instruções anteriores no [portal do Azure](https://portal.azure.com). Depois de adicionar a aplicação no **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador e, em seguida, aceder a documentação em Embedded a **configuração** seção na parte inferior. Para obter mais informações sobre a funcionalidade de documentação do embedded, consulte [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).
>

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

Nesta secção, vai criar o utilizador de teste Eduarda Almeida no portal do Azure, fazendo o seguinte:

![Criar um utilizador de teste do Azure AD][100]

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**.

    ![A ligação do Azure Active Directory](./media/box-tutorial/create_aaduser_01.png)

1. Para apresentar uma lista de utilizadores atuais, selecione **utilizadores e grupos** > **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/box-tutorial/create_aaduser_02.png)

1. Na parte superior a **todos os utilizadores** janela, selecione **Add**.

    ![Botão Adicionar](./media/box-tutorial/create_aaduser_03.png)

    O **utilizador** é aberta a janela.

1. Na **utilizador** janela, efetue o seguinte procedimento:

    ![A janela de utilizador](./media/box-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Selecione **Criar**.
 
### <a name="create-a-box-test-user"></a>Criar um utilizador de teste de caixa

Nesta secção, vai criar o utilizador de teste Eduarda Almeida na caixa. Caixa suporta o aprovisionamento de just-in-time, que está ativado por predefinição. Se um utilizador já não existir, uma nova é criada quando tentar acessar a caixa. Não é necessária ação do utilizador para criar o utilizador.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar utilizador Eduarda Almeida para utilizar o Azure início de sessão único ao conceder acesso à caixa. Para tal, faça o seguinte:

![Atribuir a função de utilizador][200]

1. No portal do Azure, abra a **aplicativos** modo de exibição, vá para o **diretório** ver e, em seguida, selecione **aplicações empresariais** > **todos os aplicativos**.

    ![A "Aplicações empresariais" e os links de "Todas as aplicações"][201] 

1. Na **aplicativos** lista, selecione **caixa**.

    ![A ligação de caixa](./media/box-tutorial/tutorial_box_app.png)  

1. No painel esquerdo, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Selecione **Add** e, em seguida, no **adicionar atribuição** painel, selecione **utilizadores e grupos**.

    ![O painel Adicionar atribuição][203]

1. Na **utilizadores e grupos** janela, na **utilizadores** lista, selecione **Eduarda Almeida**.

1. Selecione o **selecione** botão.

1. Na **adicionar atribuição** janela, selecione **atribuir**.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar seu única início de sessão em configuração do Azure AD utilizando o painel de acesso.

Quando seleciona a **caixa** mosaico no painel de acesso, é aberta a página de início de sessão para o início de sessão sua aplicação de caixa.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)
* [Configurar o aprovisionamento do utilizador](box-userprovisioning-tutorial.md)



<!--Image references-->

[1]: ./media/box-tutorial/tutorial_general_01.png
[2]: ./media/box-tutorial/tutorial_general_02.png
[3]: ./media/box-tutorial/tutorial_general_03.png
[4]: ./media/box-tutorial/tutorial_general_04.png

[100]: ./media/box-tutorial/tutorial_general_100.png

[200]: ./media/box-tutorial/tutorial_general_200.png
[201]: ./media/box-tutorial/tutorial_general_201.png
[202]: ./media/box-tutorial/tutorial_general_202.png
[203]: ./media/box-tutorial/tutorial_general_203.png

