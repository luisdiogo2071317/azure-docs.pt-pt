---
title: 'Tutorial: Integração do Azure Active Directory com BambooHR | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e BambooHR.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f826b5d2-9c64-47df-bbbf-0adf9eb0fa71
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: jeedes
ms.openlocfilehash: 77625296797ec8ed8364e7d8bff3e5a15b4b74b5
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048043"
---
# <a name="tutorial-azure-active-directory-integration-with-bamboohr"></a>Tutorial: Integração do Azure Active Directory com BambooHR

Neste tutorial, saiba como integrar BambooHR com o Azure Active Directory (Azure AD).

Integrar BambooHR no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao BambooHR.
- Pode permitir que os utilizadores automaticamente obter tiver sessão iniciada no BambooHR com o início de sessão único (SSO) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central, o portal do Azure.

Para obter mais informações sobre a integração de aplicações SaaS com o Azure AD, consulte [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com BambooHR, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma subscrição BambooHR SSO ativado

> [!NOTE]
> Quando testa os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, siga as seguintes recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obter uma avaliação gratuita de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. 

O cenário que descreve este tutorial consiste em dois blocos de construção principais:

1. Adicionando BambooHR da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="add-bamboohr-from-the-gallery"></a>Adicionar BambooHR a partir da Galeria
Para configurar a integração do BambooHR com o Azure AD, adicione BambooHR partir da Galeria à sua lista de aplicações geridas do SaaS, fazendo o seguinte:

1. Na [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione **Azure Active Directory**. 

    ![O botão do Azure Active Directory][1]

2. Selecione **aplicações empresariais** > **todas as aplicações**.

    ![O painel de aplicações empresariais][2]
    
3. Para adicionar uma aplicação, selecione **nova aplicação**.

    ![O "novo aplicativo" botão][3]

4. Na caixa de pesquisa, escreva **BambooHR**. Na lista de resultados, selecione **BambooHR**e, em seguida, selecione **Add**.

    ![BambooHR na lista de resultados](./media/bamboo-hr-tutorial/tutorial_bamboohr_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configurar e testar o SSO do Azure AD com BambooHR usando o utilizador de teste "Eduarda Almeida."

Para SSO para funcionar, o Azure AD precisa saber qual é utilizador sua contraparte no BambooHR. Em outras palavras, tem de estabelecer uma relação de ligação entre o utilizador do Azure AD e o utilizador relacionado no BambooHR.

Para estabelecer a relação de ligação no BambooHR, atribua o Azure AD **nome de utilizador** valor como o BambooHR **Username** valor.

Para configurar e testar o SSO do Azure AD com BambooHR, conclua os blocos de construção nas seções a seguir cinco.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, ativar o SSO do Azure AD no portal do Azure e configurar o SSO em seu aplicativo BambooHR efetuando o seguinte procedimento:

1. No portal do Azure, sobre o **BambooHR** página de integração de aplicações, selecione **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

2. Na **início de sessão único** janela, na **modo** na lista pendente, selecione **baseado em SAML logon**.
 
    ![Janela de início de sessão única](./media/bamboo-hr-tutorial/tutorial_bamboohr_samlbase.png)

3. Sob **BambooHR domínio e URLs**, efetue o seguinte procedimento:

    ![A secção BambooHR domínio e URLs](./media/bamboo-hr-tutorial/tutorial_bamboohr_url.png)

    a. Na **iniciar sessão no URL** caixa, escreva um URL no seguinte formato: `https://<company>.bamboohr.com`.

    b. Na **identificador** , escreva um valor: `BambooHR-SAML`.

    > [!NOTE] 
    > O **iniciar sessão no URL** valor não é real. Atualize-o com o seu URL de início de sessão real. Para obter o valor, entre em contato com o [equipa de suporte de cliente BambooHR](https://www.bamboohr.com/contact.php). 
 
4. Sob **certificado de assinatura SAML**, selecione **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/bamboo-hr-tutorial/tutorial_bamboohr_certificate.png) 

5. Selecione **Guardar**.

    ![O botão salvar](./media/bamboo-hr-tutorial/tutorial_general_400.png)

6. Sob **BambooHR Configuration**, selecione **configurar BambooHR** para abrir o **configurar início de sessão** janela. Na **referência rápida** secção, copie a **SAML único início de sessão no URL do serviço** para utilização posterior.

    ![Configuração de BambooHR](./media/bamboo-hr-tutorial/tutorial_bamboohr_configure.png) 

7. Numa nova janela, inicie sessão no site da sua empresa BambooHR como um administrador.

8. Na página inicial, faça o seguinte:
   
    ![A página BambooHR Single Sign-On](./media/bamboo-hr-tutorial/ic796691.png "início de sessão único")   

    a. Selecione **aplicações**.
   
    b. Na **aplicações** painel, selecione **Single Sign-On**.
   
    c. Selecione **SAML início de sessão único**.

9. Na **SAML Single Sign-On** painel, faça o seguinte:
   
    ![O painel de SAML Single Sign-On](./media/bamboo-hr-tutorial/IC796692.png "SAML Single Sign-On")
   
    a. Para o **Url de início de sessão de SSO** caixa, cole a **SAML único início de sessão no URL do serviço** que copiou do portal do Azure no passo 6.
      
    b. No bloco de notas, abra o certificado com codificação base 64 que transferiu a partir do portal do Azure, copie o seu conteúdo e, em seguida, cole-o para o **certificado X.509** caixa.
   
    c. Selecione **Guardar**.

> [!TIP]
> Enquanto estiver a configurar a aplicação, pode ler uma versão concisa destas instruções no [portal do Azure](https://portal.azure.com). Depois de adicionar a aplicação a partir do **do Active Directory** > **aplicações empresariais** secção, simplesmente selecione o **Single Sign-On** separador e, em seguida, aceder a documentação por meio do Embedded a **configuração** seção na parte inferior. Para obter informações, consulte [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste chamado Eduarda Almeida no portal do Azure.

   ![Criar utilizador de teste do Azure AD Eduarda Almeida][100]

Para criar um utilizador de teste no Azure AD, faça o seguinte:

1. No portal do Azure, no painel esquerdo, selecione **do Azure Active Directory**.

    ![O botão do Azure Active Directory](./media/bamboo-hr-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, selecione **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/bamboo-hr-tutorial/create_aaduser_02.png)

3. Na parte superior a **todos os utilizadores** painel, selecione **Add**.

    ![Botão Adicionar](./media/bamboo-hr-tutorial/create_aaduser_03.png)

4. Na **utilizador** janela, efetue o seguinte procedimento:

    ![A janela de utilizador](./media/bamboo-hr-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Selecione **Criar**.
 
### <a name="create-a-bamboohr-test-user"></a>Criar um utilizador de teste BambooHR

Para ativar a utilizadores do Azure AD iniciar sessão no BambooHR, configurá-los manualmente em BambooHR efetuando o seguinte procedimento:

1. Inicie sessão no seu **BambooHR** site como um administrador.

2. Na barra de ferramentas na parte superior, selecione **definições**.
   
    ![O botão configurações](./media/bamboo-hr-tutorial/IC796694.png "definição")

3. Selecione **Descrição geral**.

4. No painel esquerdo, selecione **Security** > **utilizadores**.

5. Escreva o nome de utilizador, palavra-passe e o endereço de e-mail do Azure AD válida da conta que pretende configurar.

6. Selecione **Guardar**.
        
>[!NOTE]
>Para configurar contas de utilizador do Azure AD, também pode utilizar ferramentas de criação da conta de usuário BambooHR ou APIs.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Permitir que o utilizador Eduarda Almeida para utilizar o SSO do Azure ao conceder acesso para BambooHR.

![Atribuir a função de utilizador][200] 

Para atribuir utilizador Eduarda Almeida a BambooHR, faça o seguinte:

1. No portal do Azure, abrir a vista de aplicações, vá para a vista de diretório e, em seguida, selecione **aplicações empresariais** > **todas as aplicações**.

    ![Atribuir utilizador][201] 

2. Na **aplicações empresariais** lista, selecione **BambooHR**.

    ![A ligação de BambooHR na lista de aplicações empresariais](./media/bamboo-hr-tutorial/tutorial_bamboohr_app.png)  

3. No painel esquerdo, selecione **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

4. Selecione o **Add** botão e, em seguida, no **adicionar atribuição** painel, selecione **utilizadores e grupos**.

    ![O painel Adicionar atribuição][203]

5. Na **utilizadores e grupos** janela, na **utilizadores** lista, selecione **Eduarda Almeida**.

6. Selecione o **selecione** botão.

7. Na **adicionar atribuição** janela, selecione a **atribuir** botão.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Teste a configuração de SSO do Azure AD utilizando o painel de acesso.

Quando seleciona a **BambooHR** mosaico no painel de acesso, deve obter automaticamente a sessão iniciada sua aplicação BambooHR.

Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bamboo-hr-tutorial/tutorial_general_01.png
[2]: ./media/bamboo-hr-tutorial/tutorial_general_02.png
[3]: ./media/bamboo-hr-tutorial/tutorial_general_03.png
[4]: ./media/bamboo-hr-tutorial/tutorial_general_04.png

[100]: ./media/bamboo-hr-tutorial/tutorial_general_100.png

[200]: ./media/bamboo-hr-tutorial/tutorial_general_200.png
[201]: ./media/bamboo-hr-tutorial/tutorial_general_201.png
[202]: ./media/bamboo-hr-tutorial/tutorial_general_202.png
[203]: ./media/bamboo-hr-tutorial/tutorial_general_203.png

