---
title: 'Tutorial: Integração do Azure Active Directory com o arco de Dome9 | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Dome9 arco.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4c12875f-de71-40cb-b9ac-216a805334e5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: jeedes
ms.openlocfilehash: 944f1b7ecc3fdc9f063d764a6deae49a68a24d77
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39433532"
---
# <a name="tutorial-azure-active-directory-integration-with-dome9-arc"></a>Tutorial: Integração do Azure Active Directory com Dome9 arco

Neste tutorial, saiba como integrar o arco de Dome9 com o Azure Active Directory (Azure AD).

Integrar o arco de Dome9 no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Dome9 arco.
- Pode permitir que os utilizadores automaticamente obter sessão iniciada em arco Dome9 (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Dome9 arco, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um arco Dome9 logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Dome9 arco da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-dome9-arc-from-the-gallery"></a>Adicionando Dome9 arco da Galeria
Para configurar a integração do arco Dome9 com o Azure AD, terá de adicionar Dome9 arco a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Dome9 arco a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Dome9 arco**, selecione **Dome9 arco** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Arco de Dome9 na lista de resultados](./media/dome9arc-tutorial/tutorial_dome9arc_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o arco de Dome9 com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Dome9 arco a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Dome9 arco tem de ser estabelecida.

Arco de Dome9, atribua o valor do **nome de utilizador** no Azure AD como o valor do **nome de utilizador** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o arco de Dome9, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de arco Dome9](#create-a-dome9-arc-test-user)**  - para ter um equivalente da Eduarda Almeida em arco Dome9 que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Dome9 arco.

**Para configurar o Azure AD início de sessão único com o arco de Dome9, execute os seguintes passos:**

1. No portal do Azure, sobre o **Dome9 arco** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/dome9arc-tutorial/tutorial_dome9arc_samlbase.png)

1. Sobre o **Dome9 arco domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Dome9 arco domínio e URLs únicas início de sessão em informações](./media/dome9arc-tutorial/tutorial_dome9arc_url.png)

    a. Na **identificador** caixa de texto, escreva o URL: `https://secure.dome9.com/`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://secure.dome9.com/sso/saml/yourcompanyname`

    > [!NOTE]
    > Irá selecionar o valor de nome de empresa no portal de administração dome9, que é explicado mais tarde no tutorial.

1. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Dome9 arco domínio e URLs únicas início de sessão em informações](./media/dome9arc-tutorial/tutorial_dome9arc_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://secure.dome9.com/sso/saml/<yourcompanyname>`
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de resposta e o URL de início de sessão real. Contacte [equipa de suporte de cliente de arco Dome9](https://dome9.com/about/contact-us/) obter esses valores. 

1. O aplicativo de Software de arco Dome9 espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos da "**atributos de utilizador**" secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo disso.

    ![Configurar o início de sessão único attb](./media/dome9arc-tutorial/tutorial_dome9arc_attribute.png)

1. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do Atributo  | Valor do Atributo | 
    | --------------- | --------------- | 
    | memberof | user.assignedroles | 
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único adicionar attb](./media/dome9arc-tutorial/tutorial_dome9_04.png)

    ![Configurar o início de sessão único attb de edição](./media/dome9arc-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **OK**.

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/dome9arc-tutorial/tutorial_dome9arc_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/dome9arc-tutorial/tutorial_general_400.png)
    
1. Na **Dome9 arco Configuration** secção, clique em **configurar o arco de Dome9** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de arco Dome9](./media/dome9arc-tutorial/tutorial_dome9arc_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Dome9 arco como um administrador.

1. Clique nas **definições de perfil** no canto superior direito e clique em **definições da conta**. 

    ![Configuração de arco Dome9](./media/dome9arc-tutorial/configure1.png)

1. Navegue para **SSO** e, em seguida, clique em **ATIVAR**.

    ![Configuração de arco Dome9](./media/dome9arc-tutorial/configure2.png)

1. Na secção de configuração de SSO, execute os seguintes passos:

    ![Configuração de arco Dome9](./media/dome9arc-tutorial/configure3.png)

    a. Introduza o nome da empresa no **ID da conta** caixa de texto. Este valor é a ser utilizado na resposta de que URL mencionado na secção URL portal do Azure.

    b. Na **emissor** caixa de texto, cole o valor de **ID de entidade de SAML**, que tenha copiado o formulário do portal do Azure.

    c. Na **url de ponto final de Idp** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço**, que tenha copiado o formulário do portal do Azure.

    d. Abra o seu certificado codificado em Base64 transferido no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado X.509** caixa de texto.

    e. Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/dome9arc-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/dome9arc-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/dome9arc-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/dome9arc-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-dome9-arc-test-user"></a>Criar um utilizador de teste Dome9 arco

Para permitir que os utilizadores do Azure AD iniciem sessão arco Dome9, tem de ser aprovisionados no aplicativo. Arco de Dome9 suporta o aprovisionamento de just-in-time, mas para que isso funcione corretamente, o utilizador tem de selecionar particular **função** e atribuir o mesmo para o usuário.

   >[!Note] 
   >Para **função** criação e outros detalhes de contacto [equipa de suporte de cliente de arco Dome9](https://dome9.com/about/contact-us/).

**Para Aprovisionar uma conta de utilizador manualmente, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa Dome9 arco como administrador.

1. Clique nas **utilizadores e funções** e, em seguida, clique em **utilizadores**.

    ![Adicionar o funcionário](./media/dome9arc-tutorial/user1.png)

1. Clique em **adicionar utilizador**.

    ![Adicionar o funcionário](./media/dome9arc-tutorial/user2.png)

1. Na **criar utilizador** secção, execute os seguintes passos:
    
    ![Adicionar o funcionário](./media/dome9arc-tutorial/user3.png)

    a. Na **E-Mail** caixa de texto, como o tipo de e-mail do utilizador Brittasimon@contoso.com.

    b. Na **nome próprio** caixa de texto, tipo primeiro nome do utilizador, como a Eduarda.

    c. Na **Apelido** apelido tipo do utilizador, como Simon, caixa de texto.

    d. Tornar **utilizador SSO** como **no**.

    e. Clique em **criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso Dome9 arco.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida arco Dome9, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Dome9 arco**.

    ![A ligação de arco Dome9 na lista de aplicações](./media/dome9arc-tutorial/tutorial_dome9arc_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de arco Dome9 no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação Dome9 arco.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/dome9arc-tutorial/tutorial_general_01.png
[2]: ./media/dome9arc-tutorial/tutorial_general_02.png
[3]: ./media/dome9arc-tutorial/tutorial_general_03.png
[4]: ./media/dome9arc-tutorial/tutorial_general_04.png

[100]: ./media/dome9arc-tutorial/tutorial_general_100.png

[200]: ./media/dome9arc-tutorial/tutorial_general_200.png
[201]: ./media/dome9arc-tutorial/tutorial_general_201.png
[202]: ./media/dome9arc-tutorial/tutorial_general_202.png
[203]: ./media/dome9arc-tutorial/tutorial_general_203.png

