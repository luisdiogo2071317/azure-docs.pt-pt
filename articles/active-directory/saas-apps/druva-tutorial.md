---
title: 'Tutorial: Integração do Active Directory do Azure com o Druva | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Druva.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2017
ms.author: jeedes
ms.openlocfilehash: 24f0a74f5abb625ba7350c2bd3a500fc2f296575
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54824699"
---
# <a name="tutorial-azure-active-directory-integration-with-druva"></a>Tutorial: Integração do Active Directory do Azure com o Druva

Neste tutorial, saiba como integrar o Druva com o Azure Active Directory (Azure AD).

Integrar o Druva no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Druva.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Druva (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Druva, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Druva logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Druva da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-druva-from-the-gallery"></a>Adicionando Druva da Galeria
Para configurar a integração do Druva, para o Azure AD, terá de adicionar Druva a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Druva a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Druva**, selecione **Druva** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Druva na lista de resultados](./media/druva-tutorial/tutorial_druva_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Druva com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Druva a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Druva deve ser estabelecido.

Druva, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Druva, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Druva](#create-a-druva-test-user)**  - para ter um equivalente da Eduarda Almeida na Druva que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Druva.

**Para configurar o Azure AD início de sessão único com o Druva, execute os seguintes passos:**

1. No portal do Azure, sobre o **Druva** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/druva-tutorial/tutorial_druva_samlbase.png)

1. Sobre o **Druva domínio e URLs** secção, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Configurar o início de sessão único](./media/druva-tutorial/tutorial_druva_url.png)

    Na **identificador** caixa de texto, digite o valor de cadeia de caracteres: `druva-cloud`
    
1. Verifique **Mostrar definições de URL avançadas**. Se desejar configurar a aplicação no **SP** iniciada pelo modo:

    ![Configurar o início de sessão único](./media/druva-tutorial/tutorial_druva_url1.png)
    
    Na **URL de início de sessão** caixa de texto, escreva o URL: `https://cloud.druva.com/home`

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/druva-tutorial/tutorial_druva_certificate.png) 

1. Seu aplicativo Druva espera que as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizados para sua **atributos de Token SAML** configuração. 

    ![Configurar o início de sessão único](./media/druva-tutorial/tutorial_druva_attribute.png)

1. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem anterior e execute os seguintes passos:

    | Nome do Atributo      | Valor do Atributo      |
    | ------------------- | -------------------- |
    | insync\_auth\_token |Introduza o valor gerado token |
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.
    
    ![Configurar o início de sessão único](./media/druva-tutorial/tutorial_attribute_04.png)
    
    ![Configurar o início de sessão único](./media/druva-tutorial/tutorial_attribute_05.png)
    
    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha. O valor gerado token é explicado mais tarde no tutorial.
    
    d. Clique em **OK**.    

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/druva-tutorial/tutorial_general_400.png)

1. Sobre o **Druva configuração** secção, clique em **configurar Druva** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/druva-tutorial/tutorial_druva_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Druva como administrador.

1. Aceda a **gerenciar \> definições**.

    ![As definições](./media/druva-tutorial/ic795091.png "definições")

1. Na caixa de diálogo Definições de início de sessão único, execute os seguintes passos:

    ![Único configurações de logon](./media/druva-tutorial/ic795092.png "único configurações de logon")
    
    a. Na **URL de início de sessão do fornecedor de ID** caixa de texto, cole o valor de **único URL de início de sessão no serviço**, que copiou do portal do Azure.
        
    b. Na **URL de fim de sessão do fornecedor de ID** caixa de texto, cole o valor de **URL de fim de sessão**, que copiou do portal do Azure
        
    c. Abra o certificado com codificação base 64 no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado do fornecedor de ID** caixa de texto
     
    d. Para abrir o **configurações** página, clique em **guardar**.

1. Sobre o **configurações** página, clique em **gerar o Token de SSO**.

    ![As definições](./media/druva-tutorial/ic795093.png "definições")

1. Sobre o **único início de sessão no Token de autenticação** caixa de diálogo, execute os seguintes passos:

    ![O Token de SSO](./media/druva-tutorial/ic795094.png "o Token de SSO")
    
    a. Clique em **cópia**, colar copiados valor na **valor** caixa de texto no **adicionar atributo** secção no portal do Azure.
    
    b. Clique em **Fechar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/druva-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/druva-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/druva-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/druva-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-druva-test-user"></a>Criar um utilizador de teste Druva

Para habilitar os utilizadores do Azure AD iniciar sessão no Druva, tem de ser aprovisionados em Druva. No caso de Druva, o aprovisionamento é uma tarefa manual.

**Para configurar o aprovisionamento de utilizadores, execute os seguintes passos:**

1. Inicie sessão no seu **Druva** site da empresa como administrador.

1. Aceda a **gerenciar \> utilizadores**.
   
   ![Gerir utilizadores](./media/druva-tutorial/ic795097.png "gerir utilizadores")

1. Clique em **criar um novo**.
   
   ![Gerir utilizadores](./media/druva-tutorial/ic795098.png "gerir utilizadores")

1. Na caixa de diálogo Criar um novo utilizador, execute os seguintes passos:
   
   ![Criar NewUser](./media/druva-tutorial/ic795099.png "criar NewUser")
   
   a. Na **endereço de E-Mail** caixa de texto, introduza o e-mail do utilizador, como **brittasimon@contoso.com**.
   
   b. Na **Name** caixa de texto, introduza o nome de utilizador, como **BrittaSimon**.
   
   c. Clique em **criar utilizador**.

>[!NOTE]
>Pode utilizar quaisquer outras Druva utilizador conta criação ferramentas ou APIs fornecidas pelo Druva para aprovisionar contas de utilizador do Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Druva.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Druva, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Druva**.

    ![A ligação de Druva na lista de aplicações](./media/druva-tutorial/tutorial_druva_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Druva no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Druva.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/druva-tutorial/tutorial_general_01.png
[2]: ./media/druva-tutorial/tutorial_general_02.png
[3]: ./media/druva-tutorial/tutorial_general_03.png
[4]: ./media/druva-tutorial/tutorial_general_04.png

[100]: ./media/druva-tutorial/tutorial_general_100.png

[200]: ./media/druva-tutorial/tutorial_general_200.png
[201]: ./media/druva-tutorial/tutorial_general_201.png
[202]: ./media/druva-tutorial/tutorial_general_202.png
[203]: ./media/druva-tutorial/tutorial_general_203.png

