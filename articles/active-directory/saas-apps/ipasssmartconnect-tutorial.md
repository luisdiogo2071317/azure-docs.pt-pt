---
title: 'Tutorial: Integração do Azure Active Directory com iPass SmartConnect | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e iPass SmartConnect.
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
ms.openlocfilehash: ecfdd3fae1d394e3b57fcd325f44cad0d1a98534
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39444899"
---
# <a name="tutorial-azure-active-directory-integration-with-ipass-smartconnect"></a>Tutorial: Integração do Azure Active Directory com iPass SmartConnect

Neste tutorial, saiba como integrar iPass SmartConnect com o Azure Active Directory (Azure AD).

Integrar iPass SmartConnect no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao iPass SmartConnect.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para iPass SmartConnect (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com iPass SmartConnect, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um iPass SmartConnect logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando iPass SmartConnect da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-ipass-smartconnect-from-the-gallery"></a>Adicionando iPass SmartConnect da Galeria
Para configurar a integração do iPass SmartConnect com o Azure AD, terá de adicionar iPass SmartConnect a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar iPass SmartConnect a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]

1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **iPass SmartConnect**, selecione **iPass SmartConnect** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![iPass SmartConnect na lista de resultados](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com iPass que smartconnect com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa de saber o que o utilizador de contraparte iPass SmartConnect for para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no iPass SmartConnect deve ser estabelecido.

Para configurar e testar o Azure AD início de sessão único com iPass SmartConnect, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de SmartConnect iPass](#create-an-ipass-smartconnect-test-user)**  - para ter um equivalente da Eduarda Almeida na iPass SmartConnect que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão único no portal do Azure e configurar o início de sessão único na sua iPass SmartConnect aplicação.

**Para configurar o Azure AD início de sessão único com iPass SmartConnect, execute os seguintes passos:**

1. No portal do Azure, sobre o **iPass SmartConnect** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Caixa de diálogo de início de sessão único](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_samlbase.png)

1. Sobre o **iPass SmartConnect domínio e URLs** secção, se desejar configurar a aplicação no **IDP** modo iniciado, sem a necessidade de efetuar outros passos.

    ![informações de início de sessão de único iPass SmartConnect domínio e URLs](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_url1.png)

1. Verifique a mostrar as definições de URL avançadas e executar o passo seguinte, se desejar configurar a aplicação no **SP** iniciada pelo modo:

    ![informações de início de sessão de único iPass SmartConnect domínio e URLs](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_url2.png)

    Na caixa de texto URL de início de sessão, escreva um URL: `https://om-activation.ipass.com/ClientActivation/ssolanding.go`

1. iPass SmartConnect aplicativo espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos da "**atributos de utilizador**" secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo disso.

    ![Configurar o início de sessão único](./media/ipasssmartconnect-tutorial/attribute.png)

1. Clique em **ver e editar todos os outros atributos de utilizador** caixa de seleção o **atributos de utilizador** secção para expandir os atributos. Execute os seguintes passos em cada um dos atributos apresentados-

    | Nome de atributo | Valor do atributo | Valor de espaço de nomes|
    | ---------------| --------------- |----------------|
    | firstName | User.givenName |   |
    | Apelido | User.Surname | |
    | e-mail | user.userprincipalname | |
    | o nome de utilizador | user.userprincipalname | |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/ipasssmartconnect-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/ipasssmartconnect-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Mantenha o valor de espaço de nomes em branco para essa linha.

    e. Clique em **OK**.

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_certificate.png)

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/ipasssmartconnect-tutorial/tutorial_general_400.png)

1. Para configurar o início de sessão único num **iPass SmartConnect** lado, terá de enviar o transferido **XML de metadados** e a sua **nome de domínio** para [iPass SmartConnect equipa de suporte](mailto:help@ipass.com). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/ipasssmartconnect-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/ipasssmartconnect-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/ipasssmartconnect-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/ipasssmartconnect-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-an-ipass-smartconnect-test-user"></a>Criar um utilizador de teste de SmartConnect iPass

Nesta secção, vai criar um usuário chamado Eduarda Almeida no iPass SmartConnect. Trabalhar com [equipa de suporte de iPass SmartConnect](mailto:help@ipass.com) para adicionar os utilizadores ou o domínio que é necessário para estar incluído na plataforma de SmartConnect iPass. Se o domínio for adicionado pela equipe, os utilizadores serão automaticamente aprovisionados para a plataforma de SmartConnect iPass. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso aos iPass SmartConnect.

![Atribuir a função de utilizador][200]

**Para atribuir a Eduarda Almeida a iPass SmartConnect, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201]

1. Na lista de aplicações, selecione **iPass SmartConnect**.

    ![O iPass SmartConnect link na lista de aplicações](./media/ipasssmartconnect-tutorial/tutorial_ipasssmartconnect_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

**Para testar a aplicação no fluxo SP iniciada, execute os seguintes passos:**

a. Baixe o windows iPass cliente SmartConnect [aqui](https://om-activation.ipass.com/ClientActivation/ssolanding.go).

![O iPass SmartConnect link na lista de aplicações](./media/ipasssmartconnect-tutorial/testing3.png)

b. Instale o cliente e o lançamento.

c. Clique em **começar**.

![O iPass SmartConnect link na lista de aplicações](./media/ipasssmartconnect-tutorial/testing1.png) 

d. Introduza o nome de utilizador do Azure com o domínio. Clique em **continuar**. Isso será redirecionado à página de início de sessão do Azure

![O iPass SmartConnect link na lista de aplicações](./media/ipasssmartconnect-tutorial/testing2.png) 

e. Após a autenticação com êxito, será possível iniciar a ativação do cliente. Cliente vai ser ativado.

**Para testar a aplicação no fluxo IdP iniciada, execute os seguintes passos:**

a. Inicie sessão no [ https://myapps.microsoft.com ](https://myapps.microsoft.com).

b. Clique no iPass SmartConnect aplicação.

c. Ele inicia a página de SSA, clique em **transferir aplicações para Windows** instalar iPass SmartConnect cliente.

![O iPass SmartConnect link na lista de aplicações](./media/ipasssmartconnect-tutorial/testing4.png)

d. Depois da instalação, o cliente no primeiro lançamento será automaticamente inicia a ativação depois de aceitar os termos e condições.

e. Se a ativação não for iniciado, clique no botão Ativar na página de SSA para iniciar a ativação.

f. Cliente vai ser ativado.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

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

