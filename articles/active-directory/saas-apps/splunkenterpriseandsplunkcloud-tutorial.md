---
title: 'Tutorial: Integração do Azure Active Directory com Splunk Enterprise e Splunk Cloud | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Splunk Enterprise e Splunk Cloud.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: b1eb8f198b0d9e25a6514e19794c9b1d54a4cd3e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436088"
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a>Tutorial: Integração do Azure Active Directory com Splunk Enterprise e Splunk Cloud

Neste tutorial, saiba como integrar o Splunk Enterprise e Splunk Cloud no Azure Active Directory (Azure AD).

Integrar o Splunk Enterprise e Splunk Cloud no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Splunk Enterprise e Splunk Cloud
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Splunk Enterprise e Splunk Cloud (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Splunk Enterprise e Splunk Cloud, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Splunk Enterprise e Splunk Cloud início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. A adição de Splunk Enterprise e Splunk Cloud a partir da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-splunk-enterprise-and-splunk-cloud-from-the-gallery"></a>A adição de Splunk Enterprise e Splunk Cloud a partir da Galeria
Para configurar a integração do Splunk Enterprise e Splunk Cloud no Azure AD, terá de adicionar Splunk Enterprise e Splunk Cloud a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Splunk Enterprise e Splunk Cloud a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Splunk Enterprise e Splunk Cloud**.

    ![Criar um utilizador de teste do Azure AD](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_search.png)

1. No painel de resultados, selecione **Splunk Enterprise e Splunk Cloud**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configurar e testar o Azure AD início de sessão único com Splunk Enterprise e Splunk Cloud com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Splunk Enterprise e Splunk Cloud a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Splunk Enterprise e Splunk Cloud tem de ser estabelecida.

No Splunk Enterprise e Splunk Cloud, atribuir o valor do **nome de utilizador** no Azure AD como o valor da **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Splunk Enterprise e Splunk Cloud, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Splunk Enterprise e Splunk Cloud](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**  - para ter um equivalente da Eduarda Almeida no Splunk Enterprise e Splunk Cloud que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Splunk Enterprise e Splunk Cloud.

**Para configurar o Azure AD início de sessão único com Splunk Enterprise e Splunk Cloud, execute os seguintes passos:**

1. No portal do Azure, sobre o **Splunk Enterprise e Splunk Cloud** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_samlbase.png)

1. Sobre o **Splunk Enterprise e Splunk Cloud domínio e URLs** secção, se desejar configurar a aplicação no **IDP** iniciada pelo modo:

    ![Configurar o início de sessão único](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<splunkserverUrl>/en-US/app/launcher/home`
    
    b. Na **identificador** caixa de texto, escreva o URL do seu servidor Splunk.

    c. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<splunkserver>/saml/acs`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador de real, a URL de resposta e o URL de início de sessão. Aqui iremos sugerir-lhe utilizar o valor único de cadeia de caracteres no identificador de. Contacte [equipa de suporte do Splunk Enterprise e o cliente de Cloud Splunk](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) obter esses valores. 

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_400.png)

1. Para configurar o início de sessão único num **Splunk Enterprise e Splunk Cloud** lado, terá de enviar o transferido **XML de metadados** para [Splunk Enterprise e Splunk Cloud suportam equipe](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support).

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/splunkenterpriseandsplunkcloud-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-splunk-enterprise-and-splunk-cloud-test-user"></a>Criar um utilizador de teste Splunk Enterprise e Splunk Cloud

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Splunk Enterprise e Splunk Cloud. Trabalhar com [equipa de suporte do Splunk Enterprise e Splunk Cloud](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) para adicionar os utilizadores na plataforma do Splunk Enterprise e Splunk Cloud.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso Splunk Enterprise e da nuvem de Splunk.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida para Splunk Enterprise e Splunk Cloud, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Splunk Enterprise e Splunk Cloud**.

    ![Configurar o início de sessão único](./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a sua SSOonfiguration de AD do Azure com o painel de acesso.

Quando clicar o Splunk Enterprise e Splunk Cloud mosaico no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação Splunk Enterprise e Splunk Cloud.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)


<!--Image references-->

[1]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_01.png
[2]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_02.png
[3]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_03.png
[4]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_04.png

[100]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_100.png

[200]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_200.png
[201]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_201.png
[202]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_202.png
[203]: ./media/splunkenterpriseandsplunkcloud-tutorial/tutorial_general_203.png

