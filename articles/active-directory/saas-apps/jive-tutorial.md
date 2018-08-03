---
title: 'Tutorial: Integração do Azure Active Directory com Jive | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e Jive.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 9fc5659a-c116-4a1b-a601-333325a26b46
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: jeedes
ms.openlocfilehash: cebcfb4614d1f685697bed6914f80237e175fb7b
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436561"
---
# <a name="tutorial-azure-active-directory-integration-with-jive"></a>Tutorial: Integração do Azure Active Directory com Jive

Neste tutorial, saiba como integrar Jive com o Azure Active Directory (Azure AD).

Integração do Jive com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Jive
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada para Jive (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais informações sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com Jive, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Jive início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste.
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Jive da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-jive-from-the-gallery"></a>Adicionando Jive da Galeria
Para configurar a integração do Jive com o Azure AD, terá de adicionar Jive a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Jive a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]

1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **Jive**.

    ![Criar um utilizador de teste do Azure AD](./media/jive-tutorial/tutorial_jive_search.png)

1. No painel de resultados, selecione **Jive**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/jive-tutorial/tutorial_jive_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configure e teste do Azure AD início de sessão único com Jive com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador de contraparte no Jive a um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Jive deve ser estabelecido.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor da **Username** no Jive.

Para configurar e testar o Azure AD início de sessão único com Jive, tem de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Criar um utilizador de teste do Jive](#creating-a-jive-test-user)**  - para ter um equivalente da Eduarda Almeida na Jive que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aplicação do Jive.

**Para configurar o Azure AD início de sessão único com Jive, execute os seguintes passos:**

1. No portal do Azure, sobre o **Jive** página de integração de aplicação, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.

    ![Configurar o início de sessão único](./media/jive-tutorial/tutorial_jive_samlbase.png)

1. Sobre o **Jive domínio e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/jive-tutorial/tutorial_jive_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<instance name>.jivecustom.com`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<instance name>.jiveon.com`

    > [!NOTE]
    > Estes valores não são o real. Atualize estes valores com o URL de início de sessão real e o identificador. Contacte [equipa de suporte de cliente do Jive](https://www.jivesoftware.com/services-support/) obter esses valores.

1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro XML em seu computador.

    ![Configurar o início de sessão único](./media/jive-tutorial/tutorial_jive_certificate.png)

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/jive-tutorial/tutorial_general_400.png)

1. Para configurar o início de sessão único num **Jive** lado, início de sessão no seu inquilino do Jive como administrador.

1. No menu na parte superior, clique em "**Saml**."

    ![Configurar o início de sessão único no lado de aplicação](./media/jive-tutorial/tutorial_jive_002.png)

    a. Selecione **ativada** sob a **geral** b do Guia. Clique em de "**guardar todas as definições de saml**" botão.

1. Navegue para a "**metadados de Idp**" separador.

    ![Configurar o início de sessão único no lado de aplicação](./media/jive-tutorial/tutorial_jive_003.png)

    a. Copie o conteúdo do arquivo XML de metadados baixado e, em seguida, cole-o para o **metadados de fornecedor de identidade (IDP)** caixa de texto.

    b. Clique em de "**guardar todas as definições de saml**" botão.

1. Vá para a "**mapeamento do atributo de utilizador**" separador.

    ![Configurar o início de sessão único no lado de aplicação](./media/jive-tutorial/tutorial_jive_004.png)

    a. Na **E-Mail** caixa de texto, copie e cole o nome de atributo do **correio** valor.

    b. Na **nome próprio** caixa de texto, copie e cole o nome de atributo do **givenname** valor.

    c. Na **sobrenome** caixa de texto, copie e cole o nome de atributo do **Apelido** valor.

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/jive-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.

    ![Criar um utilizador de teste do Azure AD](./media/jive-tutorial/create_aaduser_02.png)

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.

    ![Criar um utilizador de teste do Azure AD](./media/jive-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:

    ![Criar um utilizador de teste do Azure AD](./media/jive-tutorial/create_aaduser_04.png)

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.

### <a name="creating-a-jive-test-user"></a>Criar um utilizador de teste do Jive

O objetivo desta secção é criar um usuário chamado Eduarda Almeida no Jive. Jive suporta o aprovisionamento automático de utilizadores, que está por predefinição, ativada. Pode encontrar mais detalhes [aqui](jive-provisioning-tutorial.md) sobre como configurar o aprovisionamento automático de utilizadores.

Se precisar de criar manualmente o utilizador, trabalhar com [equipa de suporte de cliente do Jive](https://www.jivesoftware.com/services-support/) para adicionar os utilizadores na plataforma do Jive.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único ao conceder acesso para Jive.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida a Jive, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **Jive**.

    ![Configurar o início de sessão único](./media/jive-tutorial/tutorial_jive_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Quando clica no mosaico do Jive no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aplicação do Jive.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)
* [Configurar o aprovisionamento do utilizador](jive-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/jive-tutorial/tutorial_general_01.png
[2]: ./media/jive-tutorial/tutorial_general_02.png
[3]: ./media/jive-tutorial/tutorial_general_03.png
[4]: ./media/jive-tutorial/tutorial_general_04.png

[100]: ./media/jive-tutorial/tutorial_general_100.png

[200]: ./media/jive-tutorial/tutorial_general_200.png
[201]: ./media/jive-tutorial/tutorial_general_201.png
[202]: ./media/jive-tutorial/tutorial_general_202.png
[203]: ./media/jive-tutorial/tutorial_general_203.png
