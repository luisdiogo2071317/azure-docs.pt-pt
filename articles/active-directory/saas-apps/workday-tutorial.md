---
title: 'Tutorial: Integração do Azure Active Directory com o Workday | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Workday.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2018
ms.author: jeedes
ms.openlocfilehash: 65b103d9dd4a2d50d9d51aabb1728d759351a548
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39420996"
---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a>Tutorial: Integração do Azure Active Directory com o Workday

Neste tutorial, saiba como integrar o dia de trabalho com o Azure Active Directory (Azure AD).

Integração do Workday com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Workday.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada no Workday (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Workday, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um dia de trabalho início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Workday da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-workday-from-the-gallery"></a>Adicionando o Workday da Galeria
Para configurar a integração do Workday para o Azure AD, terá de adicionar dia de trabalho a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar dia de trabalho a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Workday**, selecione **Workday** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Dia de trabalho na lista de resultados](./media/workday-tutorial/tutorial_workday_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o Workday com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no Workday a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Workday deve ser estabelecido.

No Workday, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Workday, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste do Workday](#create-a-workday-test-user)**  - para ter um equivalente da Eduarda Almeida no Workday que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do Workday.

**Para configurar o Azure AD início de sessão único com o Workday, execute os seguintes passos:**

1. No portal do Azure, sobre o **Workday** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/workday-tutorial/tutorial_workday_samlbase.png)

1. Sobre o **Workday domínio e URLs** secção, execute os seguintes passos:

    ![Workday URLs de domínio e única informações de início de sessão](./media/workday-tutorial/tutorial_workday_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://impl.workday.com/<tenant>/login-saml2.htmld`

    b. Na **identificador** caixa de texto, escreva um URL: `http://www.workday.com`

1. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte:

    ![Workday URLs de domínio e única informações de início de sessão](./media/workday-tutorial/tutorial_workday_url1.png)

    Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://impl.workday.com/<tenant>/login-saml.htmld`
     
    > [!NOTE] 
    > Estes valores não são o real. Atualize estes valores com o URL de início de sessão real e o URL de resposta. O URL de resposta tem de ter um subdomínio por exemplo: www, wd2, wd3, wd3 impl, wd5, wd5 impl). Através de algo como "*http://www.myworkday.com*" funciona, mas "*http://myworkday.com*" não existir. Contacte [equipa de suporte de cliente do Workday](https://www.workday.com/en-us/partners-services/services/support.html) obter esses valores.  

1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/workday-tutorial/tutorial_workday_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/workday-tutorial/tutorial_general_400.png)
    
