---
title: 'Tutorial: Integração do Active Directory do Azure com o Mixpanel | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Mixpanel.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: a2df26ef-d441-44ac-a9f3-b37bf9709bcb
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4595202aa60a2b8888487d505aa8981c6f45bd8d
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56189026"
---
# <a name="tutorial-azure-active-directory-integration-with-mixpanel"></a>Tutorial: Integração do Active Directory do Azure com o Mixpanel

Neste tutorial, saiba como integrar o Mixpanel com o Azure Active Directory (Azure AD).

Integrar o Mixpanel no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso para o Mixpanel
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Mixpanel (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Mixpanel, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Mixpanel logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Mixpanel da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-mixpanel-from-the-gallery"></a>Adicionando Mixpanel da Galeria
Para configurar a integração do Mixpanel com o Azure AD, terá de adicionar Mixpanel a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Mixpanel a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Mixpanel**.

    ![Criar um utilizador de teste do Azure AD](./media/mixpanel-tutorial/tutorial_mixpanel_search.png)

1. No painel de resultados, selecione **Mixpanel**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/mixpanel-tutorial/tutorial_mixpanel_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Mixpanel com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no Mixpanel a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Mixpanel deve ser estabelecido.

No Mixpanel, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Mixpanel, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Mixpanel](#creating-a-mixpanel-test-user)**  - para ter um equivalente da Eduarda Almeida no Mixpanel que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Mixpanel.

**Para configurar o Azure AD início de sessão único com Mixpanel, execute os seguintes passos:**

1. No portal do Azure, sobre o **Mixpanel** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/mixpanel-tutorial/tutorial_mixpanel_samlbase.png)

1. Sobre o **Mixpanel domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/mixpanel-tutorial/tutorial_mixpanel_url.png)

     Na **URL de início de sessão** caixa de texto, escreva um URL como: `https://mixpanel.com/login/`

    > [!NOTE] 
    > Registe-se no [ https://mixpanel.com/register/ ](https://mixpanel.com/register/) para configurar as suas credenciais de início de sessão e contacte o [equipa de suporte de Mixpanel](mailto:support@mixpanel.com) para ativar as definições de SSO para o seu inquilino. Também pode obter o valor de início de sessão no URL se for necessário da sua equipa de suporte de Mixpanel. 
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/mixpanel-tutorial/tutorial_mixpanel_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/mixpanel-tutorial/tutorial_general_400.png)

1. Sobre o **Mixpanel configuração** secção, clique em **configurar Mixpanel** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/mixpanel-tutorial/tutorial_mixpanel_configure.png) 

1. Numa janela do browser diferente, início de sessão na sua aplicação Mixpanel como administrador.

1. Na parte inferior da página, clique em pequena **engrenagem** ícone no canto esquerdo. 
   
    ![Mixpanel Single Sign-On](./media/mixpanel-tutorial/tutorial_mixpanel_06.png) 

1. Clique nas **segurança de acesso** separador e, em seguida, clique em **alterar as definições de**.
   
    ![Definições de Mixpanel](./media/mixpanel-tutorial/tutorial_mixpanel_08.png) 

1. Sobre o **alterar o certificado** página de diálogo, clique em **Escolher ficheiro** para carregar o certificado transferido e, em seguida, clique em **seguinte**.
   
    ![Definições de Mixpanel](./media/mixpanel-tutorial/tutorial_mixpanel_09.png) 

1.  Na caixa de texto de URL de autenticação sobre a **alterar o URL de autenticação** caixa de diálogo página, cole o valor de **SAML único início de sessão no URL do serviço** que copiou do portal do Azure e, em seguida, clique em **PRÓXIMA**.
   
   ![Definições de Mixpanel](./media/mixpanel-tutorial/tutorial_mixpanel_10.png) 

1. Clique em **Concluído**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/mixpanel-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/mixpanel-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/mixpanel-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/mixpanel-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-mixpanel-test-user"></a>Criar um utilizador de teste Mixpanel

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Mixpanel. 

1. Inicie sessão site da sua empresa Mixpanel como um administrador.

1. Na parte inferior da página, clique no pequeno botão de engrenagem no canto esquerdo para abrir o **definições** janela.

1. Clique nas **equipe** separador.

1. Na **membro da Equipe** caixa de texto, escreva o endereço de e-mail da Eduarda no Azure.
   
    ![Definições de Mixpanel](./media/mixpanel-tutorial/tutorial_mixpanel_11.png) 

1. Clique em **convidar**. 

> [!Note]
> O utilizador irá receber um e-mail para configurar o perfil.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para o Mixpanel.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida para o Mixpanel, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Mixpanel**.

    ![Configurar o início de sessão único](./media/mixpanel-tutorial/tutorial_mixpanel_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Mixpanel no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Mixpanel.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/mixpanel-tutorial/tutorial_general_01.png
[2]: ./media/mixpanel-tutorial/tutorial_general_02.png
[3]: ./media/mixpanel-tutorial/tutorial_general_03.png
[4]: ./media/mixpanel-tutorial/tutorial_general_04.png

[100]: ./media/mixpanel-tutorial/tutorial_general_100.png

[200]: ./media/mixpanel-tutorial/tutorial_general_200.png
[201]: ./media/mixpanel-tutorial/tutorial_general_201.png
[202]: ./media/mixpanel-tutorial/tutorial_general_202.png
[203]: ./media/mixpanel-tutorial/tutorial_general_203.png

