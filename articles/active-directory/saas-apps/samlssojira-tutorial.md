---
title: 'Tutorial: Integração do Azure Active Directory com o SAML SSO para Jira pela resolução GmbH | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o SAML SSO para Jira pela resolução GmbH.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 20e18819-e330-4e40-bd8d-2ff3b98e035f
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: jeedes
ms.openlocfilehash: 6ae8256f3485d49d42efeb2927a6838252a1aeee
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39442912"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-jira-by-resolution-gmbh"></a>Tutorial: Integração do Azure Active Directory com o SAML SSO para Jira pela resolução GmbH

Neste tutorial, saiba como integrar o SSO SAML para Jira pela resolução GmbH com o Azure Active Directory (Azure AD).

Integrar o SSO SAML para Jira pela resolução GmbH com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao SAML SSO para Jira pela resolução GmbH
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para SAML SSO para Jira pela resolução GmbH (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o SAML SSO para Jira pela resolução GmbH, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um SAML SSO para Jira pela resolução GmbH início de sessão único na subscrição ativada

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando SAML SSO para Jira pela resolução GmbH da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-saml-sso-for-jira-by-resolution-gmbh-from-the-gallery"></a>Adicionando SAML SSO para Jira pela resolução GmbH da Galeria
Para configurar a integração de SAML SSO para Jira pela resolução GmbH com o Azure AD, terá de adicionar SAML SSO para Jira pela resolução GmbH a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Adicionar SAML SSO para Jira através da resolução GmbH partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **SAML SSO para Jira pela resolução GmbH**.

    ![Criar um utilizador de teste do Azure AD](./media/samlssojira-tutorial/tutorial_samlssojira_search.png)

1. No painel de resultados, selecione **SAML SSO para Jira pela resolução GmbH**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/samlssojira-tutorial/tutorial_samlssojira_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configurar e testar o Azure AD início de sessão único com o SAML SSO para Jira pela resolução que GmbH com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa de saber o que o equivalente utilizador no SAML SSO para Jira pela resolução GmbH for para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no SAML SSO para Jira pela resolução GmbH deve ser estabelecido.

No SAML SSO para Jira pela resolução GmbH, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o SAML SSO para Jira pela resolução GmbH, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um SAML SSO para Jira por utilizador de teste de GmbH de resolução](#creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user)**  - para ter um equivalente da Eduarda Almeida no SAML SSO para Jira pela resolução GmbH que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar do Azure AD início de sessão único no portal do Azure e configurar o início de sessão único na sua SAML SSO para Jira pela resolução GmbH aplicação.

**Para configurar o Azure AD início de sessão único, com o SAML SSO para Jira, por resolução GmbH, execute os seguintes passos:**

1. No portal do Azure, sobre o **SAML SSO para Jira pela resolução GmbH** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/samlssojira-tutorial/tutorial_samlssojira_samlbase.png)

