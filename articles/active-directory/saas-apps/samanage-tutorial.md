---
title: 'Tutorial: Integração do Azure Active Directory com Samanage | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Samanage.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f0db4fb0-7eec-48c2-9c7a-beab1ab49bc2
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 118ab72c9afc13c5792f229f9c7bc61d226553d5
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39420579"
---
# <a name="tutorial-azure-active-directory-integration-with-samanage"></a>Tutorial: Integração do Azure Active Directory com Samanage

Neste tutorial, saiba como integrar Samanage com o Azure Active Directory (Azure AD).

Integrar Samanage no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Samanage
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Samanage (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Samanage, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Samanage logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Samanage da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-samanage-from-the-gallery"></a>Adicionando Samanage da Galeria
Para configurar a integração do Samanage com o Azure AD, terá de adicionar Samanage a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Samanage a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Samanage**.

    ![Criar um utilizador de teste do Azure AD](./media/samanage-tutorial/tutorial_samanage_search.png)

1. No painel de resultados, selecione **Samanage**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/samanage-tutorial/tutorial_samanage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Samanage com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Samanage a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Samanage deve ser estabelecido.

Samanage, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Samanage, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Samanage](#creating-a-samanage-test-user)**  - para ter um equivalente da Eduarda Almeida na Samanage que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Samanage.

**Para configurar o Azure AD início de sessão único com Samanage, execute os seguintes passos:**

1. No portal do Azure, sobre o **Samanage** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/samanage-tutorial/tutorial_samanage_samlbase.png)

1. Sobre o **Samanage domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/samanage-tutorial/tutorial_samanage_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<Company Name>.samanage.com/saml_login/<Company Name>`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<Company Name>.samanage.com`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão real e o identificador, o que é explicado mais tarde no tutorial. Para obter mais detalhes contacte [equipa de suporte de cliente Samanage](https://www.samanage.com/support).    
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/samanage-tutorial/tutorial_samanage_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/samanage-tutorial/tutorial_general_400.png)

1. Sobre o **Samanage configuração** secção, clique em **configurar Samanage** para abrir **configurar início de sessão** janela. Copiar o **URL de fim de sessão e ID de entidade de SAML** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/samanage-tutorial/tutorial_samanage_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Samanage como um administrador.

1. Clique em **Dashboard** e selecione **configuração** no painel de navegação à esquerda.
   
    ![Dashboard](./media/samanage-tutorial/tutorial_samanage_001.png "Dashboard")

1. Clique em **início de sessão único**.
   
    ![Início de sessão único](./media/samanage-tutorial/tutorial_samanage_002.png "início de sessão único")

1. Navegue para **através de SAML de início de sessão** secção, execute os seguintes passos:
   
    ![Início de sessão através de SAML](./media/samanage-tutorial/tutorial_samanage_003.png "através de SAML de início de sessão")
 
    a. Clique em **ativar o início de sessão único com o SAML**.  
 
    b. Na **URL de fornecedor de identidade** caixa de texto, cole o valor de **ID de entidade de SAML** que copiou do portal do Azure.    
 
    c. Confirmar a **URL de início de sessão** corresponde a **URL de início de sessão** de **Samanage domínio e URLs** secção no portal do Azure.
 
    d. Na **URL de fim de sessão** caixa de texto, introduza o valor de **URL de fim de sessão** que copiou do portal do Azure.
 
    e. Na **emissor de SAML** caixa de texto, escreva o id de aplicação URI definido no seu fornecedor de identidade.
 
    f. Abra o seu certificado codificado de base 64 transferido a partir do portal do Azure no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **colar o seu fornecedor de identidade de x.509 certificado abaixo** caixa de texto.
 
    g. Clique em **criar utilizadores caso não existam no Samanage**.
 
    h. Clique em **atualização**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 
### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/samanage-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/samanage-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/samanage-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/samanage-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-samanage-test-user"></a>Criar um utilizador de teste Samanage

Para ativar a utilizadores do Azure AD iniciar sessão no Samanage, tem de ser aprovisionados em Samanage.  
No caso de Samanage, o aprovisionamento é uma tarefa manual.

**Para Aprovisionar uma conta de utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa Samanage como administrador.

1. Clique em **Dashboard** e selecione **configuração** no pan de navegação à esquerda.
   
    ![Programa de configuração](./media/samanage-tutorial/tutorial_samanage_001.png "configuração")

1. Clique nas **utilizadores** separador
   
    ![Os utilizadores](./media/samanage-tutorial/tutorial_samanage_006.png "utilizadores")

1. Clique em **novo utilizador**.
   
    ![Novo utilizador](./media/samanage-tutorial/tutorial_samanage_007.png "novo utilizador")

1. Tipo de **nome** e o **endereço de E-Mail** de uma conta do Azure Active Directory que pretende aprovisionar e clique em **criar utilizador**.
   
    ![Criar utilizador](./media/samanage-tutorial/tutorial_samanage_008.png "criar utilizador")
   
   >[!NOTE]
   >O titular da conta do Azure Active Directory irá receber um e-mail e seguir uma ligação para confirmar a respetiva conta até se tornar Active Directory. Pode utilizar quaisquer outras Samanage utilizador conta criação ferramentas ou APIs fornecidas pelo Samanage para aprovisionar o Azure Active Directory contas de utilizador.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Samanage.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Samanage, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Samanage**.

    ![Configurar o início de sessão único](./media/samanage-tutorial/tutorial_samanage_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Samanage no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Samanage.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/samanage-tutorial/tutorial_general_01.png
[2]: ./media/samanage-tutorial/tutorial_general_02.png
[3]: ./media/samanage-tutorial/tutorial_general_03.png
[4]: ./media/samanage-tutorial/tutorial_general_04.png

[100]: ./media/samanage-tutorial/tutorial_general_100.png

[200]: ./media/samanage-tutorial/tutorial_general_200.png
[201]: ./media/samanage-tutorial/tutorial_general_201.png
[202]: ./media/samanage-tutorial/tutorial_general_202.png
[203]: ./media/samanage-tutorial/tutorial_general_203.png

