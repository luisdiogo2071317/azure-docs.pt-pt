---
title: 'Tutorial: Integração do Azure Active Directory com a área de trabalho Autotask | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Autotask à área de trabalho.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a9a7ff71-c389-4169-aafd-d7a505244797
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: cc1ee04c9d614e895c4e8a021564e9b9405fa8c0
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39438965"
---
# <a name="tutorial-azure-active-directory-integration-with-autotask-workplace"></a>Tutorial: Integração do Azure Active Directory com Autotask à área de trabalho

Neste tutorial, saiba como integrar Autotask à área de trabalho com o Azure Active Directory (Azure AD).

Integrar Autotask à área de trabalho no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à área de trabalho Autotask
- Pode permitir que os utilizadores automaticamente obter sessão iniciada em à área de trabalho Autotask (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Autotask à área de trabalho, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um área de trabalho Autotask início de sessão único na subscrição ativado
- Tem de ser um administrador ou administrador super na área de trabalho.
- Tem de ter uma conta de administrador no Azure AD.
- Os utilizadores que passarão a utilizar esta funcionalidade tem de ter contas no local de trabalho e o Azure AD e respetivos endereços de e-mail para ambos têm de corresponder.

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Autotask à área de trabalho a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-autotask-workplace-from-the-gallery"></a>Adicionar Autotask à área de trabalho a partir da Galeria
Para configurar a integração do Autotask à área de trabalho para o Azure AD, terá de adicionar Autotask à área de trabalho a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Autotask à área de trabalho a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **à área de trabalho Autotask**, selecione **à área de trabalho Autotask** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Autotask à área de trabalho na lista de resultados](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Autotask à área de trabalho com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente na área de trabalho Autotask a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na área de trabalho Autotask deve ser estabelecido.

Na área de trabalho Autotask, atribuir o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Autotask à área de trabalho, terá de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de área de trabalho Autotask](#create-an-autotask-workplace-test-user)**  - para ter um equivalente da Eduarda Almeida na área de trabalho de Autotask que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de área de trabalho Autotask.

**Para configurar o Azure AD início de sessão único com Autotask à área de trabalho, execute os seguintes passos:**

1. No portal do Azure, sobre o **Autotask à área de trabalho** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_samlbase.png)

1. Se desejar configurar a aplicação no **IDP** iniciadas pelo modo, execute os seguintes passos no **Autotask à área de trabalho domínio e URLs** secção:

    ![Área de trabalho Autotask domínio e URLs únicas início de sessão informações para o IDP](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.awp.autotask.net/singlesignon/saml/metadata`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.awp.autotask.net/singlesignon/saml/SSO`

1. Se desejar configurar a aplicação no **SP** modo iniciado, verificação **Mostrar definições de URL avançadas** e execute os seguintes passos:

    ![Área de trabalho Autotask domínio e URLs únicas início de sessão em informações de SP](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_url1.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.awp.autotask.net/loginsso`
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de cliente de área de trabalho de Autotask](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) obter esses valores. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/autotaskworkplace-tutorial/tutorial_general_400.png)

1. Numa janela do browser web diferente, inicie sessão à área de trabalho Online com as credenciais de administrador.

    >[!Note]
    >Ao configurar o IdP, um subdomínio terá de ser especificado. Para confirmar o subdomínio correto, o início de sessão para o local de trabalho Online. Depois de iniciar sessão, tome nota para o subdomínio no URL.
    >O subdomínio é a parte entre o "https://" e ".awp.autotask.net/" e deve ser EUA, UE, AC ou au.

1. Aceda a **Configuration** > **Single Sign-On** e execute os seguintes passos:

    ![Configuração do início de sessão único de Autotask](./media/autotaskworkplace-tutorial/tutorial_autotaskssoconfig1.png)
 
    a. Selecione o **o ficheiro de metadados XML** opção e, em seguida, carregue o **XML de metadados** transferido a partir do portal do Azure.

    b. Clique em **ativar o SSO**.
    
    ![Autotask Single Sign-on aprovar configuração](./media/autotaskworkplace-tutorial/tutorial_autotaskssoconfig2.png)

    c. Selecione o **confirmo estas informações estão corretas e devo confiar este IdP** caixa de verificação.

    d. Clique em **aprovar**.
     
>[!Note]
>Se precisar de ajuda com a configuração Autotask à área de trabalho, consulte [esta página](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) para obter ajuda com sua conta da área de trabalho.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/autotaskworkplace-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/autotaskworkplace-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/autotaskworkplace-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/autotaskworkplace-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.

### <a name="create-an-autotask-workplace-test-user"></a>Criar um utilizador de teste Autotask à área de trabalho

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Autotask. Trabalhe em conjunto com [equipa de suporte à área de trabalho Autotask](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) para adicionar os utilizadores na plataforma Autotask à área de trabalho.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso à área de trabalho Autotask.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida à área de trabalho Autotask, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **à área de trabalho Autotask**.

    ![A ligação de área de trabalho Autotask na lista de aplicações](./media/autotaskworkplace-tutorial/tutorial_autotaskworkplace_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Autotask à área de trabalho no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo de área de trabalho Autotask.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)


<!--Image references-->

[1]: ./media/autotaskworkplace-tutorial/tutorial_general_01.png
[2]: ./media/autotaskworkplace-tutorial/tutorial_general_02.png
[3]: ./media/autotaskworkplace-tutorial/tutorial_general_03.png
[4]: ./media/autotaskworkplace-tutorial/tutorial_general_04.png

[100]: ./media/autotaskworkplace-tutorial/tutorial_general_100.png

[200]: ./media/autotaskworkplace-tutorial/tutorial_general_200.png
[201]: ./media/autotaskworkplace-tutorial/tutorial_general_201.png
[202]: ./media/autotaskworkplace-tutorial/tutorial_general_202.png
[203]: ./media/autotaskworkplace-tutorial/tutorial_general_203.png

