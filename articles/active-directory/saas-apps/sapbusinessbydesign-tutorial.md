---
title: 'Tutorial: Integração do Active Directory do Azure com o SAP Business ByDesign | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o SAP Business ByDesign.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: 82938920-33ba-47cb-b141-511b46d19e66
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: 1e3e4440b7d6adf9e5082217fe75b1c2d983a778
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54817100"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Tutorial: Integração do Active Directory do Azure com o SAP Business ByDesign

Neste tutorial, saiba como integrar o SAP Business ByDesign com o Azure Active Directory (Azure AD).

Integrar SAP Business ByDesign com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao SAP Business ByDesign.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o SAP Business ByDesign (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o SAP Business ByDesign, terá dos seguintes itens:

- Uma subscrição do Azure
- Um SAP Business ByDesign logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o SAP Business ByDesign da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-sap-business-bydesign-from-the-gallery"></a>Adicionando o SAP Business ByDesign da Galeria
Para configurar a integração do SAP Business ByDesign com o Azure AD, terá de adicionar o SAP Business ByDesign partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar o SAP Business ByDesign partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **SAP Business ByDesign**, selecione **SAP Business ByDesign** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Na lista de resultados do SAP Business ByDesign](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com o SAP Business ByDesign com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no SAP Business ByDesign a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no SAP Business ByDesign deve ser estabelecido.

No SAP Business ByDesign, atribuir o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o SAP Business ByDesign, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de SAP Business ByDesign](#create-an-sap-business-bydesign-test-user)**  - para ter um equivalente da Eduarda Almeida no SAP Business ByDesign que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação SAP Business ByDesign.

**Para configurar o Azure AD início de sessão único com o SAP Business ByDesign, execute os seguintes passos:**

1. No portal do Azure, sobre o **SAP Business ByDesign** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_samlbase.png)

1. Sobre o **SAP Business ByDesign domínio e URLs** secção, execute os seguintes passos:

    ![SAP Business ByDesign domínio e URLs únicas início de sessão em informações](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<servername>.sapbydesign.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<servername>.sapbydesign.com`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [equipa de suporte de cliente do SAP Business ByDesign](https://www.sap.com/products/cloud-analytics.support.html) obter esses valores.

1. Sobre o **atributos de utilizador** secção, execute os seguintes passos:

    ![SAP Business ByDesign atributo secção](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_attribute.png)
    
    a. Na **identificador de utilizador** lista, selecione a **ExtractMailPrefix()** função.
    
    b. Partir do **correio** , selecione o atributo de utilizador que pretende utilizar para a sua implementação. Por exemplo, se pretender utilizar o campo IDdeEmpregado como identificador de utilizador exclusivo e armazenou o valor do atributo a ExtensionAttribute2, em seguida, selecione user.extensionattribute2.     

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![O link de download de certificado](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/sapbusinessbydesign-tutorial/tutorial_general_400.png)

1. Na **configuração do SAP Business ByDesign** secção, clique em **configurar o SAP Business ByDesign** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração do SAP Business ByDesign](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_configure.png) 

1. Para obter SSO configurado para a sua aplicação, execute os seguintes passos:
   
    a. Iniciar sessão portal do SAP Business ByDesign com direitos de administrador.
   
    b. Navegue para **aplicativo e tarefas comuns de gestão de utilizador** e clique nas **fornecedor de identidade** separador.
   
    c. Clique em **novo fornecedor de identidade** e selecione o ficheiro XML de metadados que transferiu do portal do Azure. Ao importar os metadados, o sistema carrega automaticamente o certificado de assinatura necessária e o certificado de encriptação.
   
    ![Configurar o início de sessão único](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)
   
    d. Para incluir o **URL do serviço de consumidor de asserção** para o pedido SAML, selecione **incluem o URL de serviço de consumidor de asserção**.
   
    e. Clique em **ativar o início de sessão único**.
   
    f. Guarde as alterações.
   
    g. Clique nas **meu sistema** separador.
   
    ![Configurar o início de sessão único](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)
   
    h. Colar **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure-o para o **início de sessão do Azure AD no URL** caixa de texto.
   
    ![Configurar o início de sessão único](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)
   
    i. Especifique se o funcionário pode escolher manualmente entre o início de sessão com o ID de utilizador e palavra-passe ou SSO selecionando **seleção de fornecedor de identidade Manual**.
   
    j. Na **URL de SSO** secção, especifique o URL que deve ser utilizado o empregado para início de sessão no sistema. 
    O URL enviado para a lista suspensa de funcionário, pode escolher entre as seguintes opções:
   
    **URL não SSO**
   
    O sistema envia apenas o URL de normais do sistema ao funcionário. O funcionário não pode iniciar sessão através do SSO e tem de utilizar palavra-passe ou certificado em vez disso.
   
    **SSO URL** 
   
    O sistema envia apenas o URL de SSO ao funcionário. O funcionário pode iniciar sessão através do SSO. Pedido de autenticação é redirecionado através do IdP.
   
    **Seleção automática**
   
    Se o SSO não estiver ativo, o sistema envia o URL de normais do sistema ao funcionário. Se o SSO estiver ativo, o sistema verifica se o funcionário tem uma palavra-passe. Se uma palavra-passe estiver disponível, o URL do SSO e o URL de SSO não são enviadas para o funcionário. No entanto, se o funcionário não tiver nenhuma palavra-passe, apenas o URL de SSO é enviado ao funcionário.
   
    k. Guarde as alterações.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/sapbusinessbydesign-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/sapbusinessbydesign-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/sapbusinessbydesign-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/sapbusinessbydesign-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-sap-business-bydesign-test-user"></a>Criar um utilizador de teste de SAP Business ByDesign

Nesta secção, vai criar um usuário chamado Eduarda Almeida no SAP Business ByDesign. Trabalhe em conjunto com [equipa de suporte de cliente do SAP Business ByDesign](https://www.sap.com/products/cloud-analytics.support.html) para adicionar os utilizadores na plataforma do SAP Business ByDesign. 

> [!NOTE]
> Certifique-se de que o valor de NameID deve corresponder com o campo de nome de utilizador na plataforma do SAP Business ByDesign.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao SAP Business ByDesign.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida para SAP Business ByDesign, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **SAP Business ByDesign**.

    ![A ligação de SAP Business ByDesign na lista de aplicações](./media/sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar o início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de SAP Business ByDesign no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação SAP Business ByDesign.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)

<!--Image references-->

[1]: ./media/sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/sapbusinessbydesign-tutorial/tutorial_general_04.png

[100]: ./media/sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/sapbusinessbydesign-tutorial/tutorial_general_201.png
[202]: ./media/sapbusinessbydesign-tutorial/tutorial_general_202.png
[203]: ./media/sapbusinessbydesign-tutorial/tutorial_general_203.png

