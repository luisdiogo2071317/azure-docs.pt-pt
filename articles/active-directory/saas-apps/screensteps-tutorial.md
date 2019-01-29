---
title: 'Tutorial: Integração do Active Directory do Azure com ScreenSteps | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e ScreenSteps.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: 820446ba30e8743bc80b46fbebf60408c21ea43d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55164729"
---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Tutorial: Integração do Active Directory do Azure com ScreenSteps

Neste tutorial, saiba como integrar ScreenSteps com o Azure Active Directory (Azure AD).

Integrar ScreenSteps no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao ScreenSteps.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para ScreenSteps (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com ScreenSteps, terá dos seguintes itens:

- Uma subscrição do Azure
- Um ScreenSteps logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando ScreenSteps da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-screensteps-from-the-gallery"></a>Adicionando ScreenSteps da Galeria
Para configurar a integração do ScreenSteps com o Azure AD, terá de adicionar ScreenSteps a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar ScreenSteps a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **ScreenSteps**, selecione **ScreenSteps** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![ScreenSteps na lista de resultados](./media/screensteps-tutorial/tutorial_screensteps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com ScreenSteps com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no ScreenSteps a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no ScreenSteps deve ser estabelecido.

ScreenSteps, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com ScreenSteps, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste ScreenSteps](#create-a-screensteps-test-user)**  - para ter um equivalente da Eduarda Almeida na ScreenSteps que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo ScreenSteps.

**Para configurar o Azure AD início de sessão único com ScreenSteps, execute os seguintes passos:**

1. No portal do Azure, sobre o **ScreenSteps** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/screensteps-tutorial/tutorial_screensteps_samlbase.png)

1. Sobre o **ScreenSteps domínio e URLs** secção, execute os seguintes passos:

    ![ScreenSteps domínio e URLs únicas início de sessão em informações](./media/screensteps-tutorial/tutorial_screensteps_url.png)

    Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenantname>.ScreenSteps.com`

    > [!NOTE] 
    > Este valor não é real. Atualize este valor com o início de sessão no URL real, que é explicado posteriormente neste tutorial. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/screensteps-tutorial/tutorial_screensteps_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/screensteps-tutorial/tutorial_general_400.png)

1. Sobre o **ScreenSteps configuração** secção, clique em **configurar ScreenSteps** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de ScreenSteps](./media/screensteps-tutorial/tutorial_screensteps_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa ScreenSteps como um administrador.

1. Clique em **definições da conta**.

    ![Gerenciamento de contas](./media/screensteps-tutorial/ic778523.png "gerenciamento de contas")

1. Clique em **início de sessão único**.

    ![Autenticação remota](./media/screensteps-tutorial/ic778524.png "autenticação remota")

1. Clique em **criar o ponto final de início de sessão único**.

    ![Autenticação remota](./media/screensteps-tutorial/ic778525.png "autenticação remota")

1. Na **ponto final de início de sessão único criar** secção, execute os seguintes passos:

    ![Criar um ponto de final de autenticação](./media/screensteps-tutorial/ic778526.png "criar um ponto de final de autenticação")
    
    a. Na **Title** caixa de texto, escreva um título.
    
    b. Partir do **modo** lista, selecione **SAML**.
    
    c. Clique em **Criar**.

1. **Editar** o novo ponto final.

    ![Editar o ponto final](./media/screensteps-tutorial/ic778528.png "editar o ponto final")

1. Na **ponto final de início de sessão único editar** secção, execute os seguintes passos:

    ![Ponto final de autenticação remota](./media/screensteps-tutorial/ic778527.png "ponto final de autenticação remota")

    a. Clique em **carregamento de novo o ficheiro de certificado SAML**em seguida, carregue o certificado que transferiu a partir do portal do Azure.
    
    b. Colar **SAML único início de sessão no URL do serviço** valor, que copiou do portal do Azure para o **URL de início de sessão remoto** caixa de texto.
    
    c. Colar **URL de fim de sessão** valor, que copiou do portal do Azure para o **URL para terminar sessão** caixa de texto.
    
    d. Selecione um **grupo** para atribuir utilizadores a quando são aprovisionados.
    
    e. Clique em **Atualizar**.

    f. Cópia a **URL de consumidor de SAML** para a área de transferência e colar para o **URL de início de sessão** caixa de texto no **ScreenSteps domínio e URLs** secção.
    
    g. Retorno para o **editar o ponto final de início de sessão único**.
    
    h. Clique nas **predefinido para a conta** botão utilizem este ponto final para todos os utilizadores que iniciem sessão em ScreenSteps. Em alternativa, pode clicar a **adicionar ao Site** botão utilizem este ponto final para sites específicos de **ScreenSteps**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/screensteps-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/screensteps-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/screensteps-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/screensteps-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-screensteps-test-user"></a>Criar um utilizador de teste ScreenSteps

Nesta secção, vai criar um usuário chamado Eduarda Almeida no ScreenSteps. Trabalhar com [equipa de suporte de cliente ScreenSteps](https://www.screensteps.com/contact) para adicionar os utilizadores na plataforma ScreenSteps. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para ScreenSteps.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a ScreenSteps, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **ScreenSteps**.

    ![A ligação de ScreenSteps na lista de aplicações](./media/screensteps-tutorial/tutorial_screensteps_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico ScreenSteps no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo ScreenSteps.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/screensteps-tutorial/tutorial_general_01.png
[2]: ./media/screensteps-tutorial/tutorial_general_02.png
[3]: ./media/screensteps-tutorial/tutorial_general_03.png
[4]: ./media/screensteps-tutorial/tutorial_general_04.png

[100]: ./media/screensteps-tutorial/tutorial_general_100.png

[200]: ./media/screensteps-tutorial/tutorial_general_200.png
[201]: ./media/screensteps-tutorial/tutorial_general_201.png
[202]: ./media/screensteps-tutorial/tutorial_general_202.png
[203]: ./media/screensteps-tutorial/tutorial_general_203.png

