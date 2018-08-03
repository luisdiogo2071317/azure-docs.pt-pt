---
title: 'Tutorial: Integração do Azure Active Directory com Zoom | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Zoom.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0ebdab6c-83a8-4737-a86a-974f37269c31
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/28/2017
ms.author: jeedes
ms.openlocfilehash: 57ae31245a356a4cd5769fe71ef471922bf6faf9
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440139"
---
# <a name="tutorial-azure-active-directory-integration-with-zoom"></a>Tutorial: Integração do Azure Active Directory com Zoom

Neste tutorial, saiba como integrar o Zoom com o Azure Active Directory (Azure AD).

Integrar o Zoom no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Zoom.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para aplicar Zoom (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Zoom, precisa do seguinte:

- Uma subscrição do Azure AD
- Um Zoom de logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Zoom da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-zoom-from-the-gallery"></a>Adicionando o Zoom da Galeria
Para configurar a integração de Zoom para o Azure AD, terá de adicionar Zoom a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o Zoom da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Zoom**, selecione **Zoom** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Na lista de resultados de zoom](./media/zoom-tutorial/tutorial_zoom_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Zoom com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Zoom para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Zoom tem de ser estabelecida.

Zoom, o valor de atribuir a **nome de utilizador** no Azure AD como o valor do **nome de utilizador** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Zoom, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de Zoom](#create-a-zoom-test-user)**  - para ter um equivalente da Eduarda Almeida na Zoom que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação de Zoom.

**Para configurar o Azure AD início de sessão único com Zoom, execute os seguintes passos:**

1. No portal do Azure, sobre o **Zoom** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/zoom-tutorial/tutorial_zoom_samlbase.png)

1. Sobre o **Zoom domínio e URLs** secção, execute os seguintes passos:

    ![Domínio e URLs únicas início de sessão em informações de zoom](./media/zoom-tutorial/tutorial_zoom_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<companyname>.zoom.us`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `<companyname>.zoom.us`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente de Zoom](https://support.zoom.us/hc) obter esses valores.

1. A aplicação de Zoom espera que as asserções SAML num formato específico, o que requer a adição de mapeamentos de atributos personalizado à sua configuração de atributos de token SAML. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos da "**atributos de utilizador**" secção na página de integração de aplicações. 

    ![Configurar o início de sessão único](./media/zoom-tutorial/tutorial_attribute.png)

1. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem anterior e execute os seguintes passos:
    
    | Nome do Atributo | Valor do Atributo | Valor de espaço de nomes |
    | ------------------- | -----------|--------- |    
    | Endereço de e-mail | User.Mail | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/mail`|
    | Nome próprio | User.givenName | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`|
    | Apelido | User.Surname | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname `|
    | Número de telefone | User.telephoneNumber | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/phone`|
    | Departamento | User.Department | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/department`|

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/zoom-tutorial/tutorial_attribute_04.png)

    ![Configurar o início de sessão único](./media/zoom-tutorial/tutorial_attribute_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Na **espaço de nomes** caixa de texto, digite o valor de espaço de nomes mostrado para essa linha.
    
    e. Clique em **OK**. 
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/zoom-tutorial/tutorial_zoom_certificate.png)

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/zoom-tutorial/tutorial_general_400.png)

1. Sobre o **Zoom configuração** secção, clique em **configurar Zoom** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de zoom](./media/zoom-tutorial/tutorial_zoom_configure.png)

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Zoom como administrador.

1. Clique nas **Single Sign-On** separador.
   
    ![Guia de início de sessão único](./media/zoom-tutorial/IC784700.png "início de sessão único")

1. Clique nas **controlo de segurança** separador e, em seguida, vá para o **Single Sign-On** definições.

1. Na secção de início de sessão único, execute os seguintes passos:
   
    ![Único início de sessão na secção](./media/zoom-tutorial/IC784701.png "início de sessão único")
   
    a. Na **URL da página de início de sessão** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** que copiou do portal do Azure.
   
    b. Na **URL da página de início de sessão** caixa de texto, cole o valor de **URL de fim de sessão** que copiou do portal do Azure.
     
    c. Abra o certificado com codificação base 64 no bloco de notas, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado do fornecedor de identidade** caixa de texto.

    d. Na **emissor** caixa de texto, cole o valor de **ID de entidade de SAML** que copiou do portal do Azure. 

    e. Clique em **Guardar**.

    > [!NOTE] 
    > Para obter mais informações, visite a documentação de zoom [https://zoomus.zendesk.com/hc/en-us/articles/115005887566](https://zoomus.zendesk.com/hc/en-us/articles/115005887566)

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/zoom-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/zoom-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/zoom-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/zoom-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-a-zoom-test-user"></a>Criar um utilizador de teste de Zoom

Para habilitar os utilizadores do Azure AD iniciar sessão para aplicar Zoom, tem de ser aprovisionados em Zoom. No caso de Zoom, o aprovisionamento é uma tarefa manual.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Para Aprovisionar uma conta de utilizador, execute os seguintes passos:

1. Inicie sessão no seu **Zoom** site da empresa como administrador.
 
1. Clique nas **gestão de contas** separador e, em seguida, clique em **gestão de utilizadores**.

1. Na secção Gestão de utilizadores, clique em **adicionar utilizadores**.
   
    ![Gestão de utilizadores](./media/zoom-tutorial/IC784703.png "gestão de utilizadores")

1. Sobre o **adicionar utilizadores** página, execute os seguintes passos:
   
    ![Adicionar utilizadores](./media/zoom-tutorial/IC784704.png "adicionar utilizadores")
   
    a. Como **tipo de utilizador**, selecione **básica**.

    b. Na **E-mails** caixa de texto, escreva o endereço de e-mail de um Azure AD válido conta que pretende aprovisionar.

    c. Clique em **Adicionar**.

> [!NOTE]
> Pode utilizar quaisquer outras Zoom utilizador conta criação ferramentas ou APIs fornecidas pelo Zoom para aprovisionar o Azure Active Directory, contas de utilizador.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso para aplicar Zoom.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida para aplicar Zoom, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Zoom**.

    ![A ligação de Zoom na lista de aplicações](./media/zoom-tutorial/tutorial_zoom_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de Zoom no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de Zoom.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/zoom-tutorial/tutorial_general_01.png
[2]: ./media/zoom-tutorial/tutorial_general_02.png
[3]: ./media/zoom-tutorial/tutorial_general_03.png
[4]: ./media/zoom-tutorial/tutorial_general_04.png

[100]: ./media/zoom-tutorial/tutorial_general_100.png

[200]: ./media/zoom-tutorial/tutorial_general_200.png
[201]: ./media/zoom-tutorial/tutorial_general_201.png
[202]: ./media/zoom-tutorial/tutorial_general_202.png
[203]: ./media/zoom-tutorial/tutorial_general_203.png

