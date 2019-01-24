---
title: 'Tutorial: Integração do Active Directory do Azure com a área de trabalho Soonr | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Soonr à área de trabalho.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: b75f5f00-ea8b-4850-ae2e-134e5d678d97
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeedes
ms.openlocfilehash: 7dd02aee83d42421cdda57f4feb54dde790db68c
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54809161"
---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a>Tutorial: Integração do Active Directory do Azure com Soonr à área de trabalho

Neste tutorial, saiba como integrar Soonr à área de trabalho com o Azure Active Directory (Azure AD).

Integrar Soonr à área de trabalho no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à área de trabalho Soonr
- Pode permitir que os utilizadores automaticamente obter sessão iniciada em à área de trabalho Soonr (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Soonr à área de trabalho, terá dos seguintes itens:

- Uma subscrição do Azure
- Uma área de trabalho Soonr logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Soonr à área de trabalho a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-soonr-workplace-from-the-gallery"></a>Adicionar Soonr à área de trabalho a partir da Galeria
Para configurar a integração do Soonr à área de trabalho para o Azure AD, terá de adicionar Soonr à área de trabalho a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Soonr à área de trabalho a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **à área de trabalho Soonr**.

    ![Criar um utilizador de teste do Azure AD](./media/soonr-tutorial/tutorial_soonr_search.png)

1. No painel de resultados, selecione **à área de trabalho Soonr**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/soonr-tutorial/tutorial_soonr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Soonr à área de trabalho com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente na área de trabalho Soonr a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na área de trabalho Soonr deve ser estabelecido.

Na área de trabalho Soonr, atribuir o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Soonr à área de trabalho, terá de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de área de trabalho Soonr](#creating-a-soonr-workplace-test-user)**  - para ter um equivalente da Eduarda Almeida na área de trabalho de Soonr que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de área de trabalho Soonr.

**Para configurar o Azure AD início de sessão único com Soonr à área de trabalho, execute os seguintes passos:**

1. No portal do Azure, sobre o **Soonr à área de trabalho** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/soonr-tutorial/tutorial_soonr_samlbase.png)

1. Sobre o **Soonr à área de trabalho domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/soonr-tutorial/tutorial_soonr_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<servername>.soonr.com/singlesignon/saml/metadata`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<servername>.soonr.com/singlesignon/saml/SSO`

1. Sobre o **Soonr à área de trabalho domínio e URLs** secção, se desejar configurar a aplicação no **SP iniciada pelo modo**, execute os seguintes passos:
    
    ![Configurar o início de sessão único](./media/soonr-tutorial/tutorial_soonr_url1.png)

    a. Clique nas **Mostrar definições de URL avançadas**.

    b. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<servername>.soonr.com/singlesignon/saml/SSO`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador real, inicie sessão no URL e o URL de resposta. Contacte [equipa de suporte à área de trabalho Soonr](https://awp.autotask.net/help/) obter esses valores.
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/soonr-tutorial/tutorial_soonr_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/soonr-tutorial/tutorial_general_400.png)

1. Na **configuração da área de trabalho de Soonr** secção, clique em **configurar área de trabalho de Soonr** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/soonr-tutorial/tutorial_soonr_configure.png) 

1. Para configurar o início de sessão único num **à área de trabalho Soonr** lado, terá de enviar o transferido **XML de metadados**, **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** para [equipa de suporte à área de trabalho Soonr](https://awp.autotask.net/help/). Se definir esta definição para que a ligação de SAML SSO definidas corretamente em ambos os lados.

    >[!Note]
    >Se precisar de ajuda com a configuração Autotask à área de trabalho, consulte [esta página](https://awp.autotask.net/help/Content/0_HOME/Support_for_End_Clients.htm) para obter ajuda com sua conta da área de trabalho.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/soonr-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/soonr-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/soonr-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/soonr-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-soonr-workplace-test-user"></a>Criar um utilizador de teste Soonr à área de trabalho

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida na área de trabalho Soonr. Trabalhar com [equipa de suporte à área de trabalho Soonr](https://awp.autotask.net/help/) para criar um utilizador na plataforma. Pode fazer o pedido de suporte Soonr partir <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">aqui</a>.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso à área de trabalho Soonr.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida à área de trabalho Soonr, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **à área de trabalho Soonr**.

    ![Configurar o início de sessão único](./media/soonr-tutorial/tutorial_soonr_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.  

Quando clica no mosaico Soonr à área de trabalho no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo de área de trabalho Soonr.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/soonr-tutorial/tutorial_general_01.png
[2]: ./media/soonr-tutorial/tutorial_general_02.png
[3]: ./media/soonr-tutorial/tutorial_general_03.png
[4]: ./media/soonr-tutorial/tutorial_general_04.png

[100]: ./media/soonr-tutorial/tutorial_general_100.png

[200]: ./media/soonr-tutorial/tutorial_general_200.png
[201]: ./media/soonr-tutorial/tutorial_general_201.png
[202]: ./media/soonr-tutorial/tutorial_general_202.png
[203]: ./media/soonr-tutorial/tutorial_general_203.png