1. Sobre o **SAML SSO para Jira pela resolução GmbH domínio e URLs** secção, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Configurar o início de sessão único](./media/samlssojira-tutorial/tutorial_samlssojira_url_1.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<server-base-url>/plugins/servlet/samlsso`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<server-base-url>/plugins/servlet/samlsso`

1. Verifique **Mostrar definições de URL avançadas**. Se desejar configurar a aplicação no **SP** iniciada pelo modo:

    ![Configurar o início de sessão único](./media/samlssojira-tutorial/tutorial_samlssojira_url_2.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<server-base-url>/plugins/servlet/samlsso`
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de SAML SSO para Jira pela resolução GmbH cliente](https://www.resolution.de/go/support) obter esses valores. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/samlssojira-tutorial/tutorial_samlssojira_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/samlssojira-tutorial/tutorial_general_400.png)
    
1. Numa janela do browser web diferente, inicie sessão no seu **SAML SSO para Jira pelo portal de administração do resolução GmbH** como administrador.

1. Paire o rato sobre o ícone de roda dentada e clique nas **suplementos**.
    
    ![Configurar o início de sessão único](./media/samlssojira-tutorial/addon1.png)

1. Será redirecionado à página de acesso de administrador. Introduza o **palavra-passe** e clique em **confirmar** botão.

    ![Configurar o início de sessão único](./media/samlssojira-tutorial/addon2.png)

1. Na secção do separador de suplementos, clique em **encontrar novos suplementos**. Pesquisa **SAML único início de sessão (SSO) para JIRA** e clique em **instalar** botão para instalar o novo plug-in SAML.

    ![Configurar o início de sessão único](./media/samlssojira-tutorial/addon7.png)

1. A instalação de plug-in será iniciado. Clique em **Fechar**.

    ![Configurar o início de sessão único](./media/samlssojira-tutorial/addon8.png)

    ![Configurar o início de sessão único](./media/samlssojira-tutorial/addon9.png)

1.  Clique em **Gerir**.

    ![Configurar o início de sessão único](./media/samlssojira-tutorial/addon10.png)
    
1. Clique em **configurar** para configurar o plug-in de novo.

    ![Configurar o início de sessão único](./media/samlssojira-tutorial/addon11.png)

1. No **configuração de plug-in SAML SingleSignOn** página, clique em **adicionar IdP novo** botão para configurar as definições do fornecedor de identidade.

    ![Configurar o início de sessão único](./media/samlssojira-tutorial/addon4.png)

1. No **escolha o fornecedor de identidade de SAML** página, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/samlssojira-tutorial/addon5a.png)
 
    a. Definir **do Azure AD** como o tipo de IdP.
    
    b. Adicione **nome** do fornecedor de identidade (por exemplo, do Azure AD).
    
    c. Adicione **Descrição** do fornecedor de identidade (por exemplo, do Azure AD).
    
    d. Clique em **Seguinte**.
    
1. No **configuração do fornecedor de identidade** página, clique em **próxima** botão.

    ![Configurar o início de sessão único](./media/samlssojira-tutorial/addon5b.png)

1. No **importação SAML IdP metadados** página, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/samlssojira-tutorial/addon5c.png)

    a. Clique em **carregar ficheiro** botão e escolha o ficheiro de metadados XML que transferiu no passo 5.

    b. Clique em **importação** botão.
    
    c. Aguarde um momento até que a importação for concluída com êxito.
    
    d. Clique em **seguinte** botão.
    
1. No **atributo de ID de utilizador e a transformação** página, clique em **próxima** botão.

    ![Configurar o início de sessão único](./media/samlssojira-tutorial/addon5d.png)
    
1. No **criação do utilizador e de atualização** página, clique em **Save & seguinte** ao guardar as definições.    
    
    ![Configurar o início de sessão único](./media/samlssojira-tutorial/addon6a.png)
    
1. No **testar suas configurações** página, clique em **ignorar teste e configurar manualmente** para ignorar o teste de utilizador por agora. Este será executada na próxima seção e requer algumas definições no portal do Azure. 
    
    ![Configurar o início de sessão único](./media/samlssojira-tutorial/addon6b.png)
    
1. Na leitura de caixa de diálogo apprearing **a ignorar os meios de teste...** , clique em **OK**.
    
    ![Configurar o início de sessão único](./media/samlssojira-tutorial/addon6c.png)

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/samlssojira-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/samlssojira-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/samlssojira-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/samlssojira-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-saml-sso-for-jira-by-resolution-gmbh-test-user"></a>Criar um SAML SSO para Jira por utilizador de teste de GmbH de resolução

Para ativar a utilizadores do Azure AD iniciar sessão no SAML SSO para Jira pela resolução GmbH, eles têm de ser aprovisionados no SAML SSO para Jira pela resolução GmbH.  
No SAML SSO para Jira pela resolução GmbH, aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão na sua SAML SSO para Jira pelo site de empresa resolução GmbH como administrador.

1. Paire o rato sobre o ícone de roda dentada e clique nas **gestão de utilizadores**.

    ![Adicionar o funcionário](./media/samlssojira-tutorial/user1.png) 

1. Será redirecionado para a página de acesso de administrador para introduzir **palavra-passe** e clique em **confirmar** botão.

    ![Adicionar o funcionário](./media/samlssojira-tutorial/user2.png) 

1. Sob **gestão de utilizadores** secção, clique em **criar utilizador**.

    ![Adicionar o funcionário](./media/samlssojira-tutorial/user3.png) 

1. Sobre o **"Criar novo usuário"** caixa de diálogo página, execute os seguintes passos:

    ![Adicionar o funcionário](./media/samlssojira-tutorial/user4.png) 

    a. Na **endereço de E-Mail** caixa de texto, como o tipo de endereço de e-mail do utilizador Brittasimon@contoso.com.

    b. Na **FullName** caixa de texto, nome completo do tipo do utilizador, como a Eduarda Almeida.

    c. Na **nome de utilizador** caixa de texto, como o tipo de e-mail do utilizador Brittasimon@contoso.com.

    d. Na **palavra-passe** caixa de texto, escreva a palavra-passe do utilizador.

    e. Clique em **criar utilizador**.   

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para SAML SSO para Jira pela resolução GmbH.

![Atribuir utilizador][200] 

**Para atribuir Eduarda Almeida a SAML SSO para Jira pela resolução GmbH, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **SAML SSO para Jira pela resolução GmbH**.

    ![Configurar o início de sessão único](./media/samlssojira-tutorial/tutorial_samlssojira_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Ao clicar o SAML SSO para Jira por mosaico de GmbH resolução no painel de acesso, deve obter automaticamente com sessão iniciada para sua SAML SSO para Jira pela resolução GmbH aplicação.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/samlssojira-tutorial/tutorial_general_01.png
[2]: ./media/samlssojira-tutorial/tutorial_general_02.png
[3]: ./media/samlssojira-tutorial/tutorial_general_03.png
[4]: ./media/samlssojira-tutorial/tutorial_general_04.png

[100]: ./media/samlssojira-tutorial/tutorial_general_100.png

[200]: ./media/samlssojira-tutorial/tutorial_general_200.png
[201]: ./media/samlssojira-tutorial/tutorial_general_201.png
[202]: ./media/samlssojira-tutorial/tutorial_general_202.png
[203]: ./media/samlssojira-tutorial/tutorial_general_203.png

