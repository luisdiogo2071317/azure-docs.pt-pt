---
title: 'Tutorial: Integração do Azure Active Directory com Boomi | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Boomi.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 40d034ff-7394-4713-923d-1f8f2ed8bf36
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: jeedes
ms.openlocfilehash: cf925e0e0e7b6b4c10b6b21d17214f91473a9026
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39432954"
---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a>Tutorial: Integração do Azure Active Directory com Boomi

Neste tutorial, saiba como integrar Boomi com o Azure Active Directory (Azure AD).

Integrar Boomi no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Boomi.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Boomi (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Boomi, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Boomi logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Boomi da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-boomi-from-the-gallery"></a>Adicionando Boomi da Galeria
Para configurar a integração do Boomi com o Azure AD, terá de adicionar Boomi a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Boomi a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Boomi**, selecione **Boomi** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Boomi na lista de resultados](./media/boomi-tutorial/tutorial_boomi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Boomi com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Boomi a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Boomi deve ser estabelecido.

Boomi, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Boomi, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Boomi](#create-a-boomi-test-user)**  - para ter um equivalente da Eduarda Almeida na Boomi que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Boomi.

**Para configurar o Azure AD início de sessão único com Boomi, execute os seguintes passos:**

1. No portal do Azure, sobre o **Boomi** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/boomi-tutorial/tutorial_boomi_samlbase.png)

1. Sobre o **Boomi domínio e URLs** secção, execute os seguintes passos:

    ![Boomi domínio e URLs únicas início de sessão em informações](./media/boomi-tutorial/tutorial_boomi_url.png)

    a. Na **identificador** caixa de texto, escreva um URL: `https://platform.boomi.com/`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://platform.boomi.com/sso/<boomi-tenant>/saml`

    > [!NOTE] 
    > O valor de URL de resposta não é real. Atualize o valor com o URL de resposta real. Contacte [equipa de suporte de Boomi](https://boomi.com/company/contact/) para obter o valor.
 
1. Aplicação de Boomi espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos da "**atributos de utilizador**" secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo disso.
    
    ![Configurar o início de sessão único](./media/boomi-tutorial/tutorial_attribute.png)

1. Na **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, para cada linha, mostrada na tabela abaixo, execute os seguintes passos:

    | Nome do Atributo | Valor do Atributo |
    | -------------- | --------------- |
    | FEDERATION_ID | User.Mail |
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.
    
    ![Configurar o início de sessão único](./media/boomi-tutorial/tutorial_officespace_04.png)
    
    ![Configurar o início de sessão único](./media/boomi-tutorial/tutorial_attribute_05.png)
    
    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **OK**.

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/boomi-tutorial/tutorial_boomi_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/boomi-tutorial/tutorial_general_400.png)

1. Sobre o **Boomi configuração** secção, clique em **configurar Boomi** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de Boomi](./media/boomi-tutorial/tutorial_boomi_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Boomi como um administrador. 

1. Navegue para **nome da empresa** e aceda à **configurar**.

1. Clique nas **opções de SSO** separador e executar passos abaixo.

    ![Configurar o início de sessão único no lado de aplicação](./media/boomi-tutorial/tutorial_boomi_11.png)

    a. Verifique **ativar SAML Single Sign-On** caixa de verificação.

    b. Clique em **importação** para carregar o certificado transferido do Azure AD para **certificado do fornecedor de identidade**.
    
    c. Na **URL de início de sessão do fornecedor de identidade** caixa de texto, colocar o valor de **SAML único início de sessão no URL do serviço** partir da janela de configuração de aplicação do Azure AD.

    d. Como **localização de Id de Federação**, selecione **Id de Federação é no elemento de atributo FEDERATION_ID** botão de opção. 

    e. Clique em **guardar** botão.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/boomi-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/boomi-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/boomi-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/boomi-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-a-boomi-test-user"></a>Criar um utilizador de teste Boomi

Para habilitar os utilizadores do Azure AD iniciar sessão no Boomi, tem de ser aprovisionados em Boomi. No caso de Boomi, o aprovisionamento é uma tarefa manual.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Para Aprovisionar uma conta de utilizador, execute os seguintes passos:

1. Inicie sessão no site da sua empresa Boomi como administrador.

1. Após iniciar sessão, navegue até **gestão de utilizadores** e aceda à **utilizadores**.

    ![Os utilizadores](./media/boomi-tutorial/tutorial_boomi_001.png "utilizadores")

1. Clique em **+** ícone e o **funções de utilizador de adicionar/mantenha** é aberta a caixa de diálogo.

    ![Os utilizadores](./media/boomi-tutorial/tutorial_boomi_002.png "utilizadores")

    ![Os utilizadores](./media/boomi-tutorial/tutorial_boomi_003.png "utilizadores")

    a. Na **endereço de email do usuário** caixa de texto, como o tipo de e-mail do utilizador BrittaSimon@contoso.com.
    
    b. Na **nome próprio** caixa de texto, tipo, o primeiro nome de utilizador, como a Eduarda.

    c. Na **Apelido** caixa de texto, digite o apelido do utilizador, como Simon.
    
    d. Introduza o utilizador **ID de Federação**. Cada utilizador tem de ter um ID de federação que identifica unicamente o usuário dentro da conta.
    
    e. Atribuir a **usuário padrão** função ao utilizador. Não atribua a função de administrador porque isso ofereceria em contato com acesso de atmosfera normal, bem como acesso de início de sessão único.
    
    f. Clique em **OK**.
    
    > [!NOTE]
    > O utilizador não irá receber um e-mail de notificação de boas-vindas que contém uma palavra-passe que pode ser utilizada para iniciar sessão na conta de AtomSphere porque a palavra-passe é gerenciada através do fornecedor de identidade. Pode utilizar quaisquer outras Boomi utilizador conta criação ferramentas ou APIs fornecidas pelo Boomi para aprovisionar contas de utilizador do AAD.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Boomi.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Boomi, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Boomi**.

    ![A ligação de Boomi na lista de aplicações](./media/boomi-tutorial/tutorial_boomi_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Boomi no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Boomi.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/boomi-tutorial/tutorial_general_01.png
[2]: ./media/boomi-tutorial/tutorial_general_02.png
[3]: ./media/boomi-tutorial/tutorial_general_03.png
[4]: ./media/boomi-tutorial/tutorial_general_04.png

[100]: ./media/boomi-tutorial/tutorial_general_100.png

[200]: ./media/boomi-tutorial/tutorial_general_200.png
[201]: ./media/boomi-tutorial/tutorial_general_201.png
[202]: ./media/boomi-tutorial/tutorial_general_202.png
[203]: ./media/boomi-tutorial/tutorial_general_203.png

