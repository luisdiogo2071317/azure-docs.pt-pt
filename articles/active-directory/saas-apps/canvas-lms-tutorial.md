---
title: 'Tutorial: Integração do Azure Active Directory com tela Lms | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e a tela de LMS.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: bfed291c-a33e-410d-b919-5b965a631d45
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: jeedes
ms.openlocfilehash: af2c997f0842da751eb93f0788a7402fc7d144ae
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39433294"
---
# <a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Tutorial: Integração do Azure Active Directory com LMS baseadas em telas

Neste tutorial, saiba como integrar a tela com o Azure Active Directory (Azure AD).

Integração baseada em telas com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à tela
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para a tela (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Canvas, precisa do seguinte:

- Uma subscrição do Azure AD
- Um tela início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando a tela da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-canvas-from-the-gallery"></a>Adicionando a tela da Galeria
Para configurar a integração da tela com o Azure AD, terá de adicionar baseadas em telas a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar baseadas em telas a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **tela**.

    ![Criar um utilizador de teste do Azure AD](./media/canvas-lms-tutorial/tutorial_canvaslms_search.png)

1. No painel de resultados, selecione **tela**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/canvas-lms-tutorial/tutorial_canvaslms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com tela com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente na tela para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na tela deve ser estabelecido.

Na tela, atribua o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Canvas, precisa concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste baseada em telas](#creating-a-canvas-test-user)**  - para ter um equivalente da Eduarda Almeida na tela que está ligada à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de tela.

**Para configurar o Azure AD início de sessão único com o Canvas, execute os seguintes passos:**

1. No portal do Azure, sobre o **tela** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/canvas-lms-tutorial/tutorial_canvaslms_samlbase.png)

1. Sobre o **baseadas em telas de domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/canvas-lms-tutorial/tutorial_canvaslms_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenant-name>.instructure.com`

    b. Na **identificador** caixa de texto, digite o valor usando o seguinte padrão: `https://<tenant-name>.instructure.com/saml2`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente baseada em telas](https://community.canvaslms.com/community/help) obter esses valores. 
 
1. Sobre o **certificado de assinatura SAML** secção, copie a **THUMBPRINT** valor do certificado.

    ![Configurar o início de sessão único](./media/canvas-lms-tutorial/tutorial_canvaslms_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/canvas-lms-tutorial/tutorial_general_400.png)

1. Na **configuração baseada em telas** secção, clique em **configurar tela** para abrir **configurar início de sessão** janela. Cópia a **alterar o URL da palavra-passe, URL de fim de sessão, ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/canvas-lms-tutorial/tutorial_canvaslms_configure.png) 
 
1. Numa janela do browser web diferente, inicie sessão no site da sua empresa tela como administrador.

1. Aceda a **cursos \> contas geridas \> Microsoft**.
   
    ![Tela](./media/canvas-lms-tutorial/IC775990.png "baseadas em telas")

1. No painel de navegação à esquerda, selecione **autenticação**e, em seguida, clique em **Adicionar nova configuração de SAML**.
   
    ![Autenticação](./media/canvas-lms-tutorial/IC775991.png "autenticação")

1. Na página de integração atual, execute os seguintes passos:
   
    ![Integração atual](./media/canvas-lms-tutorial/IC775992.png "integração atual")

    a. Na **ID de entidade do IdP** caixa de texto, cole o valor de **ID de entidade de SAML** que copiou do portal do Azure.

    b. Na **URL de registo** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** que copiou do portal do Azure.

    c. Na **URL de fim de registo** caixa de texto, cole o valor de **URL de fim de sessão** que copiou do portal do Azure.

    d. Na **ligação de palavra-passe de alteração** caixa de texto, cole o valor de **alterar o URL da palavra-passe** que copiou do portal do Azure. 

    e. Na **impressão digital de certificado** caixa de texto, colar a **Thumbprint** valor do certificado que copiou do portal do Azure.      
        
    f. Partir do **atributo de início de sessão** lista, selecione **NameID**.

    g. Partir do **formato de identificador** lista, selecione **emailAddress**.

    h. Clique em **guardar as definições de autenticação**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/canvas-lms-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/canvas-lms-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/canvas-lms-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/canvas-lms-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-canvas-test-user"></a>Criar um utilizador de teste baseada em telas

Para ativar a utilizadores do Azure AD iniciar sessão na tela, tem de ser aprovisionados para tela.

Em caso de tela, o aprovisionamento de utilizadores é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **tela** inquilino.

1. Aceda a **cursos \> contas geridas \> Microsoft**.
   
   ![Tela](./media/canvas-lms-tutorial/IC775990.png "baseadas em telas")

1. Clique em **Utilizadores**.
   
   ![Os utilizadores](./media/canvas-lms-tutorial/IC775995.png "utilizadores")

1. Clique em **adicionar novo utilizador**.
   
   ![Os utilizadores](./media/canvas-lms-tutorial/IC775996.png "utilizadores")

1. Em Adicionar uma página de diálogo do novo utilizador, execute os seguintes passos:
   
   ![Adicionar utilizador](./media/canvas-lms-tutorial/IC775997.png "adicionar utilizador")
   
   a. Na **FullName** caixa de texto, introduza o nome de utilizador, como **BrittaSimon**.

   b. Na **E-Mail** caixa de texto, introduza o e-mail do utilizador, como **brittasimon@contoso.com**.

   c. Na **início de sessão** caixa de texto, introduza o endereço de e-mail do utilizador do Azure AD, como **brittasimon@contoso.com**.

   d. Selecione **o utilizador sobre a criação de conta de E-Mail**.

   e. Clique em **adicionar utilizador**.

>[!NOTE]
>Pode usar qualquer outras tela utilizador conta ferramentas de criação ou APIs fornecidos pela tela, para aprovisionar contas de utilizador do AAD.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso à tela.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a tela, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **tela**.

    ![Configurar o início de sessão único](./media/canvas-lms-tutorial/tutorial_canvaslms_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de tela no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de tela.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/canvas-lms-tutorial/tutorial_general_01.png
[2]: ./media/canvas-lms-tutorial/tutorial_general_02.png
[3]: ./media/canvas-lms-tutorial/tutorial_general_03.png
[4]: ./media/canvas-lms-tutorial/tutorial_general_04.png

[100]: ./media/canvas-lms-tutorial/tutorial_general_100.png

[200]: ./media/canvas-lms-tutorial/tutorial_general_200.png
[201]: ./media/canvas-lms-tutorial/tutorial_general_201.png
[202]: ./media/canvas-lms-tutorial/tutorial_general_202.png
[203]: ./media/canvas-lms-tutorial/tutorial_general_203.png

