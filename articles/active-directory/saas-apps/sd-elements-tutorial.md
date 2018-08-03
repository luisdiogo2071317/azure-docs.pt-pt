---
title: 'Tutorial: Integração do Azure Active Directory com elementos de SD | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e elementos de SD.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 4d5c830df47ff212d2f4d93eb48001ce3a3e2207
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39446929"
---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a>Tutorial: Integração do Azure Active Directory com elementos de SD

Neste tutorial, saiba como integrar o SD elementos com o Azure Active Directory (Azure AD).

Integrar o SD elementos com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso a elementos de SD
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para elementos de SD (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do AD do Azure com SD elementos, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um SD elementos logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando SD elementos da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-sd-elements-from-the-gallery"></a>Adicionando SD elementos da Galeria
Para configurar a integração de elementos de SD no Azure AD, terá de adicionar elementos de SD a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar elementos de SD a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **SD elementos**.

    ![Criar um utilizador de teste do Azure AD](./media/sd-elements-tutorial/tutorial_sdelements_search.png)

1. No painel de resultados, selecione **elementos SD**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/sd-elements-tutorial/tutorial_sdelements_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com elementos de SD com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte em elementos de SD a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado nos elementos SD deve ser estabelecido.

Elementos de SD, atribua o valor do **nome de utilizador** no Azure AD como o valor da **nome de utilizador** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com SD elementos, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste de elementos de SD](#creating-a-sd-elements-test-user)**  - para ter um equivalente da Eduarda Almeida em elementos de SD que está ligada à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo de elementos de SD.

**Para configurar o Azure AD início de sessão único com elementos de SD, execute os seguintes passos:**

1. No portal do Azure, sobre o **elementos SD** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/sd-elements-tutorial/tutorial_sdelements_samlbase.png)

1. Sobre o **SD elementos de domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/sd-elements-tutorial/tutorial_sdelements_url.png)

    a. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenantname>.sdelements.com/sso/saml2/metadata`

    b. Na **URL de resposta** caixa de texto, escreva um URL com o seguinte padrão: `https://<tenantname>.sdelements.com/sso/saml2/acs/`

    > [!NOTE] 
    > Estes valores não são reais. Atualize estes valores com o identificador real e o URL de resposta. Contacte [equipa de suporte de elementos de SD](mailto:support@sdelements.com) obter esses valores.

1. Aplicação de elementos de SD espera que as asserções SAML num formato específico. Configure as seguintes declarações para esta aplicação. Pode gerir os valores destes atributos a partir do **"Atributo de utilizador"** separador do aplicativo. Captura de ecrã seguinte mostra um exemplo disso.

    ![Configurar o início de sessão único](./media/sd-elements-tutorial/tutorial_sdelements_attribute.png)

1. No **atributos de utilizador** secção sobre o **início de sessão único** caixa de diálogo, configurar o atributo de token de SAML conforme mostrado na imagem e execute os seguintes passos: 

    | Nome do Atributo | Valor do Atributo |
    | --- | --- |
    | e-mail |User.Mail |
    | FirstName |User.givenName |
    | Apelido |User.Surname |

    a. Clique em **adicionar atributo** para abrir o **adicionar atributo** caixa de diálogo.

    ![Configurar o início de sessão único](./media/sd-elements-tutorial/tutorial_officespace_04.png)

    ![Configurar o início de sessão único](./media/sd-elements-tutorial/tutorial_officespace_05.png)

    b. Na **nome** caixa de texto, escreva o nome de atributo apresentado para essa linha.

    c. Partir do **valor** lista, digite o valor de atributo apresentado para essa linha.

    d. Clique em **OK**.
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **certificado (Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![Configurar o início de sessão único](./media/sd-elements-tutorial/tutorial_sdelements_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/sd-elements-tutorial/tutorial_general_400.png)

1. Sobre o **SD elementos de configuração** secção, clique em **Configurar elementos do SD** para abrir **configurar início de sessão** janela. Cópia a **ID de entidade de SAML e SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/sd-elements-tutorial/tutorial_sdelements_configure.png)

