---
title: 'Tutorial: Integração do Azure Active Directory com iPass SmartConnect | Microsoft Docs'
description: Saiba como configurar o início de sessão entre o Azure Active Directory e iPass SmartConnect.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: dee6d039-f9bb-49a2-a408-5ed40ef17d9f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: jeedes
ms.openlocfilehash: 5ae600142f7e90a7f6185c3a948a4174ce4c7797
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320528"
---
# <a name="tutorial-azure-active-directory-integration-with-ipass-smartconnect"></a>Tutorial: Integração do Azure Active Directory com iPass SmartConnect

Neste tutorial, irá aprender a integrar iPass SmartConnect com o Azure Active Directory (Azure AD).

Integrar iPass SmartConnect com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao iPass SmartConnect.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para iPass SmartConnect (Single Sign-On) com as respetivas contas do Azure AD.
- Pode gerir as contas numa localização central - portal do Azure.

Se pretender saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, consulte o artigo [que é o acesso a aplicações e início de sessão no Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com iPass SmartConnect, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um iPass SmartConnect-início de sessão único ativada subscrição

> [!NOTE]
> Para testar os passos neste tutorial, não recomendamos a utilização num ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, teste do Azure AD-início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos modulares principais:

1. Adicionar iPass SmartConnect a partir da Galeria
2. Configurar e testar o Azure AD de sessão único-

## <a name="adding-ipass-smartconnect-from-the-gallery"></a>Adicionar iPass SmartConnect a partir da Galeria
Para configurar a integração de iPass SmartConnect com o Azure AD, terá de adicionar iPass SmartConnect a partir da Galeria à sua lista de aplicações SaaS geridas.

**Para adicionar iPass SmartConnect a partir da galeria, execute os seguintes passos:**

1. No  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **do Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todas as aplicações**.

    ![O painel de aplicações da empresa][2]

3. Para adicionar a nova aplicação, clique em **nova aplicação** botão no topo da caixa de diálogo.

    ![O novo botão de aplicação][3]

4. Na caixa de pesquisa, escreva **iPass SmartConnect**, selecione **iPass SmartConnect** partir do painel de resultados, em seguida, clique em **adicionar** botão para adicionar a aplicação.

    ![iPass SmartConnect na lista de resultados](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD-início de sessão único

Nesta secção, configure e teste do Azure AD-início de sessão único com iPass que smartconnect com base num utilizador de teste chamado "Britta Simon".

Para início de sessão trabalhar, do Azure AD tem de saber que o utilizador de homólogo iPass SmartConnect for para um utilizador no Azure AD. Por outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no iPass SmartConnect tem de ser estabelecida.

Para configurar e testar o Azure AD-início de sessão único com iPass SmartConnect, tem de concluir os blocos modulares seguintes:

1. **[Configurar o Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)**  - para permitir aos utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD-início de sessão único com Britta Simon.
3. **[Criar um utilizador de teste de SmartConnect iPass](#create-an-ipass-smartconnect-test-user)**  - para ter um homólogo de Britta Simon no iPass SmartConnect que está ligada a representação do Azure AD do utilizador.
4. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar Britta Simon utilizar o Azure AD-início de sessão único.
5. **[Teste o início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD-início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão no portal do Azure e configurar o início de sessão único na sua iPass SmartConnect aplicação.

**Para configurar o Azure AD-início de sessão único com iPass SmartConnect, execute os seguintes passos:**

1. No portal do Azure, no **iPass SmartConnect** página de integração de aplicações, clique em **de sessão único-**.

    ![Configurar a ligação de início de sessão único][4]

2. No **de sessão único-** caixa de diálogo, selecione **modo** como **baseados em SAML início de sessão** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_samlbase.png)

3. No **iPass SmartConnect domínio e os URLs** secção, se pretender configurar a aplicação no **IDP** modo iniciado, não é necessário para efetuar quaisquer passos.

    ![iPass SmartConnect domínio e os URLs únicos de informações de início de sessão](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_url1.png)

4. Verifique a mostrar avançadas definições de URL e executar o passo seguinte, se pretender configurar a aplicação no **SP** iniciada modo:

    ![iPass SmartConnect domínio e os URLs únicos de informações de início de sessão](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_url2.png)

    Na caixa de texto de URL de início de sessão, escreva um URL: `https://om-activation.ipass.com/ClientActivation/ssolanding.go`

5. iPass SmartConnect aplicação espera as asserções de SAML num formato específico. Configure as seguintes afirmações para esta aplicação. Pode gerir os valores destes atributos a partir de "**atributos de utilizador**" secção na página de integração de aplicações. A seguinte captura de ecrã mostra um exemplo para este.

    ![Configurar o início de sessão único](./media/ipasssmartconnect-tutorial/attribute.png)

6. Clique em **ver e editar todos os outros atributos de utilizador** caixa de verificação no **atributos de utilizador** secção para expandir os atributos. Execute os seguintes passos em cada um dos atributos apresentadas-

    | Nome do Atributo | Valor do Atributo | Valor de espaço de nomes|
    | ---------------| --------------- |----------------|
    | firstName | User.givenName |   |
    | Apelido | User.Surname | |
    | e-mail | user.userprincipalname | |
    | o nome de utilizador | user.userprincipalname | |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/ipasssmartconnect-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/ipasssmartconnect-tutorial/tutorial_attribute_05.png)

    b. No **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Mantenha o valor de espaço de nomes em branco para essa linha.

    e. Clique em **OK**.

7. No **certificado de assinatura de SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![A hiperligação de transferência do certificado](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_certificate.png)

8. Clique em **guardar** botão.

    ![Configurar botão único início de sessão guardar](./media/ipasssmartconnect-tutorial/tutorial_general_400.png)

9. Para configurar o início de sessão único em **iPass SmartConnect** lado, terá de enviar o transferido **XML de metadados** e os seus **nome de domínio** para [iPass SmartConnect equipa de suporte](mailto:help@ipass.com). Se definir esta definição para que a ligação de SAML SSO corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção consiste em criar um utilizador de teste no portal do Azure chamado Britta Simon.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique em de **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/ipasssmartconnect-tutorial/create_aaduser_01.png)

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e "Todos os utilizadores" ligações](./media/ipasssmartconnect-tutorial/create_aaduser_02.png)

