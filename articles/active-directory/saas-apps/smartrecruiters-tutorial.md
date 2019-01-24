---
title: 'Tutorial: Integração do Active Directory do Azure com SmartRecruiters | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e SmartRecruiters.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: e96aeecd-e113-454e-89c3-58c9f44cfd4c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: jeedes
ms.openlocfilehash: 6624368e0438b8f41a0f90c5da3f7f00f4cbc86c
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54813564"
---
# <a name="tutorial-azure-active-directory-integration-with-smartrecruiters"></a>Tutorial: Integração do Active Directory do Azure com SmartRecruiters

Neste tutorial, saiba como integrar SmartRecruiters com o Azure Active Directory (Azure AD).

Integrar SmartRecruiters no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao SmartRecruiters.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para SmartRecruiters (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com SmartRecruiters, terá dos seguintes itens:

- Uma subscrição do Azure
- Um SmartRecruiters início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando SmartRecruiters da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-smartrecruiters-from-the-gallery"></a>Adicionando SmartRecruiters da Galeria
Para configurar a integração do SmartRecruiters com o Azure AD, terá de adicionar SmartRecruiters a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar SmartRecruiters a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **SmartRecruiters**, selecione **SmartRecruiters** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![SmartRecruiters na lista de resultados](./media/smartrecruiters-tutorial/tutorial_smartrecruiters_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com SmartRecruiters com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no SmartRecruiters a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no SmartRecruiters deve ser estabelecido.

SmartRecruiters, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com SmartRecruiters, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste SmartRecruiters](#create-a-smartrecruiters-test-user)**  - para ter um equivalente da Eduarda Almeida na SmartRecruiters que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo SmartRecruiters.

**Para configurar o Azure AD início de sessão único com SmartRecruiters, execute os seguintes passos:**

1. No portal do Azure, sobre o **SmartRecruiters** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/smartrecruiters-tutorial/tutorial_smartrecruiters_samlbase.png)

1. Sobre o **SmartRecruiters domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![SmartRecruiters domínio e URLs únicas início de sessão em informações](./media/smartrecruiters-tutorial/tutorial_smartrecruiters_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://www.smartrecruiters.com/web-sso/saml/<companyname>`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://www.smartrecruiters.com/web-sso/saml/<companyname>/callback`

1. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![SmartRecruiters domínio e URLs únicas início de sessão em informações](./media/smartrecruiters-tutorial/tutorial_smartrecruiters_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://www.smartrecruiters.com/web-sso/saml/<companyname>/login`
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de cliente SmartRecruiters](https://www.smartrecruiters.com/about-us/contact-us/) obter esses valores. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o certificado no seu computador.

    ![O link de download de certificado](./media/smartrecruiters-tutorial/tutorial_smartrecruiters_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/smartrecruiters-tutorial/tutorial_general_400.png)
    
1. Sobre o **SmartRecruiters configuração** secção, clique em **configurar SmartRecruiters** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de SmartRecruiters](./media/smartrecruiters-tutorial/tutorial_smartrecruiters_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa SmartRecruiters como administrador.

1. Aceda a **definições / administrador**.

    ![Configuração de SmartRecruiters](./media/smartrecruiters-tutorial/configure.png)

1. Na **Configuration** secção, clique em **Web SSO**.

    ![Configuração de SmartRecruiters](./media/smartrecruiters-tutorial/configure1.png)

1. Botão de alternar **ativar o Web SSO**.

    ![Configuração de SmartRecruiters](./media/smartrecruiters-tutorial/configure2.png)

1. Na **configuração do fornecedor de identidade**, execute os seguintes passos:

    ![Configuração de SmartRecruiters](./media/smartrecruiters-tutorial/configure4.png)

    a. Na **URL de fornecedor de identidade** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** que copiou do portal do Azure.

    b. Open **certificate(Base64)** que transferiu a partir do portal do Azure e cole o valor em **certificado do fornecedor de identidade** caixa de texto.

1. Clique em **configuração de guardar o SSO de Web**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/smartrecruiters-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/smartrecruiters-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/smartrecruiters-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/smartrecruiters-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-smartrecruiters-test-user"></a>Criar um utilizador de teste SmartRecruiters

Nesta secção, vai criar um usuário chamado Eduarda Almeida no SmartRecruiters. Trabalhar com [equipa de suporte de SmartRecruiters](https://www.smartrecruiters.com/about-us/contact-us/) para adicionar os utilizadores na plataforma SmartRecruiters. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para SmartRecruiters.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a SmartRecruiters, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **SmartRecruiters**.

    ![A ligação de SmartRecruiters na lista de aplicações](./media/smartrecruiters-tutorial/tutorial_smartrecruiters_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico SmartRecruiters no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo SmartRecruiters.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/smartrecruiters-tutorial/tutorial_general_01.png
[2]: ./media/smartrecruiters-tutorial/tutorial_general_02.png
[3]: ./media/smartrecruiters-tutorial/tutorial_general_03.png
[4]: ./media/smartrecruiters-tutorial/tutorial_general_04.png

[100]: ./media/smartrecruiters-tutorial/tutorial_general_100.png

[200]: ./media/smartrecruiters-tutorial/tutorial_general_200.png
[201]: ./media/smartrecruiters-tutorial/tutorial_general_201.png
[202]: ./media/smartrecruiters-tutorial/tutorial_general_202.png
[203]: ./media/smartrecruiters-tutorial/tutorial_general_203.png

