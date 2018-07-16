---
title: 'Tutorial: Integração do Azure Active Directory com o Tableau Server | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Tableau Server.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 4c88d6a8b22e4c5c19112c30e15bc960bc8ed1b5
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39045228"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Tutorial: Integração do Azure Active Directory com o Tableau Server

Neste tutorial, saiba como integrar o Tableau Server no Azure Active Directory (Azure AD).

Integrar o Tableau Server no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao servidor da Tableau
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o servidor da Tableau (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Tableau Server, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um servidor da Tableau logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Tableau Server da Galeria
2. Configuração e teste do Azure AD início de sessão único

## <a name="adding-tableau-server-from-the-gallery"></a>Adicionando o Tableau Server da Galeria
Para configurar a integração do Tableau Server no Azure AD, terá de adicionar o servidor da Tableau a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o servidor da Tableau a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

2. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
3. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

4. Na caixa de pesquisa, escreva **Tableau Server**.

    ![Criar um utilizador de teste do Azure AD](./media/tableauserver-tutorial/tutorial_tableauserver_search.png)

5. No painel de resultados, selecione **o servidor da Tableau**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/tableauserver-tutorial/tutorial_tableauserver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o Tableau Server com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no servidor da Tableau a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Tableau Server deve ser estabelecido.

No servidor da Tableau, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Tableau Server, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
2. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
3. **[Criar um utilizador de teste do servidor da Tableau](#creating-a-tableau-server-test-user)**  - para ter um equivalente da Eduarda Almeida no servidor da Tableau que está ligado à representação de utilizador do Azure AD.
4. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
5. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação de servidor da Tableau.

**Para configurar o Azure AD início de sessão único com o Tableau Server, execute os seguintes passos:**

1. No portal do Azure, sobre o **o servidor da Tableau** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

2. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/tableauserver-tutorial/tutorial_tableauserver_samlbase.png)

3. Sobre o **Tableau Server domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/tableauserver-tutorial/tutorial_tableauserver_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://azure.<domain name>.link`
    
    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://azure.<domain name>.link`

    c. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://azure.<domain name>.link/wg/saml/SSO/index.html`
     
    > [!NOTE] 
    > Os valores anteriores não são valores reais. Mais tarde, atualize os valores com o URL e o identificador da página de configuração de servidor da Tableau real. 

4. Aplicação de servidor da tableau espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos a partir do **"Atributos do utilizador"** secção na página de integração de aplicações. Captura de ecrã seguinte mostra um exemplo para o mesmo.
    
    ![Configurar o início de sessão único](./media/tableauserver-tutorial/3.png)
    
5. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo |
    | ---------------| --------------- |    
    | o nome de utilizador | *user.mailnickname* |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/tableauserver-tutorial/tutorial_officespace_04.png)

    ![Configurar o início de sessão único](./media/tableauserver-tutorial/tutorial_officespace_05.png)
    
    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.
    
    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **Ok**


6. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/tableauserver-tutorial/tutorial_tableauserver_certificate.png) 

7. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/tableauserver-tutorial/tutorial_general_400.png)
<CS>
8. Para obter SSO configurado para a sua aplicação, terá de iniciar sessão com o seu inquilino do servidor da Tableau como administrador.
   
   a. Na configuração do servidor da Tableau, clique nas **SAML** separador.
  
    ![Configurar o início de sessão único](./media/tableauserver-tutorial/tutorial_tableauserver_001.png) 
  
   b. Selecione a caixa de seleção de **SAML de utilização para início de sessão único**.
   
   c. URL de retorno de servidor da tableau — o URL que os utilizadores do servidor da Tableau irão aceder aos, tais como http://tableau_server. Usando http://localhost não é recomendado. Utilizar um URL com um traço à direita (por exemplo, http://tableau_server/) não é suportada. Cópia **URL de retorno de servidor da Tableau** e cole-o para o Azure AD **URL de início de sessão** caixa de texto no **Tableau Server domínio e URLs** secção.
   
   d. ID de entidade SAML, o ID da entidade identifica de forma a instalação do servidor da Tableau para o IdP. Pode introduzir o URL de servidor da Tableau novamente aqui, se assim o desejar, mas ele não tem de ser o URL de servidor da Tableau. Cópia **ID de entidade SAML** e cole-o para o Azure AD **identificador** caixa de texto no **Tableau Server domínio e URLs** secção.
     
   e. Clique nas **exportar o ficheiro de metadados** e abri-lo no aplicativo de editor de texto. Localize o URL de serviço de consumidor de asserção com Http Post e de índice 0 e copie o URL. Agora cole-o para o Azure AD **URL de resposta** na caixa de texto **Tableau Server domínio e URLs** secção.
   
   f. Localize o ficheiro de metadados de Federação transferido a partir do portal do Azure e, em seguida, carregue-na **o ficheiro de metadados de SAML Idp**.
   
   g. Clique nas **OK** botão na página de configuração de servidor da Tableau.
   
    >[!NOTE] 
    >Cliente tem de carregar qualquer certificado na configuração da Tableau Server SAML SSO e irá obter ignorada no fluxo SSO.
    >Se precisar de ajudar a configurar o SAML no servidor da Tableau, em seguida, leia este artigo [configurar SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).
    >
<CE>

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/tableauserver-tutorial/create_aaduser_01.png) 

2. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/tableauserver-tutorial/create_aaduser_02.png) 

3. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/tableauserver-tutorial/create_aaduser_03.png) 

4. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/tableauserver-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-tableau-server-test-user"></a>Criar um utilizador de teste do servidor da Tableau

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no servidor da Tableau. Precisa de aprovisionar todos os utilizadores no servidor Tableau. 

Esse nome de utilizador do utilizador deve corresponder ao valor que tiver configurado no atributo personalizado do Azure AD **nome de utilizador**. Com o mapeamento correto a integração deve funcionar [configuração do Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).

>[!NOTE]
>Se precisar de criar manualmente um utilizador, terá de contactar o administrador de servidor da Tableau na sua organização.
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao servidor da Tableau.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Tableau Server, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

2. Na lista de aplicações, selecione **Tableau Server**.

    ![Configurar o início de sessão único](./media/tableauserver-tutorial/tutorial_tableauserver_app.png) 

3. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

4. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

5. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

6. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

7. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de servidor da Tableau no painel de acesso, deve obter automaticamente com sessão iniciada para a aplicação de servidor da Tableau.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [O que é o acesso a aplicações e início de sessão único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/tableauserver-tutorial/tutorial_general_04.png

[100]: ./media/tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/tableauserver-tutorial/tutorial_general_201.png
[202]: ./media/tableauserver-tutorial/tutorial_general_202.png
[203]: ./media/tableauserver-tutorial/tutorial_general_203.png

