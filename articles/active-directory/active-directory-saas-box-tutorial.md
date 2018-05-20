---
title: 'Tutorial: Integrar do Azure Active Directory com caixa | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e caixa.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3b565c8d-35e2-482a-b2f4-bf8fd7d8731f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/8/2017
ms.author: jeedes
ms.openlocfilehash: daad9104798dc02b479b4e022287c3630e4a67a0
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="integrate-azure-active-directory-with-box"></a>Integrar o Azure Active Directory com caixa

Neste tutorial, irá aprender a integração do Azure Active Directory (Azure AD) com caixa.

Através da integração do Azure AD com caixa, obtenha as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à caixa.
- Pode permitir aos utilizadores obter a sessão iniciada no automaticamente caixa (o início de sessão único, ou SSO) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central, o portal do Azure.

Para saber mais sobre a integração de aplicações SaaS com o Azure AD, consulte [que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com caixa, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma subscrição caixa SSO ativado

> [!NOTE]
> Quando o testar os passos neste tutorial, recomendamos que efetue *não* utilizar um ambiente de produção.

Para testar os passos neste tutorial, siga estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. 

O cenário é descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar caixa da galeria do
2. Configurar e testar o Azure AD de sessão único-

## <a name="add-box-from-the-gallery"></a>Caixa de adicionar a partir da Galeria
Para configurar a integração do Azure AD com caixa, adicione caixa da Galeria à sua lista de aplicações SaaS geridas efetuando o seguinte procedimento:

1. No [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione **do Azure Active Directory**. 

    ![O botão do Azure Active Directory][1]

2. Selecione **aplicações empresariais** > **todas as aplicações**.

    ![A janela de "Aplicações da empresa"][2]
    
3. Para adicionar uma nova aplicação, selecione o **nova aplicação** botão na parte superior da janela.

    ![A aplicação"novo" botão][3]

4. Na caixa de pesquisa, escreva **caixa**, selecione **caixa** na lista de resultados e, em seguida, selecione **adicionar**.

    ![Caixa de lista de resultados](./media/active-directory-saas-box-tutorial/tutorial_box_search.png)
### <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configurar e testar o Azure AD-início de sessão único com caixa, com base num utilizador de teste chamado "Britta Simon."

Para início de sessão trabalhar, do Azure AD tem de identificar o utilizador caixa e o respetivo homólogo no Azure AD. Por outras palavras, tem de ser estabelecida uma relação de ligação entre um utilizador do Azure AD e o mesmo utilizador na caixa.

Para estabelecer a relação de ligação, atribua a caixa *Username* o valor da *nome de utilizador* no Azure AD.

Para configurar e testar o Azure AD-início de sessão único com caixa, conclua os blocos modulares nas secções junto cinco.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Ativar o Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua aplicação de caixa, efetuando o seguinte procedimento:

1. No portal do Azure, no **caixa** janela de integração de aplicações, selecione **de sessão único-**.

    ![A hiperligação "De sessão único"][4]

2. No **de sessão único-** janela, no **modo de início de sessão único** caixa, selecione **baseados em SAML início de sessão**.
 
    ![A janela "De sessão único"](./media/active-directory-saas-box-tutorial/tutorial_box_samlbase.png)

3. Em **caixa domínio e os URLs**, efetue o seguinte procedimento:

    !["Caixa domínios e URLs" único informações de início de sessão](./media/active-directory-saas-box-tutorial/url3.png)

    a. No **URL de início de sessão** caixa, escreva um URL no seguinte formato: *https://\<subdomínio >. box.com*.

    b. No **identificador** caixa de texto, tipo **box.net**.
     
    > [!NOTE] 
    > Os valores anteriores não são reais. Atualize-as com o URL de início de sessão real e o identificador. Para obter os valores, contacte o [equipa de suporte de cliente caixa](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire). 

4. Em **certificado de assinatura de SAML**, selecione **XML de metadados**e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-box-tutorial/tutorial_box_certificate.png) 

5. Selecione **Guardar**.

    ![Configurar botão único início de sessão guardar](./media/active-directory-saas-box-tutorial/tutorial_general_400.png)
    
6. Para configurar o SSO para a sua aplicação, siga o procedimento [configurar o SSO no seu próprio](https://community.box.com/t5/How-to-Guides-for-Admins/Setting-Up-Single-Sign-On-SSO-for-your-Enterprise/ta-p/1263#ssoonyourown).

> [!NOTE] 
> Se não é possível ativar as definições de SSO para a sua conta de caixa, poderá ter de contactar o [equipa de suporte de cliente caixa](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) e forneça o ficheiro XML transferido.

> [!TIP]
> Como está a configurar a aplicação, pode ler uma versão das instruções anteriores no concisa o [portal do Azure](https://portal.azure.com). Depois de adicionar a aplicação no **do Active Directory** > **aplicações empresariais** secção, selecione o **Single Sign-On** separador e, em seguida, aceder a incorporado documentação o **configuração** secção na parte inferior. Para obter mais informações sobre a funcionalidade de documentação embedded, consulte [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985).
>

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

Nesta secção, criar o utilizador de teste Britta Simon no portal do Azure da seguinte forma:

![Criar um utilizador de teste do Azure AD][100]

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**.

    ![A ligação do Azure Active Directory](./media/active-directory-saas-box-tutorial/create_aaduser_01.png)

2. Para apresentar uma lista de utilizadores atuais, selecione **utilizadores e grupos** > **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-box-tutorial/create_aaduser_02.png)

3. Na parte superior do **todos os utilizadores** janela, selecione **adicionar**.

    ![O botão de adição](./media/active-directory-saas-box-tutorial/create_aaduser_03.png)

    O **utilizador** é aberta a janela.

4. No **utilizador** janela, efetue o seguinte procedimento:

    ![A janela de utilizador](./media/active-directory-saas-box-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Selecione **Criar**.
 
### <a name="create-a-box-test-user"></a>Criar um utilizador de teste de caixa

Nesta secção, vai criar o utilizador de teste Britta Simon na caixa. Caixa suporta o aprovisionamento de just-in-time, que está ativada por predefinição. Se um utilizador já não existir, uma nova é criada quando tentar aceder à caixa. Não tem de fazer nada, para criar o utilizador.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar utilizador Britta Simon para utilizar o Azure-início de sessão único, concedendo acesso à caixa. Para tal, faça o seguinte:

![Atribuir a função de utilizador][200]

1. No portal do Azure, abra o **aplicações** vista, aceda ao **diretório** visualizar e, em seguida, selecione **aplicações empresariais** > **todos os aplicações**.

    ![O "As aplicações empresariais" e ligações de "Todas as aplicações"][201] 

2. No **aplicações** lista, selecione **caixa**.

    ![A ligação de caixa](./media/active-directory-saas-box-tutorial/tutorial_box_app.png)  

3. No painel esquerdo, selecione **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Selecione **adicionar** e, em seguida, no **adicionar atribuição** painel, selecione **utilizadores e grupos**.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** janela, no **utilizadores** lista, selecione **Britta Simon**.

6. Selecione o **selecione** botão.

7. No **adicionar atribuição** janela, selecione **atribuir**.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, teste configuração do Azure AD único início de sessão utilizando o painel de acesso.

Quando seleciona o **caixa** mosaico no painel de acesso, pode abrir a página de início de sessão para início de sessão na sua aplicação de caixa.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre a integração de aplicações SaaS no Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md)
* [Configurar o aprovisionamento de utilizadores](active-directory-saas-box-userprovisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-box-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-box-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-box-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-box-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-box-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-box-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-box-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-box-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-box-tutorial/tutorial_general_203.png

