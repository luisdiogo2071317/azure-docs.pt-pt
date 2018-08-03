---
title: 'Tutorial: Integração do Azure Active Directory com aprendizagem no trabalho | Documentos da Microsoft'
description: Saiba como configurar o início de sessão único entre o Azure Active Directory e o Aprendizado no trabalho.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 1d607174-bea1-4f40-8233-54cabe02c66a
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: jeedes
ms.openlocfilehash: 35b02adade9f202480e3649fbfcb24d18c765aa1
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39436044"
---
# <a name="tutorial-azure-active-directory-integration-with-learning-at-work"></a>Tutorial: Integração do Azure Active Directory com aprendizagem no trabalho

Neste tutorial, saiba como integrar o Aprendizado no trabalho com o Azure Active Directory (Azure AD).

Integração de Aprendizado no trabalho com o Azure AD fornece as seguintes vantagens:

- Pode controlar no Azure AD que tenha acesso ao Aprendizado no trabalho
- Pode permitir que os utilizadores automaticamente obter com sessão iniciada ao Aprendizado no trabalho (Single Sign-On) com as suas contas do Azure AD
- Pode gerir as suas contas num local central – portal do Azure

Se quiser saber mais detalhes sobre a integração de aplicações SaaS com o Azure AD, veja [o que é o acesso a aplicações e início de sessão único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com aprendizagem no trabalho, terá dos seguintes itens:

- Uma subscrição do Azure AD
- Um Learning no trabalho início de sessão único na subscrição ativado

> [!NOTE]
> Para testar os passos neste tutorial, recomendamos que não utilize um ambiente de produção.

Para testar os passos neste tutorial, deve seguir estas recomendações:

- Não utilize o seu ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, vai testar do Azure AD início de sessão único num ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Learning no trabalho da Galeria
1. Configuração e teste do Azure AD início de sessão único

## <a name="adding-learning-at-work-from-the-gallery"></a>Adicionando Learning no trabalho da Galeria
Para configurar a integração de Aprendizado no trabalho com o Azure AD, terá de adicionar Learning no trabalho a partir da Galeria à sua lista de aplicações de SaaS geridas.

**Para adicionar Learning no trabalho a partir da galeria, execute os seguintes passos:**

1. Na  **[portal do Azure](https://portal.azure.com)**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone. 

    ![Active Directory][1]

1. Navegue para **aplicações empresariais**. Em seguida, aceda a **todos os aplicativos**.

    ![Aplicações][2]
    
1. Para adicionar nova aplicação, clique em **nova aplicação** botão na parte superior de caixa de diálogo.

    ![Aplicações][3]

1. Na caixa de pesquisa, escreva **de Aprendizado no trabalho**.

    ![Criar um utilizador de teste do Azure AD](./media/learning-at-work-tutorial/tutorial_learningatwork_search.png)

1. No painel de resultados, selecione **no trabalho de Aprendizado**e, em seguida, clique em **Add** botão para adicionar a aplicação.

    ![Criar um utilizador de teste do Azure AD](./media/learning-at-work-tutorial/tutorial_learningatwork_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuração e teste do Azure AD início de sessão único
Nesta secção, configurar e testar o Azure AD início de sessão único com aprendizagem no trabalho com base num utilizador de teste chamado "Eduarda Almeida."

Para o início de sessão único funcione, o Azure AD precisa saber qual é o utilizador equivalente no Learning no trabalho para um utilizador no Azure AD. Em outras palavras, uma relação de ligação entre um utilizador do Azure AD e o utilizador relacionado no Learning no trabalho deve ser estabelecido.

Esta relação de ligação é estabelecida atribuindo o valor do **nome de utilizador** no Azure AD como o valor da **Username** no Learning no trabalho.

Para configurar e testar o Azure AD início de sessão único com aprendizagem no trabalho, terá de concluir os seguintes blocos de construção:

1. **[Configurar o Azure AD início de sessão único](#configuring-azure-ad-single-sign-on)**  - para permitir que os utilizadores utilizar esta funcionalidade.
1. **[Criar um utilizador de teste do Azure AD](#creating-an-azure-ad-test-user)**  - para testar o Azure AD início de sessão único com Eduarda Almeida.
1. **[Utilizador de teste de trabalho a criar um Aprendizado](#creating-a-learning-at-work-test-user)**  - para ter um equivalente da Eduarda Almeida no Learning no trabalho que está ligado à representação de utilizador do Azure AD.
1. **[Atribuir o utilizador de teste do Azure AD](#assigning-the-azure-ad-test-user)**  - para ativar a Eduarda Almeida utilizar o Azure AD início de sessão único.
1. **[Teste de início de sessão único](#testing-single-sign-on)**  - para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do Azure AD início de sessão único

Nesta secção, pode ativar o Azure AD início de sessão único no portal do Azure e configurar início de sessão único na sua aprendizagem para o aplicativo de trabalho.

**Para configurar o Azure AD início de sessão único com aprendizagem no trabalho, execute os seguintes passos:**

1. No portal do Azure, sobre o **de Aprendizado no trabalho** página de integração de aplicativo, clique em **início de sessão único**.

    ![Configurar o início de sessão único][4]

1. Sobre o **início de sessão único** caixa de diálogo, selecione **modo** como **baseado em SAML logon** para ativar o início de sessão único.
 
    ![Configurar o início de sessão único](./media/learning-at-work-tutorial/tutorial_learningatwork_samlbase.png)

1. Sobre o **de Aprendizado no domínio de trabalho e URLs** secção, execute os seguintes passos:

    ![Configurar o início de sessão único](./media/learning-at-work-tutorial/tutorial_learningatwork_url.png)

    a. Na **URL de início de sessão** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.sabacloud.com/Saba/Web/<company code>`

    b. Na **identificador** caixa de texto, escreva um URL com o seguinte padrão: `https://<subdomain>.sabacloud.com/Saba/saml/SSO/alias/<company name>`

    > [!NOTE] 
    > Estes valores não são o real. Atualize estes valores com o URL de início de sessão e o identificador real. Contacte [Learning na equipa de suporte de cliente de trabalho](https://www.learninga-z.com/site/contact/support) obter esses valores. 
 
1. Sobre o **certificado de assinatura SAML** secção, clique em **XML de metadados** e, em seguida, guarde o ficheiro de metadados no seu computador.

    ![Configurar o início de sessão único](./media/learning-at-work-tutorial/tutorial_learningatwork_certificate.png) 

1. Clique em **guardar** botão.

    ![Configurar o início de sessão único](./media/learning-at-work-tutorial/tutorial_general_400.png)

1. Sobre o **Learning na configuração de trabalho** secção, clique em **Learning configurar no trabalho** para abrir **configurar início de sessão** janela. Cópia a **URL de fim de sessão, o ID de entidade de SAML e o SAML único início de sessão no URL do serviço** partir o **secção de referência rápida.**

    ![Configurar o início de sessão único](./media/learning-at-work-tutorial/tutorial_learningatwork_configure.png) 

1. Para configurar o início de sessão único em **no trabalho de Aprendizado** lado, terá de enviar o transferido **XML de metadados**, **ID de entidade de SAML**, **SAML único início de sessão no serviço URL**, e **URL de fim de sessão** para [Aprendizado no suporte de trabalho](https://www.learninga-z.com/site/contact/support).

> [!TIP]
> Agora pode ler uma versão concisa destas instruções dentro do [portal do Azure](https://portal.azure.com), enquanto estiver a configurar a aplicação!  Depois de adicionar esta aplicação a partir da **do Active Directory > aplicações empresariais** secção, basta clicar o **Single Sign-On** separador e a documentação do embedded através de acesso a  **Configuração** seção na parte inferior. Pode ler mais sobre a funcionalidade de documentação do embedded aqui: [documentação do embedded do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criar um utilizador de teste do Azure AD
O objetivo desta secção é criar um utilizador de teste no portal do Azure chamado Eduarda Almeida.

![Criar utilizador do Azure AD][100]

**Para criar um utilizador de teste no Azure AD, execute os seguintes passos:**

1. Na **portal do Azure**, no painel de navegação esquerdo, clique em **Azure Active Directory** ícone.

    ![Criar um utilizador de teste do Azure AD](./media/learning-at-work-tutorial/create_aaduser_01.png) 

1. Para apresentar a lista de utilizadores, aceda a **utilizadores e grupos** e clique em **todos os utilizadores**.
    
    ![Criar um utilizador de teste do Azure AD](./media/learning-at-work-tutorial/create_aaduser_02.png) 

1. Para abrir o **usuário** caixa de diálogo, clique em **Add** na parte superior da caixa de diálogo.
 
    ![Criar um utilizador de teste do Azure AD](./media/learning-at-work-tutorial/create_aaduser_03.png) 

1. Sobre o **utilizador** caixa de diálogo página, execute os seguintes passos:
 
    ![Criar um utilizador de teste do Azure AD](./media/learning-at-work-tutorial/create_aaduser_04.png) 

    a. Na **Name** caixa de texto, tipo **BrittaSimon**.

    b. Na **nome de utilizador** caixa de texto, tipo a **endereço de e-mail** de BrittaSimon.

    c. Selecione **mostrar palavra-passe** e indique o valor da **palavra-passe**.

    d. Clique em **Criar**.
 
### <a name="creating-a-learning-at-work-test-user"></a>A criar um Aprendizado utilizador de teste de trabalho

Nesta secção, vai criar um usuário chamado Eduarda Almeida no Learning no trabalho. Trabalhar com [de Aprendizado no suporte de trabalho](https://www.learninga-z.com/site/contact/support) para adicionar os utilizadores no Aprendizado na plataforma de trabalho.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuir o utilizador de teste do Azure AD

Nesta secção, vai ativar Eduarda Almeida utilizar o Azure início de sessão único, concedendo acesso ao Aprendizado no trabalho.

![Atribuir utilizador][200] 

**Para atribuir a Eduarda Almeida ao Aprendizado no trabalho, execute os seguintes passos:**

1. No portal do Azure, abra a vista de aplicativos e, em seguida, navegue para a vista de diretório e aceda a **aplicações empresariais** , em seguida, clique em **todos os aplicativos**.

    ![Atribuir utilizador][201] 

1. Na lista de aplicações, selecione **de Aprendizado no trabalho**.

    ![Configurar o início de sessão único](./media/learning-at-work-tutorial/tutorial_learningatwork_app.png) 

1. No menu à esquerda, clique em **utilizadores e grupos**.

    ![Atribuir utilizador][202] 

1. Clique em **adicionar** botão. Em seguida, selecione **utilizadores e grupos** nos **adicionar atribuição** caixa de diálogo.

    ![Atribuir utilizador][203]

1. No **utilizadores e grupos** caixa de diálogo, selecione **Eduarda Almeida** na lista utilizadores.

1. Clique em **selecionar** botão **utilizadores e grupos** caixa de diálogo.

1. Clique em **atribua** botão **adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste de início de sessão único

Nesta secção, vai testar a configuração do Azure AD única início de sessão com o painel de acesso.

Ao clicar o Aprendizado no mosaico de trabalho no painel de acesso, deve obter automaticamente com sessão iniciada para a sua aprendizagem para o aplicativo de trabalho.
Para obter mais informações sobre o painel de acesso, consulte [introdução ao painel de acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicações SaaS com o Azure Active Directory](tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](../manage-apps/what-is-single-sign-on.md) (O que é o acesso a aplicações e o início de sessão único com o Azure Active Directory?)



<!--Image references-->

[1]: ./media/learning-at-work-tutorial/tutorial_general_01.png
[2]: ./media/learning-at-work-tutorial/tutorial_general_02.png
[3]: ./media/learning-at-work-tutorial/tutorial_general_03.png
[4]: ./media/learning-at-work-tutorial/tutorial_general_04.png

[100]: ./media/learning-at-work-tutorial/tutorial_general_100.png

[200]: ./media/learning-at-work-tutorial/tutorial_general_200.png
[201]: ./media/learning-at-work-tutorial/tutorial_general_201.png
[202]: ./media/learning-at-work-tutorial/tutorial_general_202.png
[203]: ./media/learning-at-work-tutorial/tutorial_general_203.png