3. Para abrir o **utilizador** caixa de diálogo, clique em **adicionar** na parte superior do **todos os utilizadores** caixa de diálogo.

    ![O botão de adição](./media/ipasssmartconnect-tutorial/create_aaduser_03.png)

4. No **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/ipasssmartconnect-tutorial/create_aaduser_04.png)

    a. No **nome** caixa, escreva **BrittaSimon**.

    b. No **nome de utilizador** caixa, escreva o endereço de e-mail do utilizador Britta Simon.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado no **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-an-ipass-smartconnect-test-user"></a>Criar um utilizador de teste de SmartConnect iPass

Nesta secção, vai criar um utilizador chamado Britta Simon iPass SmartConnect. Trabalhar com [iPass SmartConnect suporta equipa](mailto:help@ipass.com) para adicionar os utilizadores ou o domínio que é necessário para na lista de permissões na plataforma de SmartConnect iPass. Se o domínio for adicionado pela equipa do, irá obter automaticamente aprovisionados para a plataforma de SmartConnect iPass. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Britta Simon utilizar o Azure-início de sessão único, concedendo acesso aos iPass SmartConnect.

![Atribuir a função de utilizador][200]

**Para atribuir Britta Simon a iPass SmartConnect, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicações e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todas as aplicações**.

    ![Atribua o utilizador][201]

2. Na lista de aplicações, selecione **iPass SmartConnect**.

    ![O iPass SmartConnect ligação na lista de aplicações](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_app.png)  

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação de "Utilizadores e grupos"][202]

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** no **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Britta Simon** na lista utilizadores.

6. Clique em **selecione** botão no **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribuir** botão no **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, testar a configuração do Azure AD único início de sessão através do painel de acesso.

**Para testar a aplicação no fluxo de SP iniciada, execute os seguintes passos:**

a. Transferir o windows iPass SmartConnect cliente [aqui](https://om-activation.ipass.com/ClientActivation/ssolanding.go).

![O iPass SmartConnect ligação na lista de aplicações](./media/ipasssmartconnect-tutorial/testing3.png)

b. Instale o cliente e iniciar.

c. Clique em **começar**.

![O iPass SmartConnect ligação na lista de aplicações](./media/ipasssmartconnect-tutorial/testing1.png) 

d. Introduza o nome de utilizador do Azure com o domínio. Clique em **continuar**. Isto será redirecionado para a página de início de sessão do Azure

![O iPass SmartConnect ligação na lista de aplicações](./media/ipasssmartconnect-tutorial/testing2.png) 

e. Após a autenticação com êxito, será possível iniciar a ativação do cliente. Cliente irá obter ativado.

**Para testar a aplicação no fluxo de IdP iniciada, execute os seguintes passos:**

a. Início de sessão para [ https://myapps.microsoft.com ](https://myapps.microsoft.com).

b. Clique em iPass SmartConnect aplicação.

c. Inicia-página SSA, clique em **transferir aplicações para Windows** instalar iPass SmartConnect cliente.

![O iPass SmartConnect ligação na lista de aplicações](./media/ipasssmartconnect-tutorial/testing4.png)

d. Depois da instalação, o cliente na primeira execução será automaticamente inicia a ativação depois de a aceitar os termos e condições.

e. Se a ativação não iniciar, clique no botão Ativar da página de SSA para iniciar a ativação.

f. Cliente irá obter ativado.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão no Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/ipasssmartconnect-tutorial/tutorial_general_01.png
[2]: ./media/ipasssmartconnect-tutorial/tutorial_general_02.png
[3]: ./media/ipasssmartconnect-tutorial/tutorial_general_03.png
[4]: ./media/ipasssmartconnect-tutorial/tutorial_general_04.png

[100]: ./media/ipasssmartconnect-tutorial/tutorial_general_100.png

[200]: ./media/ipasssmartconnect-tutorial/tutorial_general_200.png
[201]: ./media/ipasssmartconnect-tutorial/tutorial_general_201.png
[202]: ./media/ipasssmartconnect-tutorial/tutorial_general_202.png
[203]: ./media/ipasssmartconnect-tutorial/tutorial_general_203.png

