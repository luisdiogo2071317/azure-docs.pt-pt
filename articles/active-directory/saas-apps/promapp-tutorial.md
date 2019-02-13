---
title: 'Tutorial: Integração do Active Directory do Azure com Promapp | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Promapp.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 418d0601-6e7a-4997-a683-73fa30a2cfb5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 758dc65b4177192047ff1b092899608b04211221
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56192836"
---
# <a name="tutorial-azure-active-directory-integration-with-promapp"></a>Tutorial: Integração do Active Directory do Azure com Promapp

Neste tutorial, saiba como integrar Promapp com o Azure Active Directory (Azure AD).

Integrar Promapp no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Promapp
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Promapp (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Promapp, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Promapp logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Promapp da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-promapp-from-the-gallery"></a>Adicionando Promapp da Galeria
Para configurar a integração do Promapp com o Azure AD, terá de adicionar Promapp a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Promapp a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Promapp**.

    ![Criar um utilizador de teste do Azure AD](./media/promapp-tutorial/tutorial_promapp_search.png)

1. No painel de resultados, selecione **Promapp**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/promapp-tutorial/tutorial_promapp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Promapp com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Promapp a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Promapp deve ser estabelecido.

Promapp, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Promapp, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Promapp](#creating-a-promapp-test-user)**  - para ter um equivalente da Eduarda Almeida na Promapp que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Promapp.

**Para configurar o Azure AD início de sessão único com Promapp, execute os seguintes passos:**

1. No portal do Azure, sobre o **Promapp** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/promapp-tutorial/tutorial_promapp_samlbase.png)

1. Sobre o **Promapp domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Configurar o início de sessão único](./media/promapp-tutorial/tutorial_promapp_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão:
    
    | |
    |--|
    | `https://go.promapp.com/TENANTNAME/`|
    | `https://au.promapp.com/TENANTNAME/`|
    | `https://us.promapp.com/TENANTNAME/`|
    | `https://eu.promapp.com/TENANTNAME/`|
    | `https://ca.promapp.com/TENANTNAME/`|
    
    > [!NOTE] 
    > Atualmente integração do Azure AD com Promapp apenas foi configurada para autenticação do serviço iniciado por exemplo, vai para um URL de Promapp inicia o processo de autenticação. No entanto, o URL de resposta é um campo obrigatório.
    
    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://DOMAINNAME.promapp.com/azuread/saml/authenticate.aspx`

1. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Configurar o início de sessão único](./media/promapp-tutorial/tutorial_promapp_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://DOMAINNAME.promapp.com/TENANTNAME/saml/authenticate`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão, identificador e o URL de resposta real. Contacte [equipa de suporte de cliente Promapp](https://www.promapp.com/about-us/contact-us/) obter esses valores.

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/promapp-tutorial/tutorial_promapp_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/promapp-tutorial/tutorial_general_400.png)

1. Sobre o **Promapp configuração** secção, clique em **configurar Promapp** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/promapp-tutorial/tutorial_promapp_configure.png) 

1. Início de sessão no site da sua empresa Promapp como administrador. 

1. No menu na parte superior, clique em **administrador**. 
   
    ![O Azure AD Single Sign-On][12]

1. Clique em **Configurar**. 
   
    ![O Azure AD Single Sign-On][13]

1. Sobre o **segurança** caixa de diálogo, execute os seguintes passos:
   
    ![O Azure AD Single Sign-On][14]
    
    a. Colar **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure para o **URL de início de sessão de SSO** caixa de texto.
    
    b. Como **SSO - modo de início de sessão único**, selecione **opcional**e, em seguida, clique em **guardar**.

    > [!NOTE]
    > **Opcional** modo é apenas para teste. Assim que estiver satisfeito com a configuração, selecione **necessário** modo para impor a todos os utilizadores para se autenticar com o Azure AD.

    c. Abra o certificado transferido no bloco de notas, copie o conteúdo de certificado sem a primeira linha (---**BEGIN CERTIFICATE**---) e a última linha (---**certificado final**---), cole-o para o  **Certificado x.509 de SSO** caixa de texto e, em seguida, clique em **guardar**.
        
> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/promapp-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/promapp-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/promapp-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/promapp-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-promapp-test-user"></a>Criar um utilizador de teste Promapp

A aplicação de Promapp suporta Just-in-Time de aprovisionamento. Isso significa, uma conta de utilizador é criada automaticamente se for necessário durante uma tentativa de aceder à aplicação através do painel de acesso.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Promapp.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Promapp, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Promapp**.

    ![Configurar o início de sessão único](./media/promapp-tutorial/tutorial_promapp_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Para testar a sua aplicação num **SP** iniciada pelo modo, terá de iniciar a autenticação do seu site Promapp. Isso pode ser feito ao clicar no botão de "Início de sessão com o início de sessão único" na sua página de início de sessão Embora **opcional** modo está ativado.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/promapp-tutorial/tutorial_general_01.png
[2]: ./media/promapp-tutorial/tutorial_general_02.png
[3]: ./media/promapp-tutorial/tutorial_general_03.png
[4]: ./media/promapp-tutorial/tutorial_general_04.png
[12]: ./media/promapp-tutorial/tutorial_promapp_05.png
[13]: ./media/promapp-tutorial/tutorial_promapp_06.png
[14]: ./media/promapp-tutorial/tutorial_promapp_07.png

[100]: ./media/promapp-tutorial/tutorial_general_100.png

[200]: ./media/promapp-tutorial/tutorial_general_200.png
[201]: ./media/promapp-tutorial/tutorial_general_201.png
[202]: ./media/promapp-tutorial/tutorial_general_202.png
[203]: ./media/promapp-tutorial/tutorial_general_203.png