1. Para obter o início de sessão único ativada, contacte o seu [equipa de suporte de elementos de SD](mailto:support@sdelements.com) e fornecê-los com o ficheiro de certificado transferido. 

1. Numa janela do browser diferente, início de sessão no seu inquilino de elementos de SD como administrador.

1. No menu na parte superior, clique em **System**e, em seguida **início de sessão único**. 
   
    ![Configurar o início de sessão único](./media/sd-elements-tutorial/tutorial_sd-elements_09.png) 

1. Sobre o **definições de início de sessão único** caixa de diálogo, execute os seguintes passos:
   
    ![Configurar o início de sessão único](./media/sd-elements-tutorial/tutorial_sd-elements_10.png) 
   
    a. Como **tipo de SSO**, selecione **SAML**.
   
    b. Na **ID de entidade do fornecedor de identidade** caixa de texto, cole o valor de **ID de entidade de SAML**, que copiou do portal do Azure. 
   
    c. Na **fornecedor único início de sessão no serviço de identidade** caixa de texto, cole o valor de **SAML único início de sessão no URL do serviço**, que copiou do portal do Azure. 
   
    d. Clique em **Guardar**.

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/sd-elements-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/sd-elements-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/sd-elements-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/sd-elements-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-sd-elements-test-user"></a>Criar um utilizador de teste de elementos de SD

O objetivo desta secção é criar um utilizador chamado Eduarda Almeida em elementos de SD. No caso de elementos de SD, criar utilizadores de elementos de SD é uma tarefa manual.

**Para criar a Eduarda Almeida nos elementos SD, execute os seguintes passos:**

1. Numa janela do browser web, início de sessão no site da sua empresa SD elementos como administrador.

1. No menu na parte superior, clique em **gestão de utilizadores**e, em seguida **utilizadores**.
   
    ![Criar um utilizador de teste de elementos de SD](./media/sd-elements-tutorial/tutorial_sd-elements_11.png) 

1. Clique em **adicionar novo utilizador**.
   
    ![Criar um utilizador de teste de elementos de SD](./media/sd-elements-tutorial/tutorial_sd-elements_12.png)
 
1. Sobre o **adicionar novo utilizador** caixa de diálogo, execute os seguintes passos:
   
    ![Criar um utilizador de teste de elementos de SD](./media/sd-elements-tutorial/tutorial_sd-elements_13.png) 
   
    a. Na **email** caixa de texto, introduza o e-mail do utilizador, como **brittasimon@contoso.com**.
   
    b. Na **nome próprio** caixa de texto, introduza o nome de utilizador, como **Eduarda**.
   
    c. Na **sobrenome** caixa de texto, introduza o apelido do utilizador, como **Simon**.
   
    d. Como **função**, selecione **utilizador**. 
   
    e. Clique em **criar utilizador**.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso aos elementos de SD.

![Atribuir utilizador][200] 

**Para atribuir Eduarda Almeida aos elementos de SD, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **SD elementos**.

    ![Configurar o início de sessão único](./media/sd-elements-tutorial/tutorial_sdelements_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

O objetivo desta secção é testar a configuração do Azure AD única início de sessão com o painel de acesso.
  
Quando clica no mosaico de elementos de SD no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação de elementos de SD.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/sd-elements-tutorial/tutorial_general_01.png
[2]: ./media/sd-elements-tutorial/tutorial_general_02.png
[3]: ./media/sd-elements-tutorial/tutorial_general_03.png
[4]: ./media/sd-elements-tutorial/tutorial_general_04.png

[100]: ./media/sd-elements-tutorial/tutorial_general_100.png

[200]: ./media/sd-elements-tutorial/tutorial_general_200.png
[201]: ./media/sd-elements-tutorial/tutorial_general_201.png
[202]: ./media/sd-elements-tutorial/tutorial_general_202.png
[203]: ./media/sd-elements-tutorial/tutorial_general_203.png

