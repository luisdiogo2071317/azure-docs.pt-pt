---
title: 'Tutorial: Integração do Azure Active Directory com CloudPassage | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e CloudPassage.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: bfe1f14e-74e4-4680-ac9e-f7355e1c94cc
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: c44b9345da32f907efacfe2b7bb1cf09de0a6345
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39427538"
---
# <a name="tutorial-azure-active-directory-integration-with-cloudpassage"></a>Tutorial: Integração do Azure Active Directory com CloudPassage

Neste tutorial, saiba como integrar CloudPassage com o Azure Active Directory (Azure AD).

Integrar CloudPassage no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao CloudPassage
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para CloudPassage (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com CloudPassage, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um CloudPassage logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando CloudPassage da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-cloudpassage-from-the-gallery"></a>Adicionando CloudPassage da Galeria
Para configurar a integração do CloudPassage com o Azure AD, terá de adicionar CloudPassage a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar CloudPassage a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **CloudPassage**.

    ![Criar um utilizador de teste do Azure AD](./media/cloudpassage-tutorial/tutorial_cloudpassage_search.png)

1. No painel de resultados, selecione **CloudPassage**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/cloudpassage-tutorial/tutorial_cloudpassage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com CloudPassage com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no CloudPassage a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no CloudPassage deve ser estabelecido.

CloudPassage, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com CloudPassage, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste CloudPassage](#creating-a-cloudpassage-test-user)**  - para ter um equivalente da Eduarda Almeida na CloudPassage que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo CloudPassage.

**Para configurar o Azure AD início de sessão único com CloudPassage, execute os seguintes passos:**

1. No portal do Azure, sobre o **CloudPassage** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/cloudpassage-tutorial/tutorial_cloudpassage_samlbase.png)

1. Sobre o **CloudPassage domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/cloudpassage-tutorial/tutorial_cloudpassage_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://portal.cloudpassage.com/saml/init/accountid`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://portal.cloudpassage.com/saml/consume/accountid`. Pode obter o valor deste atributo clicando **documentação de configuração de SSO** no **configurações de logon único** seção do CloudPassage portal.

    ![Configurar o início de sessão único](./media/cloudpassage-tutorial/tutorial_cloudpassage_05.png)
     
    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de resposta real e o URL de início de sessão. Contacte [equipa de suporte de cliente CloudPassage](https://www.cloudpassage.com/company/contact/) obter esses valores. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/cloudpassage-tutorial/tutorial_cloudpassage_certificate.png) 

1. Seu aplicativo CloudPassage espera que as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizado à sua configuração de atributos de token SAML. Captura de ecrã seguinte mostra um exemplo disso.
   
   ![Configurar o início de sessão único](./media/cloudpassage-tutorial/tutorial_cloudpassage_25.png) 

1. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem acima e execute os seguintes passos:

    | Nome do Atributo | Valor do Atributo |
    | --- | --- |
    | FirstName |User.givenName |
    | Apelido |User.Surname |
    | e-mail |User.Mail |
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/cloudpassage-tutorial/tutorial_attribute_04.png)
    
    ![Configurar o início de sessão único](./media/cloudpassage-tutorial/tutorial_attribute_05.png)
    
    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **OK**.

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/cloudpassage-tutorial/tutorial_general_400.png)
    
1. Sobre o **CloudPassage configuração** secção, clique em **configurar CloudPassage** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/cloudpassage-tutorial/tutorial_cloudpassage_configure.png) 

1. Numa janela do browser diferente, início de sessão no site da sua empresa CloudPassage como administrador.

1. No menu na parte superior, clique em **configurações**e, em seguida, clique em **administração de sites**. 
   
    ![Configurar o início de sessão único][12]

1. Clique nas **definições de autenticação** separador. 
   
    ![Configurar o início de sessão único][13]

1. Na **definições de início de sessão único** secção, execute os seguintes passos: 
   
    ![Configurar o início de sessão único][14]

    a. Selecione **sign-on(SSO) ativar único (SSO documentação de configuração)** caixa de verificação.
    
    b. Colar **ID de entidade de SAML** para o **URL de emissor SAML** caixa de texto.
  
    c. Colar **SAML único início de sessão no URL do serviço** para o **URL de ponto final SAML** caixa de texto.
  
    d. Colar **URL de fim de sessão** para o **página de destino de fim de sessão** caixa de texto.
  
    e. Abra o seu certificado transferido no bloco de notas, copie o conteúdo de certificado transferido para a área de transferência e, em seguida, cole-o para o **x 509 certificado** caixa de texto.
  
    f. Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/cloudpassage-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/cloudpassage-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/cloudpassage-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/cloudpassage-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-cloudpassage-test-user"></a>Criar um utilizador de teste CloudPassage

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no CloudPassage.

**Para criar um usuário chamado Eduarda Almeida no CloudPassage, execute os seguintes passos:**

1. Início de sessão na sua **CloudPassage** site da empresa como administrador. 

1. Na barra de ferramentas na parte superior, clique em **configurações**e, em seguida, clique em **administração de sites**. 
   
   ![Criar um utilizador de teste CloudPassage][22] 

1. Clique nas **usuários** separador e, em seguida, clique em **adicionar novo utilizador**. 
   
   ![Criar um utilizador de teste CloudPassage][23]

1. Na **adicionar novo utilizador** secção, execute os seguintes passos: 
   
   ![Criar um utilizador de teste CloudPassage][24]
    
    a. Na **nome próprio** caixa de texto, escreva a Eduarda. 
  
    b. Na **Apelido** caixa de texto, digite Simon.
  
    c. Na **nome de utilizador** caixa de texto, o **E-Mail** caixa de texto e o **volte a escrever E-Mail** caixa de texto, escreva o nome de utilizador da Eduarda no Azure AD.
  
    d. Como **tipo de acesso**, selecione **ativar o acesso do Portal de Halo**.
  
    e. Clique em **Adicionar**.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para CloudPassage.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a CloudPassage, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **CloudPassage**.

    ![Configurar o início de sessão único](./media/cloudpassage-tutorial/tutorial_cloudpassage_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção consiste em testar a configuração de SSO do Azure AD através do painel de acesso.

Quando clica no mosaico CloudPassage no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo CloudPassage.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/cloudpassage-tutorial/tutorial_general_01.png
[2]: ./media/cloudpassage-tutorial/tutorial_general_02.png
[3]: ./media/cloudpassage-tutorial/tutorial_general_03.png
[4]: ./media/cloudpassage-tutorial/tutorial_general_04.png
[12]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_07.png
[13]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_08.png
[14]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_09.png
[15]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_10.png
[22]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_15.png
[23]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_16.png
[24]: ./media/cloudpassage-tutorial/tutorial_cloudpassage_17.png

[100]: ./media/cloudpassage-tutorial/tutorial_general_100.png

[200]: ./media/cloudpassage-tutorial/tutorial_general_200.png
[201]: ./media/cloudpassage-tutorial/tutorial_general_201.png
[202]: ./media/cloudpassage-tutorial/tutorial_general_202.png
[203]: ./media/cloudpassage-tutorial/tutorial_general_203.png

