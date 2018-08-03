---
title: 'Tutorial: Integração do Azure Active Directory com o Mobile Xpense | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Mobile Xpense.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e649fc4e-3e15-4948-b977-00bfe9f7db13
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: jeedes
ms.openlocfilehash: fd35cb67d0555919a1340c428bac042b67239469
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39427344"
---
# <a name="tutorial-azure-active-directory-integration-with-mobile-xpense"></a>Tutorial: Integração do Azure Active Directory com Mobile Xpense

Neste tutorial, saiba como integrar o Mobile Xpense com o Azure Active Directory (Azure AD).

Integrar o Mobile Xpense no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Mobile Xpense.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Xpense Mobile (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Mobile Xpense, precisa do seguinte:

- Uma subscrição do Azure AD
- Um Mobile Xpense início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Mobile Xpense a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-mobile-xpense-from-the-gallery"></a>Adicionar Mobile Xpense a partir da Galeria
Para configurar a integração do Mobile Xpense com o Azure AD, terá de adicionar Mobile Xpense a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Mobile Xpense a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Mobile Xpense**, selecione **Mobile Xpense** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Mobile Xpense na lista de resultados](./media/mobilexpense-tutorial/tutorial_mobilexpense_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Xpense Mobile com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Mobile Xpense a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Mobile Xpense deve ser estabelecido.

Em Mobile Xpense, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **nome de utilizador** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Mobile Xpense, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Mobile Xpense](#create-a-mobile-xpense-test-user)**  - para ter um equivalente da Eduarda Almeida no Mobile Xpense que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação móveis Xpense.

**Para configurar o Azure AD início de sessão único com Mobile Xpense, execute os seguintes passos:**

1. No portal do Azure, sobre o **Mobile Xpense** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/mobilexpense-tutorial/tutorial_mobilexpense_samlbase.png)

1. Sobre o **Mobile Xpense domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no modo de IDP iniciado:

    ![Mobile Xpense domínio e URLs únicas início de sessão em informações](./media/mobilexpense-tutorial/tutorial_mobilexpense_url11.png)

    a. Na **identificador** caixa de texto, escreva um URL: `https://mobilexpense.com/ServiceProvider`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<sub-domain>.mobilexpense.com/NET/SSO/SAML20/SAML/AssertionConsumerService.aspx`

1. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Mobile Xpense domínio e URLs únicas início de sessão em informações](./media/mobilexpense-tutorial/tutorial_mobilexpense_url22.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<sub-domain>.mobilexpense.com/<customername>`
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de resposta real e o URL de início de sessão. Contacte [equipa de suporte do Mobile Xpense cliente](http://www.mobilexpense.net/contact) obter esses valores. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/mobilexpense-tutorial/tutorial_mobilexpense_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/mobilexpense-tutorial/tutorial_general_400.png)

1. Para configurar o início de sessão único num **Mobile Xpense** lado, terá de enviar o transferido **XML de metadados** para [equipa de suporte do Mobile Xpense](http://www.mobilexpense.net/contact). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/mobilexpense-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/mobilexpense-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/mobilexpense-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/mobilexpense-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-mobile-xpense-test-user"></a>Criar um utilizador de teste Mobile Xpense

Nesta secção, vai criar um usuário chamado Eduarda Almeida no MobileXpense. trabalhar com [equipa de suporte de MobileXpense](http://www.mobilexpense.net/contact) para adicionar os utilizadores na plataforma MobileXpense. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao Mobile Xpense.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Mobile Xpense, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Mobile Xpense**.

    ![A ligação de Mobile Xpense na lista de aplicações](./media/mobilexpense-tutorial/tutorial_mobilexpense_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Mobile Xpense no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação móveis Xpense.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/mobilexpense-tutorial/tutorial_general_01.png
[2]: ./media/mobilexpense-tutorial/tutorial_general_02.png
[3]: ./media/mobilexpense-tutorial/tutorial_general_03.png
[4]: ./media/mobilexpense-tutorial/tutorial_general_04.png

[100]: ./media/mobilexpense-tutorial/tutorial_general_100.png

[200]: ./media/mobilexpense-tutorial/tutorial_general_200.png
[201]: ./media/mobilexpense-tutorial/tutorial_general_201.png
[202]: ./media/mobilexpense-tutorial/tutorial_general_202.png
[203]: ./media/mobilexpense-tutorial/tutorial_general_203.png

