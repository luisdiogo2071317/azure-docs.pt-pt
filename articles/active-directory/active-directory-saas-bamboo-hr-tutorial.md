---
title: 'Tutorial: Integração do Azure Active Directory com BambooHR | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e BambooHR.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f826b5d2-9c64-47df-bbbf-0adf9eb0fa71
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: jeedes
ms.openlocfilehash: c63a625c1eca7008c751d6904f182f76ee5f343b
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34349726"
---
# <a name="tutorial-azure-active-directory-integration-with-bamboohr"></a>Tutorial: Integração do Azure Active Directory com BambooHR

Neste tutorial, irá aprender a integrar BambooHR com o Azure Active Directory (Azure AD).

Integrar BambooHR com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao BambooHR.
- Pode permitir que os utilizadores automaticamente obter a sessão iniciada no BambooHR utilizando início de sessão único (SSO) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central, o portal do Azure.

Para obter mais informações sobre a integração de aplicações SaaS com o Azure AD, consulte [que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com BambooHR, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma subscrição BambooHR SSO ativado

> [!NOTE]
> Ao testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, siga estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação gratuita de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. 

O cenário em que este tutorial descreve consiste em dois blocos modulares principais:

1. Adicionar BambooHR a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="add-bamboohr-from-the-gallery"></a>Adicionar BambooHR a partir da Galeria
Para configurar a integração de BambooHR com o Azure AD, adicione BambooHR na Galeria à sua lista de aplicações SaaS geridas efetuando o seguinte procedimento:

1. No [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione **do Azure Active Directory**. 

    ![O botão do Azure Active Directory][1]

2. Selecione **aplicações empresariais** > **todas as aplicações**.

    ![O painel de aplicações da empresa][2]
    
3. Para adicionar uma aplicação, selecione **nova aplicação**.

    ![A aplicação"novo" botão][3]

4. Na caixa de pesquisa, escreva **BambooHR**. Na lista de resultados, selecione **BambooHR**e, em seguida, selecione **adicionar**.

    ![BambooHR na lista de resultados](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configurar e testar o SSO do Azure AD com BambooHR utilizando o utilizador de teste "Britta Simon."

Para SSO funcionar, do Azure AD tem de saber o que utilizador respetivo homólogo consta BambooHR. Por outras palavras, tem de estabelecer uma relação de ligação entre o utilizador do Azure AD e o utilizador relacionado no BambooHR.

Para estabelecer a relação de ligação na BambooHR, atribua o Azure AD **nome de utilizador** valor como o BambooHR **Username** valor.

Para configurar e testar o SSO do Azure AD com BambooHR, conclua os blocos modulares nas secções junto cinco.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, ativar o SSO do Azure AD no portal do Azure e configurar o SSO na sua aplicação BambooHR efetuando o seguinte procedimento:

1. No portal do Azure, no **BambooHR** página de integração de aplicações, selecione **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** janela, no **modo** na lista pendente, selecione **baseados em SAML início de sessão**.
 
    ![Janela de início de sessão único](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_samlbase.png)

3. Em **BambooHR domínio e os URLs**, efetue o seguinte procedimento:

    ![A secção BambooHR domínio e os URLs](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_url.png)

    a. No **iniciar sessão no URL** caixa, escreva um URL no seguinte formato: `https://<company>.bamboohr.com`.

    b. No **identificador** caixa, escreva um valor: `BambooHR-SAML`.

    > [!NOTE] 
    > O **iniciar sessão no URL** valor não é real. A atualização com o seu URL de início de sessão real. Para obter o valor, contacte o [equipa de suporte de cliente BambooHR](https://www.bamboohr.com/contact.php). 
 
4. Em **certificado de assinatura de SAML**, selecione **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![A hiperligação de transferência do certificado](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_certificate.png) 

5. Selecione **Guardar**.

    ![O botão Guardar](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_400.png)

6. Em **BambooHR configuração**, selecione **configurar BambooHR** para abrir o **configurar início de sessão** janela. No **referência rápida** secção, copie o **único início de sessão no URL do serviço SAML** para utilização posterior.

    ![Configuração de BambooHR](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_configure.png) 

7. Numa nova janela, inicie sessão no site da sua empresa BambooHR como administrador.

8. Na página inicial, efetue o seguinte:
   
    ![A página BambooHR Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/ic796691.png "Single Sign-On")   

    a. Selecione **aplicações**.
   
    b. No **aplicações** painel, selecione **Single Sign-On**.
   
    c. Selecione **SAML-início de sessão único**.

9. No **SAML Single Sign-On** painel, efetue o seguinte procedimento:
   
    ![Painel de SAML Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/IC796692.png "SAML Single Sign-On")
   
    a. Para o **Url de início de sessão de SSO** caixa, cole o **único início de sessão no URL do serviço SAML** que copiou do portal do Azure no passo 6.
      
    b. No bloco de notas, abra o certificado codificado base-64 que transferiu a partir do portal do Azure, copie o respetivo conteúdo e, em seguida, cole-o para o **certificado x. 509** caixa.
   
    c. Selecione **Guardar**.

> [!TIP]
> Enquanto estiver a configurar a aplicação, pode ler uma versão concisa destas instruções no [portal do Azure](https://portal.azure.com). Depois de adicionar a aplicação a partir de **do Active Directory** > **aplicações empresariais** secção, basta selecionar a **Single Sign-On** separador e, em seguida, aceder a incorporado documentação através de **configuração** secção na parte inferior. Para informações, consulte [do Azure AD incorporado documentação]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste chamado Britta Simon no portal do Azure.

   ![Criar utilizador de teste do Azure AD Britta Simon][100]

Para criar um utilizador de teste no Azure AD, efetue o seguinte:

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**.

    ![O botão do Azure Active Directory](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, selecione **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_02.png)

3. Na parte superior do **todos os utilizadores** painel, selecione **adicionar**.

    ![O botão de adição](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_03.png)

4. No **utilizador** janela, efetue o seguinte procedimento:

    ![A janela de utilizador](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Selecione **Criar**.
 
### <a name="create-a-bamboohr-test-user"></a>Criar um utilizador de teste BambooHR

Para ativar o Azure AD aos utilizadores iniciar sessão BambooHR, configurá-los manualmente no BambooHR efetuando o seguinte procedimento:

1. Inicie sessão no seu **BambooHR** site como um administrador.

2. Na barra de ferramentas na parte superior, selecione **definições**.
   
    ![No botão definições](./media/active-directory-saas-bamboo-hr-tutorial/IC796694.png "definição")

3. Selecione **Descrição geral**.

4. No painel esquerdo, selecione **segurança** > **utilizadores**.

5. Escreva o nome de utilizador, endereço de e-mail do AD do Azure válido e palavra-passe da conta que pretende configurar.

6. Selecione **Guardar**.
        
>[!NOTE]
>Para configurar contas de utilizador do Azure AD, também pode utilizar ferramentas de criação de contas de utilizador BambooHR ou APIs.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Permitir que o utilizador Britta Simon para utilizar o SSO do Azure ao conceder acesso para BambooHR.

![Atribuir a função de utilizador][200] 

Para atribuir o utilizador Britta Simon a BambooHR, efetue o seguinte:

1. No portal do Azure, abra a vista de aplicações, aceda à vista de diretório e, em seguida, selecione **aplicações empresariais** > **todas as aplicações**.

    ![Atribua o utilizador][201] 

2. No **aplicações empresariais** lista, selecione **BambooHR**.

    ![A ligação de BambooHR na lista de aplicações da empresa](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_app.png)  

3. No painel esquerdo, selecione **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Selecione o **adicionar** botão e, em seguida, no **adicionar atribuição** painel, selecione **utilizadores e grupos**.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** janela, no **utilizadores** lista, selecione **Britta Simon**.

6. Selecione o **selecione** botão.

7. No **adicionar atribuição** janela, selecione o **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Teste a configuração de SSO do Azure AD utilizando o painel de acesso.

Quando seleciona o **BambooHR** na peça de mosaico do painel de acesso, que deve obter automaticamente sessão na sua aplicação BambooHR.

Para mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS no Azure Active Directory](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_203.png

