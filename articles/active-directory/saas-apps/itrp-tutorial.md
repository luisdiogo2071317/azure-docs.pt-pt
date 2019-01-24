---
title: 'Tutorial: Integração do Active Directory do Azure com ITRP | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e ITRP.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: e09716a3-4200-4853-9414-2390e6c10d98
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 1ba77414906f90d14809147e532379ae461008dc
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54817576"
---
# <a name="tutorial-azure-active-directory-integration-with-itrp"></a>Tutorial: Integração do Active Directory do Azure com ITRP

Neste tutorial, saiba como integrar ITRP com o Azure Active Directory (Azure AD).

Integrar ITRP no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao ITRP
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para ITRP (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com ITRP, terá dos seguintes itens:

- Uma subscrição do Azure
- Um ITRP logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando ITRP da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-itrp-from-the-gallery"></a>Adicionando ITRP da Galeria
Para configurar a integração do ITRP para o Azure AD, terá de adicionar ITRP a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar ITRP a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **ITRP**.

    ![Criar um utilizador de teste do Azure AD](./media/itrp-tutorial/tutorial_itrp_search.png)

1. No painel de resultados, selecione **ITRP**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/itrp-tutorial/tutorial_itrp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com ITRP com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no ITRP a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no ITRP deve ser estabelecido.

ITRP, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com ITRP, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Utilizador de teste de criação de um ITRP](#creating-an-itrp-test-user)**  - para ter um equivalente da Eduarda Almeida na ITRP que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo ITRP.

**Para configurar o Azure AD início de sessão único com ITRP, execute os seguintes passos:**

1. No portal do Azure, sobre o **ITRP** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/itrp-tutorial/tutorial_itrp_samlbase.png)

1. Sobre o **ITRP domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/itrp-tutorial/tutorial_itrp_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenant-name>.itrp.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenant-name>.itrp.com`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente ITRP](https://www.itrp.com/support) obter esses valores. 
 
1. Sobre o **certificado de assinatura SAML** secção, copie a **THUMBPRINT** valor do certificado.

    ![Configurar o início de sessão único](./media/itrp-tutorial/tutorial_itrp_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/itrp-tutorial/tutorial_general_400.png)

1. Sobre o **ITRP configuração** secção, clique em **configurar ITRP** para abrir **configurar início de sessão** janela. Copiar o **SAML único início de sessão no URL do serviço e o URL de fim de sessão** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/itrp-tutorial/tutorial_itrp_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa ITRP como administrador.

1. Na barra de ferramentas na parte superior, clique em **definições**.
   
    ![ITRP](./media/itrp-tutorial/ic775570.png "ITRP")

1. No painel de navegação esquerdo, selecione **Single Sign-On**.
   
    ![Início de sessão único](./media/itrp-tutorial/ic775571.png "início de sessão único")

1. Na secção de configuração início de sessão único, execute os seguintes passos:
   
    ![Início de sessão único](./media/itrp-tutorial/ic775572.png "início de sessão único")
    
    ![Início de sessão único](./media/itrp-tutorial/ic775573.png "início de sessão único")   

    a. Clique em **Ativar**.

    b. Na **remoto URL terminar sessão** caixa de texto, cole o valor de **URL de fim de sessão**, que copiou do portal do Azure.

    c. Na **URL de SSO SAML** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure.

    d.In **impressão digital de certificado** caixa de texto, colar a **Thumbprint** valor do certificado que copiou do portal do Azure. 
      
1. Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/itrp-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/itrp-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/itrp-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/itrp-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-an-itrp-test-user"></a>Criar um utilizador de teste ITRP

Para ativar a utilizadores do Azure AD iniciar sessão no ITRP, eles têm de ser aprovisionados para ITRP.  

No caso de ITRP, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **ITRP** inquilino.

1. Na barra de ferramentas na parte superior, clique em **registos**.
   
    ![Admin](./media/itrp-tutorial/ic775575.png "Admin")

1. No menu de pop-up, selecione **pessoas**.
   
    ![As pessoas](./media/itrp-tutorial/ic775587.png "pessoas")

1. Clique em **Adicionar nova pessoa** ("+").
   
    ![Admin](./media/itrp-tutorial/ic775576.png "Admin")

1. Na caixa de diálogo Adicionar nova pessoa, execute os seguintes passos:
   
    ![Usuário](./media/itrp-tutorial/ic775577.png "utilizador") 
      
    a. Tipo de **Name**, **E-Mail** de uma conta do AAD válida que pretende aprovisionar.

    b. Clique em **Guardar**.

>[!NOTE]
>Pode utilizar quaisquer outras ITRP utilizador conta criação ferramentas ou APIs fornecidas pelo ITRP para aprovisionar contas de utilizador do AAD. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para ITRP.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a ITRP, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **ITRP**.

    ![Configurar o início de sessão único](./media/itrp-tutorial/tutorial_itrp_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico ITRP no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo ITRP.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/itrp-tutorial/tutorial_general_01.png
[2]: ./media/itrp-tutorial/tutorial_general_02.png
[3]: ./media/itrp-tutorial/tutorial_general_03.png
[4]: ./media/itrp-tutorial/tutorial_general_04.png

[100]: ./media/itrp-tutorial/tutorial_general_100.png

[200]: ./media/itrp-tutorial/tutorial_general_200.png
[201]: ./media/itrp-tutorial/tutorial_general_201.png
[202]: ./media/itrp-tutorial/tutorial_general_202.png
[203]: ./media/itrp-tutorial/tutorial_general_203.png

