---
title: 'Tutorial: Integração do Azure Active Directory com o E-Mail de SkyDesk | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o E-Mail de SkyDesk.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 058aad72ea8e5741bc632b3c27c032613683ae78
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39444087"
---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Tutorial: Integração do Azure Active Directory com o E-Mail de SkyDesk

Neste tutorial, saiba como integrar SkyDesk E-Mail com o Azure Active Directory (Azure AD).

Integrar SkyDesk E-Mail com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao E-Mail SkyDesk
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada ao E-Mail SkyDesk (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o E-Mail de SkyDesk, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um E-Mail de SkyDesk logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma avaliação de um mês aqui [oferta de avaliação](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar SkyDesk E-Mail a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-skydesk-email-from-the-gallery"></a>Adicionar SkyDesk E-Mail a partir da Galeria
Para configurar a integração de SkyDesk E-Mail com o Azure AD, terá de adicionar SkyDesk E-Mail a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar SkyDesk E-Mail a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **SkyDesk E-Mail**.

    ![Criar um utilizador de teste do Azure AD](./media/skydeskemail-tutorial/tutorial_skydeskemail_search.png)

1. No painel de resultados, selecione **E-Mail SkyDesk**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/skydeskemail-tutorial/tutorial_skydeskemail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o E-Mail de SkyDesk com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no E-Mail de SkyDesk a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no E-Mail de SkyDesk deve ser estabelecido.

No E-Mail de SkyDesk, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o E-Mail de SkyDesk, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de E-Mail de SkyDesk](#creating-a-skydesk-email-test-user)**  - para ter um equivalente da Eduarda Almeida no E-Mail de SkyDesk que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação de E-Mail de SkyDesk.

**Para configurar o Azure AD início de sessão único com o E-Mail de SkyDesk, execute os seguintes passos:**

1. No portal do Azure, sobre o **E-Mail SkyDesk** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/skydeskemail-tutorial/tutorial_skydeskemail_samlbase.png)

1. Sobre o **SkyDesk E-Mail domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/skydeskemail-tutorial/tutorial_skydeskemail_url.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://mail.skydesk.jp/portal/<companyname>`

    > [!NOTE] 
    > O valor não é real. Atualize o valor com o URL de início de sessão real. Contacte [equipa de suporte de cliente de E-Mail SkyDesk](https://www.skydesk.sg/support/) para obter o valor. 
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/skydeskemail-tutorial/tutorial_skydeskemail_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/skydeskemail-tutorial/tutorial_general_400.png)

1. Sobre o **SkyDesk configuração de E-Mail** secção, clique em **configurar o E-Mail de SkyDesk** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/skydeskemail-tutorial/tutorial_skydeskemail_configure.png) 

1. Para ativar o SSO no **SkyDesk E-Mail**, execute os seguintes passos:

    a. Início de sessão na sua conta de E-Mail de SkyDesk como administrador.

    b. No menu na parte superior, clique em **programa de configuração**e selecione **Org**. 
    
      ![Configurar o início de sessão único](./media/skydeskemail-tutorial/tutorial_skydeskemail_51.png)
  
    c. Clique em **domínios** do painel esquerdo.
    
      ![Configurar o início de sessão único](./media/skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. Clique em **Adicionar domínio**.
    
      ![Configurar o início de sessão único](./media/skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    e. Introduza o seu nome de domínio e, em seguida, verificar o domínio.
    
      ![Configurar o início de sessão único](./media/skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f. Clique em **autenticação SAML** do painel esquerdo.
    
      ![Configurar o início de sessão único](./media/skydeskemail-tutorial/tutorial_skydeskemail_52.png)

1. Sobre o **autenticação SAML** caixa de diálogo página, execute os seguintes passos:
   
      ![Configurar o início de sessão único](./media/skydeskemail-tutorial/tutorial_skydeskemail_56.png)
   
    >[!NOTE]
    >Para usar autenticação SAML com base, deve optar por ter **domínio verificado** ou **portal URL** configuração. Pode definir o portal de URL com o nome exclusivo.
    > 
    > 
   
    ![Configurar o início de sessão único](./media/skydeskemail-tutorial/tutorial_skydeskemail_57.png)

    a. Na **URL de início de sessão** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure.
   
    b. Na **fim de sessão** caixa de texto do URL, cole o valor de **URL de fim de sessão**, que copiou do portal do Azure.

    c. **Altere o URL de palavra-passe** é opcional por isso deixe em branco.

    d. Clique em **obter chave a partir de um ficheiro** para selecionar o certificado transferido a partir do portal do Azure e, em seguida, clique em **aberto** para carregar o certificado.

    e. Como **algoritmo**, selecione **RSA**.

    f. Clique em **Ok** para guardar as alterações.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/skydeskemail-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/skydeskemail-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/skydeskemail-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/skydeskemail-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-skydesk-email-test-user"></a>Criar um utilizador de teste SkyDesk E-Mail

Nesta secção, vai criar um usuário chamado Eduarda Almeida no E-Mail de SkyDesk.

1. Clique em **acesso de utilizador** da esquerda painel no E-Mail de SkyDesk e, em seguida, introduza o seu nome de utilizador. 

    ![Configurar o início de sessão único](./media/skydeskemail-tutorial/tutorial_skydeskemail_58.png)

>[!NOTE] 
>Se precisar de criar utilizadores em massa, terá de contactar o [equipa de suporte de cliente de E-Mail SkyDesk](https://www.skydesk.sg/support/).


### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso ao E-Mail SkyDesk.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida ao E-Mail SkyDesk, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **SkyDesk E-Mail**.

    ![Configurar o início de sessão único](./media/skydeskemail-tutorial/tutorial_skydeskemail_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção consiste em testar a configuração de SSO do Azure AD através do painel de acesso.

Quando clica no mosaico de E-Mail de SkyDesk no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de E-Mail de SkyDesk.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/skydeskemail-tutorial/tutorial_general_04.png

[100]: ./media/skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/skydeskemail-tutorial/tutorial_general_201.png
[202]: ./media/skydeskemail-tutorial/tutorial_general_202.png
[203]: ./media/skydeskemail-tutorial/tutorial_general_203.png

