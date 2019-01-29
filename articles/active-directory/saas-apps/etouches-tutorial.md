---
title: 'Tutorial: Integração do Active Directory do Azure com etouches | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e etouches.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 76cccaa8-859c-4c16-9d1d-8a6496fc7520
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 801b0562c846ebfeaa94572e060750a024da3da2
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55185538"
---
# <a name="tutorial-azure-active-directory-integration-with-etouches"></a>Tutorial: Integração do Active Directory do Azure com etouches

Neste tutorial, saiba como integrar etouches com o Azure Active Directory (Azure AD).

Integrar etouches no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao etouches
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para etouches (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com etouches, terá dos seguintes itens:

- Uma subscrição do Azure
- Um etouches logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando etouches da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-etouches-from-the-gallery"></a>Adicionando etouches da Galeria
Para configurar a integração do etouches com o Azure AD, terá de adicionar etouches a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar etouches a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **etouches**, selecione **etouches** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![etouches na lista de resultados](./media/etouches-tutorial/tutorial_etouches_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com etouches com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no etouches a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no etouches deve ser estabelecido.

Etouches, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com etouches, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste etouches](#create-an-etouches-test-user)**  - para ter um equivalente da Eduarda Almeida na etouches que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo etouches.

**Para configurar o Azure AD início de sessão único com etouches, execute os seguintes passos:**

1. No portal do Azure, sobre o **etouches** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/etouches-tutorial/tutorial_etouches_samlbase.png)

1. Sobre o **etouches domínio e URLs** secção, execute os seguintes passos:

    ![etouches URLs de domínio e única informações de início de sessão](./media/etouches-tutorial/tutorial_etouches_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://www.eiseverywhere.com/saml/accounts/?sso&accountid=<ACCOUNTID>`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://www.eiseverywhere.com/<instance name>`

    > [!NOTE] 
    > Estes valores não são reais. Atualize o valor com o início de sessão real URL e o identificador, o que é explicado mais tarde no tutorial.
    > 

1. aplicação de etouches espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos a partir do **atributo de utilizador** do aplicativo. Captura de ecrã seguinte mostra um exemplo disso. 

    ![Atributo de utilizador](./media/etouches-tutorial/tutorial_etouches_attribute.png) 

1. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem e execute os seguintes passos:
    
    | Nome de atributo | Valor do atributo |
    | ------------------- | -------------------- |
    | Email | user.mail |    
    
    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Adicionar Atributo](./media/etouches-tutorial/tutorial_attribute_04.png)

    ![Adicionar a caixa de diálogo do atributo](./media/etouches-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.
    
    d. Clique em **OK**. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/etouches-tutorial/tutorial_etouches_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/etouches-tutorial/tutorial_general_400.png)

1. Para obter SSO configurado para a sua aplicação, execute os seguintes passos no aplicativo etouches: 

    ![configuração de etouches](./media/etouches-tutorial/tutorial_etouches_06.png) 

    a. Inicie sessão no **etouches** aplicativo usando os direitos de administrador.
   
    b. Vá para o **SAML** configuração.

    c. Na **definições gerais** secção, abra o certificado transferido a partir do portal do Azure no bloco de notas, copie o conteúdo e, em seguida, cole-o na caixa de texto de metadados do IDP. 

    d. Clique nas **guardar e mantenha-se** botão.
  
    e. Clique nas **metadados de atualização** botão na secção de metadados de SAML. 

    f. Isso abre a página e executar o SSO. Depois do SSO está a funcionar, em seguida, pode configurar o nome de utilizador.

    g. No campo de nome de utilizador, selecione o **emailaddress** conforme mostrado na imagem abaixo. 

    h. Cópia a **ID de entidade de SP** valor e cole-o no **identificador** caixa de texto, que está a ser **etouches domínio e URLs** secção no portal do Azure.

    i. Cópia a **URL de SSO / ACS** valor e cole-o no **iniciar sessão no URL** textbox, que está a ser **etouches domínio e URLs** secção no portal do Azure.
   
> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![O botão do Azure Active Directory](./media/etouches-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/etouches-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Botão Adicionar](./media/etouches-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![A caixa de diálogo de utilizador](./media/etouches-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="create-an-etouches-test-user"></a>Criar um utilizador de teste etouches

Nesta secção, vai criar um usuário chamado Eduarda Almeida no etouches. Trabalhar com [equipa de suporte de etouches cliente](https://www.etouches.com/event-software/support/customer-support/) para adicionar os utilizadores na plataforma etouches.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso a etouches.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a etouches, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **etouches**.

    ![A ligação de etouches na lista de aplicações](./media/etouches-tutorial/tutorial_etouches_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único


O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico etouches no painel de acesso, deve obter automaticamente sessão iniciada em seu aplicativo etouches.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/etouches-tutorial/tutorial_general_01.png
[2]: ./media/etouches-tutorial/tutorial_general_02.png
[3]: ./media/etouches-tutorial/tutorial_general_03.png
[4]: ./media/etouches-tutorial/tutorial_general_04.png

[100]: ./media/etouches-tutorial/tutorial_general_100.png

[200]: ./media/etouches-tutorial/tutorial_general_200.png
[201]: ./media/etouches-tutorial/tutorial_general_201.png
[202]: ./media/etouches-tutorial/tutorial_general_202.png
[203]: ./media/etouches-tutorial/tutorial_general_203.png

