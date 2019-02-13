---
title: 'Tutorial: Integração do Active Directory do Azure com o Software de Iglu | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Software de Iglu.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 04f66f10e0efa6d97dfa59ad7b920800296bf8f6
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56166696"
---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Tutorial: Integração do Active Directory do Azure com o Software de Iglu

Neste tutorial, saiba como integrar o Software de Iglu com o Azure Active Directory (Azure AD).

Integrar o Software de Iglu no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Software Iglu
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para o Software de Iglu (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o Software de Iglu, terá dos seguintes itens:

- Uma subscrição do Azure
- Um Software de Iglu logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Iglu Software a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-igloo-software-from-the-gallery"></a>Adicionar Iglu Software a partir da Galeria
Para configurar a integração de Software Iglu com o Azure AD, terá de adicionar Iglu Software a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Iglu Software a partir da galeria, execute os seguintes passos:**

1. Na **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Iglu Software**.

    ![Criar um utilizador de teste do Azure AD](./media/igloo-software-tutorial/tutorial_igloosoftware_search.png)

1. No painel de resultados, selecione **Software Iglu**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/igloo-software-tutorial/tutorial_igloosoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com o Software de Iglu com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Iglu Software a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Iglu Software deve ser estabelecido.

Em Software Iglu, atribuir o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com o Software de Iglu, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de Software Iglu](#creating-an-igloo-software-test-user)**  - para ter um equivalente da Eduarda Almeida na Iglu Software que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de Software Iglu.

**Para configurar o Azure AD início de sessão único com o Software de Iglu, execute os seguintes passos:**

1. No portal do Azure, sobre o **Software Iglu** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/igloo-software-tutorial/tutorial_igloosoftware_samlbase.png)

1. Sobre o **Iglu Software domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/igloo-software-tutorial/tutorial_igloosoftware_url.png)
    
    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<company name>.igloocommmunities.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<company name>.igloocommmunities.com/saml.digest`

    c. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<company name>.igloocommmunities.com/saml.digest`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Contacte [equipa de suporte de cliente de Software Iglu](https://www.igloosoftware.com/services/support) obter esses valores. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/igloo-software-tutorial/tutorial_igloosoftware_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/igloo-software-tutorial/tutorial_general_400.png)
    
1. Na **configuração do Software de Iglu** secção, clique em **configurar Software Iglu** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/igloo-software-tutorial/tutorial_igloosoftware_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Iglu Software como um administrador.

1. Vá para o **painel de controlo**.
   
     ![Painel de controlo](./media/igloo-software-tutorial/ic799949.png "painel de controlo")

1. Na **associação** separador, clique em **definições de início de sessão**.
   
    ![Inscrever-se nas definições](./media/igloo-software-tutorial/ic783968.png "inscrever-se nas definições")

1. Na secção de configuração de SAML, clique em **configurar a autenticação SAML**.
   
    ![Configuração de SAML](./media/igloo-software-tutorial/ic783969.png "configuração de SAML")
   
1. Na **configuração geral** secção, execute os seguintes passos:
   
    ![Configuração geral](./media/igloo-software-tutorial/ic783970.png "configuração geral")

    a. Na **nome da ligação** caixa de texto, escreva um nome personalizado para a sua configuração.
   
    b. Na **URL de início de sessão do IdP** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** que copiou do portal do Azure.
   
    c. Na **URL de fim de sessão do IdP** caixa de texto, cole o valor de **URL de fim de sessão** que copiou do portal do Azure.
    
    d. Selecione **resposta de fim de sessão e o tipo de pedido de HTTP** como **POST**.
   
    e. Abra sua **base 64** certificado codificado no bloco de notas transferido a partir do portal do Azure, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado público** caixa de texto.
    
1. Na **resposta e a configuração da autenticação**, execute os seguintes passos:
    
    ![Resposta e a configuração de autenticação](./media/igloo-software-tutorial/IC783971.png "resposta e a configuração de autenticação")
  
      a. Como **fornecedor de identidade**, selecione **Microsoft ADFS**.
      
      b. Como **tipo de identificador**, selecione **endereço de E-Mail**. 

      c. Na **atributo de correio eletrónico** caixa de texto, tipo **emailaddress**.

      d. Na **atributo de nome próprio** caixa de texto, tipo **givenname**.

      e. Na **último nome do atributo** caixa de texto, tipo **Apelido**.

1. Execute os seguintes passos para concluir a configuração:
    
    ![Criação do utilizador no início de sessão](./media/igloo-software-tutorial/IC783972.png "criação do utilizador no início de sessão") 

     a. Como **criação do utilizador no início de sessão**, selecione **criar um novo utilizador no seu site quando iniciam sessão**.

     b. Como **iniciar sessão nas definições**, selecione **botão de SAML de utilização no ecrã de "Iniciar sessão"**.

     c. Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [Documentação do Azure AD incorporado]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/igloo-software-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/igloo-software-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/igloo-software-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/igloo-software-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-an-igloo-software-test-user"></a>Criar um utilizador de teste de Software Iglu

Não existe nenhum item de ação para configurar o aprovisionamento do utilizador Iglu Software.  

Quando um utilizador atribuído tenta iniciar sessão no Software Iglu usando o painel de acesso, o Software de Iglu verifica se o usuário existe.  Se nenhuma conta de utilizador disponível ainda existe, é criado automaticamente pelo Iglu Software.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso ao Software Iglu.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Software Iglu, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Iglu Software**.

    ![Configurar o início de sessão único](./media/igloo-software-tutorial/tutorial_igloosoftware_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de Software Iglu no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de Software Iglu.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/igloo-software-tutorial/tutorial_general_01.png
[2]: ./media/igloo-software-tutorial/tutorial_general_02.png
[3]: ./media/igloo-software-tutorial/tutorial_general_03.png
[4]: ./media/igloo-software-tutorial/tutorial_general_04.png

[100]: ./media/igloo-software-tutorial/tutorial_general_100.png

[200]: ./media/igloo-software-tutorial/tutorial_general_200.png
[201]: ./media/igloo-software-tutorial/tutorial_general_201.png
[202]: ./media/igloo-software-tutorial/tutorial_general_202.png
[203]: ./media/igloo-software-tutorial/tutorial_general_203.png

