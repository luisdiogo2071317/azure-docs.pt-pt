---
title: 'Tutorial: Integração do Active Directory do Azure com transferência de MOVEit - integração do Azure AD | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e transferência de MOVEit - integração do Azure AD.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 37ebd8c07b71cbc8ad9697c7019032f2bc4ffd2b
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54826943"
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a>Tutorial: Integração do Active Directory do Azure com transferência de MOVEit - integração do Azure AD

Neste tutorial, saiba como integrar a transferência de MOVEit - integração do Azure AD com o Azure Active Directory (Azure AD).

Integração de transferência de MOVEit - integração do Azure AD com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD, quem tem acesso a transferência de MOVEit - integração do Azure AD.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para transferência de MOVEit - integração do Azure AD (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do AD do Azure com transferência de MOVEit - integração do Azure AD, terá dos seguintes itens:

- Uma subscrição do Azure
- Uma transferência de MOVEit - Azure AD integração início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adição de transferência de MOVEit - integração do Azure AD da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-moveit-transfer---azure-ad-integration-from-the-gallery"></a>Adição de transferência de MOVEit - integração do Azure AD da Galeria
Para configurar a integração de transferência de MOVEit - integração do Azure AD para o Azure AD, terá de adicionar a transferência de MOVEit - integração do AD do Azure da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar a transferência de MOVEit - integração do Azure AD a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **MOVEit transferência - integração do Azure AD**, selecione **MOVEit transferência - integração do Azure AD** no painel de resultados, em seguida, clique em **Add** botão para adicionar o aplicação.

    ![Transferência de MOVEit - integração do Azure AD na lista de resultados](./media/moveittransfer-tutorial/tutorial_moveittransfer_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com transferência de MOVEit - integração do Azure AD com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de equivalente na transferência de MOVEit - integração do Azure AD para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na transferência de MOVEit - integração do Azure AD tem de ser estabelecida.

Na transferência de MOVEit - integração do Azure AD, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com transferência de MOVEit - integração do Azure AD, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar uma transferência de MOVEit - utilizador de teste de integração do Azure AD](#create-a-moveit-transfer---azure-ad-integration-test-user)**  - para ter um equivalente da Eduarda Almeida na transferência de MOVEit - integração do Azure AD que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua transferência MOVEit - aplicação de integração do Azure AD.

**Para configurar o Azure AD início de sessão único com transferência de MOVEit - integração do Azure AD, execute os seguintes passos:**

1. No portal do Azure, sobre o **MOVEit transferência - integração do Azure AD** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/moveittransfer-tutorial/tutorial_moveittransfer_samlbase.png)

1. Sobre o **MOVEit transferência - integração do Azure AD domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/moveittransfer-tutorial/tutorial_moveittransfer_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://contoso.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://contoso.com/<tenatid>`

    c. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`    
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Pode consultar esses valores mais tarde em **URL de metadados do fornecedor de serviço** secção ou contacte [MOVEit transferência - equipa de suporte de cliente de integração do Azure AD](https://community.ipswitch.com/s/support) obter esses valores.

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/moveittransfer-tutorial/tutorial_moveittransfer_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/moveittransfer-tutorial/tutorial_general_400.png)
    
1. Inicie sessão seu inquilino de transferência de MOVEit como administrador.

1. No painel de navegação esquerdo, clique em **definições**.

    ![Lado de secção na aplicação de definições](./media/moveittransfer-tutorial/tutorial_moveittransfer_000.png)

1. Clique em **início de sessão único** link, que está sob **políticas de segurança-autenticação de utilizador >**.

    ![Lado de políticas na aplicação de segurança](./media/moveittransfer-tutorial/tutorial_moveittransfer_001.png)

1. Clique na ligação de URL de metadados para transferir o documento de metadados.

    ![URL de metadados do fornecedor de serviço](./media/moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
    * Certifique-se **entityID** corresponde ao **identificador** no **MOVEit transferência - integração do Azure AD domínio e URLs** secção.
    * Certifique-se **AssertionConsumerService** corresponde a localização do URL **URL de resposta** no **MOVEit transferência - integração do Azure AD domínio e URLs** secção.
    
    ![Configure o lado de início de sessão na aplicação única](./media/moveittransfer-tutorial/tutorial_moveittransfer_007.png)

1. Clique em **Add Identity Provider** botão para adicionar um novo fornecedor de identidade federada.

    ![Adicionar fornecedor de identidade](./media/moveittransfer-tutorial/tutorial_moveittransfer_003.png)

1. Clique em **procurar...**  para selecionar o ficheiro de metadados que transferiu a partir do portal do Azure, em seguida, clique em **Add Identity Provider** para carregar o ficheiro transferido.

    ![Fornecedor de identidade SAML](./media/moveittransfer-tutorial/tutorial_moveittransfer_004.png)

1. Selecione "**Sim**" como **ativado** no **Federado de editar as definições do fornecedor de identidade...**  página e clique em **guardar**.

    ![Definições do fornecedor de identidade federada](./media/moveittransfer-tutorial/tutorial_moveittransfer_005.png)

1. Na **editar federado identidade de utilizador as definições do fornecedor** página, efetue as seguintes ações:
    
    ![Editar definições do fornecedor de identidade federada](./media/moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    a. Selecione **NameID de SAML** como **nome de início de sessão**.
    
    b. Selecione **outras** como **nome completo** e, no **nome de atributo** caixa de texto, coloque o valor: `http://schemas.microsoft.com/identity/claims/displayname`.
    
    c. Selecione **outras** como **E-Mail** e, no **nome de atributo** caixa de texto, coloque o valor: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    d. Selecione **Sim** como **Auto-criar conta no início de sessão**.
    
    e. Clique em **guardar** botão.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/moveittransfer-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/moveittransfer-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/moveittransfer-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/moveittransfer-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-moveit-transfer---azure-ad-integration-test-user"></a>Criar uma transferência de MOVEit - utilizador de teste de integração do Azure AD

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida na transferência de MOVEit - integração do Azure AD. Transferência MOVEit - integração do Azure AD suporta o aprovisionamento just-in-time, que tiver ativado. Não existe nenhum item de ação para nesta secção. Um novo utilizador é criado durante uma tentativa de aceder a transferência de MOVEit - integração do Azure AD, se não existir ainda.

>[!NOTE]
>Se precisar de criar manualmente um utilizador, terá de contactar o [MOVEit transferência - equipa de suporte de cliente de integração do Azure AD](https://community.ipswitch.com/s/support).

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida a utilizar do Azure-início de sessão único, conceder acesso à transferência de MOVEit - integração do Azure AD.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a transferência de MOVEit - integração do Azure AD, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **MOVEit transferência - integração do Azure AD**.

    ![A transferência de MOVEit - integração do Azure AD link na lista de aplicações](./media/moveittransfer-tutorial/tutorial_moveittransfer_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

O objetivo desta secção consiste em testar a configuração de SSO do Azure AD através do painel de acesso.

Quando clica em transferência MOVEit - mosaico integração do Azure AD no painel de acesso, deve obter automaticamente com sessão iniciada para a sua transferência MOVEit - aplicação de integração do Azure AD. 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)


<!--Image references-->

[1]: ./media/moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/moveittransfer-tutorial/tutorial_general_04.png

[100]: ./media/moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/moveittransfer-tutorial/tutorial_general_201.png
[202]: ./media/moveittransfer-tutorial/tutorial_general_202.png
[203]: ./media/moveittransfer-tutorial/tutorial_general_203.png

