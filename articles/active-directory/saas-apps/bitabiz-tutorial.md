---
title: 'Tutorial: Integração do Active Directory do Azure com BitaBIZ | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e BitaBIZ.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 1a51e677-c62b-4aee-9c61-56926aaaa899
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: jeedes
ms.openlocfilehash: ed4bada75bad19522e74b9064601c6882331e66e
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54815893"
---
# <a name="tutorial-azure-active-directory-integration-with-bitabiz"></a>Tutorial: Integração do Active Directory do Azure com BitaBIZ

Neste tutorial, saiba como integrar BitaBIZ com o Azure Active Directory (Azure AD).

Integrar BitaBIZ no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao BitaBIZ.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para BitaBIZ (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com BitaBIZ, terá dos seguintes itens:

- Uma subscrição do Azure
- Um BitaBIZ logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando BitaBIZ da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-bitabiz-from-the-gallery"></a>Adicionando BitaBIZ da Galeria
Para configurar a integração do BitaBIZ com o Azure AD, terá de adicionar BitaBIZ a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar BitaBIZ a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **BitaBIZ**, selecione **BitaBIZ** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![BitaBIZ na lista de resultados](./media/bitabiz-tutorial/tutorial_bitabiz_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com BitaBIZ com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no BitaBIZ a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no BitaBIZ deve ser estabelecido.

BitaBIZ, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com BitaBIZ, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste BitaBIZ](#create-a-bitabiz-test-user)**  - para ter um equivalente da Eduarda Almeida na BitaBIZ que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo BitaBIZ.

**Para configurar o Azure AD início de sessão único com BitaBIZ, execute os seguintes passos:**

1. No portal do Azure, sobre o **BitaBIZ** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/bitabiz-tutorial/tutorial_bitabiz_samlbase.png)

1. Sobre o **BitaBIZ domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no modo de IDP iniciado:

    ![BitaBIZ domínio e URLs únicas início de sessão em informações](./media/bitabiz-tutorial/tutorial_bitabiz_url.png)

    Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://www.bitabiz.com/<instanceId>`

    > [!NOTE] 
    > O valor no URL acima é de apenas demonstração. Atualize o valor com o identificador real, o que é explicado mais tarde no tutorial.

1. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![BitaBIZ domínio e URLs únicas início de sessão em informações](./media/bitabiz-tutorial/tutorial_bitabiz_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva o URL: `https://www.bitabiz.com/dashboard`

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/bitabiz-tutorial/tutorial_bitabiz_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/bitabiz-tutorial/tutorial_general_400.png)
    
1. Sobre o **BitaBIZ configuração** secção, clique em **configurar BitaBIZ** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de BitaBIZ](./media/bitabiz-tutorial/tutorial_bitabiz_configure.png) 

1. Numa janela do browser web diferente, início de sessão no seu inquilino BitaBIZ como administrador.

1. Clique em **SETUP ADMIN**.

    ![Configuração de BitaBIZ](./media/bitabiz-tutorial/settings1.png)

1. Clique em **integrações do Microsoft** sob **adicionar valor** secção.

    ![Configuração de BitaBIZ](./media/bitabiz-tutorial/settings2.png)

1. Desloque para baixo para a secção **Microsoft Azure AD (ativar início de sessão único)** e execute os seguintes passos:

    ![Configuração de BitaBIZ](./media/bitabiz-tutorial/settings3.png)

    a. Copie o valor do **ID de entidade ("identificador" no Azure AD)** caixa de texto e cole-o no **identificador** caixa de texto a **BitaBIZ domínio e URLs** secção no portal do Azure. 
    
    b. Na **do Azure AD único início de sessão no URL do serviço** caixa de texto, colar **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure.
    
    c. Na **ID de entidade do Azure AD SAML** caixa de texto, colar **ID de entidade de SAML**, que copiou do portal do Azure.

    d. Abra sua transferido **Certificate(Base64)** do ficheiro no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **do Azure AD certificado de assinatura (codificado por Base64)** caixa de texto.

    e. Adicionar o seu domínio de email de negócios ou seja, nome mycompany.com na **nome de domínio** caixa de texto para atribuir o SSO para os utilizadores na sua empresa com este domínio de e-mail (não obrigatório).
    
    f. Mark **SSO ativado** a conta de BitaBIZ.
    
    g. Clique em **guardar a configuração do Azure AD** para guardar e ativar a configuração de SSO.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/bitabiz-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/bitabiz-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/bitabiz-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/bitabiz-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-bitabiz-test-user"></a>Criar um utilizador de teste BitaBIZ

Para ativar a utilizadores do Azure AD iniciar sessão no BitaBIZ, tem de ser aprovisionados em BitaBIZ.  
No caso de BitaBIZ, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa BitaBIZ como administrador.

1. Clique em **SETUP ADMIN**.

    ![BitaBIZ adicionar utilizador](./media/bitabiz-tutorial/settings1.png)

1. Clique em **adicionar utilizadores** sob **organização** secção.

    ![BitaBIZ adicionar utilizador](./media/bitabiz-tutorial/user1.png)

1. Clique em **adicionar novo funcionário**.

    ![BitaBIZ adicionar utilizador](./media/bitabiz-tutorial/user2.png)

1. Sobre o **"Add new employee"** caixa de diálogo página, execute os seguintes passos:

    ![BitaBIZ adicionar utilizador](./media/bitabiz-tutorial/user3.png)

    a. Na **nome próprio** caixa de texto, tipo, o primeiro nome de utilizador, como a Eduarda.

    b. Na **Apelido** caixa de texto, digite o apelido do utilizador, como Simon.

    c. Na **E-Mail** caixa de texto, como o tipo de endereço de e-mail do utilizador Brittasimon@contoso.com.

    d. Selecione uma data no **data de contratação**.

    e. Existem outros atributos de utilizador não obrigatórias que podem ser definidos para o utilizador. Consulte a [documento de configuração de funcionário](https://help.bitabiz.dk/manage-or-set-up-your-account/on-boarding-employees/new-employee) para obter mais detalhes.    
    
    f. Clique em **Save funcionário**.
    
    > [!NOTE]
    > O titular da conta do Azure Active Directory recebe uma mensagem de e-mail e segue uma ligação para confirmar a respetiva conta até se tornar Active Directory.
    
### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para BitaBIZ.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a BitaBIZ, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **BitaBIZ**.

    ![A ligação de BitaBIZ na lista de aplicações](./media/bitabiz-tutorial/tutorial_bitabiz_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico BitaBIZ no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo BitaBIZ.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/bitabiz-tutorial/tutorial_general_01.png
[2]: ./media/bitabiz-tutorial/tutorial_general_02.png
[3]: ./media/bitabiz-tutorial/tutorial_general_03.png
[4]: ./media/bitabiz-tutorial/tutorial_general_04.png

[100]: ./media/bitabiz-tutorial/tutorial_general_100.png

[200]: ./media/bitabiz-tutorial/tutorial_general_200.png
[201]: ./media/bitabiz-tutorial/tutorial_general_201.png
[202]: ./media/bitabiz-tutorial/tutorial_general_202.png
[203]: ./media/bitabiz-tutorial/tutorial_general_203.png

