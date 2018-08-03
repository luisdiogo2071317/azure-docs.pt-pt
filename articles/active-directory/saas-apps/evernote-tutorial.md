---
title: 'Tutorial: Integração do Azure Active Directory com Evernote | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Evernote.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 7a9282f5418737b583e29d99893df3fc81f52955
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39442637"
---
# <a name="tutorial-azure-active-directory-integration-with-evernote"></a>Tutorial: Integração do Azure Active Directory com Evernote

Neste tutorial, saiba como integrar Evernote com o Azure Active Directory (Azure AD).

Integrar Evernote no Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Evernote.
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Evernote (Single Sign-On) com as suas contas do Azure AD.
- Pode gerir as suas contas num local central – portal do Azure.

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Evernote, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Evernote logon único habilitado subscrição

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode [obtenha uma avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Evernote da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-evernote-from-the-gallery"></a>Adicionando Evernote da Galeria
Para configurar a integração do Evernote com o Azure AD, terá de adicionar Evernote a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Evernote a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![O botão do Azure Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![O painel de aplicações empresariais][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![O novo botão de aplicativo][3]

1. Na caixa de pesquisa, escreva **Evernote**, selecione **Evernote** no painel de resultados, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Evernote na lista de resultados](./media/evernote-tutorial/tutorial_evernote_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar o Azure AD início de sessão único

Nesta secção, configure e teste do Azure AD início de sessão único com Evernote com base num utilizador de teste chamado "Eduarda Almeida".

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Evernote a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Evernote deve ser estabelecido.

Evernote, atribua o valor do **nome de utilizador** no Azure AD como o valor do **Username** para estabelecer a relação de ligação.

Para configurar e testar o Azure AD início de sessão único com Evernote, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configure-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#create-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste Evernote](#create-an-evernote-test-user)**  - para ter um equivalente da Eduarda Almeida na Evernote que está ligado à representação de utilizador do Azure AD.
1. **[Atribua o utilizador de teste do Azure AD](#assign-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Testar início de sessão único](#test-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único em seu aplicativo Evernote.

**Para configurar o Azure AD início de sessão único com Evernote, execute os seguintes passos:**

1. No portal do Azure, sobre o **Evernote** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar a ligação de início de sessão única][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Caixa de diálogo de início de sessão único](./media/evernote-tutorial/tutorial_evernote_samlbase.png)

1. Sobre o **Evernote domínio e URLs** secção, execute os seguintes passos, se desejar configurar a aplicação no modo de IDP iniciado:

    ![Evernote domínio e URLs únicas início de sessão em informações](./media/evernote-tutorial/tutorial_evernote_url.png)

    Na **identificador** caixa de texto, escreva o URL: `https://www.evernote.com/saml2`

1. Verifique **Mostrar definições de URL avançadas** e executar o passo seguinte, se desejar configurar a aplicação na **SP** iniciada pelo modo:

    ![Evernote domínio e URLs únicas início de sessão em informações](./media/evernote-tutorial/tutorial_evernote_url1.png)

    Na **iniciar sessão no URL** caixa de texto, escreva o URL: `https://www.evernote.com/Login.action`   

1. Sobre o **certificado de assinatura SAML** secção, clique em **Certificate(Base64)** e, em seguida, guarde o ficheiro de certificado no seu computador.

    ![O link de download de certificado](./media/evernote-tutorial/tutorial_evernote_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o botão único início de sessão em Guardar](./media/evernote-tutorial/tutorial_general_400.png)

1. Sobre o **Evernote configuração** secção, clique em **configurar Evernote** para abrir **configurar início de sessão** janela. Cópia a **SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configuração de Evernote](./media/evernote-tutorial/tutorial_evernote_configure.png) 

1. Numa janela do browser web diferente, inicie sessão no site da sua empresa Evernote como um administrador.

1. Aceda a **consola de administração**

    ![Consola de administração](./media/evernote-tutorial/tutorial_evernote_adminconsole.png)

1. Do **consola de administração**, aceda à **"Segurança"** e selecione **"Single Sign-On"**

    ![Definição de SSO](./media/evernote-tutorial/tutorial_evernote_sso.png)

1. Configure os seguintes valores:

    ![Definição do certificado](./media/evernote-tutorial/tutorial_evernote_certx.png)
    
    a.  **Ativar SSO:** SSO está ativado por predefinição (clique em **desativar o início de sessão único** para remover o requisito de SSO)

    b. Colar **SAML início de sessão único URL do serviço** valor, que copiou do portal do Azure para o **URL de pedido de HTTP de SAML** caixa de texto.

    c. Abra o certificado transferido a partir do Azure AD no bloco de notas e copie o conteúdo, incluindo "BEGIN CERTIFICATE" e "END CERTIFICATE" e cole-o no **certificado X.509** caixa de texto. 

    d.Click **guardar alterações**

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD

O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

   ![Criar um utilizador de teste do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. No portal do Azure, no painel esquerdo, clique nas **do Azure Active Directory** botão.

    ![O botão do Azure Active Directory](./media/evernote-tutorial/create_aaduser_01.png)

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos**e, em seguida, clique em **todos os utilizadores**.

    !["Os utilizadores e grupos" e os links de "Todos os utilizadores"](./media/evernote-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior a **todos os utilizadores** caixa de diálogo.

    ![Botão Adicionar](./media/evernote-tutorial/create_aaduser_03.png)

1. Na **utilizador** diálogo caixa, execute os seguintes passos:

    ![A caixa de diálogo de utilizador](./media/evernote-tutorial/create_aaduser_04.png)

    a. Na **Name** , escreva **BrittaSimon**.

    b. Na **nome de utilizador** , escreva o endereço de e-mail do utilizador Eduarda Almeida.

    c. Selecione o **mostrar palavra-passe** caixa de verificação e, em seguida, anote o valor que é apresentado na **palavra-passe** caixa.

    d. Clique em **Criar**.
 
### <a name="create-an-evernote-test-user"></a>Criar um utilizador de teste Evernote

Para habilitar logon Evernote de utilizadores do Azure AD, tem de ser aprovisionados em Evernote.  
No caso de Evernote, o aprovisionamento é uma tarefa manual.

**Para aprovisionar contas de utilizador, execute os seguintes passos:**

1. Inicie sessão no site da sua empresa Evernote como administrador.

1. Clique nas **consola de administração**.

    ![Consola de administração](./media/evernote-tutorial/tutorial_evernote_adminconsole.png)

1. Do **consola de administração**, aceda à **"Adicionar utilizadores"**.

    ![TestUser adicionar](./media/evernote-tutorial/create_aaduser_0001.png)

1. **Adicionar membros da Equipe** no **E-Mail** caixa de texto, escreva o endereço de e-mail da conta de utilizador e clique em **convidar.**

    ![TestUser adicionar](./media/evernote-tutorial/create_aaduser_0002.png)
    
1. Depois de convite é enviado, o titular da conta do Azure Active Directory irá receber um e-mail para aceitar o convite.

### <a name="assign-the-azure-ad-test-user"></a>Atribua o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Evernote.

![Atribuir a função de utilizador][200] 

**Para atribuir a Eduarda Almeida a Evernote, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Evernote**.

    ![A ligação de Evernote na lista de aplicações](./media/evernote-tutorial/tutorial_evernote_app.png)  

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![A ligação "Utilizadores e grupos"][202]

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar atribuição][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico Evernote no painel de acesso, deve obter com sessão iniciada para a sua aplicação Evernote. Vai iniciar sessão como uma organização de conta, mas, em seguida, tem de iniciar sessão com a sua conta pessoal. 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/evernote-tutorial/tutorial_general_01.png
[2]: ./media/evernote-tutorial/tutorial_general_02.png
[3]: ./media/evernote-tutorial/tutorial_general_03.png
[4]: ./media/evernote-tutorial/tutorial_general_04.png

[100]: ./media/evernote-tutorial/tutorial_general_100.png

[200]: ./media/evernote-tutorial/tutorial_general_200.png
[201]: ./media/evernote-tutorial/tutorial_general_201.png
[202]: ./media/evernote-tutorial/tutorial_general_202.png
[203]: ./media/evernote-tutorial/tutorial_general_203.png

