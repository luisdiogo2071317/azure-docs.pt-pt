---
title: 'Tutorial: Integração do Active Directory do Azure com Intacct | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Intacct.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: d7075894bde20f56c9323568ceffe8cad4118cbc
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54810096"
---
# <a name="tutorial-azure-active-directory-integration-with-intacct"></a>Tutorial: Integração do Active Directory do Azure com Intacct

Neste tutorial, saiba como integrar Intacct com o Azure Active Directory (Azure AD).

Integrar Intacct no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Intacct
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Intacct (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Intacct, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Intacct logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Intacct da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-intacct-from-the-gallery"></a>Adicionando Intacct da Galeria
Para configurar a integração do Intacct com o Azure AD, terá de adicionar Intacct a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Intacct a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Intacct**.

    ![Criar um utilizador de teste do Azure AD](./media/intacct-tutorial/tutorial_intacct_search.png)

1. No painel de resultados, selecione **Intacct**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/intacct-tutorial/tutorial_intacct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Intacct com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Intacct a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Intacct deve ser estabelecido.

Intacct, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Intacct, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Intacct](#creating-an-intacct-test-user)**  - para ter um equivalente da Eduarda Almeida na Intacct que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Intacct.

**Para configurar o Azure AD início de sessão único com Intacct, execute os seguintes passos:**

1. No portal do Azure, sobre o **Intacct** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/intacct-tutorial/tutorial_intacct_samlbase.png)

1. Sobre o **Intacct domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/intacct-tutorial/tutorial_intacct_url.png)

    Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão:
    | |
    |--|
    | `https://<companyname>.intacct.com/ia/acct/sso_response.phtml`|
    | `https://www.intacct.com/ia/acct/sso_response.phtml` |

    > [!NOTE] 
    > Este valor não é real. Atualize este valor com o URL de resposta real. Contacte [equipa de suporte de Intacct](https://us.intacct.com/support) para obter este valor.

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/intacct-tutorial/tutorial_intacct_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/intacct-tutorial/tutorial_general_400.png)

1. Sobre o **Intacct configuração** secção, clique em **configurar Intacct** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/intacct-tutorial/tutorial_intacct_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Intacct como um administrador.

1. Clique nas **empresa** separador e, em seguida, clique em **informações da empresa**.

    ![Empresa](./media/intacct-tutorial/ic790037.png "empresa")

1. Clique nas **Security** separador e, em seguida, clique em **editar**.

    ![Segurança](./media/intacct-tutorial/ic790038.png "segurança")

1. Na **início de sessão (SSO) único** secção, execute os seguintes passos:

    ![Início de sessão único](./media/intacct-tutorial/ic790039.png "início de sessão único")

    a. Selecione **ativar o início de sessão único em**.

    b. Como **tipo de fornecedor de identidade**, selecione **SAML 2.0**.

    c. Na **URL de emissor** caixa de texto, cole o valor de **ID de entidade de SAML** que copiou do portal do Azure.
   
    d. Na **URL de início de sessão** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** que copiou do portal do Azure.

    e. Abra sua **base 64** codificado de certificado no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado** caixa.
   
    f. Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/intacct-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/intacct-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/intacct-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/intacct-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-an-intacct-test-user"></a>Criar um utilizador de teste Intacct

Para configurar a utilizadores do Azure AD para que possa iniciar Intacct, tem de ser aprovisionados em Intacct. Intacct, o aprovisionamento é uma tarefa manual.

**Para aprovisionar contas de utilizador, execute os seguintes passos:**

1. Inicie sessão no seu **Intacct** inquilino.

1. Clique nas **empresa** separador e, em seguida, clique em **utilizadores**.

    ![Os utilizadores](./media/intacct-tutorial/ic790041.png "utilizadores")
1. Clique nas **adicionar** separador.

    ![Adicione](./media/intacct-tutorial/ic790042.png "adicionar")
1. Na **informações do utilizador** secção, execute os seguintes passos:

    ![Informações do usuário](./media/intacct-tutorial/ic790043.png "informações do utilizador")

    a. Introduza o **ID de utilizador**, o **Apelido**, **nome**, o **endereço de E-Mail**, o **Title**e o **Telefone** de uma conta do Azure AD que pretende aprovisionar para o **informações de utilizador** secção.

    b. Selecione o **privilégios de administrador** de uma conta do Azure AD que pretende aprovisionar.
   
    c. Clique em **Guardar**. O titular da conta do Azure AD recebe uma mensagem de e-mail e segue uma ligação para confirmar a respetiva conta até se tornar Active Directory.

>[!NOTE]
>Para aprovisionar contas de utilizador do Azure AD, pode utilizar outras ferramentas de criação da conta de utilizador de Intacct ou APIs que são fornecidas pelo Intacct.
        
### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Intacct.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Intacct, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Intacct**.

    ![Configurar o início de sessão único](./media/intacct-tutorial/tutorial_intacct_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar seu única início de sessão em configuração do Azure AD utilizando o painel de acesso.

Quando clica no mosaico Intacct no painel de acesso, deve ser automaticamente conectado à sua aplicação Intacct.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/intacct-tutorial/tutorial_general_01.png
[2]: ./media/intacct-tutorial/tutorial_general_02.png
[3]: ./media/intacct-tutorial/tutorial_general_03.png
[4]: ./media/intacct-tutorial/tutorial_general_04.png

[100]: ./media/intacct-tutorial/tutorial_general_100.png

[200]: ./media/intacct-tutorial/tutorial_general_200.png
[201]: ./media/intacct-tutorial/tutorial_general_201.png
[202]: ./media/intacct-tutorial/tutorial_general_202.png
[203]: ./media/intacct-tutorial/tutorial_general_203.png

