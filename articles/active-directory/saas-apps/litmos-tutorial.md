---
title: 'Tutorial: Integração do Azure Active Directory com Litmos | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Litmos.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: jeedes
ms.assetid: cfaae4bb-e8e5-41d1-ac88-8cc369653036
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: a0c70ee6419280b0975d77fb213f9406286708cc
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39428007"
---
# <a name="tutorial-azure-active-directory-integration-with-litmos"></a>Tutorial: Integração do Azure Active Directory com Litmos

Neste tutorial, saiba como integrar Litmos com o Azure Active Directory (Azure AD).

Integrar Litmos no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Litmos.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Litmos (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Litmos, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Litmos logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Litmos da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-litmos-from-the-gallery"></a>Adicionando Litmos da Galeria
Para configurar a integração do Litmos com o Azure AD, terá de adicionar Litmos a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Litmos a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Litmos**, selecione **Litmos** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Litmos na lista de resultados](./media/litmos-tutorial/tutorial_litmos_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Litmos com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Litmos a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Litmos deve ser estabelecido.

Litmos, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Litmos, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Litmos](#create-a-litmos-test-user)**  - para ter um equivalente da Eduarda Almeida na Litmos que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Litmos.

**Para configurar o Azure AD início de sessão único com Litmos, execute os seguintes passos:**

1. No portal do Azure, sobre o **Litmos** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/litmos-tutorial/tutorial_litmos_samlbase.png)

1. Sobre o **Litmos domínio e URLs** secção, execute os seguintes passos:

    ![Litmos domínio e URLs únicas início de sessão em informações](./media/litmos-tutorial/tutorial_litmos_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.litmos.com/account/Login`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.litmos.com/integration/samllogin`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o real identificador e o URL de resposta, que são explicadas mais tarde no tutorial ou contacte [equipa de suporte de Litmos](https://www.litmos.com/contact-us/) obter esses valores.

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/litmos-tutorial/tutorial_litmos_certificate.png)

1. Como parte da configuração, terá de personalizar os **atributos de Token SAML** para a sua aplicação Litmos.

    ![Secção de atributo](./media/litmos-tutorial/tutorial_attribute.png)
           
    | Nome do Atributo   | Valor do Atributo |   
    | ---------------  | ----------------|
    | FirstName |User.givenName |
    | LastName  |User.Surname |
    | Email |User.Mail |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Adicionar atributo](./media/litmos-tutorial/tutorial_attribute_04.png)

    ![Adicionar atributo Dailog](./media/litmos-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **OK**.     

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/litmos-tutorial/tutorial_general_400.png)

1. Numa janela do browser diferente, início de sessão no site da sua empresa Litmos como administrador.

1. Na barra de navegação no lado esquerdo, clique em **contas**.
   
    ![Secção de contas no lado de aplicação][22] 

1. Clique nas **integrações** separador.
   
    ![Guia de integração][23] 

1. Sobre o **integrações** separador, desloque para baixo até **3rd Party integrações**e, em seguida, clique em **SAML 2.0** separador.
   
    ![SAML 2.0 secção][24] 

1. Copie o valor sob **SAML o ponto final para litmos é:** e cole-o no **URL de resposta** caixa de texto no **Litmos domínio e URLs** secção no portal do Azure. 
   
    ![Ponto final de SAML][26] 

1. No seu **Litmos** aplicativo, execute os seguintes passos:
    
     ![Aplicação de Litmos][25] 
     
     a. Clique em **ativar SAML**.
    
     b. Abra o certificado com codificação base 64 no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado X.509 de SAML** caixa de texto.
     
     c. Clique em **guardar alterações**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/litmos-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/litmos-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/litmos-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/litmos-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
  
### <a name="create-a-litmos-test-user"></a>Criar um utilizador de teste Litmos

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Litmos.  
A aplicação de Litmos suporta Just-in-Time de aprovisionamento. Isso significa, uma conta de utilizador é criada automaticamente se for necessário durante uma tentativa de aceder à aplicação através do painel de acesso.

**Para criar um usuário chamado Eduarda Almeida no Litmos, execute os seguintes passos:**

1. Numa janela do browser diferente, início de sessão no site da sua empresa Litmos como administrador.

1. Na barra de navegação no lado esquerdo, clique em **contas**.
   
    ![Secção de contas no lado de aplicação][22] 

1. Clique nas **integrações** separador.
   
    ![Separador de integrações][23] 

1. Sobre o **integrações** separador, desloque para baixo até **3rd Party integrações**e, em seguida, clique em **SAML 2.0** separador.
   
    ![SAML 2.0][24] 
    
1. Selecione **gerar automaticamente utilizadores**
   
    ![Utilizadores de gerar automaticamente][27] 

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Litmos.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Litmos, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Litmos**.

    ![A ligação de Litmos na lista de aplicações](./media/litmos-tutorial/tutorial_litmos_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.  

Quando clica no mosaico Litmos no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo Litmos. 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/litmos-tutorial/tutorial_general_01.png
[2]: ./media/litmos-tutorial/tutorial_general_02.png
[3]: ./media/litmos-tutorial/tutorial_general_03.png
[4]: ./media/litmos-tutorial/tutorial_general_04.png
[21]: ./media/litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/litmos-tutorial/tutorial_litmos_66.png

[100]: ./media/litmos-tutorial/tutorial_general_100.png

[200]: ./media/litmos-tutorial/tutorial_general_200.png
[201]: ./media/litmos-tutorial/tutorial_general_201.png
[202]: ./media/litmos-tutorial/tutorial_general_202.png
[203]: ./media/litmos-tutorial/tutorial_general_203.png