1. Na **configuração do Workday** secção, clique em **configurar Workday** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração do workday](./media/workday-tutorial/tutorial_workday_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Workday como administrador.

1. Na **caixa de pesquisa** pesquisa com o nome **Editar configuração de inquilino – segurança** na parte superior esquerda do lado da home page.
   
    ![Editar a segurança de inquilino](./media/workday-tutorial/IC782925.png "Editar segurança do inquilino")

1. Na **URLs de redirecionamento** secção, execute os seguintes passos:
   
    ![URLs de redirecionamento](./media/workday-tutorial/IC7829581.png "URLs de redirecionamento")
   
    a. Clique em **Adicionar linha**.
   
    b. Na **URL de redireccionamento de início de sessão** caixa de texto e o **URL de redirecionamento do Mobile** caixa de texto, tipo o **URL de início de sessão** inserido o **Workday domínio e URLs** secção do portal do Azure.
   
    c. No portal do Azure, no **configurar início de sessão** janela, copie a **URL de fim de sessão**e, em seguida, cole-o para o **URL de redirecionamento de fim de sessão** caixa de texto.

    d. Na **utilizado para ambientes** caixa de texto, selecione o nome do ambiente.  

    >[!NOTE]
    > O valor do atributo de ambiente está associado ao valor do URL de inquilino:  
    >-Se o nome de domínio do URL de inquilino do Workday começa com impl por exemplo: *https://impl.workday.com/\<tenant\>/login-saml2.htmld*), o **ambiente** atributo tem de ser definido para a implementação.  
    >– Se o nome de domínio é iniciado com algo mais, terá de contactar [equipa de suporte de cliente do Workday](https://www.workday.com/en-us/partners-services/services/support.html) para obter a correspondência **ambiente** valor.

1. Na **programa de configuração de SAML** secção, execute os seguintes passos:
   
    ![Configuração SAML](./media/workday-tutorial/IC782926.png "configuração SAML")
   
    a.  Selecione **ativar a autenticação SAML**.
   
    b.  Clique em **Adicionar linha**.

1. Na **fornecedores de identidade de SAML** secção, execute os seguintes passos:
   
    ![Fornecedores de identidade SAML](./media/workday-tutorial/IC7829271.png "fornecedores de identidade SAML")
   
    a. Na **nome do fornecedor de identidade** caixa de texto, escreva um nome de fornecedor (por exemplo: *SPInitiatedSSO*).
   
    b. No portal do Azure, no **configurar início de sessão** janela, copie a **ID de entidade de SAML** valor e, em seguida, cole-o para o **emissor** caixa de texto.

    ![Fornecedores de identidade SAML](./media/workday-tutorial/IC7829272.png "fornecedores de identidade SAML")
   
    c. No portal do Azure, no **configurar início de sessão** janela, copie a **URL de fim de sessão** valor e, em seguida, cole-o para o **URL de resposta de fim de sessão** caixa de texto.

    d. No portal do Azure, no **configurar o início de sessão** janela, copie a **SAML único início de sessão no URL do serviço** valor e, em seguida, cole-o no **URL do serviço de SSO de IdP** caixa de texto.

    e. Na **utilizado para ambientes** caixa de texto, selecione o nome do ambiente.

    f. Clique em **certificado de chave pública fornecedor de identidade**e, em seguida, clique em **criar**. 

    ![Crie](./media/workday-tutorial/IC782928.png "criar")

    g. Clique em **criar x509 chave pública**. 

    ![Crie](./media/workday-tutorial/IC782929.png "criar")

1. Na **chave pública do Vista x509** secção, execute os seguintes passos: 
   
    ![Chave pública do Vista x509](./media/workday-tutorial/IC782930.png "vista x509 de chave pública") 
   
    a. Na **Name** caixa de texto, escreva um nome para o seu certificado (por exemplo: *PPE\_SP*).
   
    b. Na **válido de** caixa de texto, escreva a partir do valor de atributo do seu certificado válido.
   
    c.  Na **válido para** caixa de texto, digite o valor de atributo do seu certificado válido.
   
    > [!NOTE]
    > Pode obter o válido de data e a válido até à data de certificado transferido clicando nela.  As datas estão listadas na **detalhes** separador.
    > 
    >
   
    d.  Abra o certificado com codificação base 64 no bloco de notas e, em seguida, copie o conteúdo do mesmo.
   
    e.  Na **certificado** caixa de texto, cole o conteúdo da sua área de transferência.
   
    f.  Clique em **OK**.

1. Execute os seguintes passos: 
   
    ![Configuração de SSO](./media/workday-tutorial/WorkdaySSOConfiguratio.png "configuração de SSO")
   
    a.  Na **ID do fornecedor de serviço** caixa de texto, tipo **http://www.workday.com**.
   
    b. Selecione **não Deflate o pedido de autenticação iniciado por SP**.
   
    c. Como **método de assinatura de pedidos de autenticação**, selecione **SHA256**. 
   
    ![Método de assinatura do pedido de autenticação](./media/workday-tutorial/WorkdaySSOConfiguration.png "método de assinatura do pedido de autenticação") 
   
    d. Clique em **OK**. 
   
    ![OK](./media/workday-tutorial/IC782933.png "OK")

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/workday-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/workday-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/workday-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/workday-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-workday-test-user"></a>Criar um utilizador de teste do Workday

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Workday. Trabalhar com [equipa de suporte de cliente do Workday](https://www.workday.com/en-us/partners-services/services/support.html) para adicionar os utilizadores na plataforma do Workday. Os utilizadores tem de ser criados e ativados antes de utilizar o início de sessão único. 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar do Azure-início de sessão único, concedendo acesso no Workday.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida no Workday, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Workday**.

    ![A ligação de dia de trabalho na lista de aplicações](./media/workday-tutorial/tutorial_workday_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de dia de trabalho no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do Workday.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)


<!--Image references-->

[1]: ./media/workday-tutorial/tutorial_general_01.png
[2]: ./media/workday-tutorial/tutorial_general_02.png
[3]: ./media/workday-tutorial/tutorial_general_03.png
[4]: ./media/workday-tutorial/tutorial_general_04.png

[100]: ./media/workday-tutorial/tutorial_general_100.png

[200]: ./media/workday-tutorial/tutorial_general_200.png
[201]: ./media/workday-tutorial/tutorial_general_201.png
[202]: ./media/workday-tutorial/tutorial_general_202.png
[203]: ./media/workday-tutorial/tutorial_general_203.png
