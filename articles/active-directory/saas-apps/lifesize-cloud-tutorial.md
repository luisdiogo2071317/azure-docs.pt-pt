---
title: 'Tutorial: Integração do Azure Active Directory com nuvem Lifesize | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Lifesize Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: c03456dcda2b3ee44686b070cdebb5fc81c3968c
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39449184"
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Tutorial: Integração do Azure Active Directory com nuvem Lifesize

Neste tutorial, saiba como integrar Lifesize Cloud no Azure Active Directory (Azure AD).

Integrar Lifesize Cloud no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso à nuvem Lifesize
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Lifesize Cloud (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Lifesize Cloud, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Uma Lifesize Cloud logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar Lifesize Cloud a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-lifesize-cloud-from-the-gallery"></a>Adicionar Lifesize Cloud a partir da Galeria
Para configurar a integração da Lifesize Cloud com o Azure AD, terá de adicionar Lifesize Cloud a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Lifesize Cloud a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Lifesize Cloud**.

    ![Criar um utilizador de teste do Azure AD](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_search.png)

1. No painel de resultados, selecione **Lifesize Cloud**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Lifesize Cloud com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente na Lifesize Cloud a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado na Lifesize Cloud deve ser estabelecido.

Na Lifesize Cloud, atribuir o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Lifesize Cloud, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste da Lifesize Cloud](#creating-a-lifesize-cloud-test-user)**  - para ter um equivalente da Eduarda Almeida na Cloud de Lifesize que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação na Cloud de Lifesize.

**Para configurar o Azure AD início de sessão único com Lifesize Cloud, execute os seguintes passos:**

1. No portal do Azure, sobre o **Lifesize Cloud** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_samlbase.png)

1. Sobre o **Lifesize Cloud domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://login.lifesizecloud.com/ls/?acs`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://login.lifesizecloud.com/<companyname>`

     
1. Verifique **Mostrar definições de URL avançadas**, executar o passo seguinte:    
   
    ![Configurar o início de sessão único](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_url1.png)

    Na **estado de reencaminhamento** caixa de texto, escreva um URL com o seguinte padrão: `https://webapp.lifesizecloud.com/?ent=<identifier>`
   
   > [!NOTE] 
   >Tenha em atenção que estes não são os valores reais. terá de atualizar estes valores com o URL de início de sessão, o estado de reencaminhamento e identificador real. Contacte [equipa de suporte de cliente de Cloud Lifesize](https://www.lifesize.com/support) obter o URL de início de sessão e valores de identificador e pode obter o valor de estado de reencaminhamento da configuração de SSO, que é explicado mais tarde no tutorial.

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/lifesize-cloud-tutorial/tutorial_general_400.png)

1. Na **configuração de Cloud Lifesize** secção, clique em **configurar a Cloud de Lifesize** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_configure.png) 

1. Para obter SSO configurado para a sua aplicação, início de sessão na aplicação Lifesize Cloud com privilégios de administrador.

1. No canto superior direito clique no seu nome e, em seguida, clique nas **definições avançadas**.
   
    ![Configurar o início de sessão único](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

1. Nas definições de avanço agora, clique no **SSO configuração** ligação. Isso abrirá a página de configuração de SSO para a sua instância.
   
    ![Configurar o início de sessão único](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

1. Agora, configure os seguintes valores na configuração do SSO da interface do Usuário.    
   
    ![Configurar o início de sessão único](./media/lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)
    
    a. Na **emissor do fornecedor de identidade** caixa de texto, cole o valor de **ID de entidade de SAML** que copiou do portal do Azure.

    b.  Na **URL de início de sessão** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço** que copiou do portal do Azure.

    c. Abra o certificado com codificação base 64 no bloco de notas transferido a partir do portal do Azure, copie o conteúdo do mesmo para a área de transferência e, em seguida, cole-os para o **certificado X.509** caixa de texto.
  
    d. Nos mapeamentos de atributos de SAML para a caixa de texto nome, introduza o valor como **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**
    
    e. No mapeamento de atributo de SAML para o **Apelido** caixa de texto introduza o valor como **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
    
    f. No mapeamento de atributo de SAML para o **E-Mail** caixa de texto introduza o valor como **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

1. Para verificar a configuração, pode clicar no **teste** botão.
   
    >[!NOTE]
    >Para fins de teste concluída com êxito, precisa concluir o Assistente de configuração no Azure AD e também fornecem acesso a utilizadores ou grupos que podem realizar o teste.

1. Ativar o SSO, verificando no **ativar o SSO** botão.

1. Agora, clique no **atualização** botão, para que todas as definições são guardadas. Isso irá gerar o valor de RelayState. Copie o valor de RelayState, que é gerado na caixa de texto, cole-a no **estado de reencaminhamento** caixa de texto em **Lifesize Cloud domínio e URLs** secção. 

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/lifesize-cloud-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/lifesize-cloud-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/lifesize-cloud-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/lifesize-cloud-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-lifesize-cloud-test-user"></a>Criar um utilizador de teste Lifesize Cloud

Nesta secção, vai criar um usuário chamado Eduarda Almeida na Lifesize Cloud. Lifesize cloud suporta o aprovisionamento automático de utilizadores. Após a autenticação com êxito do Azure AD, o utilizador será aprovisionado automaticamente no aplicativo. 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso à nuvem Lifesize.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Lifesize Cloud, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Lifesize Cloud**.

    ![Configurar o início de sessão único](./media/lifesize-cloud-tutorial/tutorial_lifesize-cloud_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico de Lifesize Cloud no painel de acesso, deve obter a página de início de sessão da aplicação de Lifesize Cloud.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/lifesize-cloud-tutorial/tutorial_general_04.png

[100]: ./media/lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/lifesize-cloud-tutorial/tutorial_general_201.png
[202]: ./media/lifesize-cloud-tutorial/tutorial_general_202.png
[203]: ./media/lifesize-cloud-tutorial/tutorial_general_203.png

